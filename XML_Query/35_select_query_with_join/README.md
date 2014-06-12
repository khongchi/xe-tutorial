
# XE 데이터베이스

TODO: 목차..

## select 쿼리 - join

XML 쿼리를 이용하여 left join(또는 left outer join), right join(또는 right outer join)을 사용할 수 있습니다.

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

`<table>`요소의 `type` 속성에 사용할 join(left join|left outer join|right join|right outer join)을 명시한 후, `<table>`요소의 자식요소로 `<contidion>`요소를 추가하여 on절에서 사용될 조건문을 작성합니다.

> full outer join과 inner join은 제공하지 않습니다. inner join 대신, 암묵적인 inner join(implicit inner join)을 사용할 수 있습니다.



