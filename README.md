# AuthService - Expense Tracker Authentication Microservice

A secure authentication microservice built with Spring Boot, providing JWT-based authentication and authorization for the Expense Tracker application.

## 📋 Description

This microservice handles user authentication, registration, and token management using JWT (JSON Web Tokens) with refresh token support. It implements role-based access control (RBAC) and provides secure endpoints for user management.

## 🚀 Features

- User Registration (Signup)
- User Login with JWT Authentication
- Access Token & Refresh Token mechanism
- Role-based Authorization (USER, ADMIN)
- Password encryption using BCrypt
- MySQL database integration
- Spring Security configuration
- RESTful API endpoints

## 🛠️ Technologies Used

- **Java**: 21+
- **Spring Boot**: 3.5.6
- **Spring Security**: 6.x
- **Spring Data JPA**: For database operations
- **MySQL**: 8.0+ (Database)
- **JWT (JSON Web Tokens)**: For authentication
- **Lombok**: To reduce boilerplate code
- **Maven**: Build tool
- **Hibernate**: ORM framework

## 📦 Prerequisites

Before running this application, ensure you have the following installed:

1. **Java Development Kit (JDK)**: Version 21 or higher
   - Download from: https://www.oracle.com/java/technologies/downloads/

2. **MySQL Server**: Version 8.0 or higher
   - Download from: https://dev.mysql.com/downloads/mysql/

3. **Maven**: Version 3.6+ (or use the included Maven Wrapper)
   - Download from: https://maven.apache.org/download.cgi

4. **IDE** (Optional but recommended):
   - IntelliJ IDEA, Eclipse, or VS Code

## 🔧 Installation & Setup

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd AuthService
```

### Step 2: Configure MySQL Database

1. Start your MySQL server

2. Create the database (or let the application create it automatically):

```sql
CREATE DATABASE IF NOT EXISTS auth_service_db;
```

3. Update database credentials in `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/auth_service_db?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=your_mysql_password
```

**Note**: Replace `your_mysql_password` with your actual MySQL password.

### Step 3: Build the Project

Using Maven Wrapper (Windows):
```cmd
mvnw.cmd clean install
```

Using Maven Wrapper (Linux/Mac):
```bash
./mvnw clean install
```

Or using installed Maven:
```bash
mvn clean install
```

### Step 4: Run the Application

Using Maven Wrapper (Windows):
```cmd
mvnw.cmd spring-boot:run
```

Using Maven Wrapper (Linux/Mac):
```bash
./mvnw spring-boot:run
```

Or run the JAR directly:
```bash
java -jar target/AuthService-0.0.1-SNAPSHOT.jar
```

The application will start on **http://localhost:8080**

## 📡 API Endpoints

### Public Endpoints (No Authentication Required)

#### 1. User Registration
```http
POST /api/auth/v1/signup
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "password": "securePassword123",
  "roles": "USER"
}
```

**Response:**
```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "roles": "USER"
}
```

#### 2. User Login
```http
POST /api/auth/v1/login
Content-Type: application/json

{
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "550e8400-e29b-41d4-a716-446655440000"
}
```

#### 3. Refresh Access Token
```http
POST /api/auth/v1/refreshToken
Content-Type: application/json

{
  "token": "550e8400-e29b-41d4-a716-446655440000"
}
```

**Response:**
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "550e8400-e29b-41d4-a716-446655440000"
}
```

### Protected Endpoints (Require Authentication)

For protected endpoints, include the JWT token in the Authorization header:
```http
Authorization: Bearer <your-access-token>
```

## 🗂️ Project Structure

```
AuthService/
├── src/
│   ├── main/
│   │   ├── java/com/expenseTracker/AuthService/
│   │   │   ├── auth/                    # Security configurations
│   │   │   │   ├── JwtAuthFilter.java   # JWT authentication filter
│   │   │   │   ├── SecurityConfig.java  # Spring Security configuration
│   │   │   │   └── UserConfig.java      # User-related beans
│   │   │   ├── controller/              # REST Controllers
│   │   │   │   ├── AuthController.java  # Authentication endpoints
│   │   │   │   └── TokenController.java # Token management
│   │   │   ├── entities/                # JPA Entities
│   │   │   │   ├── RefreshToken.java
│   │   │   │   ├── UserInfo.java
│   │   │   │   └── UserRole.java
│   │   │   ├── model/                   # DTOs
│   │   │   │   └── UserInfoDto.java
│   │   │   ├── repository/              # Data repositories
│   │   │   │   ├── RefreshTokenRepository.java
│   │   │   │   └── UserRepository.java
│   │   │   ├── request/                 # Request DTOs
│   │   │   │   ├── AuthRequestDTO.java
│   │   │   │   └── RefreshTokenRequestDTO.java
│   │   │   ├── response/                # Response DTOs
│   │   │   │   └── JwtResponseDTO.java
│   │   │   ├── service/                 # Business logic
│   │   │   │   ├── CustomUserDetails.java
│   │   │   │   ├── JwtService.java
│   │   │   │   ├── RefreshTokenService.java
│   │   │   │   └── UserDetailsServiceImpl.java
│   │   │   └── AuthServiceApplication.java
│   │   └── resources/
│   │       └── application.properties   # Configuration file
│   └── test/                            # Test files
├── pom.xml                              # Maven configuration
└── README.md
```

## ⚙️ Configuration

Key configurations in `application.properties`:

```properties
# Application Name
spring.application.name=AuthService

# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/auth_service_db?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=root

# JPA/Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# Server Configuration
server.port=8080
```

## 🔐 Security Features

- **Password Encryption**: BCrypt hashing algorithm
- **JWT Authentication**: Stateless authentication mechanism
- **Refresh Tokens**: Long-lived tokens for obtaining new access tokens
- **Role-Based Access Control**: USER and ADMIN roles
- **CORS & CSRF**: Configurable security policies

## 🧪 Testing

Run the tests using:

```bash
mvnw.cmd test
```

Or:

```bash
mvn test
```

## 🐛 Troubleshooting

### Port 8080 already in use
If you get an error that port 8080 is already in use:

**Windows:**
```cmd
netstat -ano | findstr :8080
taskkill /PID <process-id> /F
```

**Linux/Mac:**
```bash
lsof -i :8080
kill -9 <process-id>
```

Or change the port in `application.properties`:
```properties
server.port=8081
```

### Database Connection Issues

1. Ensure MySQL server is running
2. Verify credentials in `application.properties`
3. Check if the database exists or allow auto-creation with `createDatabaseIfNotExist=true`

### JWT Token Errors

- Ensure the secret key in `JwtService.java` is properly configured
- Check token expiration times
- Verify the Authorization header format: `Bearer <token>`

## 📝 User Roles

- **USER**: Standard user role (default)
- **ADMIN**: Administrator role with elevated privileges

## 🔄 Token Expiration

- **Access Token**: 30 minutes (configurable in JwtService)
- **Refresh Token**: 7 days (configurable in RefreshTokenService)

## 👥 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is part of the Expense Tracker application.

## 📞 Support

For issues and questions, please open an issue in the repository.

## 🙏 Acknowledgments

- Spring Boot Team for the excellent framework
- Spring Security for robust security features
- JWT.io for JWT implementation resources

---

**Built with ❤️ using Spring Boot**

