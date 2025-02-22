
# Java Web

## Singleton Pattern
Single temp pattern insures that only one instance of the class 
is created and share throughout the application

In SpringBoot the default scope of beans is a Singleton, 
meaning this spring IOC container creates only one instead 
of a given be and reuse it whenever needed.


✅ When an object should be shared across the entire application, like:

- Database connection pools  
- Configuration classes  
- Service layer components  
- Logging utilities  


🚫 When NOT to use Singleton Mode?

- If you need different instances for different requests (use `@Scope("prototype")` instead).
- If objects hold user-specific data (like session-based objects).

```java
@Component
@Scope("prototype")
public class DatabaseConnection {
    // ...
}
``` 


| Scope                   | Instances Created      | Best Use Case                                          |
| ----------------------- | ---------------------- | ------------------------------------------------------ |
| **Singleton (Default)** | 1 (Shared globally)    | Shared services, database connections, utility classes |
| **Prototype**           | New instance each time | Temporary objects, non-shared components               |
| **Request**             | 1 per HTTP request     | Handling request-specific data in web apps             |
| **Session**             | 1 per user session     | Storing user-specific data in web apps                 |
| **Application**         | 1 for entire app       | Shared configuration, global objects                   |


## LomBok annotation compile:
RESTful controller will map the URI request to create a response body. During the process, getter and setter methods of entities will be invoked. 

Therefore, if error `Resolved [org.springframework.web.HttpMediaTypeNotAcceptableException: No acceptable representation]`. The problem might due to `@Data` annotation missing or not compiled properly.

## Request from through NginX, Docker container, backend Tomcat, SpringBoot 

- [Client]  Chrome visits http://localhost:8081
- [Nginx]   Serves index.html (Vue app)
- [Vue]     fetch('/api/depts')
- [Nginx]   Proxies request -> localhost:8080/depts
- [Backend] Queries database, returns JSON
- [Nginx]   Forwards JSON to client
- [Vue]     Updates UI with data

## Request param map to Object attributes 
`@RequestParam`  
[https://www.baeldung.com/spring-request-param](https://www.baeldung.com/spring-request-param)

ps: `api/del?id=1,2,3` can be received by `fun(Integer[] ids)` or `fun(@RequestParam List<Integer> ids)`

## Page Split Select (Partial Records)
- `LIMIT rowIndex, pageSize`
- PageHelper

## Filtering Select
- Controller Auto Encapsulate GET request params into QueryObject
- Dynamic SQL
    - `<if test=...> </if>` in myBatis
    - `<where>` in myBatis

## Batch Insert Records
- Dynamic SQL  

    ```xml
    <foreach collection="objList" item="obj" separator=",">
        (#{obj.id}, #{obj.begin})
    </foreach>
    ```
    - Return Primary Key: `@Options(useGeneratedKeys = true, keyProperty = "empID")`. Working with `AUTOINCREMENT`, can assign object's `empID` attribute to what it is generated in Database.


## Serve Push Notification
- Polling
- Long Polling: Blocking request at server end, until there is response or timeout
- Server-sent event
- WebSocket: Full Duplex
    - Client (Browser):  
    ```javascript
    let ws = new WebSocket("ws://localhost/xxx");
    ws.onopen = function(){};
    ws.onmessage = function(evt) {
      this.data = evt.data
    };
    ws.onclose = function(){};
    ```
    - Server:  
        - Endpoints: takes in websocket message, initialized while handshaking.
        i.e. `@OnOpen`, `@OnMessage`, etc.
    
## Transaction control in MyBatis

- Add `@Transactional` to service methods which includes more than one Mapper function (multiple database operation)
    - Other options: on Class, Interfaces
- Since by default, transaction only rollback when RunTImeException. You can use `@Transaction(rollBackFor = {Exception.class})
- Transaction propagation:  
    - default: required.
    - other option: Require_New  
i.e. If one service invoke another service, some transaction (logData) should always commit no matter other database operations. In other word, some methods does not only serve one purpose, including multiple database operation logics

## Select result Encapsulation
- `<resultType>`
- `<resultMap>` used for define which sql result is assigned to object attribute


## File upload
- UUID
- Max size setting
- File type is store as `byte[]`. i.e. `File.getBytes()`
- OSS: object storage service
    - Cloud service i.e. aliyun. With sophisticated APIs and Util classes. Refer to documentation
- Configuration and injection from application.yml:  
    1. On variable
```java
@Value("${aliyun.oss.endpoint}")
private String endpoint;


/* -------application.yml----------
aliyun:
    oss:
        endpoint: https://aliyun.com/bulabula
        bucketName: myProj
*/
```

    2. On Class  
```java
@ConfigurationProperties(prefix = "aliyun.oss")
public class Properties {
    private String endpoint;
    private String bucket;
}
```


## Global Exception handler
Any mapper, controller or service throws exceptions. Global Handler will return formal error json (i.e.`Result("error")`) to client.

## Cookie, Session, Token

Key problem to solve:

- Access status and context of HTTP requests


## ServletRequest, ServletResponse
When a Servlet accepts a call from a client, 
then it receives two objects, one is a ServletRequest and 
the other is the ServletResponse. ServletRequest encapsulates 
the Communications from the client to the server, 
while ServletResponse encapsulates the Communication 
from the Servlet back to the client. The Object of the 
ServletRequest is used to provide the client request 
information data to the Servlet as a content type, 
a content length, parameter names, and the values, 
header information and the attributes, etc.

[https://www.geeksforgeeks.org/servlet-request-interface/](https://www.geeksforgeeks.org/servlet-request-interface/)

## Aspect Oriented Programming AOP
Problem to solve:  

- Repeated and common methods leading verbose code
- Avoiding revise core logic code while adding supplementary features

Key concepts:  

- Joint Point
    - Data type: `ProceedingJointPoint` for around, `JointPoint` for others.
    - Can access: target class, object, methods and arguments
- Advice
    - Defined by annotations
    - `@Around`, `@Before`, `@After`, ...
    - Process of advice methods: 
        - Alphabetical order (a -> z -> JointPoint -> z -> a)
        - `@Order` (0 to 9 to jointPoint to 9 to 0)
- PointCut
    - Defined either by
    - Annotation `@PointCut` on `pt()` 
    - Execution annotation on advice methods `@Before("execution(* com.example.service.*.*(..))")`. 
    You can have boolean operation for multiple execution points.
    - Or customize annotation
- Aspect
- Target
    - The object that includes joint point methods

Process:  
Application will use proxy object and execute methods in the proxy object,
instead of target object. Proxy object encapsulates target object logic into AOP logic (Advice methods).


