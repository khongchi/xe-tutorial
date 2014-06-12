
# XE 데이터베이스

TODO: 목차..

## select 쿼리 - 서브쿼리

XML 쿼리는 서브쿼리를 제공합니다.

### select절에서 서브쿼리

select절에서 subquery를 사용할 수 있습니다.

```
select 	*, (select count(*) as "count" 
			from "xe_documents" as "documents" 
			where "documents"."user_id" = "member"."user_id" 
 		) as "totaldocumentcount" 
 from "xe_member" as "member" 
 where "user_id" = 7
 ```
 
```
<query id="getStatistics" action="select">
    <tables>
        <table name="member" alias="member" />
    </tables>
    <columns>
        <column name="*" />
        <query id="getMemberDocumentCount" alias="totalDocumentCount">
            <tables>
                <table name="documents" alias="documents" />
            </tables>
            <columns>
                <column name="count(*)" alias="count" />
            </columns>
            <conditions>
                <condition operation="equal" column="documents.user_id"
                           default="member.user_id" />
            </conditions>
        </query>
    </columns>
    <conditions>
        <condition operation="equal" column="user_id" var="user_id" />
    </conditions>
</query>

```

#### where절에서의 subquery

where절에서 subquery를 사용할 수 있습니다.

```
SELECT * 
FROM xe_member as member 
WHERE regdate = (SELECT MAX(regdate) as regdate 
				FROM xe_documents as documents 
				 WHERE documents.user_id = member.user_id) 
```

```
<query id="getMemberInfo" action="select">
    <tables>
        <table name="member" alias="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <query operation="equal" column="regdate" alias="documentMaxRegdate">
            <tables>
                <table name="documents" alias="documents" />
            </tables>
            <columns>
                <column name="max(regdate)" alias="maxregdate" />
            </columns>
            <conditions>
                <condition operation="equal" column="documents.user_id"
                           var="member.user_id" notnull="notnull" />
            </conditions>
        </query>
    </conditions>
</query> 
```

#### from절에서의 subquery

from절에서 subquery를 사용할 수 있습니다.

서브쿼리를 `table` 요소로 사용하세요. `table` 요소에 `query` 속성을 'true'으로 지정하고, 서브쿼리를 넣으면 됩니다.

```
SELECT m.member_srl, m.nickname, m.regdate, a.count 
FROM 
	(SELECT documents.member_srl as member_srl, count(*) as count 
	 FROM xe_documents as documents 
	 GROUP BY documents.member_srl) as a 
LEFT JOIN xe_members m on m.member_srl = a.member_srl
```

```
<query id="getMemberInfo" action="select">
    <tables>
        <table query="true" alias="a">
            <table>
                <table name="documents" alias="documents" />
            </table>
            <columns>
                <column name="member_srl" alias="member_srl" />
                <column name="count(*)" alias="count" />
            </columns>
            <groups>
                <group column="member_srl" />
            </groups>
        </table>
        <table name="member" alias="m" type="left join">
            <conditions>
                <condition operation="equal" column="m.member" default="a.member_srl" />
            </conditions>
        </table>
    </tables>
    <columns>
        <column name="m.member_srl" />
        <column name="m.nickname" />
        <column name="m.regdate" />
        <column name="a.count" />
    </columns>
</query>
```

