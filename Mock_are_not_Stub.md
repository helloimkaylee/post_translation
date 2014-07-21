Mock aren't Stub.
==========

**Stub과 Mock의 차이**

* 테스트 결과가 검증되는 방식의 차이.

  -> 즉, 상태 검증 (State verification)과 행위검증 (behavior verification)

* 테스팅과 설계가 같이 이루어지는지에 대한 철학적 차이

-> Mockist Style of TDD.

-> Classical Style of TDD.


#일반적인 테스트 (Regular Tests)
유닛테스트는 전형적인 네가지 단계의 순서를 따른다.
- setup, exercise, verify, teardown



#모의객체를 이용한 테스트
(Tests with Mock Objects)


**setup 단계**
- classic & mock 공통점 : 데이터 작업하려는 부분의 객체를 셋팅
- 차이점 : mock은 가짜 db를 셋팅, classic은 진짜 db를 셋팅

**Exercise단계**
- classic & mock 공통점 : 테스트 해보고싶은 것을 호출

**verify 단계**
- 차이점 : mock은 행위 검증을 사용 , classic은 상태 검증을 사용 



#모의객체와 스텁의 차이 

(The Difference Between Mocks and Stubs)
-mock은 테스트에서 실제 real DB를 사용하지 않는 방법중에 하나 인데, 이런 것에는 여러가지 언어들이 있다.

**Test Double 이란?**
테스트를 할때 진짜 객체대신 가짜 객체를 지칭하는 용어 이다.
네가지 종류가 있다.
```sh
- Dummy
- Fake
- Stubs
- Mocks 
```
이 중에  **Mock만이 행위 검증 사용을 추구하고 나머지 더블은 상태검증을 사용한다.**

예를들어 메일 전송 메소드에서
**Mock**은 "send"메소드를 호출 했는지 등등 행위를 검증한다.
**Stub**은 실제 한건의 메일이 보내졌는지, 결과에대한 상태를 검증한다.



#고전적 테스팅과 모의객체 테스팅 
(Classical and Mockist Testing)
**classical TDD:**
중요한건 실제 객체를 사용하고 실제 객체를 사용하기 힘든것만 가짜객체를 사용 

**Mockist TDD:**
모든 것을 다 가짜 객체를 사용하려 한다.




#차이점들 중에서 선택하기 
(Choosing Between the Differences)

TDD 운전 (Driving TDD)Mockist TDD 는 시스템 외부에서 철번째 테스트를 작성하면서 스토리를 개발 (outside-in) 
필요성 주도 개발 스타일 = need-driven development
class TDD는 도메인 모델에 집중하면서 개발 그후 UI (middle-out)
둘다 레이어 단위 개발이 아니라 기능단위로 개발




#픽스처 준비 (Fixture Setup)
classic 은 많은 협력되어있는 객체들을 생성해야 하고, 보통 이러한 객체들은 테스트때마다 생성되고 사라진다.
그러나 mockist 는 모의객체만을 작성하면 된다.


그러나 classic은 재사용하기위해서 setup메소드에 넣으면 된다.


mock은 classic에게 Fixture만드는데 많은 비용이 든다 하고,
classic은 mock에게 재사용하면되지만 mock은 테스트때마다 만들어야 한다고 서로에게 비난한다.





#테스트의 고립성 (Test Isolation)
mock으로 하면 그 메소드(?)만 실패하지만, classic은 다른 테스트가 실패할수도 있다.
중요한 점은 모든 class에 대한 정교한 테스트가 확실하게 분리되어 있어야 한다.ebay에서 하던 test는 unit테스트로, 
유닛테스트 이자 작은 통합테스트와 같았다.이는 다른점에서 보면 classic의 특징처럼 다른 테스트가 실패하게 만들기도 했다.
예를들어 실제 db에서 값을 변경할 경우 그 값을 바라보고 있던 다른 unit test는  false가 나기도 했다. 

결론은, 내가 이제까지 ebay 에서 작성했던 test는 어떤 TDD방식도 아닌, 그냥 단순 unit test 이었으며,
이의 특징이 classic 방식의 TDD와 비슷한 정도.




#테스트와 구현의 결합성 
(Coupling Tests to Implementations)
Class방식은 결과에만 집중하기 때문에 결합도가 낮은 반면, Mock은 메소드 구현에 많은 결합이 있을 수 있다.
이러한 결합도 Coupling은 몇가지 문제를 발생할 수 있다.
1. 테스트 주도 개발의 영향
 classic은 결과, 어떤일이 일어났는지 만을 중요로 하고, Mock은 중간에 어떤 행동이 일어날지를 생각하게 만든다.
2. 커플링은 리팩토링에 어려움을 준다.



#설계 스타일 (Design Style)
Mock은 외부에서 내부로 접근하는 outside-in방식,classic은 도메인부터 펼쳐나가는 스타일인 middle-out방식이다.
