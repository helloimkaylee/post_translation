Dependency Injection [의존성 주입]
=========
    

 의존성이란?
-----------
>한마디로 말하면 호출할때 다른것도 함께 호출되는것.
즉, 의존되어 있는 것.

의존성이 문제되는 이유?
-----------------------

**1. TDD** 관점에서 볼까?

>예를 들어 보자. 나는 에스크로 개발자. 그러나 테스트 환경에서의 에스크로 결제 성공이란 멀고도 험난한 여정이다.

>테스트 결제시 실제 카드사의 서버와의 연동이 필요하다.

>그래서 실제로 van사의 카드결제 테스트 환경과 사내 서버가 연결되어 있는데,
카드사의 테스트환경이 죽으면 공교롭게 우리 또한 테스트를 할 수가 없다.

>van사의 테스트가 먹통이면 우리또한 놀아야 했다.

>왜냐? **의존성**이 높았기 때문에.

>이런 상황은 효율적이지 못하다.
의존성을 끊어줄 필요가 있다.



**2. 소스코드 빌드** 관점에서 볼까?

```sh
public class HomeWork
{
    public void homeWork 
    {
        Working working = new DependencyInjectionHomeWork();
    }
}
```
>- 위의 상황에서 만약 **DependencyInjectionHomeWork()** 가 없다면 빌드 조차 안될 것이다.
- **DependencyInjectionHomeWork()** 이름이 **DependencyInjectionHomeWorkForYou()**변경된다면, **DIHW()**가 정의되있는 파일 과 위의 파일 모두 수정 해주어야 한다. 



그럼 DI를 어떻게 이용할까?
==========================

>처음 제목을 봤을땐, 의존성 주입을 위의 개념과 반대로 '의존성이 있어야 된다는 건가?' 라는 생각을 했다능..

> **의존성 주입은 내 생각에 "필요할때 의존성을 주입해줄께. 넌 신경끄고 코딩이나 잘해" 뭐 이런 의미로 이해된다. 또는 "의존성을 한 곳에 다 주입해둬. 내가 알아서 필요할때 셋팅해줄께"(by IOC) 의미 같다.**


>실장님이 좋아하는 마틴느님의 그림이 삽입이 안되네..

![di](http://martinfowler.com/articles/injector.gif)
>출처 : http://martinfowler.com/articles/injection.html

>여기 figure2 에서처럼 두개의 소스파일 사이에는 Assembler 와 같이 중개자(?) 같은게 있다.
여기에다가 막 정의 하면된다.
정의를 인터페이스에다 했다면 참조할 곳에서 그 인터페이스를 상속받아 사용하면된다.

>그리고 이렇게 정의해두면 이것들을 알아서 전달 해주는 개념이 **IoC (Inversion Of Control)**

>**[제어의 역전**(?)**]**이다.
container에게 object의 생성과 관계 설정권한을 위임 하는 식.

>이렇게 결합도가 떨어지면 유지보수에도 좋다.

>이런 컨테이너중 유명한건 spring 이라고 한다.

>그러나 .net 에서도 angular.js 에서도 사용 할 수 있다.










만약 의존성이 없었다면 위의 예시들은 어떻게 되었을까?
======================================================
**1. TDD**관점에서는,

>van사와의 서버 연결되는 부분에서 결과는 언제나 ok로 반환 시키도록 셋팅해놓는다면, van사의 테스트 환경의 에러 여부와는 상관없이
우리쪽 결제시스템 테스트를 성공적으로 마칠 수 있을 것이다.

**2. 소스코드 빌드** 관점에서는,

>**DependencyInjectionHomeWork()**가 없더라도 빌드는 될것이다.
이름이 변경되었어도, 위의 파일에서는 수정해줄 필요가 없을 것이다.

**3. 이쯤에서 정리해보는 장점**
> 재사용성이 좋다.

> 코드 가독성이 좋다.

> 물론 유지보수에도 좋고, 생상성 & 효율성 등등에 좋다.

> TDD환경에 좋다.



                                 
