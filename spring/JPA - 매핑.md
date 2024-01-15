# JPA - 매핑

JPA 3번쨰 내용 정리입니다.

2편에선 JPA의 내부 동작 원리를 이해하기 위해 영속성 컨텍스트에 대한 내용을 다루었고 이번 글에서는 실제 객체와 테이블을 JPA에서 매핑시켜주기 위한 다양한 어노테이션과 기본키에 대한 개념을 이해하는것을 목표로 정리하겠습니다.

</br>
</br>

### 객체와 테이블 매핑

```
객체와 테이블 매핑 : @Entity, @Table

필드와 컬럼 매핑 : @Column

기본 키 매핑 : @Id

연관관계 매핑 : @ManyToOne, @JoinColumn
```

사실 실제 자바의 객체를 JPA의 엔티티로 등록하는것은 어렵지 않다. `@Entity` 어노테이션을 클래스에 붙이는것만으로 JPA는 테이블과 매핑해주기 때문이다.

다만 주의할점으로는 *기본 생성자 필수* 이며 `final, enum, interface, inner` 클래스는 사용할 수 없으며 필드에 `final` 키워드도 사용할 수 없다.


</br>
</br>

***주요 어노테이션 설명***
```

@TABLE(name = "example")

@Column(unique = true, length = 10)

```

✔️ @Table

내부 파라미터로 준 name의 값에 따른 TABLE을 매핑해준다.("example" 이라는 테이블을 찾아 매핑)

✔️ @Column(unique = true, length = 10)

해당 필드의 값이 유일해야 함을 나타낸다. 즉, 해당 필드가 중복된 값을 가질 수 없도록 데이터베이스에서 유니크 제약 조건을 생성하게 한다.(length는 말 그대로 길이 조건)

</br>
</br>


### 데이터베이스 스키마 자동 생성 속성

`yml` 파일이나 `properties` 파일에 설정하는 `hibernate.hbm2ddl.auto` 속성 정보에 대한 내용이다

</br>

```
create : 기존 테이블 삭제 후 다시 생성(DROP + CREATE)
create - drop : create와 같으나 종료시점에 테이블을 DROP
update : 변경분만 반영(운영 DB에는 사용 X)
validate : 엔티티와 테이블이 정상 매핑되었는지만 확인
none : 사용하지 않음
```

update 속성은 추가만 반영이 되며 필드를 삭제했다고 하더라도 DB 컬럼이 삭제되진 않는다.

- 개발 초기 단계는 create 또는 update
- 테스트 서버는 update 또는 validate(create 사용 시 데이터가 다 날아간다)
- 스테이징과 운영 서버는 validate 또는 none

</br>

=> 여러명이 사용하는 개발 또는 테스트서버에서도 create나 update는 지양하는 자세가 좋다.(운영 장비에는 절대로 create, update 사용하지 않는다.)

</br>

### Entity 생성 예제

``` java
@Entity
public class Member {

	@Id
	private Long id;

	@Column(name = "name")
	private String username;

	private int age;

	@Enumerated(EnumType.STRING)
	private RoleType roleType;

	@Temporal(TemporalType.TIMESTAMP)
	private Date createdDate;

	@Temporal(TemporalType.TIMESTAMP)
	private Date lastModifiedDate;

	@Lob
	private String description;

    @Transient
    private int temp;

}

```

아래는 헷갈리는 어노테이션에 대한 이해를 돕기 위한 내용이다. 

✔️ @Euumerated
자바의 Enum 타입의 데이터를 매핑시켜줄때 사용하는 어노테이션으로 파라미터 값으로

숫자형 데이터 `ORDINAL`, 문자형 타입이라면 `STRING` 으로 사용하고 기본 값은 `ORDINAL`이다. 

주의할점은 EnumType의 ORDINAL Enum의 순서를 그대로 저장하고, STRING은 Enum의 이름을 그대로 저장하는데 ORDINAL은 사용하지 않는다.

그 이유로는 만약 EnumType이 추가 된 경우 `ORDINAL` 속성은 자바 소스의 위치를 기준으로 값을 부여하기 때문에 이전에 위치했던 `EnumType` 과 구분이 안되는 문제가 발생할 수 있다.

이러한 위험성을 가지고 있기 때문에 기본 설정인 `ORDINAL` 은 사용하지 않는다.

</br>
</br>

✔️ @Temporal

`@Temporal` 이라는 어노테이션을 보면 내부에 아래와 같이 작성되어 있다.

``` java
package javax.persistence;

/**
 * Type used to indicate a specific mapping of <code>java.util.Date</code> 
 * or <code>java.util.Calendar</code>.
 *
 * @since 1.0
 */
public enum TemporalType {

    /** Map as <code>java.sql.Date</code> */
    DATE, 

    /** Map as <code>java.sql.Time</code> */
    TIME, 

    /** Map as <code>java.sql.Timestamp</code> */
    TIMESTAMP
}
```

자바의 DateTime 안에는 날짜와 시간 정보가 모두 있지만, 데이터 베이스는 시간 타입을 구분해서 사용하기 떄문에 상세 정보(@Date, @TimeStamp ..) 등의 정보를 지정해줄때 사용하는 어노테이션이다.

</br>

하지만 `자바 1.8` 이후 ***LocalDate***와 ***LocalDateTime***을 사용할 수 있기 때문에 위를 무시하고 필드 정보에 ***LocalDate***, ***LocalDateTime***을 사용하는게 더 편리하다.

</br>
</br>

✔️ @Lob

Clob 타입의 데이터를 매핑시켜주는 어노테이션이다.

매핑하는 필드 타입이 문자면 `CLOB`, 나머지는 `BLOB` 으로 매핑시킨다.


</br>
</br>

✔️ @Transient

위 어노테이션은 Entity 클래스 필드를 매핑에서 제외시킬떄 사용하는 어노테이션이다.

주로 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용한다.

</br>
</br>


### 기본키 매핑

기본 키 매핑을 적용하는 예제 코드이다.

``` java
@Entity
public class Member {

	@Id
    @GeneratedValue(strategy = GenerationType.Identity)
	private Long id;
    ...
```

사용할 필드에 `@Id`를 붙이면 된다. JPA는 설정한 필드를 PK 컬럼으로 지정해준다.

</br>

✔️ @GeneratedValue

***1. GenerationType.Identity***

3개의 생성 전략이 있는데 위에서 사용한 `GenerationType.Identity`는 id 컬럼에 auto_increment 속성을 주는 전략이다.

즉 자바에선 해당 필드 값에 대한 설정을 하지 않고 이후 추가 된 값에 대해 DB가 알아서 값을 세팅해주고 적재하는 전략이다.

이 전략의 단점으로는 ID 값이 실제 DB에 들어간 이후에 알 수 있기 때문에 이전에 `영속성 컨텍스트` 의 `1차 캐시` 값이 설정될수가 없다.

그래서 위 전략을 사용하는 경우에 한해 영속성 컨텍스트는 `em.persist()` 시점에 즉시 `INSERT SQL`이 발생하고 해당 `ID`값을 조회 후 `1차 캐시`에 세팅한다.

그렇기 때문에 원래 영속성 컨테스트의 속성인 `commit` 을 호출 했을 떄 한번에 DB 적재되는게 불가능하다.

</br>
</br>

***2. GenerationType.SEQUENCE***

말 그대로 데이터베이스에 있는 시퀀스 오브젝트를 통해서 값을 생성하는 전략이다.

위 전략을 사용하면 JPA가 신규 데이터에 대해 INSERT 하기 이전에 

`call next value for hibernate_sequence`를 호출해서 다음 seq 값을 조회 후 세팅해서 insert sql을 생성해주고 세팅된 값이 DB에 들어가게 된다.(hibernate_sequence는 기본)

위 전략을 사용했을떄 시퀀스를 매핑해주려고 한다면 `@SequenceGenerator` 를 사용해 매핑할 데이터베이스 시퀀스를 지정해줄 수 있다.

또한 이 전략을 사용했을때에도, `영속성 컨텍스트`는 `1차 캐시`에 값을 지정해주기 위해 `시퀀스` 전략을 위해 사용하는 테이블을 호출해서 시퀀스 값을 가져온다.

</br>
</br>

***3. GenerationType.TABLE***

키 생성 전용 테이블을 하나 만들어서 데이터베이스 시퀀스를 흉내내는 전략이다.

모든 데이터베이스 적용이 가능하지만 성능이 상대적으로 떨어진다는 단점이 있다.

</br>
</br>

### 기본 키 사용 전략

기본 키 제약조건은 null이 아니고, 유일해야하며, 변하면 안되는 컬럼을 사용한다.

하지만 미래까지 변하지 않는 자연키를 찾기는 어렵다.(대리키, 대체키를 사용한다.)

권장 방식은 : `Long형 + 대체키 + 키 생성전략`을 사용한다.

비즈니스 정보를 키로 가져오는것은 권장하지 않는다.

</br>
</br>

## Reference

[자바 ORM 표준 JPA](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)



