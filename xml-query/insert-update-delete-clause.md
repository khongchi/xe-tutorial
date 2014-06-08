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

앞서 select 쿼리에서 보았던 `condition`요소의 속성들(`var`, `default`, `filter`, `notnull`, `minlength`, `maxlength`)을 동일하게 사용할 수 있습니다.


## update 쿼리

```
<query id="updateMember" action="update">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="password" var="password" notnull="notnull" />
        <column name="user_name" var="user_name" notnull="notnull" minlength="2" maxlength="40" />
    </columns>
    <conditions>
        <condition operation="equal" column="member_srl" var="member_srl" notnull="notnull" filter="number" />
    </conditions>
</query>
```
```
UPDATE xe_member as member SET password = :password, user_name = :user_name WHERE member_srl = :member_srl;
```

update 쿼리는 `query` 요소의 `action` 속성을 'update'로 지정합니다.


## delete 쿼리
```
<query id="deleteMember" action="delete">
    <tables>
        <table name="member" />
    </tables>
    <conditions>
        <condition operation="equal" column="member_srl" var="member_srl" notnull="notnull" filter="number" />
    </conditions>
</query>
```
```
DELETE FROM xe_member WHERE member_srl = :member_srl;
```

delete 쿼리는 `query` 요소의 `action` 속성을 'delete'로 지정합니다.
