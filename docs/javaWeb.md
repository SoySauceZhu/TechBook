
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


| Scope               | Instances Created         | Best Use Case                                    |
|---------------------|-------------------------|------------------------------------------------|
| **Singleton (Default)** | 1 (Shared globally)      | Shared services, database connections, utility classes |
| **Prototype**       | New instance each time   | Temporary objects, non-shared components      |
| **Request**         | 1 per HTTP request       | Handling request-specific data in web apps    |
| **Session**         | 1 per user session       | Storing user-specific data in web apps        |
| **Application**     | 1 for entire app         | Shared configuration, global objects         |


## Debug Tip:
RESTful controller will map the URI request to create a response body. During the process, getter and setter methods of entities will be invoked. 

Therefore, if error `Resolved [org.springframework.web.HttpMediaTypeNotAcceptableException: No acceptable representation]`. The problem might due to `@Data` annotation missing or not compiled properly.
 