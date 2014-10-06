###Weak Reference [약한참조]란?

대부분의 개체들은 그들이 더이상 참조 하지 않을때까지 메모리에 유지된다.
이말은 참조되는 동안은 가비지 컬렉터의 대상이 되지 않는 다는 뜻이다.

하지만 약한 참조로 만든다면, 참조하고 있는 동안에도 가비지 컬렉터의 대상이되며, 
GC에 의해 수집될 수 있다는 뜻이다.
또한 GC의 대상이지만 여전히 응용 프로그램에서도 해당 개체에 계속 액세스 할 수 있다는 뜻이다.

약한 참조는 강한 참조가 없을 때 개체가 수집되지 전까지의 임의의 시간 동안만 유효 하다.

###사용방법

1. 개체에 대한 약한 참조를 설정하려면 추적할 개체의 인스턴스를 사용하여 WeakReference를 만듭니다.
2. 그런 다음 Target 속성을 해당 개체로 설정하고 해당 개체 참조를 null로 설정 한다.


###Example

먼저, 약한 참조는 constructor 호출에 의해 생성된다.

아래 코드를 보면 가지비 컬렉터가 GC.Collect();에 의해 실행되는 것을 알 수 있다.

GC가 실행되면 약한 참조의 포인터는 더이상 존재하지 않는다.

만약 GC.Collect();가 실행되지 않았더라면, 여전히 존재 할 것이다.

```sh
//Program that uses WeakReference: C#

using System;
using System.Text;

class Program
{
    /// <summary>
    /// Points to data that can be garbage collected any time.
    /// </summary>
    static WeakReference _weak;

    static void Main()
    {
	// Assign the WeakReference.
	_weak = new WeakReference(new StringBuilder("perls"));

	// See if weak reference is alive.
	if (_weak.IsAlive)
	{
	    Console.WriteLine((_weak.Target as StringBuilder).ToString());
	}

	// Invoke GC.Collect.
	// ... If this is commented out, the next condition will evaluate true.
	GC.Collect();

	// Check alive.
	if (_weak.IsAlive)
	{
	    Console.WriteLine("IsAlive");
	}

	// Finish.
	Console.WriteLine("[Done]");
	Console.Read();
    }
}

//Output

perls     //GC 전에는 여전히 남아있다.
[Done]    //GC 후에는 삭제됨.


```


약한 참조는 가비지컬렉터에 영향을 준다. 약한 참조 쓰는게 과연 좋은 방법일까?
안타깝게도 가비지 컬렉터는 항상 예상가능한 때 실행되는게 아니다. 

그러므로, 만약 큰 캐시를 사용한다면 이는 퍼포먼스가 안좋게 나오게 만들것이다.

참고1[URL](http://www.dotnetperls.com/weakreference)


###약한 참조 사용 지침

1. 긴 약한 참조는 개체가 종료된 후 개체의 상태를 예측할 수 없어서 필요한 경우에만 사용 합니다.

2. 개체 크기가 작을 때는 포인터 크기가 그와 같거나 그보다 클 수 있으므로 약한 참조를 사용하지 않습니다.

3. 메모리 관리 문제를 자동으로 해결하기 위한 수단으로 약한 참조를 사용하지 않는 것이 좋습니다.

참고2[URL](http://msdn.microsoft.com/ko-kr/library/ms404247(v=vs.110).aspx)


