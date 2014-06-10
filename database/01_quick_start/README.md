
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


## Quick Start

XE에서는 DB SQL 쿼리를 사용하기 위하여 'XML 쿼리 언어'(XML Query Language)를 제공합니다.  각 DBMS의 네이티브 SQL 대신 XML 쿼리 언어를 이용함으로써 다양한 DBMS에 범용적으로 사용할 수 있는 쿼리를 만들수 있습니다. 또한 쿼리의 재사용이 가능해집니다.

XE에서 DB에 질의하는 단계는 아래와 같습니다.

* 테이블 스키마 정의
* XML 쿼리문 정의
* XML 쿼리문의 실행

### 테이블 스키마 정의

XE에서 DB에 질의를 하기 위해서는 사용되는 DB테이블의 스키마를 먼저 정의해 놓아야 합니다. 스키마를 정의하기 위하여 XE의 'XML 스키마 언어'(XML Schema Language)를 사용합니다.

아래는 XML 스키마 언어를 이용하여 XE 회원테이블(member table)의 스키마를 정의한 예입니다.

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

XE에서는 DB질의시 대상이 되는 테이블의 스키마 정보를 항상 참조합니다. 따라서 XE에서는 스키마 정보가 등록되지 않은 테이블은 사용될 수 없습니다.

> Note: XML 스키마 언어의 자세한 사용법은 차후 제공될 예정입니다.

### XML 쿼리문 정의

XML 쿼리문의 한 예입니다. XE의 회원테이블에서 회원을 조회하는 쿼리입니다.

```
<!-- modules/member/queries/getMemberInfo.xml -->

<query id="getMemberInfo" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="user_id" var="user_id" notnull="notnull" />
    </conditions>
</query>
```

XML 쿼리 언어의 자세한 사용법

* [XML 쿼리](xml-query/xml-query.md)

### XML 쿼리문의 실행

등록되어 있는 XML 쿼리문은 XE의 PHP 코드에서 필요할 때마다 질의할 수 있습니다. `executeQuery()`, `executeQueryArray()`함수가 사용됩니다.

아래는 `user_id`가 'admin'인 회원을 검색하는 코드입니다.

```
$arg = new stdClass();
$arg->user_id = 'admin';
$output = executeQuery('member.getMemberInfo', $arg);

if($output->toBool())
{
	$member = $output->data;
	echo $member->nickname;	
}
```



