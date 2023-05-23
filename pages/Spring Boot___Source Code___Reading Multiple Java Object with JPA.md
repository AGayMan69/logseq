- ### Step 1: Add new method to [[Data Access Object (DAO)]] interface
	- ```java
	  public interface StudentDAO {
	  	...
	      List<Student> findAll();
	  }
	  
	  ```
-
- ### Step 2: Add new method to [[Data Access Object (DAO)]] implementation
	- ```java
	  import jakarta.persistence.EntityManager;
	  import jakarta.persistence.TypedQuery;
	  
	  import java.util.List;
	  
	  @Repository // for Spring component scanning
	  public class StudentDAOImpl implements StudentDAO{
	  
	      private EntityManager entityManager;
	  	...
	  
	      @Override
	      public List<Student> findAll() {
	          TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student", Student.class);
	  
	          return theQuery.getResultList();
	      }
	  }
	  ```
-
- ### Step 3: Update main app
	- ```java
	  @SpringBootApplication
	  public class CruddemoApplication {
	  
	      public static void main(String[] args) {
	          SpringApplication.run(CruddemoApplication.class, args);
	      }
	  
	      @Bean
	      public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
	          return runner -> {
	  
	              queryForStudents(studentDAO);
	          };
	      }
	  
	      private void queryForStudents(StudentDAO studentDAO) {
	          // get a list of students
	          List<Student> theStudents = studentDAO.findAll();
	  
	          // display list of students
	          for (Student tempStudent : theStudents) {
	              System.out.println(tempStudent);
	          }
	      }
	  ```