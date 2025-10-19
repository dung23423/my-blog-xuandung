---
title: "OOP trong Java: Khi lý thuyết gặp thực tế - Những bài học từ dự án thực chiến"
date: 2025-10-14
draft: false
tags: ["Java", "OOP", "Object-Oriented Programming", "Design Patterns"]
categories: ["Java"]
description: "Encapsulation, Inheritance, Polymorphism, Abstraction - không chỉ là lý thuyết khô khan. Đây là cách tôi áp dụng OOP để giải quyết vấn đề thực tế."
summary: "OOP không phải để thi. Đây là cách tôi dùng 4 trụ cột OOP để refactor một đống spaghetti code thành kiến trúc sạch đẹp."
feature: "https://images.unsplash.com/photo-1555066931-4365d14bab8c?w=800&h=600&fit=crop"
showTableOfContents: true
---

Tôi còn nhớ rất rõ ngày đầu tiên học về OOP. Giáo viên vẽ lên bảng 4 từ: **Encapsulation, Inheritance, Polymorphism, Abstraction**. Tôi note lại, học thuộc định nghĩa, làm bài tập, và nghĩ rằng mình đã "hiểu" OOP.

Cho đến khi tôi phải refactor một project có 1500 dòng code trong một file duy nhất. Code hoạt động, nhưng mỗi lần thêm feature mới, tôi phải sửa 10 chỗ khác nhau. Mỗi lần fix bug, 3 bug mới xuất hiện.

Đó là lúc tôi nhận ra: **Hiểu lý thuyết OOP ≠ Biết áp dụng OOP**.

![OOP Real World](https://images.unsplash.com/photo-1517694712202-14dd9538aa97?w=1200&h=600&fit=crop)

Bài viết này không phải để giải thích lại 4 trụ cột OOP (Google có đầy). Đây là câu chuyện về cách tôi **thực sự** học được OOP - qua việc phá vỡ và xây dựng lại code của chính mình.

---

## 🔥 Vấn đề: Khi code của bạn trở thành "spaghetti"

### **Dự án gốc: Student Management System (Version 1.0)**

Tháng đầu học Java, tôi tự hào với project này. Nó hoạt động! Có GUI! Lưu được database! Nhưng...
```java
// StudentManagementApp.java - 1500 dòng code trong 1 file
public class StudentManagementApp {
    // 50+ biến global
    static Scanner scanner = new Scanner(System.in);
    static Connection connection;
    static String[] studentNames = new String[100];
    static double[] studentGPAs = new double[100];
    static String[] studentEmails = new String[100];
    static int studentCount = 0;
    
    public static void main(String[] args) {
        // 200 dòng logic trong main()
        while (true) {
            System.out.println("1. Add Student");
            System.out.println("2. View Students");
            System.out.println("3. Update Student");
            // ... 20 options khác
            
            int choice = scanner.nextInt();
            
            if (choice == 1) {
                // 50 dòng code inline
                System.out.print("Enter name: ");
                String name = scanner.next();
                // validation logic...
                // database logic...
                // business logic...
                // tất cả lộn xộn ở đây
            } else if (choice == 2) {
                // Another 50 dòng code
            }
            // ... và cứ thế tiếp tục
        }
    }
}
```

**Vấn đề khi thêm feature mới:**

Khách hàng (bạn tôi) yêu cầu: *"Thêm Teacher management vào app"*

Tôi mất **3 ngày** vì:
- ❌ Phải copy-paste code Student và sửa lại
- ❌ Mỗi lần sửa phải scroll qua 1500 dòng code
- ❌ Không dám refactor vì sợ break mọi thứ
- ❌ Bug xuất hiện ở những chỗ không ngờ tới

Đó là lúc mentor của tôi nhìn code và nói: *"This is not how you Java."*

![Spaghetti Code](https://images.unsplash.com/photo-1555949963-ff9fe0c870eb?w=1200&h=600&fit=crop)

---

## 🎯 Giải pháp: OOP đến cứu nguy

Mentor của tôi ngồi xuống và hỏi:

> "Nếu đây là thế giới thực, Student và Teacher có gì giống nhau?"

Tôi nghĩ một chút: *"Cả hai đều là... người? Đều có tên, tuổi, email..."*

> "Chính xác. Vậy tại sao code lại viết riêng biệt?"

Một câu hỏi đơn giản, nhưng nó thay đổi cách tôi code mãi mãi.

---

## 📐 Principle 1: Encapsulation - Đóng gói dữ liệu

### **Vấn đề với code cũ:**
```java
// Trước khi refactor
static String[] studentNames = new String[100];
static double[] studentGPAs = new double[100];

// Ở bất kỳ đâu trong code
studentGPAs[5] = -1.5;  // Không ai kiểm tra! GPA âm!?
studentNames[10] = null;  // NPE waiting to happen
```

Dữ liệu scattered everywhere. Không có validation. Chaos.

### **Sau khi áp dụng Encapsulation:**
```java
public class Student {
    // Private - Không ai có thể access trực tiếp
    private String id;
    private String name;
    private String email;
    private double gpa;
    
    // Constructor với validation
    public Student(String id, String name, String email, double gpa) {
        this.id = id;
        setName(name);  // Dùng setter để validate
        setEmail(email);
        setGpa(gpa);
    }
    
    // Getters
    public String getName() {
        return name;
    }
    
    public double getGpa() {
        return gpa;
    }
    
    // Setters với validation
    public void setName(String name) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        this.name = name.trim();
    }
    
    public void setGpa(double gpa) {
        if (gpa < 0.0 || gpa > 4.0) {
            throw new IllegalArgumentException("GPA must be between 0.0 and 4.0");
        }
        this.gpa = gpa;
    }
    
    public void setEmail(String email) {
        // Email validation
        String emailRegex = "^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+$";
        if (!email.matches(emailRegex)) {
            throw new IllegalArgumentException("Invalid email format");
        }
        this.email = email;
    }
    
    // Business logic
    public String getGrade() {
        if (gpa >= 3.6) return "Excellent";
        if (gpa >= 3.2) return "Good";
        if (gpa >= 2.5) return "Average";
        return "Poor";
    }
    
    public boolean isEligibleForScholarship() {
        return gpa >= 3.5;
    }
}
```

**Lợi ích ngay lập tức:**

✅ **Validation tập trung:** Một chỗ duy nhất để check data  
✅ **Bug giảm 80%:** Không còn invalid data  
✅ **Dễ maintain:** Muốn đổi logic? Chỉ sửa 1 file  
✅ **Testable:** Có thể viết unit test dễ dàng

![Encapsulation Benefits](https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=1200&h=600&fit=crop)

**Real-world impact:**

Trước đây: Thêm validation cho email mất **2 giờ** (sửa 15 chỗ)  
Bây giờ: Chỉ mất **5 phút** (sửa 1 method)

---

## 🌳 Principle 2: Inheritance - Tái sử dụng code thông minh

### **Vấn đề: Code lặp lại**

Khi thêm Teacher, tôi copy-paste toàn bộ Student code. Kết quả:
```java
// Student.java - 300 dòng
class Student {
    private String id;
    private String name;
    private String email;
    private LocalDate dateOfBirth;
    // ... 20 properties khác
    
    // 50 methods
}

// Teacher.java - 290 dòng (gần giống hệt)
class Teacher {
    private String id;
    private String name;
    private String email;
    private LocalDate dateOfBirth;
    // ... 20 properties GIỐNG Student
    
    // 45 methods GIỐNG Student
    // + 5 methods riêng
}
```

Có gì sai? **Mọi thứ!**

- Sửa bug ở Student, phải sửa lại ở Teacher
- Thêm feature ở Student, phải code lại ở Teacher
- 600 dòng code có thể giảm còn 400

### **Giải pháp: Inheritance**
```java
// Base class - Chứa những gì chung
public abstract class Person {
    // Common properties
    protected String id;
    protected String name;
    protected String email;
    protected String phone;
    protected LocalDate dateOfBirth;
    
    // Constructor
    public Person(String id, String name, String email) {
        this.id = id;
        setName(name);
        setEmail(email);
    }
    
    // Common methods
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        this.name = name;
    }
    
    public int getAge() {
        if (dateOfBirth == null) return 0;
        return Period.between(dateOfBirth, LocalDate.now()).getYears();
    }
    
    // Abstract method - Child phải implement
    public abstract String getRole();
    public abstract void displayInfo();
}

// Student - Chỉ code phần khác biệt
public class Student extends Person {
    // Student-specific properties
    private double gpa;
    private String major;
    private int enrollmentYear;
    
    public Student(String id, String name, String email, double gpa) {
        super(id, name, email);  // Gọi constructor của Person
        setGpa(gpa);
    }
    
    @Override
    public String getRole() {
        return "Student";
    }
    
    @Override
    public void displayInfo() {
        System.out.println("=== STUDENT INFO ===");
        System.out.println("ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Email: " + email);
        System.out.println("GPA: " + gpa);
        System.out.println("Major: " + major);
    }
    
    // Student-specific methods
    public String getGrade() {
        if (gpa >= 3.6) return "Excellent";
        if (gpa >= 3.0) return "Good";
        return "Average";
    }
    
    public boolean isEligibleForScholarship() {
        return gpa >= 3.5;
    }
    
    // Getters/Setters
    public void setGpa(double gpa) {
        if (gpa < 0 || gpa > 4.0) {
            throw new IllegalArgumentException("Invalid GPA");
        }
        this.gpa = gpa;
    }
    
    public double getGpa() {
        return gpa;
    }
}

// Teacher - Chỉ code phần riêng
public class Teacher extends Person {
    // Teacher-specific properties
    private String department;
    private double salary;
    private List<String> courses;
    
    public Teacher(String id, String name, String email, String department) {
        super(id, name, email);
        this.department = department;
        this.courses = new ArrayList<>();
    }
    
    @Override
    public String getRole() {
        return "Teacher";
    }
    
    @Override
    public void displayInfo() {
        System.out.println("=== TEACHER INFO ===");
        System.out.println("ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Email: " + email);
        System.out.println("Department: " + department);
        System.out.println("Courses: " + String.join(", ", courses));
    }
    
    // Teacher-specific methods
    public void assignCourse(String course) {
        if (!courses.contains(course)) {
            courses.add(course);
            System.out.println("Assigned course: " + course);
        }
    }
    
    public void removeCourse(String course) {
        courses.remove(course);
        System.out.println("Removed course: " + course);
    }
    
    public int getTeachingLoad() {
        return courses.size();
    }
}
```

**Kết quả:**

✅ Code giảm từ 600 dòng → 400 dòng (33% reduction)  
✅ Sửa bug ở Person → Tất cả child classes đều được fix  
✅ Thêm Admin, Staff chỉ mất 10 phút  
✅ Code dễ đọc, dễ maintain hơn nhiều

![Inheritance Tree](https://images.unsplash.com/photo-1558618666-fcd25c85cd64?w=1200&h=600&fit=crop)

---

## 🎭 Principle 3: Polymorphism - Một interface, nhiều implementations

### **Tình huống thực tế:**

Tôi cần display thông tin cho Student, Teacher, và Admin. Cách cũ:
```java
// Before Polymorphism - Ugly!
if (person instanceof Student) {
    Student s = (Student) person;
    System.out.println("Student: " + s.getName());
    System.out.println("GPA: " + s.getGpa());
} else if (person instanceof Teacher) {
    Teacher t = (Teacher) person;
    System.out.println("Teacher: " + t.getName());
    System.out.println("Department: " + t.getDepartment());
} else if (person instanceof Admin) {
    Admin a = (Admin) person;
    System.out.println("Admin: " + a.getName());
    System.out.println("Role: " + a.getRole());
}
```

Thêm một loại person mới? Phải sửa hàng tá chỗ!

### **Với Polymorphism:**
```java
// One method to rule them all
public void displayPerson(Person person) {
    person.displayInfo();  // Gọi method của child class tương ứng
}

// Usage - Clean & Simple
List<Person> people = new ArrayList<>();
people.add(new Student("S001", "An", "an@email.com", 3.8));
people.add(new Teacher("T001", "Bình", "binh@email.com", "CS"));
people.add(new Admin("A001", "Cường", "cuong@email.com", "IT Manager"));

for (Person person : people) {
    displayPerson(person);  // Polymorphism at work!
    System.out.println("---");
}
```

**Output:**
```
=== STUDENT INFO ===
ID: S001
Name: An
GPA: 3.8
---
=== TEACHER INFO ===
ID: T001
Name: Bình
Department: CS
---
=== ADMIN INFO ===
ID: A001
Name: Cường
Role: IT Manager
---
```

**Magic?** Không! Đó là **Polymorphism**.

Java tự động gọi đúng method của từng class. Thêm 10 loại person mới? Code trên vẫn hoạt động mà không cần sửa!

![Polymorphism in Action](https://images.unsplash.com/photo-1516116216624-53e697fedbea?w=1200&h=600&fit=crop)

### **Real-world example: Payment System**
```java
// Payment Interface
interface PaymentMethod {
    boolean processPayment(double amount);
    String getPaymentType();
}

// Multiple Implementations
class CreditCardPayment implements PaymentMethod {
    private String cardNumber;
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing credit card payment: $" + amount);
        // Credit card logic
        return true;
    }
    
    @Override
    public String getPaymentType() {
        return "Credit Card";
    }
}

class PayPalPayment implements PaymentMethod {
    private String email;
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing PayPal payment: $" + amount);
        // PayPal API logic
        return true;
    }
    
    @Override
    public String getPaymentType() {
        return "PayPal";
    }
}

class BankTransferPayment implements PaymentMethod {
    private String accountNumber;
    
    @Override
    public boolean processPayment(double amount) {
        System.out.println("Processing bank transfer: $" + amount);
        // Bank API logic
        return true;
    }
    
    @Override
    public String getPaymentType() {
        return "Bank Transfer";
    }
}

// Order class - Không cần biết payment method cụ thể
class Order {
    private PaymentMethod paymentMethod;
    
    public void checkout(double amount) {
        System.out.println("Processing order...");
        boolean success = paymentMethod.processPayment(amount);
        if (success) {
            System.out.println("✓ Order completed via " + 
                             paymentMethod.getPaymentType());
        }
    }
    
    public void setPaymentMethod(PaymentMethod method) {
        this.paymentMethod = method;
    }
}

// Usage - Super flexible
Order order = new Order();

// Khách hàng 1: Credit Card
order.setPaymentMethod(new CreditCardPayment("1234-5678"));
order.checkout(100.0);

// Khách hàng 2: PayPal
order.setPaymentMethod(new PayPalPayment("user@email.com"));
order.checkout(200.0);

// Thêm Bitcoin payment? Chỉ cần tạo class mới implement PaymentMethod!
```

**Lợi ích:**
- Thêm payment method mới không cần sửa Order class
- Dễ test (mock PaymentMethod)
- Flexible và scalable

---

## 🎨 Principle 4: Abstraction - Che giấu complexity

### **Vấn đề: Quá nhiều details**

User không cần biết bạn lưu data vào MySQL hay MongoDB. Họ chỉ cần "save" và "load".
```java
// BAD - Expose implementation details
public class StudentDatabase {
    public void saveToMySQL(Student s) {
        // MySQL specific code
    }
    
    public void saveToMongoDB(Student s) {
        // MongoDB specific code
    }
}

// User phải biết database nào đang dùng
database.saveToMySQL(student);  // Tightly coupled!
```

### **GOOD - Abstraction**
```java
// Interface - Abstract away implementation
public interface StudentRepository {
    void save(Student student);
    Student findById(String id);
    List<Student> findAll();
    void update(Student student);
    void delete(String id);
}

// MySQL Implementation
public class MySQLStudentRepository implements StudentRepository {
    private Connection connection;
    
    @Override
    public void save(Student student) {
        String sql = "INSERT INTO students (id, name, gpa) VALUES (?, ?, ?)";
        try (PreparedStatement stmt = connection.prepareStatement(sql)) {
            stmt.setString(1, student.getId());
            stmt.setString(2, student.getName());
            stmt.setDouble(3, student.getGpa());
            stmt.executeUpdate();
        } catch (SQLException e) {
            throw new RuntimeException("Error saving student", e);
        }
    }
    
    @Override
    public Student findById(String id) {
        // MySQL query
        return null;
    }
    
    // Implement other methods...
}

// MongoDB Implementation
public class MongoDBStudentRepository implements StudentRepository {
    private MongoCollection<Document> collection;
    
    @Override
    public void save(Student student) {
        Document doc = new Document()
            .append("id", student.getId())
            .append("name", student.getName())
            .append("gpa", student.getGpa());
        collection.insertOne(doc);
    }
    
    @Override
    public Student findById(String id) {
        // MongoDB query
        return null;
    }
    
    // Implement other methods...
}

// Service Layer - Không biết database nào
public class StudentService {
    private StudentRepository repository;
    
    // Dependency Injection
    public StudentService(StudentRepository repository) {
        this.repository = repository;
    }
    
    public void enrollStudent(Student student) {
        // Business logic
        if (student.getGpa() < 2.0) {
            throw new IllegalArgumentException("GPA too low");
        }
        
        // Save - Không quan tâm MySQL hay MongoDB
        repository.save(student);
    }
}

// Usage - Flexible!
// Development: Dùng MySQL
StudentService service1 = new StudentService(new MySQLStudentRepository());

// Production: Chuyển sang MongoDB chỉ cần đổi 1 dòng
StudentService service2 = new StudentService(new MongoDBStudentRepository());

// Testing: Dùng Mock
StudentService service3 = new StudentService(new MockStudentRepository());
```

**Benefits:**

✅ **Flexibility:** Đổi database không cần sửa business logic  
✅ **Testability:** Dễ dàng mock cho testing  
✅ **Maintainability:** Thay đổi implementation không ảnh hưởng client code  
✅ **Team work:** Người khác có thể implement interface theo cách riêng

![Abstraction Layers](https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=1200&h=600&fit=crop)

---

## 🏗️ Putting It All Together: Final Architecture

Sau khi refactor hoàn toàn, đây là kiến trúc cuối cùng:
```
src/
├── models/
│   ├── Person.java (abstract base class)
│   ├── Student.java
│   ├── Teacher.java
│   └── Admin.java
│
├── repositories/
│   ├── Repository.java (interface)
│   ├── StudentRepository.java (interface)
│   ├── MySQLStudentRepository.java
│   └── MongoDBStudentRepository.java
│
├── services/
│   ├── StudentService.java
│   ├── TeacherService.java
│   └── AuthService.java
│
├── controllers/
│   ├── StudentController.java
│   └── TeacherController.java
│
└── utils/
    ├── Validator.java
    └── DatabaseConnection.java
```

**Before vs After:**

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Files | 1 | 15 | Better organization |
| Lines/File | 1500 | 100-200 | More readable |
| Code Duplication | ~40% | <5% | DRY principle |
| Bug Fix Time | Hours | Minutes | Maintainability |
| Add Feature Time | Days | Hours | Extensibility |
| Test Coverage | 0% | 80% | Quality |

---

## 💡 Lessons Learned

### **1. OOP là mindset, không phải syntax**

Bạn có thể viết Java mà không OOP. Nhưng bạn sẽ khổ.

### **2. Đừng over-engineer từ đầu**

Version 1.0 của tôi cũng không hoàn hảo. Refactor là phần của process.

### **3. SOLID principles là bạn**

- **S**ingle Responsibility
- **O**pen/Closed
- **L**iskov Substitution
- **I**nterface Segregation
- **D**ependency Inversion

Học sau khi đã hiểu OOP.

### **4. Practice, practice, practice**

Đọc 100 bài về OOP không bằng refactor 1 dự án thực tế.

### **5. Code review**

Nhờ người khác review code. Tôi học được nhiều nhất từ feedback.

---

## 🎯 Thử thách cho bạn

Hãy lấy một dự án cũ của bạn và tự hỏi:

1. Có methods dài hơn 50 dòng không?
2. Có code lặp lại không?
3. Có global variables không cần thiết không?
4. Nếu thêm feature mới, phải sửa bao nhiêu chỗ?

Nếu câu trả lời là "Có", đó là dấu hiệu cần refactor với OOP.

---

## 📚 Resources

**Sách:**
- "Head First Object-Oriented Analysis and Design"
- "Clean Code" by Robert C. Martin
- "Refactoring" by Martin Fowler

**Courses:**
- [Object-Oriented Programming in Java - Coursera](https://www.coursera.org/learn/object-oriented-java)
- [Design Patterns in Java - Udemy](https://www.udemy.com/course/design-patterns-java/)

**Practice:**
- Refactor một dự án cũ
- Code review với bạn bè
- Contribute to open source

---

## 💭 Kết luận

OOP không làm code của bạn chạy nhanh hơn. Nó không làm app của bạn có nhiều feature hơn. Nhưng nó làm:

✅ Code **dễ đọc** hơn  
✅ **Dễ maintain** hơn  
✅ **Dễ scale** hơn  
✅ **Dễ collaborate** hơn

Và quan trọng nhất: Nó làm bạn trở thành **better developer**.

6 tháng trước, tôi viết code hoạt động. Hôm nay, tôi viết code **mà người khác có thể đọc và maintain**. Đó là sự khác biệt giữa **coder** và **software engineer**.

**The journey from spaghetti to clean code continues... 🚀**

---

**Bạn đã refactor code nào với OOP chưa? Chia sẻ kinh nghiệm nhé!**

**#Java #OOP #CleanCode #SoftwareEngineering #Programming**