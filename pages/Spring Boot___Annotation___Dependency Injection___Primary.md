id:: 646a2c71-91fe-41cc-8c8e-31a065bbb13f
```java
import org.springframework.context.annotation.Primary;
...
@Component
@Primary // mark primary to resolve multiple implementation
public class TrackCoach implements Coach{
	...
}
```
