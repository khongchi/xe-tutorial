
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


## select 쿼리 - 고급

이 장에서는 join과 subquery를 이용한 select 쿼리 방법을 설명합니다.

### join

left join(또는 left outer join), right join(또는 right outer join)을 사용할 수 있습니다.

```
SELECT files.* 
FROM xe_files as files 
	left join xe_member as member on files.member_srl = member.member_srl
WHERE files.isvalid = 'Y';
```

```
<query id="getFileList" action="select">
    <tables>
        <table name="files" alias="files" />
        <table name="member" alias="member" type="left join">
            <conditions>
                <condition operation="equal" column="files.member_srl" default="member.member_srl" />
            </conditions>
        </table>
    </tables>
    <columns>
        <column name="files.*" />
    </columns>
    <conditions>
        <condition operation="equal" column="files.isvalid" var="isvalid" pipe="and" />
    </conditions>
</query>
```

`table`요소의 `type` 속성에 사용할 join(left join|left outer join|right join|right outer join)을 명시한 후, `table`요소의 자식요소로 `contidion`요소를 추가하여 on절에서 사용될 조건문을 작성합니다.

> full outer join과 inner join은 제공하지 않습니다. inner join 대신, 암묵적인 inner join(implicit inner join)을 사용할 수 있습니다.



### sub query

#### select절에서의 subquery

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



