```csharp
Q_1. 
What is the output of the program below? Explain your answer.

class Program {
  private static string result;
 
  static void Main() {
    SaySomething();
    Console.WriteLine(result);
  }
 
  static async Task<string> SaySomething() {
    await Task.Delay(5);
    result = "Hello world!";
    return “Something”;
  }
}
Also, would the answer change if we were to replace await Task.Delay(5); with Thread.Sleep(5)? Why or why not?
```

```csharp
A_1.
result가 아직 초기화 되지 않아서 못 써묵는다.

await 를 뺀 Thread.Sleep 은 비동기가 아닌 동기화 이다.
출력은 Hello world가 나오겠지만 비동기 처리된건 아니다. 

await 를 제거하면 5초 기다린 후에 SaySomething()을 반환할 것이다.

```


```csharp
Q_2.
Is the comparison of time and null in the if statement below valid or not? Why or why not?

static DateTime time;
/* ... */
if (time == null)
{
	/* do something */
}

A_2.
같지 않다.
왜냐믄 time이 널이 아니기 때문에?
default 값이 null이 아니다. 
```

```csharp

Q_3.
What is the output of the short program below? Explain your answer.

class Program {
  static String location;
  static DateTime time;
 
  static void Main() {
    Console.WriteLine(location == null ? "location is null" : location);
    Console.WriteLine(time == null ? "time is null" : time.ToString());
  }
}

A_3.
location is null
1900.01.01 


strgin은 null포함이 가능하니 널이라서 location is null 이 반환되지만,
time은 null이 안되니 default 값이 반환될듯
```


```csharp

Q_4.
Given an array of ints, write a C# method to total all the values that are even numbers.


A_4.
using System;
					
public class Program
{
	public static void Main()
	{
		
		int[] total = {1,2,3,4,5,6,7,8,9};
		
		foreach(int even in total)
		{
			
			if(even % 2 ==0)
			{ 
				Console.WriteLine(even + ",");
			}
		}
		
	}
}

```

```csharp

Q_5.
Given an instance circle of the following class:

public sealed class Circle {
  private double radius;
  
  public double Calculate(Func<double, double> op) {
    return op(radius);
  }
}
write code to calculate the circumference of the circle, without modifying the Circle class itself.

A_5.

using System;

public class Program
{
	public static void Main()
	{
	  	Circle circle = new Circle(10);	
		Console.WriteLine(circle.Calculate(r => 2 * Math.PI * r));
		Console.WriteLine(circle.Calculate(r => Math.PI * r * r));
	}			
	public sealed class Circle 
	{	
		private double radius;
		
		public Circle(double radius)
		{
			this.radius = radius;
		}
  
  		public double Calculate(Func<double, double> op) 
		{
			
			return op(radius);
  		}
	}
	
}

```
