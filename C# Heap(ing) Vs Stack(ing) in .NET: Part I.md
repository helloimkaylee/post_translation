C# Heap(ing) Vs Stack(ing) in .NET: Part I


###Stack vs. Heap 도대체 뭐가 다른거야?

![stack_heap](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory01122006130034PM/Images/heapvsstack1.gif)

Stack은 지금 우리 코드에서 실행되는 트랙을 킵하는것과 가깝고,
Heap은 오브젝트 트랙을 킵하는것과 가깝다.

스택은 박스가 일렬로 쌓여있는 모습으로 생각하면된다. 그래서 박스를 사용하려면 오로지 스택 가장 맨 위에 있는 박스만 사용 가능 하다.
박스를 메소드라고 생각하면, 실행이 끝난 메소드는 가장 맨 위에 있는 박스 일테고, 실행이 다 끝났으니 스택 밖으로 던져버리고, 그 아래에 나타난 박스를 또 실행 후 던져버리면 된다. 

그럼 힙은 어떨까?
(힙은 정보를 홀딩하고 있기 때문에 언제든 어디서든 참조할수 있다는것 빼곤 스택과 비슷하다.)

힙은, 스택처럼 접근하는데 제약이 없다. 스택은 맨위에서부터만 순서대로 접근이 가능하지만, 힙은 아니다. 언제 어디서든 접근 할 수 있다.

힙은, 힙이라는 단어 말 그대로 빨래 더미정도로 생각하면 된다. 빨래더미에 있는 옷들 중 우리가 필요한 어떤거에도 빠르게 직접 접근 가능하고 치우기 전까지는 시간구애 없이 접근 가능 하다.

###그럼 Stack 과 Heap은 무슨일을 할까?

코드가 실행될때에는 네가지의 주된 타입이 있다.
Value Type, Reference Type,
Pointers, Instruction.

Pointer는 
CLR(Common Language Runtime)에 의해  관리되어진다. 이말은 우리는 포인트를 통해 접근한다는 뜻이다. 포인트는 메모리 안에 하나의 공간 덩어리? 인데, 이것이 다른 메모리 공간을 가르키고 있다. 포인터는 우리가 스택이나 힙에 올려놓은 것이자, 메모리 주소나 null값이 들어가 있다.

###그럼 어디서 어떤일이 어떻게 일어나는 걸까?

우선 아래 두개 황금룰이 기억해야 한다.
1. 참조 타입은 항상 힙에 저장된다.(쉽지?)
2. 값 타입과 포인터는 항상 선언되어진 곳으로만 이동한다.

참고 : 메소드는 스택안에 존재하는 것이 아니다. 이해를 돕기위해 그린 것일뿐..!


코드가 메소드가 실행되도록 콜 하면, 컴파일되도록 명령하고 메소드 테이블을 존재하도록 명령한다. 이것은 메소드의 파라메터를 스레드 스택에 올린다. 메소드 안의 변수들은 스택 가장 위쪽에 위치하게 된다.  

예를 한번 살펴 보자
```
  public int ReturnValue()
  {
        int x = new int();
        x = 3;
        int y = new int();
        y = x;      
        y = 4;          
        return x;
  }
```

여기에서 리턴값인 x는 얼마가 될까?
답은 3
값 타입은 각각의 스택박스에 저장되어 있다고 생각하면 쉽다.

그럼 이건?

```
Public class MyInt
{
    public int MyValue;
}


public int ReturnValue2()
{
    MyInt x = new MyInt();
    x.MyValue = 3;
    MyInt y = new MyInt();
    y = x;                 
    y.MyValue = 4;              
    return x.MyValue;
}


```

답은 4
왜냐면

![s2](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory01122006130034PM/Images/heapvsstack13.gif)

힙에 저장된걸 함께 참조 하고 있으니까, y에 값을 대입하면 y포인터가 가르키는 heap의 값이 바뀌게되고, 이는 x 의 포인트도 같은곳을 바라보고 있기 때문에 4인 것이다.

