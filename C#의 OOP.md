OOP내 주요 개념
----

Object란 모델컨셉이다.

1. 예를들어 오브젝트는
어떤 '것' 이다
ex)customer
2. 오브젝트는 데이터를 가지고 있다.
ex) name, id
3. 오브젝트는 어떠한 행동이다.
ex) make a customer preferred

디자인 하는 타임에는 아직 데이터를 가지고 있지 않는다. 그러나 컨테이너가 되어서 런타임때 데이터를 갖게된다.


What is Object Oriented Programming?
언어는 오브젝트 컨셉으로 디자인 되었다. 오브젝트는 속성과 행동을 가진다.
이 뜻은 즉, 언어는 일반적으로 아래와 같은 것이 가능하다. 

**Encapsulation[ ]**
안에 어떤 데이터가 들어있는지 알 필요없다. 코드를 펜스를 쳐서 묶어서 펜스밖에 있는 사람에게 전달할수 있다. 그러나 밖에 있는 사람은 그 안에 코드가 뭐가 있는지 알 필요없이 메소드명을 보고 방을 들여다보는 창문정도로, 확인만 해도 된다. 예를들어 customer안에   실제 name 데이터가 들어있는지 신경쓸 필요없다. name을 어떻게 가지고 오는지 조차 알 필요없다. 외부사람은 그저 name이라는 속성을 꺼내올수 있다는 것만 알면된다.

또한 캡슐화 했다는 것은 이런 의미도 줄 수 있다.






**Inheritance[ 상속성 ]**

코드를 재사용 할 수 있다. 



**Polymorphism[ 다형성 ]**
많은 형태를 가지고 있다는 것
ex) Shampoo

이건 상속성의 다른 관점과 비슷하다.
어떻게 그것들이 상속받는것에 따라 다른 타입, 다른 모양을 갖는다.

오브젝트

#What is a Managed Language.
런타임환경에 의한 서비스에 의존하는 것

C#도 컴파일을 통한 언어중 하나이다.


#Why C# for OOP?
C#은 다양한 어플리케이션을 만들수 있게 디자인 되어 있다.
1. General Purpose
2. Object Oriented
3. Focused on dev productivity









Managed code는 Common Language Runtime(CLR)때 실행된다.

CLR은 아래와 같은 특징을 가진다.
1. Automatic memory manaagement
2. Exception Handling
3. standard Types 
4. Security




미션의 글에서는 
>캡슐화에 대해서 
"마음대로 변경해서는 안되는 데이터는 접근하지 못하게 숨겨두고 method를 통해서만 접근해야된다."라고 설명하며 **"c언어의 구조체는 캡슐화가 없어서 객체가 아니다"** 라고 말했다.

그런데 msdn의 캡슐화 정의는 다음과 같다.

>캡슐화는 개체 지향 프로그래밍의 첫 번째 기둥 또는 원칙으로도 불립니다. 캡슐화 원칙에 따르면 **클래스 또는 구조체에서는 클래스 또는 구조체 외부 코드에서 각 멤버로의 액세스 가능성을 지정할 수 있습니다.** 클래스 또는 어셈블리 외부에서 사용할 목적이 아닌 메서드 및 변수는 코딩 오류 또는 악의적인 이용의 가능성을 방지하기 위해 숨길 수 있습니다.

액세스 한정자 (public, private 등등)로 접근 가능성을 조절 할 수 있고,
여기서 말하는 멤버에는 필드, 속성, 메서드,이벤트 연산자 등등을 포함한다.


#캡슐화란?
안의 내용물은 private으로 감추고 터치포인트 정도를 public으로 그것들과 분리한 객체를 통해 접근 가능하도록 할 수 있다. 
C# supports encapsulation via:
1. Unified Type System.
2. Classes and Interfaces
3. Properties, Methods and Events




#C# & Inheritance[ 상속 ]
상속은 캡슐화 및 다형성과 함께 개체 지향 프로그래밍의 세 가지 주요 특징 또는 기둥 중 하나입니다. 상속을 사용하면 다른 클래스에 정의된 동작을 다시 사용, 확장 및 수정하는 새 클래스를 만들 수 있습니다. 멤버가 상속되는 클래스를 기본 클래스라고 하고 해당 멤버를 상속하는 클래스를 파생 클래스라고 합니다. 파생 클래스는 직접 기본 클래스를 하나만 가질 수 있습니다. 그러나, 상속은 전이적입니다. ClassC가 ClassB에서 파생되고 ClassB가 ClassA에서 파생되는 경우 ClassC는 ClassB 및 ClassA에서 선언된 멤버를 상속합니다.



#C# & Polymorphism [ 다형성 ]
클래스는 베이스타입이나 인터페이스 타입을 상속 받아서 자기만에 고유한 타입을 만들 수 있음.

virtual 선언을 통해서도 이룰수 있음 

파생 멤버는 override 키워드를 사용하여 해당 메서드가 가상 호출에 참여한다는 의도를 명시적으로 나타내야 한다
