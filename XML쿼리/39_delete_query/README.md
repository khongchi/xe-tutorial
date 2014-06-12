
# XE 데이터베이스

TODO: 목차..

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
