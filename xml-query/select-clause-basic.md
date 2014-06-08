## select 쿼리 - 기본

XML 쿼리의 최상위 요소는 `<query></query>`입니다.

`query`요소는 속성(attribute)으로 id와 action를 가집니다. 각 속성은 쿼리의 id와 쿼리의 타입(select, insert, update, delete)을 나타냅니다.


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

column 요소를 사용하여 특정 컬럼만 조회할 수 있습니다.

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

### group by절

group 요소를 사용하여 group by절을 작성할 수 있습니다.

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
SELECT * FROM member WHERE user_id = ':user_id'; -- :user_id => user_id 변수의 값이 바인딩 됨
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

`var` 속성을 사용하여 쿼리에 변수를 할당할 수 있습니다.

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


`var`속성이 없거나 `var`속성에 변수가 할당되지 않았을 경우 `default` 속성을 사용하여 기본값을 지정할 수 있습니다.



### 다중 where절

조건이 두개 이상일 경우 `pipe`속성 사용할 수 있습니다. `pipe`속성은 and나 or를 지정할 수 있습니다.

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

중첩된 조건을 가진 where절의 경우 `group`요소를 사용할 수 있습니다.

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
```


> Note: group by에서 사용했던 `group`과 동일한 명칭을 사용합니다. 주의하세요.

