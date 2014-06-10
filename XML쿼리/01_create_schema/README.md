
# XE 데이터베이스


- [Quick Start]()
- [Schema]()
	- [create schema]()
	- [alter schema]()
- [Query]()
	- [SELECT 쿼리]()
	- [GROUP BY절]()
	- [WHERE절]()
	- [ORDER BY절, LIMIT절 & 페이징 처리(navigation)]()
	- [JOIN]()
	- [Sub Query]()
	- [INSERT 쿼리]()
	- [UPDATE 쿼리]()
	- [DELETE 쿼리]()


## 테이블 생성하기

XE의 구성요소 중에서 DB 테이블을 생성할 수 있는 요소는 오직 '모듈' 뿐입니다. 그 예로 회원 모듈은 member, member_group 등의 테이블을 가집니다. 

테이블을 생성하려면, XML Schema 언어를 사용하여 테이블 스키마를 정의해야 합니다. 아래는 XML 스키마 언어를 이용하여 정의한 회원 테이블(member table)의 스키마입니다.

```
<!-- modules/member/schemas/member.xml -->

<table name="member">
    <column name="member_srl" type="number" size="11" notnull="notnull" primary_key="primary_key" />
    <column name="user_id" type="varchar" size="80" notnull="notnull" unique="unique_user_id" />
    <column name="email_address" type="varchar" size="250" notnull="notnull" unique="unique_email_address" />
    <column name="password" type="varchar" size="60" notnull="notnull" />
    <column name="email_id" type="varchar" size="80" notnull="notnull" />
    <column name="email_host" type="varchar" size="160" index="idx_email_host" />
    <column name="user_name" type="varchar" size="40" notnull="notnull" />
    <column name="nick_name" type="varchar" size="40" notnull="notnull" unique="unique_nick_name" />
    <column name="find_account_question" type="number" size="11" />
    <column name="find_account_answer" type="varchar" size="250" />
    <column name="homepage" type="varchar" size="250" />
    <column name="blog" type="varchar" size="250" />
    <column name="birthday" type="char" size="8" />
    <column name="allow_mailing" type="char" size="1" default="Y" notnull="notnull" index="idx_allow_mailing" />
    <column name="allow_message" type="char" size="1" default="Y" notnull="notnull" />
    <column name="denied" type="char" size="1" default="N" index="idx_is_denied" />
    <column name="limit_date" type="date" />
    <column name="regdate" type="date" index="idx_regdate" />
    <column name="last_login" type="date" index="idx_last_login" />
    <column name="change_password_date" type="date" />
    <column name="is_admin" type="char" size="1" default="N" index="idx_is_admin" />
    <column name="description" type="text" />
    <column name="extra_vars" type="text" />
    <column name="list_order" type="number" size="11" notnull="notnull" index="idx_list_order" />
</table>

```

테이블 스키마는  테이블 하나당 하나의 xml파일에 작성해야 하며, 파일명은 테이블명과 일치해야 합니다. 스키마 파일은 모듈 폴더 안의 schemas 폴더에 저장하세요.

XE는 모듈을 설치할 때, 모듈의 schemas 폴더에 등록된 스키마들을 조회하여 테이블을 생성합니다.

## XML 스키마 정의

```
<table name="member">
...
</table>
```

XML스키마의 최상위 요소(element)는 `table`입니다.  `name`속성은 테이블명입니다.


```
    <column name="member_srl" type="number" size="11" notnull="notnull" primary_key="primary_key" />
```

`column`요소는 테이블의 컬럼을 정의합니다. 속성은 아래와 같습니다.

* name - 컬럼명
* type - 데이터 타입
* size - 데이터 사이즈
* notnull - notnull 여부
* primary_key - 기본키 여부
* index - 이 컬럼에 인덱스를 지정합니다.
* unique - 이 컬럼에 unique index를 지정합니다.
* default - 기본값을 지정합니다.



