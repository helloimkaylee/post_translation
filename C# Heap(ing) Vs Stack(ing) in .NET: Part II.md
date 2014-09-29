이번 파트에서는 메소드를 호출할때 일어나는 일을 살펴볼꺼임.

먼저, 우리가 메소드를 호출할때,

1. 메소드를 실행시키기위한 정보를 저장하기위해 메모리 공간이 Stack 에 할당된다. 이를 Stack Frame 이라고 한다.
2. 메소드의 파라메터들은 복사된다. 
3. 컨트롤은 지금 실행되고있는 메소드에 전달되고, 스레드는 코드를 실행시킨다. 이런 이유로 스택프레임을 콜 스택이라고 부르기도 한다.


###Passing Value Type
```
 class Class1
 {
      public void Go()
      {
          int x = 5;
          AddFive(x);

          Console.WriteLine(x.ToString());
          
      }

      public int AddFive(int pValue)
      {
          pValue += 5;
          return pValue;
      }
 }


```
위에 와 같이 메소드 안에서 AddFive라는 메소드를 파라메터와 함께 호출하게 되면, x라는 아규먼트를 복사한 pValue가 생성된다.
AddFive 호출이 다 끝나고 나면 pValue도 AddFive()도 쓸모 없게 되며, 원래 값(x)는 잘 보존되어 있다. 그래서 output의 값은 '5'가 된다.

![partr_!](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory2B01142006125918PM/Images/heapvsstack2-4.gif)

그럼 같은 상황에서 파라메터가 단일값이 아니라 아주 큰  struct라면 어떻게 될까? 똑같이 파라메터를 넘기기위해  그 큰 struct가 복사되어 두개가 생기는 일이 발생할 것 이다. 
```
    public struct MyStruct
    {
    long a, b, c, d, e, f, g, h, i, j, k, l, m;
    }
    
    
    public void Go()
    {
     MyStruct x = new MyStruct();
     DoSomething(x);
      
    }
    
    
    public void DoSomething(MyStruct pValue)
    {
            // DO SOMETHING HERE....
    }
```
아래처럼 말이다!


![part2_2](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory2B01142006125918PM/Images/heapvsstack2-5.gif)


이짓을 수천번 한다고 생각해봐라.. 
이것이야 말로 비효율의 끝장판 아니겠는가!

이문제를 해결하는 방법은?
바로reference로 전달 하는 것이다..!!!
아규먼트앞에 ref 를 붙히고, 받는 파라메터 앞에도 ref를 붙히면된다.

```
  public void Go()
  {
     MyStruct x = new MyStruct();
     DoSomething(ref x);
      
  }

   public struct MyStruct
   {
       long a, b, c, d, e, f, g, h, i, j, k, l, m;
   }

   public void DoSomething(ref MyStruct pValue)
   {
            // DO SOMETHING HERE....
   }


```
ref를 붙히면 파라메터를 복사하는것이 아니라, 원래 아규먼트를 바라보게 된다.
그럼 아래 소스의 output은 얼마가 될까?
```
  public void Go()
  {
     MyStruct x = new MyStruct();
     x.a = 5;
     DoSomething(ref x);

     Console.WriteLine(x.a.ToString());
       
  }

  public void DoSomething(ref MyStruct pValue)
  {
           pValue.a = 12345;
  }


```

x 와 pValue는 같은 것을 보고 있으니, pValue.a가 변경되면 DoSomething 메소드 호출이 끝나더라도 x.a 값이 변경되어 '12345'를 출력할 것이다

이것이 바로 Passing Value Types 방법이다. 




###Passing Reference Type 

```

  public class MyInt
  {
     public int MyValue;
  }
   
  public void Go()
  {
     MyInt x = new MyInt();
     x.MyValue = 2;

     DoSomething(x);

     Console.WriteLine(x.MyValue.ToString());
      
  }

  public void DoSomething(MyInt pValue)
  {
       pValue.MyValue = 12345;
  }



```
1. MyInt는 클래스이고, New로 생성했으니  Reference Type 으로 Heap에 클래스가 생기겠죠? heap을 참조해야되니 stack에는 x 라는 pointer만 생겼을테고,

2. Dosomething에서 파라메터 pValue로 x를 받았으니 x pointer를 복사한 pValue pointer가 생긴다. 이 포인터도 x pointer와 같이 heap을 가르킨다. 

![part2_3](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory2B01142006125918PM/Images/heapvsstack2-8.gif)


같은 것을 가르키고 있고 pValue.MyValue를 '12345'로 변경했으니, x 의 output역시 '12345'가 된다.


그럼 reference type을 reference(ref)시키면 어떻게 될까?

```
    public class Thing
    {
        
    }
    
    public class Animal:Thing
    {
        public int Weight;
    }
    
    public class Vegetable:Thing
    {
        public int Length;
    }
    
    
    public void Go()
    {
        Thing x = new Animal();
    
        Switcharoo(ref x);
    
        Console.WriteLine(
            "x is Animal    :   "
            + (x is Animal).ToString());
    
        Console.WriteLine(
            "x is Vegetable :   "
            + (x is Vegetable).ToString());
      
    }
    
    public void Switcharoo(ref Thing pValue)
    {
        pValue = new Vegetable();
    }
    
```

코드문 순서를 먼저 보자.

1. Go() 메소드를 호출하면 x 포인터는 스택에 생기고, Animal은 Heap에 생긴다.
2. Switchroo()가 호출되면 파라메터 pValue는 x 포인터를 가르킨다.
3. Vegetable()을 생성하면서 heap 에 생성된다.
4. x포인터가 가르키는 heap 개체의 주소가 변경된다. 


![part2_4](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory2B01142006125918PM/Images/heapvsstack2-10.gif)

결과적으로  Animal()은 더이상 사용하지 않게 되고 출력값은 아래와 같다.
```
x is Animal    :   False
x is Vegetable :   True

```
만약 우리가 ref를 사용하지 않았더라면 결과값은 반대로 나왔을 것이다.



참고[URL](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory2B01142006125918PM/csharp_memory2B.aspx)

