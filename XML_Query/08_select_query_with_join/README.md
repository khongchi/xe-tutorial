# XML Query

<!-- index start -->
TODO: 목차...
<!-- index end -->

## SELECT 쿼리 - JOIN

XML 쿼리를 이용하여 LEFT JOIN(또는 LEFT OUTER JOIN), RIGHT JOIN(또는 RIGHT OUTER JOIN)을 사용할 수 있습니다.

```
SELECT files.* 
FROM xe_files AS files 
 LEFT join xe_member AS member on files.member_srl = member.member_srl
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

`<table>`요소의 `type` 속성에 사용할 JOIN(LEFT JOIN|LEFT OUTER JOIN|RIGHT JOIN|RIGHT OUTER JOIN)을 명시한 후, `<table>`요소의 자식요소로 `<contidion>`요소를 추가하여 on절에서 사용될 조건문을 작성합니다.

> FULL OUTER JOIN과 INNER JOIN은 제공하지 않습니다. INNER JOIN 대신, 암묵적인 INNER JOIN(implicit INNER JOIN)을 사용할 수 있습니다.



