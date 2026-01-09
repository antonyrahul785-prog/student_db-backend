# student_db-backend
  to intialize the backend server for student database management system
 # node .\src\scripts\initDatabase.js
 #npm run dev
  to run the backend server in development mode
I'll provide complete Postman tests with **raw JSON** format for all APIs. Here are comprehensive test collections:

## **1. POSTMAN ENVIRONMENT VARIABLES SETUP**

Create environment variables:
- `base_url`: http://localhost:5000/api
- `auth_token`: (will be set after login)
- `admin_token`: (for admin operations)
- `trainer_token`: (for trainer operations)
- `student_token`: (for student operations)
- `counselor_token`: (for counselor operations)
- `employee_token`: (for employee operations)
- `user_id`: (will be set dynamically)
- `student_id`: (will be set dynamically)
- `course_id`: (will be set dynamically)
- `batch_id`: (will be set dynamically)
- `enrollment_id`: (will be set dynamically)
- `payment_id`: (will be set dynamically)
- `lead_id`: (will be set dynamically)
- `attendance_id`: (will be set dynamically)

## **2. AUTHENTICATION APIs (`authRoutes.js`)**

### **2.1 Register User**
```json
POST {{base_url}}/api/auth/register
Content-Type: application/json

{
    "username": "testuser",
    "email": "test@example.com",
    "password": "password123",
    "role": "student",
    "name": "Test User",
    "phone": "1234567890"
}
```

### **2.2 Login User**
```json
POST {{base_url}}/api/auth/login
Content-Type: application/json

{
    "email": "test@example.com",
    "password": "password123"
}
```

### **2.3 Get Current User Profile**
```json
GET {{base_url}}/api/auth/me
Authorization: Bearer {{auth_token}}
```

### **2.4 Update Profile**
```json
PUT {{base_url}}/api/auth/profile
Authorization: Bearer {{auth_token}}
Content-Type: application/json

{
    "username": "updateduser",
    "phone": "9876543210",
    "name": "Updated Name",
    "address": "123 Updated Street"
}


### **2.6 Logout**
```json
POST {{base_url}}/api/auth/logout
Authorization: Bearer {{auth_token}}
```

### **2.7 Forgot Password**
```json
POST {{base_url}}/api/auth/forgot-password
Content-Type: application/json

{
    "email": "test@example.com"
}
```

### **2.8 Reset Password**
```json
PUT {{base_url}}/auth/reset-password/:token
Content-Type: application/json

{
    "password": "newpassword123",
    "confirmPassword": "newpassword123"
}
```

## **3. USER MANAGEMENT APIs (`userRoutes.js`)**

### **3.1 Get All Users (Admin)**
```json
GET {{base_url}}/users
Authorization: Bearer {{admin_token}}
```

### **3.2 Create New User (Admin)**
```json
POST {{base_url}}/users
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "username": "newtrainer",
    "email": "trainer@example.com",
    "password": "password123",
    "role": "trainer",
    "name": "Trainer Name",
    "phone": "1234567890"
}
```

### **3.3 Get User by ID**
```json
GET {{base_url}}/users/{{user_id}}
Authorization: Bearer {{admin_token}}
```

### **3.4 Update User (Admin)**
```json
PUT {{base_url}}/users/{{user_id}}
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "username": "updatedtrainer",
    "phone": "9876543210",
    "email": "updated@example.com"
}
```

### **3.5 Update User Permissions**
```json
PUT {{base_url}}/users/{{user_id}}/permissions
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "permissions": [
        {
            "module": "students",
            "canView": true,
            "canCreate": true,
            "canEdit": true,
            "canDelete": false
        },
        {
            "module": "courses",
            "canView": true,
            "canCreate": false,
            "canEdit": false,
            "canDelete": false
        }
    ]
}
```

### **3.6 Change User Role**
```json
PUT {{base_url}}/users/{{user_id}}/role
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "role": "trainer"
}
```



## **4. STUDENT MANAGEMENT APIs (`studentRoutes.js`)**

### **4.1 Get All Students**
```json
GET {{base_url}}/students
Authorization: Bearer {{auth_token}}
```

### **4.2 Create New Student**
```json
POST {{base_url}}/students
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "1234567890",
    "dateOfBirth": "2000-01-01",
    "address": "123 Main St, City",
    "gender": "male",
    "education": "Bachelor's Degree",
    "emergencyContact": {
        "name": "Jane Doe",
        "phone": "0987654321",
        "relationship": "Mother"
    },
    "guardian": {
        "name": "Robert Doe",
        "phone": "1122334455",
        "email": "robert@example.com"
    }
}
```

### **4.3 Get Student by ID**
```json
GET {{base_url}}/students/{{student_id}}
Authorization: Bearer {{auth_token}}
```

### **4.4 Update Student**
```json
PUT {{base_url}}/students/{{student_id}}
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "name": "John Updated",
    "phone": "9998887776",
    "address": "456 New Street"
}
```

### **4.5 Enroll Student in Course**
```json
POST {{base_url}}/students/{{student_id}}/enroll
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "courseId": "{{course_id}}",
    "batchId": "{{batch_id}}",
    "fees": {
        "total": 10000,
        "discount": 1000,
        "final": 9000,
        "paid": 3000,
        "due": 6000
    },
    "enrollmentDate": "2024-01-15"
}
```

### **4.6 Get Student Fee Summary**
```json
GET {{base_url}}/students/{{student_id}}/fee-summary
Authorization: Bearer {{auth_token}}
```

## **5. COURSE MANAGEMENT APIs (`courseRoutes.js`)**

### **5.1 Get All Courses**
```json
GET {{base_url}}/courses
Authorization: Bearer {{auth_token}}
```

### **5.2 Create New Course**
```json
POST {{base_url}}/courses
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "name": "Full Stack Web Development",
    "description": "Comprehensive web development course covering frontend and backend technologies",
    "category": "Programming",
    "duration": {
        "value": 12,
        "unit": "weeks"
    },
    "fees": {
        "regular": 20000,
        "earlyBird": 15000,
        "installment": 5000
    },
    "prerequisites": ["Basic HTML", "Basic JavaScript", "Basic CSS"],
    "learningOutcomes": [
        "Build responsive websites",
        "Create REST APIs",
        "Deploy applications",
        "Work with databases"
    ],
    "syllabus": [
        {
            "module": "HTML & CSS",
            "topics": ["HTML5", "CSS3", "Flexbox", "Grid"]
        },
        {
            "module": "JavaScript",
            "topics": ["ES6+", "DOM Manipulation", "Async Programming"]
        }
    ]
}
```

### **5.3 Get Course by ID**
```json
GET {{base_url}}/courses/{{course_id}}
Authorization: Bearer {{auth_token}}
```

### **5.4 Update Course**
```json
PUT {{base_url}}/courses/{{course_id}}
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "name": "Updated Course Name",
    "description": "Updated description",
    "fees": {
        "regular": 22000,
        "earlyBird": 18000
    }
}
```

### **5.5 Add Batch to Course**
```json
POST {{base_url}}/courses/{{course_id}}/batches
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "name": "Batch 2024-01",
    "startDate": "2024-02-01",
    "endDate": "2024-04-30",
    "instructor": "{{user_id}}",
    "maxStudents": 30,
    "schedule": {
        "days": ["Monday", "Wednesday", "Friday"],
        "time": "18:00-20:00"
    },
    "classroom": "Room 101"
}
```

### **5.6 Add Review to Course**
```json
POST {{base_url}}/courses/{{course_id}}/reviews
Authorization: Bearer {{student_token}}
Content-Type: application/json

{
    "rating": 5,
    "comment": "Excellent course with great instructors!",
    "studentName": "Test Student"
}
```

## **6. BATCH MANAGEMENT APIs (`batchRoutes.js`)**

### **6.1 Get All Batches**
```json
GET {{base_url}}/batches
Authorization: Bearer {{auth_token}}
```

### **6.2 Create New Batch**
```json
POST {{base_url}}/batches
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "course": "{{course_id}}",
    "name": "Web Dev Batch 2024",
    "startDate": "2024-03-01",
    "endDate": "2024-06-01",
    "instructor": "{{user_id}}",
    "maxStudents": 25,
    "schedule": "Mon, Wed, Fri 6-8 PM",
    "status": "upcoming",
    "description": "Intensive web development batch"
}
```

### **6.3 Add Student to Batch**
```json
POST {{base_url}}/batches/{{batch_id}}/students
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "studentId": "{{student_id}}"
}
```

### **6.4 Add Session to Batch**
```json
POST {{base_url}}/batches/{{batch_id}}/sessions
Authorization: Bearer {{trainer_token}}
Content-Type: application/json

{
    "topic": "Introduction to JavaScript",
    "date": "2024-03-01T10:00:00.000Z",
    "duration": 120,
    "description": "Basic JavaScript concepts and syntax",
    "materials": ["slides.pdf", "exercise.js"]
}
```

## **7. ENROLLMENT MANAGEMENT APIs (`enrollmentRoutes.js`)**

### **7.1 Get All Enrollments**
```json
GET {{base_url}}/enrollments
Authorization: Bearer {{auth_token}}
```

### **7.2 Create New Enrollment**
```json
POST {{base_url}}/enrollments
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "student": "{{student_id}}",
    "course": "{{course_id}}",
    "batch": "{{batch_id}}",
    "fees": {
        "total": 15000,
        "discount": 2000,
        "final": 13000,
        "paid": 5000,
        "due": 8000
    },
    "paymentPlan": "installment",
    "status": "active",
    "enrollmentDate": "2024-01-20"
}
```

### **7.3 Update Enrollment Progress**
```json
PUT {{base_url}}/enrollments/{{enrollment_id}}/progress
Authorization: Bearer {{trainer_token}}
Content-Type: application/json

{
    "moduleId": "module_123",
    "moduleName": "JavaScript Fundamentals",
    "completed": true,
    "score": 85,
    "completedDate": "2024-02-15"
}
```

### **7.4 Mark Attendance for Enrollment**
```json
POST {{base_url}}/enrollments/{{enrollment_id}}/attendance
Authorization: Bearer {{trainer_token}}
Content-Type: application/json

{
    "date": "2024-02-01",
    "status": "present",
    "session": "session_123",
    "remarks": "Participated actively"
}
```

### **7.5 Submit Assignment**
```json
POST {{base_url}}/enrollments/{{enrollment_id}}/assignments/assignment_123/submit
Authorization: Bearer {{student_token}}
Content-Type: application/json

{
    "submission": "Assignment submission content or file URL",
    "submittedDate": "2024-02-10",
    "comments": "Please review my assignment"
}
```

## **8. ATTENDANCE MANAGEMENT APIs (`attendanceRoutes.js`)**

### **8.1 Mark Attendance**
```json
POST {{base_url}}/attendance
Authorization: Bearer {{trainer_token}}
Content-Type: application/json

{
    "student": "{{student_id}}",
    "date": "2024-02-01",
    "status": "present",
    "batch": "{{batch_id}}",
    "session": "session_123",
    "remarks": "On time"
}
```

### **8.2 Generate Attendance Report**
```json
POST {{base_url}}/attendance/report
Authorization: Bearer {{trainer_token}}
Content-Type: application/json

{
    "startDate": "2024-01-01",
    "endDate": "2024-01-31",
    "batch": "{{batch_id}}",
    "format": "pdf"
}
```

### **8.3 Get Student Attendance**
```json
GET {{base_url}}/attendance/student/{{student_id}}
Authorization: Bearer {{auth_token}}
```

### **8.4 Get Batch Attendance**
```json
GET {{base_url}}/attendance/batch/{{batch_id}}
Authorization: Bearer {{trainer_token}}
```

## **9. CONTENT MANAGEMENT APIs (`contentRoutes.js`)**

### **9.1 Create Content**
```json
POST {{base_url}}/content
Authorization: Bearer {{trainer_token}}
Content-Type: application/json

{
    "title": "JavaScript Fundamentals PDF",
    "type": "document",
    "course": "{{course_id}}",
    "description": "Complete guide to JavaScript basics",
    "tags": ["javascript", "beginner", "programming"],
    "access": ["student", "trainer"],
    "fileUrl": "/uploads/javascript-guide.pdf"
}
```

### **9.2 Share Content with Students**
```json
POST {{base_url}}/content/{{content_id}}/share
Authorization: Bearer {{trainer_token}}
Content-Type: application/json

{
    "students": ["{{student_id}}", "student_id_2"],
    "message": "Please review this material before next class"
}
```

### **9.3 Get Course Content**
```json
GET {{base_url}}/content/course/{{course_id}}
Authorization: Bearer {{auth_token}}
```

## **10. PAYMENT MANAGEMENT APIs (`paymentRoutes.js`)**

### **10.1 Create Payment**
```json
POST {{base_url}}/payments
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "student": "{{student_id}}",
    "amount": 5000,
    "paymentMode": "online",
    "paymentDate": "2024-02-01",
    "referenceNumber": "PAY202402011234",
    "purpose": "Course fees installment",
    "enrollment": "{{enrollment_id}}",
    "receivedBy": "{{user_id}}",
    "remarks": "First installment payment"
}
```

### **10.2 Verify Payment**
```json
PUT {{base_url}}/payments/{{payment_id}}/verify
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "verified": true,
    "verifiedBy": "{{user_id}}",
    "verificationDate": "2024-02-01"
}
```

### **10.3 Refund Payment**
```json
PUT {{base_url}}/payments/{{payment_id}}/refund
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "amount": 2000,
    "reason": "Course cancellation",
    "refundDate": "2024-02-05",
    "refundMethod": "bank_transfer",
    "remarks": "Partial refund as per policy"
}
```

## **11. LEAD MANAGEMENT APIs (`leadRoutes.js`)**

### **11.1 Create Lead**
```json
POST {{base_url}}/leads
Authorization: Bearer {{counselor_token}}
Content-Type: application/json

{
    "name": "Prospective Student",
    "email": "prospect@example.com",
    "phone": "1234567890",
    "source": "website",
    "interestedIn": ["{{course_id}}"],
    "status": "new",
    "notes": "Interested in web development course",
    "assignedTo": "{{user_id}}",
    "followUpDate": "2024-02-05"
}
```

### **11.2 Add Communication to Lead**
```json
POST {{base_url}}/leads/{{lead_id}}/communications
Authorization: Bearer {{counselor_token}}
Content-Type: application/json

{
    "type": "call",
    "date": "2024-02-01",
    "summary": "Called to discuss course details",
    "nextAction": "Send course brochure",
    "nextActionDate": "2024-02-03",
    "notes": "Seems very interested, requested demo session"
}
```

### **11.3 Convert Lead to Student**
```json
POST {{base_url}}/leads/{{lead_id}}/convert
Authorization: Bearer {{counselor_token}}
Content-Type: application/json

{
    "convertToStudent": true,
    "enrollmentData": {
        "course": "{{course_id}}",
        "batch": "{{batch_id}}",
        "fees": {
            "total": 15000,
            "discount": 1000
        }
    }
}
```

## **12. ANALYTICS APIs (`analyticsRoutes.js`)**

### **12.1 Get Dashboard Stats**
```json
GET {{base_url}}/analytics/dashboard
Authorization: Bearer {{admin_token}}
```

### **12.2 Get Revenue Analytics**
```json
GET {{base_url}}/analytics/revenue
Authorization: Bearer {{admin_token}}
Content-Type: application/json

{
    "startDate": "2024-01-01",
    "endDate": "2024-01-31",
    "groupBy": "week"
}
```

### **12.3 Get Student Analytics**
```json
GET {{base_url}}/analytics/students
Authorization: Bearer {{auth_token}}
Content-Type: application/json

{
    "timeframe": "month",
    "status": "active"
}
```

### **12.4 Get Course Analytics**
```json
GET {{base_url}}/analytics/courses
Authorization: Bearer {{auth_token}}
Content-Type: application/json

{
    "includeEnrollments": true,
    "includeRevenue": true
}
```


## **14. IMPORTANT NOTES:**

1. **Token Management**: Always use the appropriate token for each endpoint
2. **ID References**: Some endpoints require IDs from other resources (e.g., `course_id`, `batch_id`)
3. **File Uploads**: For file upload endpoints, use Postman's form-data with file selection
4. **Validation**: All requests include validation - ensure all required fields are provided
5. **Permissions**: Different roles have different access levels
6. **Error Handling**: Check for 401 (Unauthorized), 403 (Forbidden), and 400 (Bad Request) responses

## **15. POSTMAN COLLECTION STRUCTURE:**

Organize your Postman collection like this:
```
LMS API Collection
├── Authentication
│   ├── Register
│   ├── Login
│   ├── Profile
│   └── Password
├── User Management
├── Student Management
├── Course Management
├── Batch Management
├── Enrollment Management
├── Attendance Management
├── Content Management
├── Payment Management
├── Lead Management
└── Analytics
```


Each folder should have the appropriate tests and pre-request scripts set up.

