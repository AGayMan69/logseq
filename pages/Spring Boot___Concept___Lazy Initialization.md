- #+BEGIN_NOTE
  When the [[Spring Boot]] applications start, all beans are initialized e.g. [@Component](Spring Boot/Concept/Bean/Component) , [@RestController](Spring Boot/Concept/Bean/RestController), etc...
  #+END_NOTE
-
- Instead, we can specify lazy initialization with [@Lazy](Spring Boot/Annotation/Component)
- {{embed ((646a3c71-91fb-4d17-a8bc-90f71961000f))}}
-
- **With [@Lazy](Spring Boot/Annotation/Component) added, the bean will only be initialized in following cases:**
	- It is needed for [Dependency Injection](Spring Boot/Concept/Dependency Injection)
	- It is explicitly requested
-
- ==Global Configuration==
	- to avoid tedious work of putting [@Lazy](Spring Boot/Annotation/Component) on every single bean, we can set a [Configuration](Spring Boot/Concept/Configuration)
	- In File: application.properties... 
	  ```properties
	  spring.main.lazy-initialization=true
	  ```
-
- **Pro**
	- Faster startup time
	- Only create object as needed
- **Cons**
	- Configuration problem can't bee detected on startup
	- Memory management is hard
	-