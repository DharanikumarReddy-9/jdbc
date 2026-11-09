# Sumpositive Employee Onboarding System (Web & Backend)

Full-stack individual development project adhering strictly to Sumpositive Technical Standards (Java 21, Spring Boot, Spring Security JWT, Spring Data JPA, PostgreSQL, React + TypeScript + Vite).

---

## 1. Project Architecture & Isolation Model
- **Backend**: Spring Boot 3.3.0 REST API running on Java 21. Uses stateless JWT authentication.
- **Frontend**: React 18 + TypeScript + Vite client.
- **Database**: PostgreSQL with developer-designed schema enforcing relational integrity and strict user isolation.
- **Security**: Endpoints check `SecurityContextHolder` populated by `JwtAuthenticationFilter`. A user can only read and mutate their own onboarding records (`user_id` enforced on every service query).

---

## 2. PostgreSQL Database Setup
1. Create a local PostgreSQL database:
   ```sql
   CREATE DATABASE sumpositive_db;
   ```
2. Execute the schema definitions found in `schema.sql`:
   ```bash
   psql -U postgres -d sumpositive_db -f schema.sql
   ```

---

## 3. Running Backend (Spring Boot)
1. Configure credentials in `backend/src/main/resources/application.properties` (or export environment variables `SPRING_DATASOURCE_USERNAME` and `SPRING_DATASOURCE_PASSWORD`).
2. Run from Eclipse or terminal:
   ```bash
   cd backend
   mvn clean spring-boot:run
   ```
3. The server runs at `http://localhost:8080`.
   - Swagger / OpenAPI: `http://localhost:8080/swagger-ui/index.html`

---

## 4. Running Web Frontend (React + TS + Vite)
1. Navigate to the frontend directory:
   ```bash
   cd frontend
   npm install
   npm run dev
   ```
2. Open `http://localhost:5173` in your browser.

---

## 5. End-to-End Onboarding Flow
1. **Registration / Login**: Register a user (`/api/auth/register`). The backend saves BCrypt-hashed password and seeds 5 steps and 3 checklist items.
2. **Step 1 (Orientation)**: Candidate reads guidelines and acknowledges (`/api/onboarding/acknowledge-step`).
3. **Step 2 (Personal Details)**: Candidate submits phone, DOB, address, and emergency contact details (`/api/onboarding/personal-details`).
4. **Step 3 (Company Policies)**: Candidate signs policy compliance (`/api/onboarding/acknowledge-step`).
5. **Step 4 (Joining Checklist)**: Candidate toggles ID card verification, laptop setup, and bank account setup (`/api/onboarding/checklist`). Step 4 marks complete when all 3 are completed.
6. **Step 5 (Final Confirmation)**: Candidate submits final onboarding (`/api/onboarding/complete`). The backend validates that all 4 prior steps are finished before allowing 100% completion.
