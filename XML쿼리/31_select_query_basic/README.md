
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



## select 쿼리 - 기본

XML 쿼리의 최상위 요소(element)는 `<query></query>`입니다.

`query` 요소는 속성(attribute)으로 `id`와 `action`을 가집니다. 각 속성은 쿼리의 id와 쿼리의 타입(select, insert, update, delete)을 나타냅니다.


### select절

member 테이블의 모든 row를 조회할 수 있습니다.

```
<query id="getMember" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
</query>
```

```
SELECT * FROM member;
```

`column` 요소를 사용하여 특정 컬럼만 조회할 수 있습니다.

```
<query id="getMember" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="user_id" />
        <column name="nick_name" />
    </columns>
</query>
```

```
SELECT user_id, nick_name FROM member;
```

### alias 속성 사용하기

`alias` 속성을 이용하여 별칭을 지정할 수 있습니다. `table`, `column`, `query` 요소에서 사용할 수 있습니다.


```
<query id="getMember" action="select">
    <tables>
        <table name="member"  alias="m" />
    </tables>
    <columns>
        <column name="m.user_id" alias="uid" />
        <column name="m.nick_name" alias="nick" />
    </columns>
</query>
```

```
SELECT m.user_id as uid, nick_name as nick FROM member as m;
```


### group by절

`group` 요소를 사용하여 group by절을 작성할 수 있습니다.

```
<query id="getMember" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="email_host" />
    </columns>
	<groups>
		<group column="email_host" />
	</groups>
</query>
```

```
SELECT email_host FROM member GROUP BY email_host;
```

### where절

where절은 `condition` 요소를 사용하여 작성할 수 있습니다. where절은 select, update, delete 쿼리에서 모두 동일하게 사용됩니다.

```
<query id="getMemberInfo" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="user_id" default="admin" var="user_id" />
    </conditions>
</query>
```

```
SELECT * FROM member WHERE user_id = ':user_id'; 
-- :user_id => user_id 변수의 값이 바인딩 됨
```

#### operation 속성

`condition` 요소의 `operation` 속성은  조건 연산자 지정합니다. 아래의 연산자를 가질 수 있습니다.


* equal : column = (var|default)
* more : column >= (var|default)
* excess : column > (var|default)
* less : column <= (var|default)
* below : column < (var|default)
* notequal : column != (var|default)
* notnull : column is not null
* null : column is null
* like_prefix : column like 'var|default%'
* like_tail : column like '%var|default'
* like : column like '%var|default%'
* in : column in (var|default)
* notin : column not in (var|default) 


#### var 속성

`condition` 요소의 `var` 속성을 사용하여 쿼리에 변수를 할당할 수 있습니다.

executeQeury()함수를 실행할 때, 두번째 파라메터에 할당한 변수의 값이 바인딩됩니다.

```
$arg = new stdClass();
$arg->user_id = 'khongchi'; // user_id인 var 속성에 'khongchi'가 바인딩 됨
$output = executeQuery('member.getMemberInfo', $arg);
```

```
SELECT * FROM member WHERE user_id = 'khongchi';
```


#### default 속성

`condition` 요소에 `var` 속성이 없거나 `var`속성에 변수가 바인딩되지 않았을 경우 `default` 속성을 사용하여 기본값을 지정할 수 있습니다.


#### notnull 속성

`condition` 요소의 `notnull`속성을 이용하여 사용자가 `var` 속성에 변수 바인딩을 빼트리지 않도록 차단할 수 있습니다.

`notnull=notnull`로 지정하면 됩니다. 단, `notnull=notnull`이 지정되어 있고 `var` 속성에 변수가 바인딩 되지 않아도 `default` 속성이 지정되어 있을 경우에는 쿼리가 정상적으로 실행됩니다. 이럴 경우 default 값을 사용하게 됩니다.


#### filter 속성

`condition` 요소의 `filter`속성을 이용하면 `var`속성에 바인딩된 변수의 값을 실제 DB에 질의하기 전에 필터링 할 수 있습니다.

```
<query id="getMemberInfo" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="user_id" default="admin" var="user_id" filter="user_id" />
    </conditions>
</query>
```

`filter`의 종류는 아래와 같습니다.

* email, email_address: 메일 형식
* homepage: http|https:// 같은 웹사이트 주소 형식
* userid, user_id: XE 의 사용자 id 형식(첫 두 글자는 알파벳이어야 합니다. 세 번째 문자부터 number+alphabet+ _ 형식으로 합니다.)
* number: 숫자 허용
* alpha: 알파벳 허용
* alpha_number: 숫자와 문자 모두 허용

`var` 속성에 바인딩된 변수의 값이 지정된 `filter`에 충족하지 못하면 쿼리는 실행되지 않습니다.

#### maxlength, minlength 속성

`condition` 요소의 `maxlength`, `minlength` 속성을 사용하여 바인딩된 변수의 최소/최대 길이를 필터링할 수 있습니다.





### 다중 where절

조건이 두개 이상일 경우 `pipe` 속성 사용할 수 있습니다. `pipe`속성은 'and'나 'or'를 지정할 수 있습니다.

```
<query id="getMemberInfo" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="user_id" var="user_id"/>
        <condition operation="equal" column="is_admin" default="Y" pipe="and" />
    </conditions>
</query>
```

```
SELECT * FROM member WHERE user_id = ':user_id' and is_admin = 'Y';
```

### 중첩 where절

중첩된 조건을 가진 where절의 경우 `group` 요소를 사용할 수 있습니다.

```
<query id="getMemberList" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="is_admin" var="is_admin" />
        <group pipe="and">
            <condition operation="equal" column="user_id" var="s_user_id" />
            <condition operation="equal" column="user_name" var="s_user_name" pipe="or" />
        </group>
    </conditions>
</query>
```

```
SELECT * FROM member WHERE is_admin = ':is_admin' and (user_id = ':s_user_id' or user_name = ':s_user_name' )
-- ex) SELECT * FROM member WHERE is_admin = 'Y' and (user_id = 'khongchi' or user_name = '콩치' )
```


> Note: group by에서 사용했던 `group`과 동일한 명칭을 사용합니다. 주의하세요.


### order by절

XML 쿼리문에서는 order by, offset, limit절 그리고 paging 처리를 위한 pagination을 구현하기 위해 `navigation` 요소를 사용합니다.

`navigation` 요소는 자식요소로 `index`, `list_count`, `page_count`, `page` 요소를 갖습니다.


order by절은 `index` 요소로 작성할 수 있습니다.

`index` 요소의 `var` 속성은 정렬 기준 컬럼명이 바인딩되는 변수명이며, `order` 속성은 정렬 차순(내림차순, 오름차순)이 바인딩되는 변수명입니다.

```
<query id="getMemberList" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="is_admin" var="is_admin" />
    </conditions>
    <navigation>
        <index var="sort_index" default="list_order" order="sort_order" />
        <list_count var="list_count" default="20" />
        <page_count var="page_count" default="10" />
        <page var="page" default="1" />
    </navigation>
</query>
```
```
SELECT * FROM member WHERE is_admin = ':is_admin' ORDER BY :sort_index :sort_order OFFSET 
-- ex) SELECT * FROM member WHERE is_admin = 'Y' ORDER BY user_id desc
```


> Note: XML 쿼리에서 order by나 offset, limit절은 별도로 사용할 수 없습니다. `navigation` 요소를 통해 항상 동시에 사용됩니다.

### offset, limit절 및 pagination

`navigation` 요소의 자식요소인 `list_count`, `page_count`, `page` 요소를 사용하여 작성됩니다.

각 요소의  `var` 속성은 변수바인딩, `default` 속성은 기본값을 지정합니다.

#### list_count 요소

'limit'에 해당됩니다.

#### page_count 요소

pagination에서 출력될 페이지 수를 지정합니다.

#### page 요소

pagination에서 현재 페이지를 지정합니다.

```
<query id="getMemberList" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="is_admin" var="is_admin" />
    </conditions>
    <navigation>
        <index var="sort_index" default="list_order" order="sort_order" />
        <list_count var="list_count" default="20" />
        <page_count var="page_count" default="10" />
        <page var="page" default="1" />
    </navigation>
</query>
```
```
SELECT * FROM member WHERE is_admin = ':is_admin' ORDER BY :sort_index :sort_order
-- 예) 한 페이지에 20(default)개씩 출력할 때, 1(default)페이지에 해당하는 목록을 조회 => limit 0, 20

```

> pagination은 게시판의 목록 페이지와 같이 쿼리 결과를 뷰페이지에 출력할 때, 목록 하단에 나오는 page navigation을 처리하기 위한 기능입니다.






