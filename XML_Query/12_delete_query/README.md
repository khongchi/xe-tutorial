# XML Query

<!-- index start -->

- [Quick Start](/)
- [테이블 생성하기](01_create_schema/)
- [테이블 변경하기](02_alter_schema/)
- [XML 쿼리 기본](03_xml_query/)
- [SELECT 쿼리](04_select_query_basic/)
- [SELECT 쿼리 - GROUP BY절](05_select_query_with_groupby/)
- [SELECT 쿼리 - WHERE절](06_select_query_with_where/)
- [SELECT 쿼리 - ORDER BY절](07_select_query_with_navigation/)
- [SELECT 쿼리 - JOIN](08_select_query_with_join/)
- [SELECT 쿼리 - 서브쿼리](09_select_query_with_subquery/)
- [INSERT 쿼리](10_insert_query/)
- [UPDATE 쿼리](11_update_query/)
- [DELETE 쿼리](12_delete_query/)
- [쿼리 실행하기](13_execute_query/)

<!-- index end -->

## DELETE 쿼리
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

DELETE 쿼리는 `query` 요소의 `action` 속성을 'delete'로 지정합니다.
