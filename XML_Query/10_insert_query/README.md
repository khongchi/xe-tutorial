# XML Query

TODO: 목차...

## insert 쿼리

```
<query id="insertMember" action="insert">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="member_srl" var="member_srl" filter="number" notnull="notnull" />
        <column name="user_id" var="user_id" filter="userid" notnull="notnull" />
    </columns>
</query>
```

```
INSERT INTO member (member_srl, user_id) VALUES (:member_srl, :user_id);
```

insert 쿼리는 `query` 요소의 `action` 속성을 'insert'로 지정합니다.

`table` 요소는 row를 삽입할 table명을 지정하고 `column`요소에는 value를 지정합니다.

앞서 SELECT 쿼리에서 보았던 `condition`요소의 속성들(`var`, `default`, `filter`, `notnull`, `minlength`, `maxlength`)을 동일하게 사용할 수 있습니다.

