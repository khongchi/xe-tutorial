# XML Query

<!-- index start -->
TODO: 목차...
<!-- index end -->

## SELECT 쿼리 - GROUP BY절

`<group>` 요소를 사용하여 GROUP BY절을 작성할 수 있습니다.

```
<query id="getMember" action="select">
    <tables>
        <table name="member" />
    </tables>
    <columns>
        <column name="email_host" />
    </columns>
	<groups>
		<group column="email_host" />
	</groups>
</query>
```

```
SELECT email_host FROM member GROUP BY email_host;
```
