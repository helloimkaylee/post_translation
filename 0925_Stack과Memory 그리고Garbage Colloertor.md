Call Stack 이란..?

콜스택이라 함은 
메소드 안에 메소드가 호출될 경우
순서대로 타는 것을 말한다. 

Stack이란?

프로그램을 실행하는데 필요한 메모리 공간으로 메소드가 호출되는데 필요한 메모리는 스택에 저장된다.
ex) 지역변수, 매개변수, 리턴값

Stack Frame 이란?

메소드를 호출하기 전에 반드시 호출에 필요한 메모리를 스택에 만들어야 하는데,

이때 하나의 메소드에 필요한 메모리 덩어리를 묶어서 스택프레임 이라고 한다.

즉, 하나의 메소드에는 하나의 스택프레임이 존재하는 것이다.

메소드 호출과 스택 메모리

하나의 메소드를 호출하면 그 메소드에 해당하는 하나의 스택프레임이 스택에 만들어진다. 
스택에 만들어진 메모리는 메소드의 호출이 끝나는 순간 스택 메모리에서 제거된다.

스택이라는 것은 기본 구조가 LIFO, 즉 마지막에 들어간 것이 가장 먼저 나오는 구조이다.

예를들어 Main() 안에 Calculation() 이라는 메소드가 있고 그안에  Sum()이라는 메소드가 있다고 가정하자.

```
Main()
{
    Calculation()
    {
        Sum()
        {
         ...
        }
    }
}
```

프로그램이 실행되면 가장 먼저 Main()이 스택에 저장되고, 그 다음에 Calculation(), Sum() 순서대로 실행됨 스택에 저장된다.

그럼 아까 스택은 LIFO방식이니 세개의 메소드가 끝나는 순서는 거꾸로 Sum(), Calculation(), Main()순으로 끝나게 된다.






스택은 이렇게 쌓인 구조를 생각하면 된다. 

```
|                   |
 -------------------  
|  Sum()            |
 -------------------
|  Calculation()    |
 -------------------
|  Main()           |
 -------------------
```

스택이란 메모리가 마지막에 들어간게 처음에 나오는 구조로서 LIFO가 적용된다.


Heap이란?
C# 에서의 메모리 구성은 스택과 힙으로 구성된다. 힙은 스택에 반대되는 개념의 메모리이다. 
위에서 이야기 했듯이 스택에는 지역변수, 매개변수, 리턴값등 변수들이 저장되고,

힙에서는  New를 이용해서 생성한 메모리가 저장된다.

즉, 한줄로 요약하면, 메소드 내에서 선언된 변수는 스택에 저장되고 new를 이용해서 생성한 메모리는 힙에 저장되는 것이다.

```
class StackHeap
{
    private int a;          //Heap
    private int b;          //Heap
    
    public static void Main()
    {
        int x;              //Stack
        int y;              //Stack
                
        StackHeap sh        //Stack 
        = new StackHeap();  //Heap
    }
}
```


Heap Memory제거는 가비지 컬렉션이..!!

스택안에 메모리는 LIFO라서 어디가  마지막이고 어디부터 메모리를 쌓으면 되는지 알 수 있다. 물론 중괄호가 끝나는 지점에 메모리가 다 소멸된다.

그러나 힙은 LIFO가 아니라서 중간에 사용되지 않는 메모리들은 가비지 컬렉션에 의해 삭제 될 수 있다.
이럴경우 메모리 중간에 구멍이 생기게 되는데,

이런 것들은 다음 Heap메모리가 들어오면 사용가능한 메모리를 효율적으로 활용할 수 없기 때문에 
가비지 컬렉터는 사용하지 않는 메모리를 해제 시켜주는 작업 뿐만 아니라,
사용하는 메모리들을 한곳으로 몰아주는 역할도 한다. (메모리 조각 모음?)

남아있는 메모리를 효율적으로 사용할 수 있게끔 말이다.





가비지 컬렉터의 좋은 예!
문자열 조합할때 '+'로 연결하면 간단히 문자열 조합이 이루어 지지만 모든 객체가 ToString()을 지원하기 때문에 문자열끼리만 조합되는게 아니라 int, float등의 값도 알아서 문자열로 변환 조합된다.

```
Class Names
{
    public string[] name = new string [100];
    public void Print()
    {
        for(int index = 0; index < name.Length; index ++)
        {
            string output = "[" + index + "]" + name;
            Console.WriteLine(output);
        }
    }
}
```
문제는 이러면 가비지가 많이 발행한다는 점~.
"+" 연산자로 두 값을 연결할 때마다 새로운 String 인스턴스가 생성된다. 연이어 "+" 연산자가 나오기 때문에 다시금 새로운 string 인스턴스가 생성되고, 이전에 만들어진 string 인스턴스는 가비지가 된다.

string조합을 위해 "+" 호출이 많아질수록 많은 가비지가 만들어지는 것이다.

그래서 문자열을 조합하는 동안 새로운 객체를 생성하지 않는 String.Text.StringBuilder 객체를 사용하는 것이 좋다.
이는 Append()메소드를 통해 문자열을 추가하며, String객체를 만들어 내는게 아니라, 이미 잡아놓은 메모리 공간에 문자열만 복사해 뒀다가 한번에 ToString()으로 string 객체를 생성해 낸다.

```
class NewNames
{
    public string[] name = new string[100];
    private StringBuilder sb = new StringBuilder();
    public void Print()
    {
        sb.Clear();
        for(int index = 0; index < name.Length; index ++)
        {
            sb.AppendFormat("[ {0} ] {1}", index, name.ToString());
        }
        Cosole.WriteLine(sb.Tostring());
    }
}
```




