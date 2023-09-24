*Technical WorkFlow*
---
**EMPLOYER WORKFLOW:**

**1. Registration (React Web Interface):**
   - React frontend sends POST request with user details to Node.js backend.
   - Backend processes data and stores it in the database.
   - Return success or error response to frontend.

**2. Login (React Web Interface):**
   - React frontend sends POST request with login credentials to Node.js backend.
   - Backend verifies credentials against database.
   - Return JWT token or equivalent to frontend for authenticated sessions.

**3. Contract Creation (React Web Interface):**
   - React frontend provides form interface for contract details.
   - On submission, sends POST request with contract data to Node.js backend.
   - Backend stores contract data in the database.
   
**4. Generate Employee Link (Node.js Backend):**
   - Backend generates a unique link, potentially with a token or ID referencing the contract.
   - Frontend displays the link for the employer to share.

**5. Confirmation of Contract Acceptance (React Web Interface & Node.js Backend):**
   - Backend sends a webhook or notification when a contract is accepted by the employee.
   - React frontend displays a confirmation to the employer.

**6. Invoice Management (React Web Interface & SolanaPay JS SDK):**
   - React frontend lists invoices, fetching data from the backend.
   - For payment, frontend integrates with SolanaPay JS SDK to generate Solana Pay links for each invoice.

---

**EMPLOYEE WORKFLOW:**

**1. Contract Review via Generated Link (React Web Interface):**
   - Employee clicks the link, which loads the contract details from the Node.js backend.
   - React frontend displays the contract for review.

**2. Contract Acceptance (React Web Interface):**
  
a. If not registered:
- Employee navigates to the registration page and fills in the required details.
- React frontend sends a POST request with registration details to the Node.js backend.
- Backend processes the data and stores it in the database.

b. If already registered or after registration:
- Employee logs in using the username and password.
- Upon successful login, employee is presented with the contract acceptance option.

c. Contract Acceptance:
- Upon agreement, employee clicks "Accept". The action sends a POST request to the Node.js backend to update contract status.

**3. Mobile App Activities (FlutterFlow Mobile Interface):**

   **a. Login to the Mobile App:**
      - FlutterFlow interface sends a POST request with login details to Node.js backend.
      - Backend verifies and returns a JWT token or equivalent.

   **b. Invoice Generation (FlutterFlow & SolanaPay JS SDK):**
      - FlutterFlow provides an interface for generating invoices.
      - On confirmation, it integrates with SolanaPay JS SDK to generate the Solana Pay link for the invoice.

**4. Monitoring Payments (FlutterFlow Mobile Interface):**
   - FlutterFlow fetches invoice data from the Node.js backend.
   - Employees can monitor payment statuses through the app.

---
**Backend API Endpoints (Node.js):**
Certainly! Let's expand on the endpoints to be more comprehensive and cover all potential requirements:

**User Management:**

1. `POST /register`
   - Payload: `{ username, password, email, userType (employer/employee), solanaWalletAddress }`
   - Description: Registers a new user (both employers and employees).

2. `POST /login`
   - Payload: `{ username, password }`
   - Description: Logs in a user and returns an authentication token or session.

3. `GET /user/:userId`
   - Description: Retrieves the details of a specific user.

4. `PUT /user/:userId`
   - Payload: `{ ...updatedDetails }`
   - Description: Updates user details.

**Contract Management:**

5. `POST /createContract`
   - Payload: `{ employerId, contractName, scopeOfWork, startDate, currency, frequencyType (monthly/weekly), payAmount, terminationDays }`
   - Description: Adds a new contract.

6. `GET /getContract/:id`
   - Description: Fetches a specific contract's details.

7. `GET /listContracts/:employerId`
   - Description: Lists all contracts created by a specific employer.

8. `PUT /updateContract/:id`
   - Payload: `{ ...updatedContractDetails }`
   - Description: Updates details of a specific contract.

9. `POST /acceptContract`
   - Payload: `{ contractId, employeeId }`
   - Description: Marks a contract as accepted by an employee.

**Invoice Management:**

10. `POST /generateInvoice`
    - Payload: `{ userId, contractId }`
    - Description: Creates a new invoice and returns a Solana pay link.

11. `GET /listInvoices/:userId`
    - Description: Fetches all invoices associated with a user (either created by or for them).

12. `GET /getInvoice/:invoiceId`
    - Description: Retrieves details of a specific invoice.

13. `PUT /updateInvoice/:invoiceId`
    - Payload: `{ ...updatedInvoiceDetails }`
    - Description: Updates details of a specific invoice.

14. `DELETE /deleteInvoice/:invoiceId`
    - Description: Deletes a specific invoice.

**Payment Management**

15. `GET /listPayments/:userId`
    - Description: Lists all payments made by or for a specific user.

16. `GET /getPayment/:paymentId`
    - Description: Retrieves details of a specific payment.

---

**React Routes (React Web Interface):**

1. **User Management:**
  
   - **`/register`**
     - View: Registration page.
     - Purpose: Allows users (employer/employee) to register.

   - **`/login`**
     - View: Login page.
     - Purpose: Allows users to log in.

   - **`/profile/:userId`**
     - View: User's profile page.
     - Purpose: Displays user details and allows for edits. 

2. **Contract Management:**

   - **`/contracts/create`**
     - View: Contract creation page.
     - Purpose: Allows employers to create a new contract.

   - **`/contracts/:contractId`**
     - View: Specific contract details page.
     - Purpose: Displays contract details and allows the employee to review and accept.

   - **`/contracts/list`**
     - View: Contracts listing page.
     - Purpose: Displays all the contracts created by the logged-in employer or assigned to the logged-in employee.

3. **Invoice Management:**
   
   - **`/invoices/:invoiceId`**
     - View: Specific invoice details page.
     - Purpose: Displays invoice details and the associated Solana Pay link (in QR form).

   - **`/invoices/list`**
     - View: Invoices listing page.
     - Purpose: Displays all the invoices created by or for the logged-in user.

4. **Payment Management:**

   - **`/payments/list`**
     - View: Payments listing page.
     - Purpose: Displays all payments made by or for the logged-in user.

   - **`/payments/:paymentId`**
     - View: Specific payment details page.
     - Purpose: Displays details of a specific payment.

5. **Dashboard/Home:**

   - **`/dashboard`** or **`/`**
     - View: Dashboard or Home page.
     - Purpose: Overview of user activities, including recent contracts, invoices, and payments.

6. **Miscellaneous:**

   - **`/settings`**
     - View: User settings page.
     - Purpose: Allows users to modify account settings.

   - **`/help`**
     - View: Help and FAQ page.
     - Purpose: Provides guidance and answers common questions.

   - **`/about`**
     - View: About page.
     - Purpose: Provides information about the platform, its purpose, and creators.

   - **`/contact`**
     - View: Contact page.
     - Purpose: Allows users to contact support or the platform administrators.

---
