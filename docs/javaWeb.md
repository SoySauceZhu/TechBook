
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


## Debug Tip:
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

## Page Split Select (Partial Records)
- `LIMIT rowIndex, pageSize`
- PageHelper

## Filtering Select
- Controller Auto Encapsulate GET request params into QueryObject
- Dynamic SQL
    - `<if>` in myBatis
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
i.e. If one service invoke another service, some transaction (logData) should always commit no matter other database operations

