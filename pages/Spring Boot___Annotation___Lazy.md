id:: 646a3c71-91fb-4d17-a8bc-90f71961000f
```java
import org.springframework.context.annotation.Lazy;
...
@Component
@Lazy
public class TrackCoach implements Coach{
    public TrackCoach() {
        System.out.println("In constructor: " + getClass().getSimpleName());
    }
	...
}
```
