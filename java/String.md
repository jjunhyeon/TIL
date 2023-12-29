# String

개발하며 가장 많이 사용하고 있는 자료형 중 하나인 String에 대해 정리하기 위해 작성하는 글입니다.

String은 불변객체입니다. 그로 인해 문제를 풀다보면 마구잡이로 String을 사용하면 성능 이슈로 실패하기도 합니다.

**왜 그런것인지 String을 이해하고 사용하기 위해 정리해보겠습니다.**

<br>

우선 String은 문자열을 다루는 클래스이며 이처럼 문자열을 처리하기 위한 클래스 3가지가 있습니다. 

왜 존재하는지에 대해 이해하고 있어야 필요한 상황에 맞춰 적절하게 사용할 수 있을겁니다.

<br>

 > String, StringBuilder, StringBuffer 모두 문자열을 저장하고 관리하는 클래스

<br>

알고리즘을 공부하다보면 String 으로는 효율성 에러가 나는 코드가 StringBuilder를 사용하면 기가막히게 통과하는 케이스가 있습니다. 

결론부터 말하자면 이유는 String은 불변 객체이고, StringBuilder는 가변 객체이기 때문입니다.

불변 객체라는 의미는 객체의 값이 변할 수 없다는 것이고 이는 할당된 메모리 공간이 변하지 않는다는 의미입니다.

<br/>

**String 예제 코드**

``` java
String name = "KIM";
name += "JH";

System.out.println("my name::" + name);
```
물론 값은 정상적으로 출력됩니다. 

하지만 여기서 중요한점은  불변 객체인 `String`이 어떻게 **새로운 값을 담아 출력**할 수 있냐는 것이다.

String은 보이는것처럼 기존 문자열 뒤에 붙이는 것이 아닙니다.

새로운 String 객체를 만든 후, 새 String 객체에 연결된 문자열을 저장하고, 그 객체를 참조합니다.

위에서 생성한 name은 자바 `Heap영역`에 생성된 후(리터럴 방식으로 생성 시 : `String pool` 영역) `가비지 컬렉션` 대상이 되어 회수된 후 새롭게 생성된 객체에 "KIMJH" 이라는 값이 할당된 것입니다.

<br>

*String 객체는 위와 같은 이유로 문자열 조작이 많은 경우 , **성능이 떨어집니다.***

<br>

>*그럼 String 쓰지마?*

그렇지는 않다. 

*StringBuffer*나 *StringBuilder*로 문자열을 조작할 때에도 버퍼의 크기를 처리하는 연산이 필요하므로 ***많은 양의 문자열 수정이 필요한 경우가 아니라면*** 오히려 String 객체를 사용하는게 낫다.

<br>

---

***StringBuilder vs StringBuffer***

String에 대해선 어떻게 써야할지 감을 잡았는데, 그럼 StringBuilder와 StringBuffer는 어떻게 구분해야할까?

StringBuffer와 StringBuilder 클래스는 String과 다르게 가변적인 특성을 가지고 있으며, 굉장히 유사한 클래스이다.

<br>

이 두 클래스의 차이점은 ***동기화 여부*** 이다.
- StringBuffer는 각 메서드별로 Synchronized Keyword가 존재하며, 멀티스레드 환경에서도 동기화를 지원
- 반면, StringBuilder는 동기화를 보장하지 않습니다.

=> 이를 멀티 쓰레드 환경에서 안전(Safe) 여부로 구분하기도 한다

<br>

JVM에 생성된 Heap Area에 존재하는 StringBuilder에 동시에 여러 Thread에서 값을 접근하면 StringBuilder는 안전하지 않습니다. 안전하지 않다는 말은 예상치 못한 순서로 메소드가 실행되거나 내부 상태가 일관성 없게 수정될 수 있다는 의미입니다.

따라서, 멀티쓰레드 환경에서는 StringBuffer 를 활용해 동기화(Synchronized) 를 활용해 개발해야 안전하게 처리 할 수 있습니다.

✔️ Synchronized

여러개의 스레드가 한 개의 자원에 접근하려 할 때, 현재 데이터를 사용하고 있는 스레드를 제외하곤 데이터에 접근 할 수 없도록 막는 역할을 함.


<br>

---

**결론**
- String은 짧은 문자열 연산을 하는 경우에 사용한다.
- StringBuffer는 쓰레드에 안전한 프로그램 개발시, StringBuilder는 스레드 안전여부에 상관없는 프로그램 개발 시 사용한다.
- 단순히 성능만을 고려한다면 StringBuilder > StringBuffer > String



---
<br>

# 참고 
[자바 성능 튜닝 이야기](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=32526713)

[String-StringBuilder-StringBuffer 비교](https://inpa.tistory.com/entry/JAVA-%E2%98%95-String-StringBuffer-StringBuilder-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%84%B1%EB%8A%A5-%EB%B9%84%EA%B5%90)

[성능에 대한 내용](https://12bme.tistory.com/42)










