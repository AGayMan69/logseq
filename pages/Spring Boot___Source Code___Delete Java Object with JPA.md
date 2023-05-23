- ### Step 1: Add new method to [[Data Access Object (DAO)]] interface
	- ```java
	  import com.bedge.cruddemo.entity.Student;
	  
	  public interface StudentDAO {
	  	...
	        
	      void delete(Integer id);
	  
	  }
	  
	  ```
-
- ### Step 2: Add new method to [[Data Access Object (DAO)]] implementation
	- ```java
	  import com.bedge.cruddemo.entity.Student;
	  import jakarta.persistence.EntityManager;
	  import jakarta.transaction.Transactional;
	  
	  @Repository 
	  public class StudentDAOImpl implements StudentDAO {
	      private EntityManager entityManager;
	  	...
	        
	      @Override
	      @Transactional
	      public void delete(Integer id) {
	          // retrieve the student
	          Student theStudent = entityManager.find(Student.class, id);
	  
	          // delete the student
	          entityManager.remove(theStudent);
	      }
	  
	  }
	  
	  ```
-
- ### Step 3: Update main app
	- ```java
	  package com.bedge.cruddemo;
	  
	  import com.bedge.cruddemo.dao.StudentDAO;
	  import com.bedge.cruddemo.entity.Student;
	  import org.springframework.boot.CommandLineRunner;
	  import org.springframework.boot.SpringApplication;
	  import org.springframework.boot.autoconfigure.SpringBootApplication;
	  import org.springframework.context.annotation.Bean;
	  
	  import java.util.List;
	  import java.util.concurrent.atomic.AtomicIntegerFieldUpdater;
	  
	  @SpringBootApplication
	  public class CruddemoApplication {
	  
	      public static void main(String[] args) {
	          SpringApplication.run(CruddemoApplication.class, args);
	      }
	  
	      @Bean
	      public CommandLineRunner commandLineRunner(StudentDAO studentDAO) {
	          return runner -> {
	  
	              deleteAllStudents(studentDAO);
	          };
	      }
	  
	  
	      private void deleteStudent(StudentDAO studentDAO) {
	          int studentId = 2;
	          System.out.println("Deleting student id: " + studentId);
	          studentDAO.delete(studentId);
	      }
	  
	  }
	  
	  ```