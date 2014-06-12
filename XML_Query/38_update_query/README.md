
# XE 데이터베이스

TODO: 목차..


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

