- Alternative solution to [@Qualifier](Spring Boot/Concept/Dependency Injection/Qualifier)
- Instead of specifying the dependency name in the object, `@Primary` to place in the implementation of the dependency
- {{embed ((646a2c71-91fe-41cc-8c8e-31a065bbb13f))}}
-
- Notice in the object source code, we don't have to put [@Qualifier](Spring Boot/Concept/Dependency Injection/Qualifier) to specify the dependency implementation
- ```java
  @RestController // rest controller
  public class DemoController {
      private Coach myCoach;
      @Autowired
      public void setMyCoach(Coach myCoach) {
          this.myCoach = myCoach;
      }
  	...
  }
  ```
- background-color:: darkcyan
  #+BEGIN_WARNING
  Unlike [@Qualifier](Spring Boot/Concept/Dependency Injection/Qualifier) Please make sure that only one `@Primary` is add for multiple implementation 
  #+END_WARNING
-