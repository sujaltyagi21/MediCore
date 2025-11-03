
##MediCore
# 1. Project Summary

The Spring Boot Clinic Management System is a strong backend system made to make managing a medical clinic easier.
It helps with user login, keeping track of patient records, making appointments, handling payments, and organizing documents, all in one place, so clinic work is done more efficiently.

# 2. The Issue

In the real world, many clinics still use old methods like paper forms, spreadsheets, and separate software.
This causes many problems for everyone:

* **For Patients:** Scheduling an appointment often means waiting on the phone for a long time. Getting their medical records, test results, or bills is a slow and difficult process.
* **For Doctors and Staff:** Keeping track of patient records is hard and slow. Searching for physical files, managing a complete history, keeping up with schedules, and working with the billing team uses up time that should be spent helping patients.
* **For Administrators:** There isn't a single place to keep track of everything. Managing staff, checking clinic performance, making sure bills are correct, and keeping patient records secure and up to date is tough, leading to delays and lost money.

# 3. The Fix

This Clinic Management System solves these issues by offering a digital, central platform.

* **Improved Patient Experience:** Patients can use APIs to sign up, log in, book appointments (`POST /api/appointments`), and view their own profiles and files, making it much easier and faster for them.
* **Helpful for Medical Staff:** Doctors and employees get a full view of patient data. They can handle patient cases digitally, see complete appointment histories, and access all related documents quickly, helping them provide better care.
* **Easy Administrative Control:** Administrators have a strong tool to run the clinic. With different access levels, they can manage staff, departments, and services. The link between patient cases, services, and invoices helps with accurate billing, and the whole system acts as a secure, single source of information.

# 4. Features

Based on the database structure, the system offers these important features:

* **User Management & Login:** Safe registration and login for different user roles like Patient, Doctor/Employee, and Administrator.
* **Role-Based Access:** Controls access and permissions based on who the user is.
* **Patient Management:** Patients can create profiles, view their medical history, update their details, and interact with the system.
* **Doctor / Employee Management:** Full control over doctors and other staff, tied to specific departments.
* **Department Management:** Administrators can create and manage different departments in the clinic like Cardiology or Dermatology.
* **Appointment Scheduling:** Patients can easily book, view, and change appointments with available doctors. The system tracks appointment status and keeps a history.
* **Patient Case Management:** Doctors can set up, update, and manage detailed patient records, including treatments, diagnoses, and related services.
* **Service Management:** (Inferred) Define and manage medical services provided, each with specific costs.
* **Billing & Invoicing:** (Inferred) Generate bills for services and appointments with automatic cost calculation based on `PatientCase` costs.
* **Document Handling:** Safe uploading, storing, and retrieving of documents like lab results or prescriptions linked to patients, appointments, or cases.

# 5. Technologies Used (Estimated)

* **Backend Framework:** Spring Boot
* **Security:** Spring Security for user authentication and authorization
* **Database:** PostgreSQL (based on common setups in Spring Boot/JPA and typical ERD practices)
* **ORM:** Spring Data JPA or Hibernate
* **Build Tool:** Maven or Gradle
* **Programming Language:** Java

# 6. Starting Up (Conceptual)

To get started with a local environment:

1.  **Clone the Repository:** `git clone [repository-url]`
2.  **Set Up Database:** Configure a PostgreSQL database (e.g., named `ClinicManagementSystem`) and update `application.properties` with the connection details.
3.  **Build & Run:** Use Maven (`./mvnw spring-boot:run`) or Gradle (`./gradlew bootRun`) to start the app.

# 7. REST API Endpoints (Conceptual)

The system provides several RESTful APIs, some examples are:

* `POST /api/auth/register` (Patient Registration)
* `POST /api/auth/login` (User Authentication)
* `GET /api/patients/{id}` (Get Patient Profile)
* `POST /api/appointments` (Book a New Appointment)
* `PUT /api/appointments/{id}/status` (Change Appointment Status)
* `GET /api/doctors` (List All Doctors)
* `POST /api/admin/services` (Create New Service - Only for Admins)
* `GET /api/documents/patient/{patientId}` (Get Patient Documents)

# 8. Entity Classes & Database Schema

This ERD shows the database tables and their relationships. The entity classes below map directly to this structure.

![Entity Diagram](./diagram.png)

### Role
* id
* role_name String(64)
* List<HasRole> has_roles (One to Many)

### hasRole
* id
* time_from LocalDateTime
* time_to LocalDateTime
* is_active Boolean
* Role role(Many to One)
* Employee employee (Many to One)

### Employee
* id
* first_name String(64)
* last_name String(64)
* username String(64)
* password String(64)
* email String(255)
* phone String(64)
* is_active Boolean
* List<HasRole> has_roles (One to Many)
* List<InDepartment> in_departments (One to Many)

### InDepartment
* id
* time_from LocalDateTime
* time_to LocalDateTime
* is_active Boolean
* Department department (Many to One)
* Employee employee (Many to One)
* List<Schedule> schedules (One to Many)
* List<Appointment> appointments (One to Many)
* List<Document> documents (One to Many)

### Schedule
* id
* date LocalDate
* time_start LocalDateTime
* time_end LocalDateTime
* InDepartment in_department (Many to One)

### Department
* id
* name String(64)
* Clinic clinic (Many to One)

### Clinic
* id
* name String(64)
* address String(255)
* details String(255)
* List<Department> departments (One to Many)

### Patient
* id
* first_name String(64)
* last_name String(64)
* List<PatientCase> patient_cases (One to Many)
* List<Document> documents (One to Many)

### PatientCase
* id
* start_time LocalDateTime
* end_time LocalDateTime
* in_progress Boolean
* total_cost BigDecimal
* amount_paid BigDecimal
* Patient patient (Many to One)
* List<Appointment> appointments (One to Many)
* List<Document> documents (One to Many)

### Appointment
* id
* created_time LocalDateTime
* start_time LocalDateTime
* end_time LocalDateTime
* Status status (Many to One)
* List<StatusHistory> status_histories (One to Many)

### Status
* id
* name String(64)
* List<Appointment> appointments (One to Many)
* List<StatusHistory> status_histories (One to Many)

### StatusHistory
* id
* time LocalDateTime
* details String(255)
* Status status (Many to One)
* Appointment appointment (Many to One)

### DocumentType
* id
* name String(64)
* List<Document> documents (One to Many)

### Document
* id
* document_internal_id String(64)
* name String(255)
* created_time LocalDateTime
* url String(255)
* details
* DocumentType document_type (Many to One)
* Patient patient (Many to One)
* PatientCase patient_case (Many to One)
* Appointment appointment (Many to One)
* InDepartment in_department (Many to One)

# 9. Flowchart: Patient Appointment Booking

This flowchart shows the complete steps when a patient books an appointment.
It visualizes the whole process from logging in to confirming an appointment, showing how the user moves through the system to complete the task.

http://googleusercontent.com/image_generation_content/0
