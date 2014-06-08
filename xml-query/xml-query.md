## XML 쿼리

### XML 쿼리의 작성 및 등록

XML 쿼리는 XML  쿼리 언어(XML Query Language)로 작성되며, 하나의 쿼리는 하나의 XML파일에 저장됩니다.

쿼리 XML파일은 3가지 경로에 등록될 수 있습니다.

* 애드온 디렉토리의 하위 queries 디렉토리

	예) ./addons/adminlogging/queries/

* 모듈 디렉토리의 하위 queries 디렉토리


	예) ./modules/member/queries/

* 위젯 디렉토리의 하위 queries 디렉토리


	예) ./widgets/content/queries/
	
* [select 쿼리 - 기본](select-clause-basic.md)
* [select 쿼리 - 고급](select-clause-basic.md)
* [insert 쿼리](insert-update-delete-clause.md)
* [update 쿼리](insert-update-delete-clause.md)
* [delete 쿼리](insert-update-delete-clause.md)
	
### XML 쿼리 ID

XML  쿼리는 각각 쿼리 ID를 가집니다. 이 쿼리 ID는 쿼리 XML파일의 파일명이어야 하며, XML 쿼리문 내에도 명시됩니다.

```
<!-- ./modules/member/queries/getMemberInfo.xml -->

<query id="getMemberInfo" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="*" />
    </columns>
    <conditions>
        <condition operation="equal" column="user_id" var="user_id" notnull="notnull" />
    </conditions>
</query>

```

위 쿼리문은 ID가 `getMemberInfo`이며 `getMemberInfo.xml`에 저장됩니다.

### XML 쿼리의 실행

등록해 놓은 XML 쿼리문은 XE의 PHP 코드에서 필요할 때마다 executeQuery(), executeQueryArray()함수를 이용하여 질의할 수 있습니다.

```
function executeQuery($xml_query_name, $args = null); 
```

executeQuery() 함수는 ./classes/db/DB.classs.php에 있는 DB::executeQuery() 함수의 별칭(alias)입니다.

이 함수는 실제 DB 데이터를 조작하고 사용된 DB에 따라 XML 쿼리가 네이티브 SQL로 파싱된 후에 결과를 수신합니다

executeQueryArray() 함수는 결과가 항상 array로 리턴되는 것 이외에는 executeQuery()함수와 동일합니다.

```
// set query variables
$arg = new stdClass();
$arg->user_id = 'admin';

// execute query
$output = executeQuery('member.getMemberInfo', $arg);

// using query result
if($output->toBool())
{
    $member = $output->data;
    echo $member->nickname; 
}
```


