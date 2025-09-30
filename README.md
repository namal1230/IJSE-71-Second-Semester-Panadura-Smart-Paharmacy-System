# If you can't open this folders please refer this 
Back-End(SpringBoot) - <a href="https://github.com/namal1230/Spring-Back-End.git">Link For SpringBoot Repository</a>
Front-End(Angular) -  <a href="https://github.com/namal1230/SPM-System-Front-End.git">Link For Angular Repository</a>

<img width="1351" height="716" alt="Screenshot 2025-09-21 204723" src="https://github.com/user-attachments/assets/7a0a56d2-b5ca-48ff-ae73-a2b5c1103c38" />
<img width="582" height="282" alt="Screenshot 2025-09-21 204842" src="https://github.com/user-attachments/assets/dbd8d684-44f7-41da-b00e-7d87e99b2041" />
<img width="1365" height="713" alt="Screenshot 2025-09-21 204912" src="https://github.com/user-attachments/assets/6cb4d0c4-634e-4319-a64d-fedf125e7c01" />
<img width="1347" height="589" alt="Screenshot 2025-09-21 205037" src="https://github.com/user-attachments/assets/e2cba5a1-6416-4099-b6ed-5c46ce627ab9" />
<img width="1347" height="642" alt="Screenshot 2025-09-24 105435" src="https://github.com/user-attachments/assets/adcde940-7134-4587-8a57-1ce0bd21e194" />

# Pharmacy Management System — Youtube Link
https://youtu.be/ec5TVxN1Vfk?si=XUls1A0Ri8eCMC6p

# Pharmacy Management System — README

> **Full-stack** Pharmacy Management System
>
> Frontend: **Angular**
> Backend: **Spring Boot** + **Spring Security**
> Extras: **Google OAuth2**, **Spring AI**, **Spring Mail (Gmail SMTP)**, **Logs management (AOP + Logback)**, **Payment Integration (Stripe / PayHere)**

---

## Table of Contents

1. Project summary (TL;DR)
2. Features
3. Tech stack
4. Prerequisites
5. Quick start (local)
6. Backend (Spring Boot) — step-by-step

   * Configure database
   * Configure OAuth2 (Google)
   * Configure Spring Mail (Gmail)
   * Configure Spring AI
   * Configure Payments (Stripe example)
   * Logs management (AOP + Logback)
   * Build & run
7. Frontend (Angular) — step-by-step

   * Configure environment
   * Run development server
   * Build for production
8. Database setup & seed data
9. Docker (optional) — quick example
10. Testing
11. API docs / Postman
12. Common issues & troubleshooting
13. Environment variables / config checklist
14. Deployment notes
15. Contributing & contact

---

## 1. Project summary (TL;DR)

This project is a Pharmacy Management System that lets users search for medicines, receive notifications, and pay for orders online. The backend is built with Spring Boot (secured with Spring Security and Google OAuth2). Spring AI helps with recommendations and Spring Mail sends email notifications. Logs are collected through an AOP-based logging aspect and Logback. Payments are supported (example integration with Stripe; PayHere can be used for Sri Lanka).

## 2. Features

* User registration and Google sign-in (OAuth2)
* Role-based access control (USER, ADMIN)
* Medicine search and pharmacy listings
* Smart recommendations via Spring AI
* Email notifications (Spring Mail / Gmail)
* Online payment (Stripe or PayHere)
* Admin dashboard for inventory and user management
* Logs management for audit and debugging

## 3. Tech stack

* Frontend: Angular (TypeScript)
* Backend: Spring Boot, Spring Data JPA
* Security: Spring Security, OAuth2 (Google)
* Email: Spring Mail (SMTP)
* AI: Spring AI (placeholder/integration layer)
* Logging: SLF4J + Logback + Spring AOP Aspect
* Database: MySQL / PostgreSQL (configurable)
* Payment: Stripe (demo) / PayHere (Sri Lanka)
* Build: Maven (backend), npm/Angular CLI (frontend)

## 4. Prerequisites

* Java 17 (LTS) or later
* Maven 3.6+
* Node.js 18+ and npm (or yarn)
* Angular CLI (if you want to run `ng` commands)
* MySQL 8+ or PostgreSQL 12+
* Git
* (Optional) Docker & Docker Compose
* Accounts / credentials:

  * Google Cloud project with OAuth 2.0 Client ID
  * Stripe account (for payment integration) or PayHere merchant account
  * Gmail account with App Password (or other SMTP provider) for sending mail
  * (Optional) API key for Spring AI backend/service if you use a hosted provider

## 5. Quick start (local)

1. Clone the repo:

```bash
git clone https://github.com/your-username/pharmacy-management-system.git
cd pharmacy-management-system
```

2. Backend (configure env — see section 6) and run:

```bash
cd backend
mvn clean package
mvn spring-boot:run
# or
java -jar target/pharmacy-backend-0.0.1-SNAPSHOT.jar
```

3. Frontend (configure env — see section 7) and run:

```bash
cd frontend
npm install
npm start
# or
ng serve --open
```

4. Open the UI at `http://localhost:4200` and API at `http://localhost:8080` (default)

## 6. Backend (Spring Boot) — step-by-step

### 6.1 Clone & open

```bash
cd backend
# open in your IDE (IntelliJ/Eclipse/VS Code)
```

### 6.2 Configure the database

Edit `src/main/resources/application.yml` (or `application.properties`) and set datasource values. Example (MySQL):

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/pharmacy_db?useSSL=false&serverTimezone=UTC
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

> Tip: For production, prefer `ddl-auto: validate` and use migration tools (Flyway / Liquibase).

### 6.3 Configure Google OAuth2 (Spring Security)

Create credentials in Google Cloud Console (OAuth 2.0 Client ID for Web Application):

* Authorized redirect URIs: `http://localhost:8080/login/oauth2/code/google` (if you let backend handle OAuth)

Add these properties (example `application.yml`):

```yaml
spring:
  security:
    oauth2:
      client:
        registration:
          google:
            client-id: ${GOOGLE_CLIENT_ID}
            client-secret: ${GOOGLE_CLIENT_SECRET}
            redirect-uri: '{baseUrl}/login/oauth2/code/{registrationId}'
            scope:
              - openid
              - profile
              - email
```

If your Angular app directly handles Google Sign-In, configure the frontend with the Google Client ID and send the ID token to backend for validation.

### 6.4 Configure Spring Mail (Gmail SMTP)

Best option with Gmail: enable 2FA and create an **App Password** for the project, then use it as `spring.mail.password`. Example properties:

```properties
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=${SPRING_MAIL_USERNAME} # your Gmail address
spring.mail.password=${SPRING_MAIL_PASSWORD} # app password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

If you prefer OAuth2 for mail sending, configure OAuth2 for Gmail SMTP — more secure for production.

### 6.5 Configure Spring AI (integration placeholder)

If you use a hosted AI provider, store the API key in env and create a Spring service to call it. Example properties:

```properties
spring.ai.api-key=${SPRING_AI_API_KEY}
spring.ai.endpoint=${SPRING_AI_ENDPOINT:https://api.example-ai.com}
```

Create a lightweight service `AiRecommendationService` that calls the provider and returns suggestions.

### 6.6 Configure Payment (Stripe example)

Add Stripe secret key to env:

```properties
stripe.api.key=${STRIPE_SECRET_KEY}
stripe.webhook.secret=${STRIPE_WEBHOOK_SECRET}
```

Backend example (controller):

```java
@PostMapping("/payments/create")
public ResponseEntity<Map<String, Object>> createPayment(@RequestBody Map<String, Object> payload) {
  long amount = Long.parseLong(payload.get("amount").toString());
  PaymentIntentCreateParams params = PaymentIntentCreateParams.builder()
      .setAmount(amount)
      .setCurrency("lkr")
      .build();

  Stripe.apiKey = stripeApiKey; // set from config
  PaymentIntent intent = PaymentIntent.create(params);
  return ResponseEntity.ok(Map.of("clientSecret", intent.getClientSecret()));
}
```

**Webhooks**: Configure `POST /api/v1/payments/webhook` to receive events (payment succeeded, failed) and verify using the webhook secret.

If you prefer PayHere (Sri Lanka), configure the merchant ID, secret, and IPN/webhook endpoint accordingly.

### 6.7 Logs management (AOP + Logback)

Use SLF4J + Logback as logging framework. Add a logging aspect to capture entry/exit of service methods and important events.

**Sample Aspect** (`LoggingAspect.java`):

```java
@Aspect
@Component
public class LoggingAspect {
  private final Logger logger = LoggerFactory.getLogger(this.getClass());

  @Pointcut("within(com.example.pharmacy..*)")
  public void appPackagePointcut() {}

  @Around("appPackagePointcut()")
  public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
    logger.info("Enter: {}.{}() with args = {}",
      joinPoint.getSignature().getDeclaringTypeName(),
      joinPoint.getSignature().getName(),
      Arrays.toString(joinPoint.getArgs()));

    Object result = joinPoint.proceed();

    logger.info("Exit: {}.{}() with result = {}",
      joinPoint.getSignature().getDeclaringTypeName(),
      joinPoint.getSignature().getName(),
      result);

    return result;
  }
}
```

**logback-spring.xml** (simple rolling file + console):

```xml
<configuration>
  <include resource="org/springframework/boot/logging/logback/defaults.xml"/>
  <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">...</appender>
  <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">...
  </appender>

  <root level="INFO">
    <appender-ref ref="CONSOLE"/>
    <appender-ref ref="FILE"/>
  </root>
</configuration>
```

Logs can be shipped to external systems (ELK, Splunk) later for monitoring.

### 6.8 Build & run

```bash
mvn clean package
mvn spring-boot:run
# or
java -jar target/pharmacy-backend-*.jar
```

## 7. Frontend (Angular) — step-by-step

### 7.1 Open & install

```bash
cd frontend
npm install
```

### 7.2 Configure environment

Edit `src/environments/environment.ts` and `environment.prod.ts` with values:

```ts
export const environment = {
  production: false,
  apiBaseUrl: 'http://localhost:8080/api',
  googleClientId: 'YOUR_GOOGLE_CLIENT_ID',
  stripePublishableKey: 'pk_test_...'
};
```

If using backend-driven OAuth (recommended), the frontend will redirect users to the backend login endpoint. If using client-side Google SignIn, use the `googleClientId` and transfer the id\_token to the backend for verification.

### 7.3 Run dev server

```bash
npm start
# or
ng serve --open
```

Visit `http://localhost:4200`.

### 7.4 Build for production

```bash
ng build --configuration production
# Copy built files to your server or serve via nginx
```

## 8. Database setup & seed data

Create DB and user (MySQL example):

```sql
CREATE DATABASE pharmacy_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'pharma_user'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON pharmacy_db.* TO 'pharma_user'@'localhost';
FLUSH PRIVILEGES;
```

Optionally include `data.sql`/`schema.sql` or use Flyway migrations for schema and seed data. Place migrations in `src/main/resources/db/migration` if using Flyway.

## 9. Docker (optional) — quick example

`docker-compose.yml` sketch:

```yaml
version: '3.8'
services:
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: pharmacy_db
      MYSQL_USER: pharma_user
      MYSQL_PASSWORD: secure_password
    ports:
      - "3306:3306"
    volumes:
      - db-data:/var/lib/mysql

  backend:
    build: ./backend
    ports:
      - "8080:8080"
    environment:
      DB_USERNAME: pharma_user
      DB_PASSWORD: secure_password
      # other envs
    depends_on:
      - db

  frontend:
    build: ./frontend
    ports:
      - "4200:80"

volumes:
  db-data:
```

## 10. Testing

* Backend unit & integration tests: `mvn test`
* Frontend unit tests: `ng test`
* End-to-end: `ng e2e` or use Cypress / Playwright

## 11. API docs / Postman

* If you include `springdoc-openapi-ui`, you can access Swagger UI at `http://localhost:8080/swagger-ui.html`.
* Optionally add a Postman collection (export and include it in `/docs/postman_collection.json`).

## 12. Common issues & troubleshooting

* **CORS errors (Angular ↔ Backend):** ensure `@CrossOrigin` or a global CORS config in Spring Boot allows `http://localhost:4200`.
* **OAuth redirect\_mismatch:** the redirect URI set in Google Cloud Console must match the one your app sends.
* **Gmail auth failed:** if you get `535` or authentication errors, use an **App Password** or configure OAuth2 for SMTP.
* **Stripe webhook signature mismatch:** ensure you use the correct webhook secret and raw request body when verifying signature.
* **Database connection errors:** check JDBC URL, user, and that the DB accepts connections from your host.
* **Port conflicts:** default ports are 8080 (backend) and 4200 (frontend); change as needed.

## 13. Environment variables / config checklist

Create a `.env` (or use your host config) with the following keys:

```
# Database
DB_URL=jdbc:mysql://localhost:3306/pharmacy_db
DB_USERNAME=pharma_user
DB_PASSWORD=secure_password

# Google OAuth2
GOOGLE_CLIENT_ID=...
GOOGLE_CLIENT_SECRET=...

# Spring Mail (Gmail)
SPRING_MAIL_USERNAME=your.email@gmail.com
SPRING_MAIL_PASSWORD=app_password_or_oauth_token

# Spring AI
SPRING_AI_API_KEY=...
SPRING_AI_ENDPOINT=...

# Stripe (payments)
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# App
APP_BASE_URL=http://localhost:8080
FRONTEND_BASE_URL=http://localhost:4200
```

## 14. Deployment notes

* Use HTTPS in production and secure your client secrets (do NOT commit `.env` or `application.yml` with secrets).
* Set `spring.jpa.hibernate.ddl-auto=validate` and use a migration tool (Flyway/Liquibase).
* Use a production SMTP provider (SendGrid, Mailgun) or configure Gmail with OAuth2.
* Rotate keys and use a secrets manager (Vault, AWS Secrets Manager).
* Consider containerizing and using kubernetes for scaling.

## 15. Contributing & contact

* Fork the repository and create a feature branch for changes.
* Write tests for changes and keep the code style consistent.

---
