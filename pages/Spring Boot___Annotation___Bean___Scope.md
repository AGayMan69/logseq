- ==Singleton==
	- ```java
	  import org.springframework.beans.factory.config.ConfigurableBeanFactory;
	  import org.springframework.context.annotation.Scope;
	  ...
	  
	  @Component // Mark this class as a spring bean
	  @Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
	  public class CricketCoach implements Coach{
	  	...
	  }
	  ```
-
- ==Prototype==
	- ```java
	  import org.springframework.beans.factory.config.ConfigurableBeanFactory;
	  import org.springframework.context.annotation.Scope;
	  ...
	  
	  @Component // Mark this class as a spring bean
	  @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
	  public class CricketCoach implements Coach{
	  	...
	  }
	  ```
-
-