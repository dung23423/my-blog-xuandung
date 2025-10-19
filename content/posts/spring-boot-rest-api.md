---
title: "Xây dựng REST API với Spring Boot: Từ Hello World đến Production-Ready trong 1 tuần"
date: 2025-10-14
draft: false
tags: ["Java", "Spring Boot", "REST API", "Backend", "Web Development"]
categories: ["Java", "Spring Boot"]
description: "Câu chuyện về cách tôi build một REST API hoàn chỉnh với Spring Boot trong 7 ngày - từ setup project đến deploy production với authentication, validation, và error handling."
summary: "7 ngày. Từ 'What is Spring Boot?' đến một REST API production-ready với JWT authentication. Đây là roadmap chi tiết tôi đã đi qua."
feature: "https://images.unsplash.com/photo-1618477388954-7852f32655ec?w=800&h=600&fit=crop"
showTableOfContents: true
---

Tuần trước, sếp gọi vào phòng: *"Em có thể build API để app mobile connect vào không?"*

Tôi: *"Dạ được ạ!"* (Trong đầu: Uhm... Spring Boot là gì nhỉ?)

1 tuần sau, tôi deploy thành công một REST API có đầy đủ:
- ✅ CRUD operations
- ✅ JWT Authentication
- ✅ Input validation
- ✅ Error handling
- ✅ Database integration
- ✅ API documentation

Nghe có vẻ impossible? Tôi cũng nghĩ vậy. Nhưng Spring Boot makes magic happen.

![Spring Boot Development](https://images.unsplash.com/photo-1618477388954-7852f32655ec?w=1200&h=600&fit=crop)

Đây là câu chuyện về 7 ngày đó - và roadmap để bạn có thể làm được điều tương tự.

---

## 🎯 Tại sao Spring Boot?

Trước khi bắt đầu, câu hỏi quan trọng: **Tại sao không dùng pure Java?**

### **Life without Spring Boot:**
```java
// Traditional Java Servlet - Pain!
@WebServlet("/api/students")
public class StudentServlet extends HttpServlet {
    
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
            throws ServletException, IOException {
        
        // Manual JSON parsing
        // Manual database connection
        // Manual error handling
        // Manual everything!
        
        Connection conn = null;
        try {
            conn = DriverManager.getConnection(DB_URL, USER, PASS);
            // 50 lines of boilerplate...
            
        } catch (SQLException e) {
            // Manual error response
            resp.setStatus(500);
            resp.getWriter().write("{\"error\": \"Database error\"}");
        } finally {
            // Manual cleanup
            if (conn != null) conn.close();
        }
    }
}
```

**Problems:**
- Too much boilerplate code
- Manual JSON conversion
- Manual dependency injection
- Manual configuration
- No auto-reload during development

### **Life with Spring Boot:**
```java
@RestController
@RequestMapping("/api/students")
public class StudentController {
    
    @Autowired
    private StudentService studentService;
    
    @GetMapping
    public List<Student> getAllStudents() {
        return studentService.findAll();
    }
}
```

**That's it!** Spring Boot handles:
- JSON serialization/deserialization ✅
- Dependency injection ✅
- Database connection pooling ✅
- Error handling ✅
- Server configuration ✅

![Spring Boot Magic](https://images.unsplash.com/photo-1555066931-4365d14bab8c?w=1200&h=600&fit=crop)

---

## 📅 7-Day Roadmap

### **Day 1: Setup & First Endpoint**

**Morning: Project Setup**

1. Đến [start.spring.io](https://start.spring.io)
2. Chọn dependencies:
   - Spring Web
   - Spring Data JPA
   - MySQL Driver
   - Lombok
   - Spring Boot DevTools

3. Generate & Import vào IDE

![Spring Initializr](https://images.unsplash.com/photo-1517694712202-14dd9538aa97?w=1200&h=600&fit=crop)

**Project Structure:**
```
student-api/
├── src/main/java/com/example/studentapi/
│   ├── StudentApiApplication.java  (Main class)
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── model/
│   └── dto/
├── src/main/resources/
│   └── application.properties
└── pom.xml
```

**Afternoon: First Endpoint**
```java
// Model - Student.java
@Entity
@Table(name = "students")
@Data  // Lombok generates getters/setters
@NoArgsConstructor
@AllArgsConstructor
public class Student {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    @Column(nullable = false)
    private Double gpa;
    
    @Column(name = "enrollment_year")
    private Integer enrollmentYear;
    
    @CreationTimestamp
    private LocalDateTime createdAt;
    
    @UpdateTimestamp
    private LocalDateTime updatedAt;
}
```
```java
// Repository - StudentRepository.java
@Repository
public interface StudentRepository extends JpaRepository<Student, Long> {
    // Spring Data JPA tự generate queries!
    Optional<Student> findByEmail(String email);
    List<Student> findByGpaGreaterThan(Double gpa);
}
```
```java
// Controller - StudentController.java
@RestController
@RequestMapping("/api/students")
public class StudentController {
    
    @Autowired
    private StudentRepository studentRepository;
    
    // GET all students
    @GetMapping
    public List<Student> getAllStudents() {
        return studentRepository.findAll();
    }
    
    // GET student by ID
    @GetMapping("/{id}")
    public ResponseEntity<Student> getStudentById(@PathVariable Long id) {
        return studentRepository.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
    
    // POST create student
    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }
}
```

**Configuration - application.properties:**
```properties
# Database
spring.datasource.url=jdbc:mysql://localhost:3306/student_db
spring.datasource.username=root
spring.datasource.password=password

# JPA
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# Server
server.port=8080
```

**Run:**
```bash
mvn spring-boot:run
```

**Test với Postman:**
```
GET http://localhost:8080/api/students
POST http://localhost:8080/api/students
Body: {
  "name": "John Doe",
  "email": "john@example.com",
  "gpa": 3.8,
  "enrollmentYear": 2023
}
```

**Result:** API hoạt động! 🎉

**Lessons learned:**
- Spring Boot auto-configuration làm mọi thứ
- JpaRepository cung cấp CRUD methods free
- @RestController tự động serialize JSON

---

### **Day 2-3: Service Layer & Advanced CRUD**

**Why Service Layer?**

Trước đây, tôi để business logic trong Controller:
```java
// ❌ BAD: Controller biết quá nhiều
@PostMapping
public Student createStudent(@RequestBody Student student) {
    // Validation logic
    if (student.getGpa() < 0 || student.getGpa() > 4.0) {
        throw new IllegalArgumentException("Invalid GPA");
    }
    
    // Business logic
    if (studentRepository.findByEmail(student.getEmail()).isPresent()) {
        throw new DuplicateEmailException();
    }
    
    // Save
    return studentRepository.save(student);
}
```

**Problem:** Controller quá phức tạp, khó test, khó reuse.

**Solution: Service Layer**
```java
// Service - StudentService.java
@Service
@Transactional
public class StudentService {
    
    @Autowired
    private StudentRepository studentRepository;
    
    public List<Student> findAll() {
        return studentRepository.findAll();
    }
    
    public Student findById(Long id) {
        return studentRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Student", "id", id));
    }
    
    public Student create(StudentDTO dto) {
        // Validation
        validateStudent(dto);
        
        // Check duplicate email
        if (studentRepository.findByEmail(dto.getEmail()).isPresent()) {
            throw new DuplicateResourceException("Student with email " + 
                dto.getEmail() + " already exists");
        }
        
        // Convert DTO to Entity
        Student student = new Student();
        student.setName(dto.getName());
        student.setEmail(dto.getEmail());
        student.setGpa(dto.getGpa());
        student.setEnrollmentYear(dto.getEnrollmentYear());
        
        // Save
        return studentRepository.save(student);
    }
    
    public Student update(Long id, StudentDTO dto) {
        Student student = findById(id);
        
        validateStudent(dto);
        
        // Update fields
        student.setName(dto.getName());
        student.setEmail(dto.getEmail());
        student.setGpa(dto.getGpa());
        student.setEnrollmentYear(dto.getEnrollmentYear());
        
        return studentRepository.save(student);
    }
    
    public void delete(Long id) {
        Student student = findById(id);
        studentRepository.delete(student);
    }
    
    // Advanced queries
    public List<Student> findTopStudents(Double minGpa) {
        return studentRepository.findByGpaGreaterThan(minGpa);
    }
    
    private void validateStudent(StudentDTO dto) {
        if (dto.getName() == null || dto.getName().trim().isEmpty()) {
            throw new ValidationException("Name is required");
        }
        
        if (dto.getGpa() < 0 || dto.getGpa() > 4.0) {
            throw new ValidationException("GPA must be between 0.0 and 4.0");
        }
        
        // Email validation
        String emailRegex = "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$";
        if (!dto.getEmail().matches(emailRegex)) {
            throw new ValidationException("Invalid email format");
        }
    }
}
```
```java
// DTO - StudentDTO.java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class StudentDTO {
    private String name;
    private String email;
    private Double gpa;
    private Integer enrollmentYear;
}
```
```java
// Controller - Now clean and simple!
@RestController
@RequestMapping("/api/students")
public class StudentController {
    
    @Autowired
    private StudentService studentService;
    
    @GetMapping
    public ResponseEntity<List<Student>> getAllStudents() {
        return ResponseEntity.ok(studentService.findAll());
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<Student> getStudentById(@PathVariable Long id) {
        return ResponseEntity.ok(studentService.findById(id));
    }
    
    @PostMapping
    public ResponseEntity<Student> createStudent(@RequestBody StudentDTO dto) {
        Student created = studentService.create(dto);
        return ResponseEntity
            .status(HttpStatus.CREATED)
            .body(created);
    }
    
    @PutMapping("/{id}")
    public ResponseEntity<Student> updateStudent(
            @PathVariable Long id,
            @RequestBody StudentDTO dto) {
        return ResponseEntity.ok(studentService.update(id, dto));
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteStudent(@PathVariable Long id) {
        studentService.delete(id);
        return ResponseEntity.noContent().build();
    }
    
    @GetMapping("/top")
    public ResponseEntity<List<Student>> getTopStudents(
            @RequestParam(defaultValue = "3.5") Double minGpa) {
        return ResponseEntity.ok(studentService.findTopStudents(minGpa));
    }
}
```

**Benefits of Service Layer:**
- ✅ Separation of concerns
- ✅ Reusable business logic
- ✅ Easy to test
- ✅ Transactional support

![3-Tier Architecture](https://images.unsplash.com/photo-1558494949-ef010cbdcc31?w=1200&h=600&fit=crop)

---

### **Day 4: Exception Handling**

Remember bài trước về Exception Handling? Giờ apply vào Spring Boot!
```java
// Custom Exceptions
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String resource, String field, Object value) {
        super(String.format("%s not found with %s: %s", resource, field, value));
    }
}

@ResponseStatus(HttpStatus.CONFLICT)
public class DuplicateResourceException extends RuntimeException {
    public DuplicateResourceException(String message) {
        super(message);
    }
}

@ResponseStatus(HttpStatus.BAD_REQUEST)
public class ValidationException extends RuntimeException {
    public ValidationException(String message) {
        super(message);
    }
}
```
```java
// Global Exception Handler
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.NOT_FOUND.value(),
            ex.getMessage(),
            LocalDateTime.now()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(DuplicateResourceException.class)
    public ResponseEntity<ErrorResponse> handleDuplicate(DuplicateResourceException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.CONFLICT.value(),
            ex.getMessage(),
            LocalDateTime.now()
        );
        return ResponseEntity.status(HttpStatus.CONFLICT).body(error);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(ValidationException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.BAD_REQUEST.value(),
            ex.getMessage(),
            LocalDateTime.now()
        );
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(Exception ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.INTERNAL_SERVER_ERROR.value(),
            "An unexpected error occurred",
            LocalDateTime.now()
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}

// Error Response DTO
@Data
@AllArgsConstructor
public class ErrorResponse {
    private int status;
    private String message;
    private LocalDateTime timestamp;
}
```

**Test Error Handling:**
```bash
# 404 Not Found
GET http://localhost:8080/api/students/999

Response:
{
  "status": 404,
  "message": "Student not found with id: 999",
  "timestamp": "2025-10-14T10:30:00"
}

# 409 Conflict (Duplicate Email)
POST http://localhost:8080/api/students
{
  "email": "existing@email.com"
}

Response:
{
  "status": 409,
  "message": "Student with email existing@email.com already exists",
  "timestamp": "2025-10-14T10:31:00"
}
```

---

### **Day 5: Validation với Bean Validation**

Thay vì validate manually, dùng annotations!
```java
// Add dependency
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```
```java
// StudentDTO với validation annotations
@Data
@NoArgsConstructor
@AllArgsConstructor
public class StudentDTO {
    
    @NotBlank(message = "Name is required")
    @Size(min = 2, max = 100, message = "Name must be between 2 and 100 characters")
    private String name;
    
    @NotBlank(message = "Email is required")
    @Email(message = "Email must be valid")
    private String email;
    
    @NotNull(message = "GPA is required")
    @DecimalMin(value = "0.0", message = "GPA must be at least 0.0")
    @DecimalMax(value = "4.0", message = "GPA must be at most 4.0")
    private Double gpa;
    
    @NotNull(message = "Enrollment year is required")
    @Min(value = 2000, message = "Enrollment year must be after 2000")
    @Max(value = 2030, message = "Enrollment year must be before 2030")
    private Integer enrollmentYear;
}
```
```java
// Controller - Add @Valid annotation
@PostMapping
public ResponseEntity<Student> createStudent(@Valid @RequestBody StudentDTO dto) {
    Student created = studentService.create(dto);
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}
```
```java
// Handle Validation Errors
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<ValidationErrorResponse> handleValidationErrors(
        MethodArgumentNotValidException ex) {
    
    Map<String, String> errors = new HashMap<>();
    
    ex.getBindingResult().getFieldErrors().forEach(error -> 
        errors.put(error.getField(), error.getDefaultMessage())
    );
    
    ValidationErrorResponse response = new ValidationErrorResponse(
        HttpStatus.BAD_REQUEST.value(),
        "Validation failed",
        errors,
        LocalDateTime.now()
    );
    
    return ResponseEntity.badRequest().body(response);
}

@Data
@AllArgsConstructor
class ValidationErrorResponse {
    private int status;
    private String message;
    private Map<String, String> errors;
    private LocalDateTime timestamp;
}
```

**Test Validation:**
```bash
POST http://localhost:8080/api/students
{
  "name": "J",
  "email": "invalid-email",
  "gpa": 5.0
}

Response:
{
  "status": 400,
  "message": "Validation failed",
  "errors": {
    "name": "Name must be between 2 and 100 characters",
    "email": "Email must be valid",
    "gpa": "GPA must be at most 4.0"
  },
  "timestamp": "2025-10-14T11:00:00"
}
```

![Validation](https://images.unsplash.com/photo-1454165804606-c3d57bc86b40?w=1200&h=600&fit=crop)

---

### **Day 6: JWT Authentication**

Security layer - chỉ authenticated users mới access được API.
```java
// Add dependencies
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
```
```java
// User Entity
@Entity
@Table(name = "users")
@Data
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(nullable = false)
    private String password;  // Hashed
    
    private String email;
    
    @Enumerated(EnumType.STRING)
    private Role role;
}

enum Role {
    ROLE_USER,
    ROLE_ADMIN
}
```
```java
// JWT Utility
@Component
public class JwtTokenUtil {
    
    @Value("${jwt.secret}")
    private String secret;
    
    @Value("${jwt.expiration}")
    private Long expiration;
    
    public String generateToken(String username) {
        return Jwts.builder()
            .setSubject(username)
            .setIssuedAt(new Date())
            .setExpiration(new Date(System.currentTimeMillis() + expiration))
            .signWith(SignatureAlgorithm.HS512, secret)
            .compact();
    }
    
    public String getUsernameFromToken(String token) {
        return Jwts.parser()
            .setSigningKey(secret)
            .parseClaimsJws(token)
            .getBody()
            .getSubject();
    }
    
    public boolean validateToken(String token) {
        try {
            Jwts.parser().setSigningKey(secret).parseClaimsJws(token);
            return true;
        } catch (Exception e) {
            return false;
        }
    }
}
```
```java
// Auth Controller
@RestController
@RequestMapping("/api/auth")
public class AuthController {
    
    @Autowired
    private AuthenticationManager authenticationManager;
    
    @Autowired
    private JwtTokenUtil jwtTokenUtil;
    
    @Autowired
    private UserService userService;
    
    @PostMapping("/login")
    public ResponseEntity<JwtResponse> login(@RequestBody LoginRequest request) {
        // Authenticate
        Authentication authentication = authenticationManager.authenticate(
            new UsernamePasswordAuthenticationToken(
                request.getUsername(),
                request.getPassword()
            )
        );
        
        // Generate token
        String token = jwtTokenUtil.generateToken(request.getUsername());
        
        return ResponseEntity.ok(new JwtResponse(token));
    }
    
    @PostMapping("/register")
    public ResponseEntity<User> register(@RequestBody RegisterRequest request) {
        User user = userService.register(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(user);
    }
}
```

**Usage:**
```bash
# 1. Register
POST http://localhost:8080/api/auth/register
{
  "username": "john",
  "password": "password123",
  "email": "john@example.com"
}

# 2. Login
POST http://localhost:8080/api/auth/login
{
  "username": "john",
  "password": "password123"
}

Response:
{
  "token": "eyJhbGciOiJIUzUxMiJ9..."
}

# 3. Access protected endpoint
GET http://localhost:8080/api/students
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9...
```

---

### **Day 7: Documentation & Testing**

**Swagger/OpenAPI Documentation:**
```java
// Add dependency
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-ui</artifactId>
    <version>1.7.0</version>
</dependency>
```

**Access:** `http://localhost:8080/swagger-ui.html`

Auto-generated interactive API documentation! 🎉

![Swagger UI](https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=1200&h=600&fit=crop)

**Unit Testing:**
```java
@SpringBootTest
@AutoConfigureMockMvc
class StudentControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private StudentService studentService;
    
    @Test
    void shouldReturnAllStudents() throws Exception {
        // Given
        List<Student> students = Arrays.asList(
            new Student(1L, "John", "john@email.com", 3.8, 2023, null, null),
            new Student(2L, "Jane", "jane@email.com", 3.9, 2023, null, null)
        );
        
        when(studentService.findAll()).thenReturn(students);
        
        // When & Then
        mockMvc.perform(get("/api/students"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$", hasSize(2)))
            .andExpect(jsonPath("$[0].name").value("John"))
            .andExpect(jsonPath("$[1].name").value("Jane"));
    }
    
    @Test
    void shouldCreateStudent() throws Exception {
        // Given
        StudentDTO dto = new StudentDTO("John", "john@email.com", 3.8, 2023);
        Student created = new Student(1L, "John", "john@email.com", 3.8, 2023, null, null);
        
        when(studentService.create(any())).thenReturn(created);
        
        // When & Then
        mockMvc.perform(post("/api/students")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(dto)))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.name").value("John"));
    }
}
```

---

## 🚀 Deployment

**Dockerize:**
```dockerfile
FROM openjdk:17-jdk-slim
COPY target/student-api-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "/app.jar"]
```
```bash
docker build -t student-api .
docker run -p 8080:8080 student-api
```

**Or deploy to Heroku/Railway/Render** - one click deployment!

---

## 💡 Lessons Learned

### **What I wish I knew on Day 1:**

1. **Start simple, add complexity gradually**
   - Day 1: Just CRUD
   - Day 2-3: Service layer
   - Day 4-5: Validation & error handling
   - Day 6-7: Security & docs

2. **Spring Boot handles 80% of boilerplate**
   - Auto-configuration
   - Dependency injection
   - JSON serialization
   - Database connection

3. **Follow conventions**
   - Controller → Service → Repository
   - DTOs for request/response
   - Custom exceptions
   - Global error handling

4. **Testing is not optional**
   - Write tests as you code
   - MockMvc for integration tests
   - @MockBean for unit tests

5. **Documentation matters**
   - Swagger/OpenAPI = free docs
   - README with API examples
   - Postman collection

---

## 📊 Final Architecture
```
Client (Mobile/Web)
    ↓ HTTP Request
┌───────────────────────┐
│   Controller Layer    │ ← @RestController
│   - Receive requests  │
│   - Validate input    │
│   - Return response   │
└───────────────────────┘
    ↓
┌───────────────────────┐
│   Service Layer       │ ← @Service
│   - Business logic    │
│   - Transactions      │
│   - Validation        │
└───────────────────────┘
    ↓
┌───────────────────────┐
│   Repository Layer    │ ← @Repository
│   - Database access   │
│   - Query generation  │
└───────────────────────┘
    ↓
┌───────────────────────┐
│      Database         │
│      (MySQL)          │
└───────────────────────┘
```

---

## 📚 Resources

**Official Docs:**
- [Spring Boot Reference](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Spring Data JPA](https://spring.io/projects/spring-data-jpa)
- [Spring Security](https://spring.io/projects/spring-security)

**Tutorials:**
- [Spring Boot Official Guides](https://spring.io/guides)
- [Baeldung Spring Tutorials](https://www.baeldung.com/spring-boot)
- [Spring Boot YouTube Playlist](https://www.youtube.com/playlist?list=PLqq-6Pq4lTTbx8p2oCgcAQGQyqN8XeA1x)

**Practice:**
- Build Todo API
- Build E-commerce API
- Build Social Media API

---

## 💭 Kết luận

1 tuần trước, Spring Boot là một mystery. Hôm nay, tôi có một production-ready REST API với đầy đủ features.

**Key takeaways:**

✅ Spring Boot giảm boilerplate 80%  
✅ Follow layered architecture (Controller → Service → Repository)  
✅ Use DTOs để decouple API từ database  
✅ Global exception handling for consistent errors  
✅ Bean Validation for clean validation  
✅ JWT for stateless authentication  
✅ Swagger for instant documentation  

**The best part?** Sau khi setup xong, thêm features mới rất nhanh. Tôi build một CRUD entity mới trong 15 phút!

Spring Boot không phải magic. Nó chỉ là convention over configuration. Một khi hiểu convention, bạn code nhanh như gió.

**From 'What is Spring Boot?' to Production API in 7 days. You can do it too! 🚀**

---

**Bạn đã build API nào với Spring Boot chưa? Share project nhé!**

**#SpringBoot #RestAPI #Java #Backend #WebDevelopment**