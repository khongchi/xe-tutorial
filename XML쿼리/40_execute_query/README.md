
#XE 데이터베이스


TODO: 목차...



## 쿼리 실행하기

아래는 `user_id`가 'admin'인 회원을 검색하는 코드입니다.

```
$arg = new stdClass();
$arg->user_id = 'admin';
$output = executeQuery('member.getMemberInfo', $arg);

if($output->toBool())
{
	$member = $output->data;
	echo $member->nickname;	
}
```

### executeQuery 함수 실행

한번 작성하여 등록 된 XML 쿼리는 `executeQuery()`, `executeQueryArray()`함수를 사용하여 실행할 수 있습니다.


```
/* ./config/func.inc.php */

function executeQuery($query_id, $args = NULL, $arg_columns = NULL)	
function executeQueryArray($query_id, $args = NULL, $arg_columns = NULL

```

#### 첫번째 파라메터

첫번째 파라메터는 XML쿼리 ID입니다. 

XML쿼리는 애드온, 모듈, 위젯이 소유할 수 있습니다. 소유하는 구성요소에 따라 아래와 같이 첫번째 파라메터의 규칙이 바뀝니다.

쿼리가 소속된 구성요소 | 규칙 | 예시
-------|-------|-------
모듈 | `module_name`.`query_id` | `board.getBoardList`
애드온| addon.`addon_name`.`query_id`|`addon.counter.getCount`
위젯|widget.`widget_name`.`query_id`|`widget.content.getDocumentList`


#### 두번째 파라메터

두번째 파라메터는 쿼리에 바인딩 될 변수의 모음입니다.

```
/* ./modules/member/queries/getMemberInfo.xml */

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
위 쿼리의 `user_id` 변수에  바인딩시키고 싶은 값('admin')이 있다면 아래와 같이 두번째 파라메터를 지정하세요.

```
$arg = new stdClass();
$arg->user_id = 'admin';
$output = executeQuery('member.getMemberInfo', $arg);
```

### 쿼리 실행 결과

결과값은 Object 클래스의 객체로 반환됩니다

```
// $output을 json 형식으로 출력

{
	"error":0,
	"message":"success",
	"variables":{
	"_query":"SELECT * FROM documents LIMIT 10",
	"_elapsed_time":"0.00082"
	},
	"httpStatusCode":null,
	"total_count":4181,
	"total_page":210,
	"page":1,
	"data":{
		.....
	},
	"page_navigation":{		// navigation을 사용한 쿼리일 경우
		"total_count":4181,
		"total_page":210,
		"cur_page":1,
		"page_count":"10",
		"first_page":1,
		"last_page":210,
		"point":0
	}
}
```

#### 성공 여부 조회
```
$output->toBool() // true면 성공, false이면 실패
```

#### 성공시 결과 데이터 조회
```
$rows = $output->data;
```

`executeQuery()`함수는 `$output->data`에 담긴 row가 두개 이상이면 array로 반환합니다.

`executeQueryArray()` 함수는 `$output->data`의 row 개수와 상관없이 array로 반환합니다. row가 0개일 경우 빈 array 반환합니다.

#### 실패시 오류메세지 보기

```
$output->getMessage();
```



