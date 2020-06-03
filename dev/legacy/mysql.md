# MySQL 서비스 생성 및 연결

{% hint style="info" %}
C.F. CLI 를 이용한 서비스 생성 및 바인딩은 해당 [메뉴얼](../../service/mysql/mysql-sub-2.md)에서 확인하기 바란다.
{% endhint %}

### MySQL 서비스 생성 및 바인딩 \(UI\)

#### 위치 : 마켓플레이스 &gt; 서비스 &gt;  MySQL Database

![](../../.gitbook/assets/image%20%28173%29.png)

#### 서비스 추가 및 바인딩 앱 선택

 앱의 상세 설정 페이지에서도 서비스 바인딩 할 수 있다. 

![](../../.gitbook/assets/image%20%28174%29.png)

#### "앱 & 서비스 &gt; 앱 &gt; 앱 상세" 에서 바인딩 되었는지 확인 

![](../../.gitbook/assets/image%20%28172%29.png)

### WAS\(API\) 서비스 동작 확인

#### 서비스 재시작 및 확인

Driver Class 가 맞지 않다고 

```text
$ cf restart cf-legacy-api
...

$ cf logs cf-legacy-api [ --recent ]
...
~ : ERR Loading class `com.mysql.jdbc.Driver'. This is deprecated. 
The new driver class is `com.mysql.cj.jdbc.Driver'. 
The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
...

~ : SQL Error: 1146, SQLState: 42S02
~ : Table 'op_f2702132_560d_4461_b510_efba367905bd.todo' doesn't exist
~ : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; 
        nested exception is org.springframework.dao.InvalidDataAccessResourceUsageException: 
        could not extract ResultSet; SQL [n/a]; 
        nested exception is org.hibernate.exception.SQLGrammarException: could not extract ResultSet] with root cause
~ : java.sql.SQLSyntaxErrorException: Table 'op_f2702132_560d_4461_b510_efba367905bd.todo' doesn't exist
```

#### 관리도구 \(phpMyAdmin\) 으로 접속하여 확인할 수 있음 ... 

