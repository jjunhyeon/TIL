## JPA 

JPA의 기본 개념과 영속성 컨텍스트에 대해 다시 정리하며 이해하기 위해 작성합니다. 

<br>

## 1.Entity ##

Java 소스에서 테이블과 매핑한 클래스를 만드는 방법이다.


``` java
import jakarta.persistence.*;

@Entity  // 이 클래스는 DB 테이블과 매핑됨
@Table(name = "users")  // 테이블 이름 지정 (선택)
public class User {

    @Id  // Primary Key
    @GeneratedValue(strategy = GenerationType.IDENTITY)  // Auto Increment
    private Long id;

    @Column(nullable = false, length = 100) // DB 컬럼 속성 지정
    private String userName;
    ...
}
```

**✔️ @Entity** <br>
: 해당 클래스는 JPA가 관리하는 엔티티 클래스

**✔️@Id** <br>
: Primary Key 

**✔️@GeneratedValue**  <br>
: 기본 키 자동 증가 (Auto Increment)

**✔️@Column** <br>
: 말 그대로 해당 엔티티의 컬럼이다.

<br>

<br>

## 2.Persistence Context ##

다음은 JPA를 사용하면서 가장 중요하다 생각하는 **영속성 컨텍스트**이다.

``` java
@Transactional
public void testPersistence(Long userId) {
    User userA = userRepository.findById(userId).orElseThrow(); 
    userA.setUserName("Updated Name");  // 영속성 컨텍스트에서 값 변경됨

    User userB = userRepository.findById(userId);  
    System.out.println(userB.getUserName());  // 같은 트랜잭션에서는 변경된 값 "Updated Name" 출력

    // commit 시점에 변경 사항이 자동으로 DB에 반영됨 (flush 발생)
}
```
✔️ 같은 트랜잭션에서는 userA와 userB가 동일한 객체 <br>
✔️ 변경 사항은 DB가 아니라 영속성 컨텍스트에 저장 <br>
✔️ 트랜잭션이 끝날 때 flush()가 호출되며 변경 사항이 DB에 반영됨 <br>

<br>
<br>

`영속성 컨텍스트` <br>
**JPA가 관리하는 엔티티 객체의 집합**을 의미한다.

데이터베이스와 상호작용하는 동안, 객체들은 영속성 컨텍스트에 의해 관리되며, 이 객체들은 영속 상태에 있다고한다.

✔️ 영속성 컨텍스트의 역할
- 데이터베이스와 객체 간의 매핑을 관리하고, 캐시 역할을 하며, 엔티티의 상태를 추적한다.

- 엔티티가 영속성 컨텍스트에 포함되면, 그 객체에 대한 변경 사항은 트랙잭션 커밋 시 자동으로 데이터베이스에 반영된다.

<br>
<br>

## ✅ 느낀점

📌 영속성 ***컨텍스트의 역할***에 대해 이해할 수 있었다.

프로젝트 소스 분석 중 `findById`를 통해 조회한 객체가 update 메서드가 없음에도 왜 바뀐건지, 바로 이해가 되지 않아 정리를 시작했는데 정리하며 객체의 상태 변화와 업데이트되는 과정에 대해 이해할 수 있었다.


