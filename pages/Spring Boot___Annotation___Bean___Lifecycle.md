- `@PostConstruct`
  id:: 646a6924-4f31-490a-8be5-cb12e5fb73b7
	- ```java
	  import jakarta.annotation.PostConstruct;
	  ...
	  @Component // Mark this class as a spring bean
	  public class CricketCoach implements Coach{
	  	...
	      // define bean init method
	      @PostConstruct
	      public void doMyStartupStuff() {
	          System.out.println("In doMyStartupStuff():" + getClass().getSimpleName());
	      }
	  }
	  ```
- `@PreDestroy`
  id:: 646a6a85-6841-4f53-b7ea-846b8eaa2ed1
	- id:: 646a6a8d-313f-41d8-aa5e-abf2a2527d40
	  ```java
	  import jakarta.annotation.PostConstruct;
	  ...
	  @Component // Mark this class as a spring bean
	  public class CricketCoach implements Coach{
	  	...
	      // define bean destroy method
	      @PreDestroy
	      public void doMyCleanupStuff() {
	          System.out.println("In doMyCleanupStuff():" + getClass().getSimpleName());
	      }
	  }
	  ```