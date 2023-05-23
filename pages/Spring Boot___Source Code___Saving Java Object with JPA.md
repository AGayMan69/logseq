- ### Step 1: Define [[Data Access Object (DAO)]] interface
	- ```java
	  import com.bedge.cruddemo.entity.Student;
	  
	  public interface StudentDAO {
	      void save(Student theStudent);
	  }
	  ```
-
- ### Step 2: Define [[Data Access Object (DAO)]] implementation
	- ```java
	  import com.bedge.cruddemo.entity.Student;
	  import jakarta.persistence.EntityManager;
	  import jakarta.transaction.Transactional;
	  import org.springframework.beans.factory.annotation.Autowired;
	  import org.springframework.stereotype.Repository;
	  
	  @Repository // for Spring component scanning
	  public class StudentDAOImpl implements StudentDAO{
	      // define field for entity manager
	      private EntityManager entityManager;
	  
	      // inject entity manager using constructor injection
	      @Autowired
	      public StudentDAOImpl(EntityManager entityManager) {
	          this.entityManager = entityManager;
	      }
	  
	      @Override
	      @Transactional
	      public void save(Student theStudent) {
	          entityManager.persist(theStudent);
	      }
	  }
	  ```
	- Notice that [@Transactional](Spring Boot/Concept/Data Persistence/JPA/Transactional) annotation is added to handle the transaction management since the save action update the database
-
- ### Step 3: Update the main app
	- ```java
	  @SpringBootApplication
	  public class CruddemoApplication {
	  
	  	public static void main(String[] args) {
	  		SpringApplication.run(CruddemoApplication.class, args);
	  	}
	  
	  	@Bean
	      // inject the studentDAO
	  	public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
	  		return runner -> {
	  			createStudent(studentDAO);
	  		};
	  	}
	  
	  	private void createStudent(StudentDAO studentDAO) {
	  		// create the student object
	  		System.out.println("Creating new student object");
	  		Student tempStudent = new Student("Paul", "Doe", "paul@luv2code.com");
	  
	  		// save the student object
	  		System.out.println("Saving the student ...");
	  		studentDAO.save(tempStudent);
	  
	  		// display id of the saved student
	  		System.out.println("Saved student. Generated id: " + tempStudent.getId());
	  	}
	  }
	  ```