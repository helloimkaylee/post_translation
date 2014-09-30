ICloneable

###카피지만, 카피 아닌, 카피같은 너 (A copy is not a copy)

value type 이 heap에 있는것과 reference type 이 heap에 있는거와 뭐가 다를까?

먼저 value type먼저 살펴보자. 

1. Dude 라는 클래스안에 Name 엘리먼트와 Shoe 2개를 포함한다. 
2. CopyDude()메소드로 Dude 클래스를 새로 만든다.

```
public struct Shoe
{
	public  string Color;
}

public class Dude
{
	public string Name;
	public Shoe RightShoe;
	public Shoe LeftShoe;
	
	public Dude CopyDude()
	{
		Dude newPerson = new Dude();
		newPerson.Name = Name;
		newPerson.LeftShoe = LeftShoe;
		newPerson.RightShoe = RightShoe;
		
		return newPerson;
	}
	
	public override string ToString()
	{
		return (Name + " :Dude!, I Have a " + RightShoe.Color + 
				"shoe on my right foot, and a " + LeftShoe.Color+ " on my left foot");
	}
}


```

1. Dude class는 varialbe type 이다. Shoe struct가 class의 맴버이고 그 클래스는 heap 에 있기때문이다.

![part3_1](http://www.c-sharpcorner.com/UploadFile/rmcochran/chsarp_memory401152006094206AM/Images/heapvsstack3-1.gif)


아래를 실행하면 결과는 어떻게 나올까?




```

public static void Main()
{
	Dude Bill = new Dude();
	Bill.Name = "Bill";
	Bill.LeftShoe = new Shoe();
	Bill.RightShoe = new Shoe();
	Bill.LeftShoe.Color = Bill.RightShoe.Color = "Blue";

	Dude Ted = Bill.CopyDude();
	Ted.Name = "Ted";
	Ted.LeftShoe.Color = Ted.RightShoe.Color = "Red";

	Console.WriteLine(Bill.ToString());
	Console.WriteLine(Ted.ToString());
}


```
output은 아래와 같이 나올 것이다.

```
Bill : Dude!, I have a Blue shoe on my right foot, and a Blue on my left foot.

Ted : Dude!, I have a Red shoe on my right foot, and a Red on my left foot.

```


만약 우리가 Shoe를 reference type으로 만들면 어떻게 될까? 
(위에 Shoe 는 struct로 value type 임)

```
	//struct니까 값타입  --> 이럼 카피한 각각이 있으니 정상, 
	//그러나 요걸 class로 바꾸면 바로 Bill & Ted 모두 빨간신발이 됨.
    public class Shoe
    {
        public string Color;
    }

```

그럼  output은 아까와는 다르게 나올 것이다.

```
Bill : Dude!, I have a Red shoe on my right foot, and a Red on my left foot
Ted : Dude!, I have a Red shoe on my right foot, and a Red on my left foot

```


원치않는 결과가 나왔는가? Heap 에 무슨일이 있었나 볼까?


![part3_2](http://www.c-sharpcorner.com/UploadFile/rmcochran/chsarp_memory401152006094206AM/Images/heapvsstack3-2.gif)


1. Main() 안에서 New Shoe() 를 각각 Left 와 Right에서 했으니 두개의 다른  Shoe객체가 heap에 생긴다.

2. stack에는 heap에 생성된 Shoe 객체를 가르키는 pointer가 생긴다.

4. 그런데 Dude Ted = Bill.CopyDude();을 하면서 Pointer가 복사되었다.
5. 즉, Bill과 Ted는 같은 shoe 를 보고있는것.

우리가 원한건 이게 아닌데....
라고 생각든다면 우리 reference type의 Shoe를 value type 처럼 작동하게 만들 수 있다. 물론 추가작업이 들긴 하지만...


다행스럽게도 우리에겐 ICloneable 이라는 인터페이스가 있었으니..!!!

이 인터페이스는 어떻게 shoe 라는 참조타입이 공유되는 에러를 피하고 우리가 원하는 결과값이 나오게끔 복제가 되는지 알 수 있을 것이다.

ICloneable interface를 사용하려면 반드시 Clone을 만들어줘야 한다.

```
    ICloneable consists of one method: Clone()
    
    public object Clone()
    {
    
    }

	//ICloneable 을 상속해주려면 clone()에 대해서 필요하다.
    public class Shoe : ICloneable
    {
        public string Color;
        #region ICloneable Members
        
        //그래서 여기에 새로 클론을 만들어주면됨.
        public object Clone()
        {
            Shoe newShoe = new Shoe();
            newShoe.Color = Color.Clone() as string;
            return newShoe;
        }
    
    }

```
1. Clone()안에 Shoe를 생성하고, 모든 레퍼런스 타입을 복제하고 value type을 복사하고 새로운 오브젝트를 리턴한다.

2. 이미 알고있겠지만, string 클래스는 이미 ICloneable 을 상속받고 있기 때문에 우리는 그저  Color.Clone() 이라고 바로 쓸 수 있다.

3. CopyDude() 메소드에서는 shoes를 복사(copy)하는게 아니라 복제(clone)해주는게 필요하다.

```
    public Dude CopyDude()
    {
        Dude newPerson = new Dude();
         newPerson.Name = Name;
         
         //clone() 만들어 줬으니 적용해줘야지~!
         newPerson.LeftShoe = LeftShoe.Clone() as Shoe;
         newPerson.RightShoe = RightShoe.Clone() as Shoe;

         return newPerson;
    }



```
 자, 이제 main을 돌리면 우리는 원래 원하던 ooutput을 reference type의 class를 써서, + ICloneable 인터페이스를 써서 구현 하였다!
 
 그럼 지금 Heap 상태는 어떨까?
 
 ![part3_3](http://www.c-sharpcorner.com/UploadFile/rmcochran/chsarp_memory401152006094206AM/Images/heapvsstack3-3.gif)
 


그럼 한단계 더 레벨업 해서 CopyDude() 대신에 ICloneable 써서 깔끔하게 처리해보자.

```

	//좀더 미국코딩스럽게 하기 위해서 Dude에 ICloneable을 상속 해주는 걸로 변경
    public class Dude : ICloneable
    {
        public string Name;
        public Shoe RightShoe;
        public Shoe LeftShoe;
        
        public override string ToString()
        {
            return (Name + " : Dude!, I have a " + RightShoe.Color  +
                    " shoe on my right foot, and a " +
                     LeftShoe.Color + " on my left foot.");
        }
        
        
        public object Clone()
        {
            Dude newPersion = new Dude();
            newPerson.Name = Name.Clone() as string;
            newPerson.LeftShoe = LeftShoe.Clone() as Shoe;
            newPerson.RightShoe = RightShoe.Clone() as Shoe;
            
            return newPerson;
        }
        
        //그리고  Main() 안에는 Dude.Clone()을 사용하도록 수정
        
        public Static void Main()
        {
            Class1 pgm = new Class1();
            
            Dude Bill = new Dude();
            Bill.Name = "Bill";
            Bill.LeftShoe = new Shoe();
            Bill.RightShoe = new Shoe();
            Bill.LeftShoe.Clolr = Bill.RightShoe.Color = "Blue";
            
            //원래 요기가 Bill.CopyDude(); 였음 ㅎㅎ
            //Dude에 clone을 만들어줬으니 클론해주면서 as Dude 라고 해줌.
            Dude Ted = Bill.Clone() as Dude;
            Ted.Name = "Ted";
            Ted.LeftShoe.Color = Ted.RightShoe.Color = "Red";
            
            Console.WriteLine(Bill.ToString());
            Console.WriteLine(Ted.ToString());
        }
    
        
    }

```

###결론
만약 당신이 오브젝트를 카피(복사) 하려고 계획중이라면, 반드시 ICloneable 를 상속해라. 이건 reference type 이 value type 처럼 행동하게 해준다. 

###아직 의문점 이 남았다면 이런거겠지?

그럼 struct로 쓰면되지 왜 class로 써서 ICloneable을 상속받는 둥 난리?
struct로 선언하면 
1. 얕은복사를 할 수 없다. 
2. 2. c# 에서 struct는 valure type  이라 reference로 사용 할 수 없다

는 단점 때문에...!!!
