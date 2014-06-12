# XML Query

TODO: 목차...

## SELECT 쿼리 - 기본

XML 쿼리의 최상위 요소(element)는 `<query>`입니다.

`<query>` 요소는 속성(attribute)으로 `id`와 `action`을 가집니다. 각 속성은 쿼리의 id와 쿼리의 타입(select, insert, update, delete)을 나타냅니다.


### SELECT절

`<table>`요소는 FROM절을 구성합니다. 테이블을 지정합니다.

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

`<column>`요소를 사용하여 SELECT절을 구성합니다. 특정 컬럼만 조회할 수 있습니다.

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
SELECT m.user_id AS uid, nick_name AS nick FROM member AS m;
```
