Mock aren't Stub.
==========

**Stub과 Mock의 차이**

* 테스트 결과가 검증되는 방식의 차이.

  -> 즉, 상태 검증 (State verification)과 행위검증 (behavior verification)

* 테스팅과 설계가 같이 이루어지는지에 대한 철학적 차이

-> Mockist Style of TDD.

-> Classical Style of TDD.


##일반적인 테스트 (Regular Tests)
유닛테스트는 전형적인 네가지 단계의 순서를 따른다.
- setup, exercise, verify, teardown



##모의객체를 이용한 테스트 (Tests with Mock Objects)


**setup 단계**
- classic & mock 공통점 : 데이터 작업하려는 부분의 객체를 셋팅
- 차이점 : mock은 가짜 db를 셋팅, classic은 진짜 db를 셋팅

**Exercise단계**
- classic & mock 공통점 : 테스트 해보고싶은 것을 호출

**verify 단계**
- 차이점 : mock은 행위 검증을 사용 , classic은 상태 검증을 사용 



##모의객체와 스텁의 차이 (The Difference Between Mocks and Stubs)
mock은 테스트에서 실제 real DB를 사용하지 않는 방법중에 하나 인데, 이런 것에는 여러가지 언어들이 있다.

**Test Double 이란?**
테스트를 할때 진짜 객체대신 가짜 객체를 지칭하는 용어 이다.
네가지 종류가 있다.
```sh
- Dummy Object 
  : 가장 기본적인 유형으로, 매개변수 값과 같이 작업을 수행하는 메소드가 없는, 값 전달 마을 위한 객체를 말한다.
- Stub
  : 아직 개발되지 않은 클래스나 메소드가 처리 후 리턴해야 하는 값을 전달해 주는 역할을 한다. 대부분 그 값은 하드코딩 되어     있다. 
- Spy
  : Spy는 Stub과 비슷하지만, 어떤 작업을 수행했는지에 대한 이력(log)를 남긴다는 점이 다르다. 
- Fake Object 
  : 테스트에 직접저긴 연관은 없지만, 테스트하고자 하는 시스템과 연계되는 시스템이 너무 느리거나, DB가 아직 구성되지 않았을 경우에 부분이 실제 존재하는 것처럼 처리하는 부분을 말한다. 예를들어 DB 테이블의 데이터 정보를 Hash Map같은 객체에 저장해 메소리를 통해서 처리할 수 있도록 하는 것이 여기에 속한다.
- Test Stubs 

- Mocks Object 
  : 어떻게 보면 Dummy, Stub, Spy를 통합해 놓은 것과 비슷하다고 볼 수 있는 것이 Mock이다. 하지만 Mock은 보통 라이브러리를 사용하여 동적으로 데이터를 처리해줄 부분을 생성한다는 점이 다르다. 어떻게 보면 Mock은 Stub의 기능에 검증 기능을 추가한 형태라고 생각하는 것이 이해하기 가장 쉬울 것이다. 
```
이 중에  **Mock만이 행위 검증 사용을 추구하고 나머지 더블은 상태검증을 사용한다.**

예를들어 메일 전송 메소드에서
**Mock**은 "send"메소드를 호출 했는지 등등 행위를 검증한다.
**Stub**은 실제 한건의 메일이 보내졌는지, 결과에대한 상태를 검증한다.


>###2014.09.01 추가사항
> 예를들어 ware house에 충분한 재고가 있다면 order를 한 만큼 ware house에서 제거 후 order성공 여부와 남은 재고를 반환 해주고 , 재고가 충분치 않다면 재고가 아무 행동도 하지 않고 order 성공 여부는 false를 반환한다. 

> stub 으로 하게 된다면, ware house를 호출 시 성공 여부인 true , false 를 반환 하도록 하드 코딩 할 수 있다. 또한 제고를 반환하는 것도 하드 코딩으로 가능 하다. 

> 그러나 실제 ware house 에서 remove() 메소드를 호출 했는지등은 알 수 없고, 오로지 하드코딩 되어 있는 리턴 값만 확인 할 수 있다. Mock을 사용한다면 이와 같은 행위 등을 했는지 확인 가능 하다.  

##고전적 테스팅과 모의객체 테스팅 (Classical and Mockist Testing)
**classical TDD:**
중요한건 실제 객체를 사용하고 실제 객체를 사용하기 힘든것만 가짜객체를 사용 

**Mockist TDD:**
모든 것을 다 가짜 객체를 사용하려 한다.




##차이점들 중에서 선택하기 (Choosing Between the Differences)

**TDD 운전 (Driving TDD)**
**Mockist TDD** 는 시스템 외부에서 철번째 테스트를 작성하면서 스토리를 개발 (outside-in) 
필요성 주도 개발 스타일 = need-driven development

**class TDD**는 도메인 모델에 집중하면서 개발 그후 UI (middle-out)
둘다 레이어 단위 개발이 아니라 기능단위로 개발




##픽스처 준비 (Fixture Setup)
classic 은 많은 협력되어있는 객체들을 생성해야 하고, 보통 이러한 객체들은 테스트때마다 생성되고 사라진다.
그러나 mockist 는 모의객체만을 작성하면 된다.


그러나 classic은 재사용하기위해서 setup메소드에 넣으면 된다.


mock은 classic에게 Fixture만드는데 많은 비용이 든다 하고,
classic은 mock에게 재사용하면되지만 mock은 테스트때마다 만들어야 한다고 서로에게 비난한다.





##테스트의 고립성 (Test Isolation)
mock으로 하면 그 메소드(?)만 실패하지만, classic은 다른 테스트가 실패할수도 있다.
중요한 점은 모든 class에 대한 정교한 테스트가 확실하게 분리되어 있어야 한다.

ebay에서 하던 test는 unit테스트로, 유닛테스트 이자 작은 통합테스트와 같았다.

이는 다른점에서 보면 classic의 특징처럼 다른 테스트가 실패하게 만들기도 했다.
예를들어 실제 db에서 값을 변경할 경우 그 값을 바라보고 있던 다른 unit test는  false가 나기도 했다. 

결론은, 내가 이제까지 ebay 에서 작성했던 test는 어떤 TDD방식도 아닌, 그냥 단순 unit test 이었으며,
이의 특징이 classic 방식의 TDD와 비슷한 정도.




##테스트와 구현의 결합성 
(Coupling Tests to Implementations)
Class방식은 결과에만 집중하기 때문에 결합도가 낮은 반면, Mock은 메소드 구현에 많은 결합이 있을 수 있다.
이러한 결합도 Coupling은 몇가지 문제를 발생할 수 있다.
1. 테스트 주도 개발의 영향
 classic은 결과, 어떤일이 일어났는지 만을 중요로 하고, Mock은 중간에 어떤 행동이 일어날지를 생각하게 만든다.
2. 커플링은 리팩토링에 어려움을 준다.



##설계 스타일 (Design Style)
Mock은 외부에서 내부로 접근하는 outside-in방식,classic은 도메인부터 펼쳐나가는 스타일인 middle-out방식이다.

##총 결론!
 

Mock TDD방식 언제사용?

1. 실제 DB를 연결하는등의 시간봐 비용이 많이 드는 작업등을 제대로 구현하기 힘든 경우 사용.
2. 의존성때문에 구현하기 힘든 경우.

(Classic방식에 비해) Mock 방식의 TDD의 장점:

1. 다른 테스트에 방해를 주지 않고 원하는 메소드나 행동에 대한 테스트가 가능하다.
2. 결과에 대한 상태만을 테스트 하는 것이 아니라, 행위에 대한 검증이 이루어 진다.
3. methodA에서 methodB를 호출시 classic방식이라면 A에 대한 반환값만 확인 가능하겠지만, mock방식은 행위검증 방식이라 Call 이 제대로 되었는지까지 확인이 가능하다.


단점:

1. classic에 비해 결합도가 있어서 리팩토링에 어려움이 존재 할 수 있다.
2. Mock은 Mock일 뿐이다. Mock 객체를 이용해서 아무리 잘 동작하는 코드를 만들었다 하더라도, 실제 객체가 끼어들어 왔 
  을 때도 잘 동작하리라는 보장은 없다.

.net에서 사용가능한 Mock library

EasyMock (http://blog.outsider.ne.kr/991)

Moq


참고 원본 문서 : https://sites.google.com/a/jabberstory.net/testing/mocksArentStubs
