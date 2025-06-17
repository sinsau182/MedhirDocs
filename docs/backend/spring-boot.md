# Medhir Backend Documentation

## Overview
Medhir is a comprehensive HR and Employee Management System built with Spring Boot. The backend provides a robust API for managing various aspects of employee management, including authentication, leave management, attendance tracking, payroll, and more.

## Technology Stack
- **Framework**: Spring Boot
- **Database**: MongoDB
- **Authentication**: JWT (JSON Web Tokens)
- **File Storage**: MinIO
- **Build Tool**: Gradle
- **Container**: Docker

## Core Modules

### 1. Authentication & Authorization
- **Base Path**: `/auth` and `/api/auth`
- **Key Features**:
  - User registration and login
  - JWT-based authentication
  - Role-based access control
  - Password management
  - Employee authentication

### 2. Employee Management
- **Base Path**: `/employee` and `/hradmin`
- **Key Features**:
  - Employee CRUD operations
  - Employee profile management
  - Document management (Aadhar, PAN, Passport, etc.)
  - Company association
  - Role management

### 3. Leave Management
- **Base Path**: `/leave` and `/api/leave-balance`
- **Key Features**:
  - Leave application and approval
  - Leave balance tracking
  - Leave type management
  - Manager approval workflow
  - Public holidays management

### 4. Attendance Management
- **Base Path**: `/api/attendance`
- **Key Features**:
  - Attendance record management
  - Bulk attendance upload
  - Monthly attendance reports
  - Attendance filtering and search

### 5. Payroll Management
- **Base Path**: `/payslip`
- **Key Features**:
  - Payslip generation
  - TDS settings
  - Salary components
  - Monthly payroll processing

### 6. Expense Management
- **Base Path**: `/expenses`
- **Key Features**:
  - Expense tracking
  - Receipt management
  - Expense approval workflow
  - Expense categories

### 7. Income Management
- **Base Path**: `/income`
- **Key Features**:
  - Income tracking
  - Income categories
  - Income reports

### 8. Lead Management
- **Base Path**: `/leads`
- **Key Features**:
  - Lead tracking
  - Lead status management
  - Lead assignment

### 9. Reimbursement Management
- **Base Path**: `/reimbursements`
- **Key Features**:
  - Reimbursement requests
  - Document upload
  - Approval workflow
  - Status tracking

## Security

### Authentication Flow
1. User registration/login through `/auth/register` or `/auth/login`
2. JWT token generation upon successful authentication
3. Token validation for subsequent requests
4. Role-based access control for different endpoints

### Role Hierarchy
- **SUPERADMIN**: Full system access
- **HRADMIN**: HR management access
- **MANAGER**: Team management access
- **EMPLOYEE**: Basic employee access

## API Endpoints

### Authentication
```
POST /auth/register - Register new user
POST /auth/login - User login
POST /api/auth/password/change - Change password
GET /api/auth/password/status - Check password status
```

### Employee Management
```
GET /hradmin/generate-employee-id/{companyId} - Generate employee ID
GET /hradmin/companies/{employeeId} - Get employee companies
GET /employee - Get all employees
POST /employee - Create new employee
PUT /employee/update-request - Update employee details
```

### Leave Management
```
POST /leave/apply - Apply for leave
GET /api/leave-balance/current/{employeeId} - Get current leave balance
GET /api/leave-balance/{employeeId}/{month}/{year} - Get monthly leave balance
GET /manager/leave/status/{status}/{managerId} - Get team leaves by status
```

### Attendance Management
```
POST /api/attendance/upload - Upload attendance records
GET /api/attendance/month/{month}/year/{year} - Get monthly attendance
```

### Payroll Management
```
GET /payslip/generate/{employeeId}/{month}/{year} - Generate payslip
GET /tds-settings/company/{companyId} - Get TDS settings
POST /tds-settings - Create TDS settings
PUT /tds-settings/company/{companyId} - Update TDS settings
```

### Expense Management
```
POST /expenses - Create expense
GET /expenses - Get all expenses
```

### Income Management
```
POST /income - Create income record
GET /income - Get all income records
```

### Lead Management
```
POST /leads - Create lead
GET /leads - Get all leads
GET /leads/{leadId} - Get lead by ID
PUT /leads/{leadId} - Update lead
DELETE /leads/{leadId} - Delete lead
```

### Reimbursement Management
```
POST /reimbursements - Create reimbursement request
GET /reimbursements - Get all reimbursements
```

## Error Handling
The system implements a global exception handler that provides consistent error responses across all endpoints. Common error scenarios include:
- Resource not found (404)
- Bad request (400)
- Unauthorized access (401)
- Forbidden access (403)
- Internal server error (500)

## File Storage
The system uses MinIO for file storage, handling:
- Employee documents
- Attendance records
- Reimbursement receipts
- Profile images

## Best Practices
1. All endpoints follow RESTful conventions
2. Input validation using Jakarta Validation
3. Proper error handling and logging
4. Secure file upload handling
5. Role-based access control
6. JWT-based authentication
7. Cross-origin resource sharing (CORS) configuration

## Development Guidelines
1. Follow the existing code structure and patterns
2. Implement proper validation for all inputs
3. Use appropriate HTTP methods and status codes
4. Document all new endpoints
5. Implement proper error handling
6. Follow security best practices
7. Write unit tests for new features

## Deployment
The application can be deployed using:
1. Docker containerization
2. Jenkins CI/CD pipeline
3. Gradle build system

## Monitoring and Logging
- Application logs are managed using SLF4J
- Global exception handler logs all errors
- Performance monitoring through Spring Boot Actuator 

# Medhir Backend Technical Documentation

## System Architecture

### 1. Application Structure
The application follows a modular, layered architecture with clear separation of concerns. This architecture is based on the principles of:

1. **Single Responsibility Principle (SRP)**
   - Each module has a single, well-defined responsibility
   - Clear boundaries between different layers
   - Independent development and testing

2. **Dependency Inversion Principle (DIP)**
   - High-level modules don't depend on low-level modules
   - Both depend on abstractions
   - Promotes loose coupling

3. **Interface Segregation Principle (ISP)**
   - Clients shouldn't depend on interfaces they don't use
   - Small, focused interfaces
   - Better maintainability

```
com.medhir.rest/
├── config/                 # Configuration classes for Spring Boot, Security, MinIO, etc.
├── controller/            # REST controllers handling HTTP requests and responses
├── service/              # Business logic implementation and transaction management
├── repository/           # MongoDB data access layer with custom queries
├── model/               # Domain entities and MongoDB document models
├── dto/                 # Data Transfer Objects for request/response handling
├── exception/           # Custom exception classes and error handling
├── security/            # JWT authentication and authorization
└── utils/               # Helper classes and common utilities
```

### 2. Core Design Patterns

#### 2.1 Layered Architecture
The application implements a strict layered architecture following these principles:

1. **Controller Layer (Presentation Layer)**
   - **Purpose**: Handles HTTP requests and responses
   - **Responsibilities**:
     - Request validation
     - Input sanitization
     - Response formatting
     - Error handling
   - **Design Patterns Used**:
     - MVC (Model-View-Controller)
     - DTO (Data Transfer Object)
   - **Example Implementation**:
   ```java
   @RestController
   @RequestMapping("/employee")
   public class EmployeeController {
       @Autowired
       private EmployeeService employeeService;
       
       @PostMapping
       public ResponseEntity<EmployeeDTO> createEmployee(@Valid @RequestBody EmployeeRequest request) {
           // Input validation
           // Request processing
           // Response generation
       }
   }
   ```

2. **Service Layer (Business Logic Layer)**
   - **Purpose**: Implements business rules and logic
   - **Responsibilities**:
     - Business rule validation
     - Transaction management
     - Complex operations orchestration
     - Cross-cutting concerns
   - **Design Patterns Used**:
     - Facade Pattern
     - Strategy Pattern
     - Template Method Pattern
   - **Example Implementation**:
   ```java
   @Service
   public class EmployeeService {
       @Autowired
       private EmployeeRepository repository;
       
       @Transactional
       public EmployeeDTO createEmployee(EmployeeRequest request) {
           // Business logic
           // Data validation
           // Entity creation
       }
   }
   ```

3. **Repository Layer (Data Access Layer)**
   - **Purpose**: Manages data persistence
   - **Responsibilities**:
     - Database operations
     - Query optimization
     - Data mapping
   - **Design Patterns Used**:
     - Repository Pattern
     - Data Access Object (DAO)
     - Unit of Work
   - **Example Implementation**:
   ```java
   @Repository
   public interface EmployeeRepository extends MongoRepository<Employee, String> {
       @Query("{ 'companyId': ?0, 'status': 'ACTIVE' }")
       List<Employee> findActiveEmployeesByCompany(String companyId);
   }
   ```

#### 2.2 Dependency Injection
Spring's IoC container manages dependencies using the following principles:

1. **Inversion of Control (IoC)**
   - Framework controls program flow
   - Decouples components
   - Promotes testability

2. **Dependency Injection Types**:
   a. **Constructor Injection**
      - Most preferred method
      - Clear dependencies
      - Immutable dependencies
      ```java
      @Service
      public class EmployeeService {
          private final EmployeeRepository repository;
          private final MinioService minioService;
          
          @Autowired
          public EmployeeService(EmployeeRepository repository, MinioService minioService) {
              this.repository = repository;
              this.minioService = minioService;
          }
      }
      ```

   b. **Field Injection**
      - Used sparingly
      - Less explicit dependencies
      - More difficult to test
      ```java
      @Service
      public class EmployeeService {
          @Autowired
          private EmployeeRepository repository;
      }
      ```

### 3. Security Implementation

#### 3.1 JWT Authentication Flow
The authentication system implements a secure token-based authentication mechanism:

1. **Token Generation Process**
   - **Purpose**: Create secure, signed tokens
   - **Components**:
     - Header (algorithm, token type)
     - Payload (claims, user data)
     - Signature (verification)
   - **Implementation**:
   ```java
   @Service
   public class JwtService {
       public String generateToken(UserDetails userDetails) {
           return Jwts.builder()
               .setSubject(userDetails.getUsername())
               .claim("roles", userDetails.getAuthorities())
               .setIssuedAt(new Date())
               .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 24))
               .signWith(SignatureAlgorithm.HS512, secretKey)
               .compact();
       }
   }
   ```

2. **Token Validation Process**
   - **Purpose**: Verify token authenticity
   - **Steps**:
     - Extract token from request
     - Verify signature
     - Validate claims
     - Set authentication
   - **Implementation**:
   ```java
   @Component
   public class JwtAuthenticationFilter extends OncePerRequestFilter {
       @Override
       protected void doFilterInternal(HttpServletRequest request, 
                                     HttpServletResponse response, 
                                     FilterChain filterChain) {
           // Extract token
           // Validate token
           // Set authentication
       }
   }
   ```

3. **Role-Based Access Control (RBAC)**
   - **Purpose**: Implement fine-grained access control
   - **Components**:
     - Role definitions
     - Permission mapping
     - Access rules
   - **Implementation**:
   ```java
   @Configuration
   @EnableWebSecurity
   public class SecurityConfig {
       @Bean
       public SecurityFilterChain filterChain(HttpSecurity http) {
           http.authorizeRequests()
               .requestMatchers("/hradmin/**").hasRole("HRADMIN")
               .requestMatchers("/manager/**").hasRole("MANAGER")
               .requestMatchers("/employee/**").hasRole("EMPLOYEE")
               .anyRequest().authenticated();
           return http.build();
       }
   }
   ```

### 4. Data Flow Architecture

#### 4.1 Request Processing Pipeline
The system implements a comprehensive request processing pipeline:

1. **Request Reception**
   - **Purpose**: Handle incoming HTTP requests
   - **Components**:
     - Request validation
     - File handling
     - Input sanitization
   - **Implementation**:
   ```java
   @RestController
   public class EmployeeController {
       @PostMapping("/employee")
       public ResponseEntity<?> createEmployee(
           @Valid @RequestBody EmployeeRequest request,
           @RequestParam("profileImage") MultipartFile profileImage) {
           // Input validation
           // File validation
           // Request processing
       }
   }
   ```

2. **Business Logic Processing**
   - **Purpose**: Implement business rules
   - **Components**:
     - Transaction management
     - Business rule validation
     - Complex operations
   - **Implementation**:
   ```java
   @Service
   public class EmployeeService {
       @Transactional
       public EmployeeDTO createEmployee(EmployeeRequest request, MultipartFile profileImage) {
           // Validate business rules
           // Process file upload
           // Create employee record
           // Send notifications
       }
   }
   ```

3. **Data Persistence**
   - **Purpose**: Manage data storage
   - **Components**:
     - Database operations
     - Query optimization
     - Data mapping
   - **Implementation**:
   ```java
   @Repository
   public interface EmployeeRepository extends MongoRepository<Employee, String> {
       @Query(value = "{ 'email': ?0 }", exists = true)
       boolean existsByEmail(String email);
   }
   ```

### 5. File Handling System

#### 5.1 MinIO Integration
The system implements a robust file storage solution:

1. **Configuration**
   - **Purpose**: Set up MinIO client
   - **Components**:
     - Client configuration
     - Bucket management
     - Access policies
   - **Implementation**:
   ```java
   @Configuration
   public class MinioConfig {
       @Bean
       public MinioClient minioClient() {
           return MinioClient.builder()
               .endpoint(minioEndpoint)
               .credentials(minioAccessKey, minioSecretKey)
               .build();
       }
   }
   ```

2. **File Operations**
   - **Purpose**: Manage file storage
   - **Components**:
     - File upload
     - File download
     - File deletion
   - **Implementation**:
   ```java
   @Service
   public class MinioService {
       public String uploadFile(MultipartFile file, String bucket, String objectName) {
           // Validate file
           // Upload to MinIO
           // Generate secure URL
       }
       
       public void deleteFile(String bucket, String objectName) {
           // Delete from MinIO
       }
   }
   ```

### 6. Error Handling System

#### 6.1 Global Exception Handler
The system implements a comprehensive error handling mechanism:

1. **Exception Categories**
   - **Validation Errors**: Input validation failures
   - **Business Logic Errors**: Rule violations
   - **Security Errors**: Authentication/Authorization failures
   - **System Errors**: Unexpected exceptions

2. **Implementation**:
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ApiErrorResponse> handleResourceNotFoundException(
            ResourceNotFoundException ex) {
        ApiErrorResponse error = new ApiErrorResponse(
            LocalDateTime.now(),
            HttpStatus.NOT_FOUND.value(),
            "Resource Not Found",
            ex.getMessage()
        );
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ApiErrorResponse> handleValidationException(
            ValidationException ex) {
        // Handle validation errors
    }
}
```

### 7. Database Design

#### 7.1 MongoDB Collections
The system uses MongoDB with the following collection structure:

1. **Employee Collection**
   - **Purpose**: Store employee information
   - **Schema**:
   ```json
   {
     "_id": "ObjectId",
     "employeeId": "String",
     "firstName": "String",
     "lastName": "String",
     "email": "String",
     "role": "String",
     "companyId": "String",
     "documents": [{
       "type": "String",
       "url": "String",
       "uploadDate": "Date"
     }],
     "status": "String",
     "createdAt": "Date",
     "updatedAt": "Date"
   }
   ```

2. **Leave Collection**
   - **Purpose**: Manage leave records
   - **Schema**:
   ```json
   {
     "_id": "ObjectId",
     "employeeId": "String",
     "leaveType": "String",
     "startDate": "Date",
     "endDate": "Date",
     "status": "String",
     "approverId": "String",
     "approvalDate": "Date",
     "reason": "String"
   }
   ```

### 8. Background Processing

#### 8.1 Asynchronous Operations
The system implements background processing for long-running tasks:

1. **Task Scheduling**
   - **Purpose**: Execute periodic tasks
   - **Components**:
     - Cron jobs
     - Task scheduling
     - Job management
   - **Implementation**:
   ```java
   @Service
   public class PayrollService {
       @Async
       public CompletableFuture<Void> processMonthlyPayroll(String month, String year) {
           // Process payroll
           // Generate reports
           // Send notifications
       }
       
       @Scheduled(cron = "0 0 1 * * ?")
       public void scheduledPayrollProcessing() {
           // Monthly payroll processing
       }
   }
   ```

### 9. Monitoring and Logging

#### 9.1 Logging Implementation
The system implements comprehensive logging:

1. **Logging Strategy**
   - **Purpose**: Track system operations
   - **Components**:
     - Log levels
     - Log categories
     - Performance monitoring
   - **Implementation**:
   ```java
   @Slf4j
   @Service
   public class EmployeeService {
       public EmployeeDTO createEmployee(EmployeeRequest request) {
           log.info("Creating new employee: {}", request.getEmail());
           try {
               // Process request
               log.info("Employee created successfully: {}", employee.getId());
           } catch (Exception e) {
               log.error("Error creating employee: {}", e.getMessage(), e);
               throw e;
           }
       }
   }
   ```

### 10. Deployment Architecture

#### 10.1 Docker Configuration
The system uses Docker for containerization:

1. **Multi-stage Build**
   - **Purpose**: Optimize container size
   - **Components**:
     - Build stage
     - Runtime stage
   - **Implementation**:
   ```dockerfile
   # Multi-stage build
   FROM maven:3.8.4-openjdk-17 AS build
   WORKDIR /app
   COPY pom.xml .
   COPY src ./src
   RUN mvn clean package

   FROM openjdk:17-jdk-slim
   WORKDIR /app
   COPY --from=build /app/target/*.jar app.jar
   EXPOSE 8080
   ENTRYPOINT ["java", "-jar", "app.jar"]
   ```

#### 10.2 Kubernetes Configuration
The system uses Kubernetes for orchestration:

1. **Deployment Configuration**
   - **Purpose**: Manage container deployment
   - **Components**:
     - Pod configuration
     - Service definition
     - Environment variables
   - **Implementation**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: medhir-backend
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: medhir-backend
     template:
       metadata:
         labels:
           app: medhir-backend
       spec:
         containers:
         - name: medhir-backend
           image: medhir-backend:latest
           ports:
           - containerPort: 8080
           env:
           - name: SPRING_PROFILES_ACTIVE
             value: "prod"
           - name: MONGODB_URI
             valueFrom:
               secretKeyRef:
                 name: mongodb-secret
                 key: uri
   ```

This enhanced documentation provides detailed theoretical explanations and implementation specifics for each component of the system. Would you like me to expand on any particular section further? 
