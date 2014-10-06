###Graphing
GC의 포인트를 살펴보자~!
먼저, 무엇이 쓰레기 인지 아닌지 확실하게 결정할수 있어야만 GC가 효과적으로 가비지를 수집할 것이다.
이를 위해서 언제나 함께 하고 있는 좋은 우리의 친구 두명을 상상해보자.
Joseph Ivan Thomas (JIT) and Cindy Lorraine Richmond (CLR).

조셉이라고 쓰고 저스트인타임이라고 부르는 친구 하나와, 신디라고 쓰고 CLR이라고 부르는 친구. ㅎㅎㅎ

조셉과 신디는 그들이 유지해야되는 트랙 리스트를 우리에게 전달해준다. 
우리는 필요하거나 유지하고싶은 어떤 것이든 그 마스터 리스트를 그래프에 유지한다. 

예를들어 TV를 보관하고 싶은데 리모콘은 티비가 아니니까 버리는일은 없다. 티비와 리모콘을 함께 보관하겠지~. 마찬가지로 컴퓨터를 보관하고 싶으면 키보드와 모니터는 한 셋트니까 (분명) 함께 보관 할것이다 .

이런 방식으로 GC가 무엇을 킵하고 버리는지 결정한다. JIT와 CLR 로 부터 루트(root)에 참조 되어 있는 리스트를 받아서 그래프에 보관해둔다.

루트는 다양한 형태를 가지지만 보통 힙을 가르키는 스택과 전역변수이다.

루트로부터 시작하여 모든 개체를 방문하게 되며 방문한 모든 개체에 포함된 해당 개체를 표시하는 개체 포인터를 따라 이동합니다. 

이러한 방식으로 수집기는 접근 가능한 개체 또는 라이브 개체를 확인합니다. 
접근할 수 없는 다른 개체는 이제 폐기됩니다.

루트는 아래와 같은 것들을 보관한다.

1. Global/ static ponters.
2. Pointers on the stack.
3. CPU register pointers.


![part4_1](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/Images/Stacking_Heaping1.gif)


오브젝트 1,5 는 Root에서 직접 참조하고 3은 간접적으로 참조하고 있다.
위 예시에처럼 1은 TV가 될 것이고, 3은 리모컨 정도가 될것이다.

모든 개체가 그래프되어지면, 우리는 Compacting 단계로 넘어갈 수 있다.

###Compacting

자, 이제는 우리가 보존하고 싶어하는 개체들의 그래프가 있다.

![part4_2](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/Images/Stacking_Heaping2.gif)

다행스럽게도, 관리되는 힙에 새로운 무언가를 넣으려고 하기전까진 우리는 공간을 마련해주기 위해 청소하지 않아도 된다. 


개체2번은 더이상 필요없으니 Gc가 없앤 후 개체 3을 아래로 옮겨준다.


![part4_3](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/Images/Stacking_Heaping3.gif)

이런식으로 4번도 착업 하면 중간중간 빈 공간없이 필요한 것들끼리 쫙~ 붙는다. 이게 바로 컴팩트된 힙 이란 말씀! ㅎㅎ

이상태로 신디에게 새로운 개체를 요기에다가 (맨위 노란 화살표 있는 곳) 넣어주면 된다고 알려 줄 수 있다.

![part4_4](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/Images/Stacking_Heaping5.gif)


참고로 개체를 움직이는 짓은 매우 비용이 많이 든다. 
그럼 이렇게 개체를 욺직이는 짓을 줄여줄 수 있다면  GC는 프로세스는 적게 카피하교 비용을 덜 발생시킬 것이다. 



###CLR에 의해 관리되는 힙들 밖에있는 것들은 어떨까? (= Unmanaged, Non-Managed)


집이 관리되는 힙이라고 하면, 청소를 시작하면 집은 청소가 가능하지만 차는 어떻게 관리하지? 만약 랩탑은 집에 있는데 랩탑 베터리는 차에 있다면..?

코드에서는 파일이나 데이터베이스커넥션, 네트워크 커넥션 등등이 될 것 이다.
이런 Non-managed 리소스들을 정리해주는 방법은 바로 finalizer...!!

개체들에 finalizer를 설정 하면 finalization queue 에 들어간다. 

![part4_5](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/Images/Stacking_Heaping6.gif)

이중에 2, 4가 더이상 참조되지 않으니 GC 대상이 된다.

개체 4는 finalizer가 셋팅되어 있어서 finalization queue에 참조된 채로 시작한다. 

2와 4중 4는 finalization queue에 들어가 있으나, 2는  queue에도 없으니 삭제!
GC가 finalization queue 를 보고 '4는 여전히 heap에 참조 되어 있네~' 이러면서 얘를 Freachable queue로 보낸다. 

![part4_6](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/Images/Stacking_Heaping7.gif)


그러다 finalizer가 개체4 스레드에 의해 실행이 끝나면 freachable queue에서 삭제된다. 그럼 이제 GC 컬렉션의 대상이 된다. ( 아까 개체 2처럼) 바로 사라지는게 아니라 다음 GC 회전때까진 살아있다.

![part4_7](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/Images/Stacking_Heaping8.gif)


###IDisposable

finalizer말고 더 괜찮은 방법으로 관리되지 않는 리소스들을 청소하는 방법은 없을까?

그것은 바로...IDisposable를 사용하는 것...!!!

그 안에 Dispose() 메소드를 셋팅해주면 된다.

IDisposable 은 using 이라는 키워드와 함께 사용한다. using()이 끝날때  Dispose()가 호출된다.


```sh
using(rec)
{
    //Do something
    

}   //Dispose called here
```




###결론
GC 퍼포먼스를 상승시키기 위해 우리가 할 수있는 것들은..


1. Clean Up !
* 리소스를 오픈해놓은 채로 떠나지마라! 반드시 오픈되어있는 커넥션과 관리되지 않는 오브젝트는 최대한 빨리 close시켜라.

2. 참조를 남발하지 마라!
* 오브젝트를 참조할때는 반드시 이유가 타장해야된다.
기억해라! 우리의 오브젝트가 살아있다면, 절대 GC에 수집되지 않을 것이다. 만약 class참조가 다 끝난 거라면, reference를 null로 셋팅해서 삭제하도록 할 수 있다.

3. finalizer 를 사용해라.
* finalizer는 비용이 드는 작업이다.함부로 사용하면 안된다. 우리는 finalizer 대신에 IDusposible 사용하면 된다 이게 더 효과적이다.
닷넷에서는 비관리 리소스에 대한 더욱 신속하고 명시적인 해제를 위해 IDisposable인터페이스의 Dispose메소드를 구현하는 방식의 표준 Dispose패턴을 사용할 것을 권장한다.


4. Object와 그 하위것들을 함께 유지시켜라!
* GC안에서 근본적으로 조각조각들을 각각 던지는 것보단, 큰 메모리 덩어리를 함께 카피하는 것이 훨씬 더 쉽다.


참고 [URL](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory_401282006141834PM/csharp_memory_4.aspx)
