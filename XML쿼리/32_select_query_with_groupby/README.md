
# XE 데이터베이스

TODO: 목차..




## select 쿼리 - group by절

`<groups><group>` 요소를 사용하여 group by절을 작성할 수 있습니다.

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
