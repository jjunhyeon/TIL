*시작하며*  <br>

***JPA의 영속성 컨텍스트 개념을 명확히 정리하기 위해 작성합니다.***

<br>

### 영속성컨텍스트란 무엇인가?
DB에서 조회한 시점의 데이터를 애플리케이션 메모리에서 관리하는 공간을 말하며 이를 영속성컨텍스트라고 한다.

``` JAVA
@Transactional
public void updateMember(Long id) {
    Member member1 = memberRepository.findById(id).get();
    Member member2 = memberRepository.findById(id).get();

    member1.setName("A");
    member2.setName("B");
}
```

개념을 정리할 수 있는 소스코드이다. 코드를 보며 다시 JPA의 핵심 개념을 정리합니다.

코드의 meber1과 member2는 동일한 DB 인스턴스 객체이다. 같은 트랜잭션 내부에서 조회한 엔티티는 영속성 컨텍스트의 1차 캐시에 의해 항상 동일한 객체 인스턴스를 반환하기 때문이다.

### 알아서 update sql 이 발생하는 이유
결론적으로, JPA는 위 트랜잭션 종료 직전, 1번 UPDATE SQL이 발생하며 객체 데이터와 DB 데이터를 동기화 시켜준다.

이게 가능한 이유가 영속성컨텍스트의 존재 이유이다. 
영속성 컨텍스트는 엔티티를 최초로 조회했을때의 데이터를 관리한다. 이렇게 관리하는 이유는 [트랜잭션 커밋 직전 flush 시점]의 데이터와 서로 비교하기 위함이며 이러한 개념을 JPA에선 *Dirty Checking(변경 감지)* 라고 말한다.

이러한 기능을 제공하는 이유는 아무래도 개발자에게 있어 DB 상태를 직접 관리하는 것이 아니라 객체의 상태관리에 집중하는것을 목표로 한것이 아닐까 싶다.


### ① 왜 setter를 여러 번 호출해도 UPDATE는 1번일까?

### Dirty Checking이 동작하지 않는 경우 






