# 📚 Library Management System

> A full-stack library management web application developed collaboratively as a CA2 project at Republic Polytechnic.

Built with **Node.js, Express.js, EJS, MySQL, and Bootstrap**, the system provides separate Admin and User experiences for managing books, publishers, borrowing workflows, profiles, and access permissions.

---

## 🔗 Project Links

- 💻 **GitHub Repository:** [View Source Code](https://github.com/MargaretPabustan/Library-Management-System)
- 🎥 **Demo Video:** [Watch Project Demo](https://drive.google.com/drive/folders/1d8AHHRZ4vZE0HREq-y-GdGHiKwQpN_iK?usp=sharing)
- 🌐 **Live Demo:** [View Live Application](https://library-management-system-6-ixpj.onrender.com)

---

## 📁 Project Structure

```text
library-1/
├── app.js
├── package.json
├── package-lock.json
├── public/
│   └── images/
└── views/
    ├── addBook.ejs
    ├── addPublisher.ejs
    ├── book.ejs
    ├── cart.ejs
    ├── checkout-success.ejs
    ├── homepage.ejs
    ├── login.ejs
    ├── profile.ejs
    ├── publishers.ejs
    ├── register.ejs
    ├── updateBook.ejs
    ├── updatePublisher.ejs
    └── ...
```

---

## 🧪 Sample Routes

| Method | Route | Description |
|---|---|---|
| GET | `/library` | View available books |
| GET | `/publishers` | View publishers |
| POST | `/addPublisher` | Add a publisher |
| POST | `/login` | Authenticate a user |

---

## ✨ Key Features

### 🧾 Book Management — Sahana

- Full CRUD functionality with RBAC
- Book image uploads
- Book search functionality
- Stock and availability handling

### 🏷️ Publisher Management — Margaret

- Create, view, update, and delete publishers
- Search publishers by name, address, or country
- View publisher information associated with books
- Apply **role-based access control (RBAC)** to publisher management
- Restrict publisher modifications to administrators

### 🔐 User Authentication & RBAC — Yi Xi

- User registration, login, and logout
- Session-based authentication
- **Role-based access control (RBAC)** for Admin and User accounts
- `checkAuthenticated` middleware for protected routes
- `checkAdmin` middleware for administrative operations
- Role-based access restrictions

### 👤 User Profile — Yi Xi

- View personal profile information
- Update profile details
- View borrowing information

### 📦 Borrow Cart & Loans — Sahana / Samuel

- Add books to a borrowing cart
- Review selected books before checkout
- Simulate the borrowing process
- Display checkout confirmation
- View borrowed books
- Return borrowed books
- Track loan information

### 📋 Admin Dashboard

- Manage users, books, and publishers
- Access administrative-only functionality
- Perform CRUD operations within the library catalogue

---

## 🧰 Tech Stack

| Layer | Technology |
|---|---|
| Backend | Node.js, Express.js |
| Frontend | HTML, CSS, Bootstrap, EJS |
| Database | MySQL |
| Authentication | express-session |
| File Upload | Multer |
| Database Driver | mysql2 |
| Other | connect-flash, body-parser |

---

## 🏗️ System Architecture

```text
Client
(Browser / EJS Views)
        ↓
Express.js
(Route Handlers + Business Logic)
        ↓
MySQL Database
```

The application follows a **monolithic Express.js architecture** using **server-side rendering with EJS**.

---

## ⚙️ How the System Works

- **EJS** renders the server-side interface.
- **Express.js** handles routing and application logic.
- **Middleware & sessions** manage authentication and role-based access.
- **MySQL** stores application data and supports CRUD operations.
- **Bootstrap** provides a responsive frontend.

### Key Design Characteristics

- Server-side rendered **EJS** application
- **Session-based authentication & RBAC**
- **Middleware-protected routes**
- **MySQL relational database**
- **CRUD-based system modules**

---

## 👥 Team Contributions & System Scope

This project was developed collaboratively as a **CA2 full-stack web application**, with each team member responsible for different system modules.

### 📚 Module Ownership

#### Books Page — Sahana

- Developed full CRUD functionality for books
- Implemented book image uploads
- Implemented book search functionality
- Handled stock and availability logic
- Integrated RBAC into book management

#### 🔐 Login, Registration & Users — Yi Xi

- Developed user registration and authentication
- Implemented user management and CRUD functionality
- Implemented session-based authentication
- Developed RBAC middleware using `checkAuthenticated` and `checkAdmin`
- Controlled access according to user roles

#### 🏷️ Publisher Page — Margaret (My Contribution)

I was responsible for developing the **Publisher Management module**, including database integration, CRUD functionality, search, and role-based access control.

**Key Responsibilities:**

- Implemented full **CRUD operations** for publishers
- Integrated the module with **MySQL**
- Implemented publisher search by **name, address, and country**
- Applied **admin-only access control**
- Integrated the module with **Yi Xi's authentication and RBAC implementation**
- Implemented **server-side rendering using EJS**

**Publisher Routes — Node.js / Express.js**

```text
GET    /publishers
GET    /addPublisher
GET    /updatePublisher/:id
GET    /deletePublisher/:id
```

The Publisher module was implemented within the project's existing Express architecture, with route handling, application logic, and database queries managed through `app.js`.

#### 📦 Loan Page — Samuel

- Developed the borrowing workflow
- Implemented book return functionality
- Implemented loan tracking functionality

---

## 🔐 Authentication & Role-Based Access Control

The application uses **session-based authentication and role-based access control (RBAC)**.

Authentication and authorisation are implemented using Express middleware:

- `checkAuthenticated` verifies whether a user is logged in.
- `checkAdmin` restricts administrative operations to users with the Admin role.
- EJS conditional rendering controls the visibility of administrative actions.

This ensures that sensitive operations such as creating, updating, and deleting publishers are restricted to authorised users.

---

## 💻 Backend Implementation — Node.js / Express.js

The Publisher module uses **Express.js routing, authentication middleware, RBAC, and MySQL queries** to manage publisher records.

### Example: Protected Publisher Routes

```javascript
app.get('/addPublisher', checkAuthenticated, checkAdmin, (req, res) => {
    res.render('addPublisher', {
        user: req.session.user
    });
});

app.get('/updatePublisher/:id', checkAuthenticated, checkAdmin, (req, res) => {
    const publisher_id = req.params.id;

    pool.query(
        'SELECT * FROM publishers WHERE publisher_id = ?',
        [publisher_id],
        (error, results) => {
            if (error) {
                return res.status(500).send('Error retrieving publisher');
            }

            if (results.length > 0) {
                res.render('updatePublisher', {
                    publishers: results[0]
                });
            } else {
                res.status(404).send('Publisher not found');
            }
        }
    );
});

app.get('/deletePublisher/:id', checkAuthenticated, checkAdmin, (req, res) => {
    const publisher_id = req.params.id;

    pool.query(
        'DELETE FROM publishers WHERE publisher_id = ?',
        [publisher_id],
        (error) => {
            if (error) {
                return res.status(500).send('Error deleting publisher');
            }

            res.redirect('/publishers');
        }
    );
});
```

The routes demonstrate:
- Express.js routing
- Authentication middleware
- Role-based authorisation
- Route parameters
- MySQL database queries
- Error handling
- Server-side page rendering

---

## 🖥️ Frontend Implementation — EJS

Administrative controls are conditionally rendered based on the authenticated user's role.

```ejs
<% if (user && user.role === 'admin') { %>

    <th>Edit</th>
    <th>Delete</th>

<% } %>

<% if (user && user.role === 'admin') { %>

    <td>
        <a
            href="/updatePublisher/<%= publisher.publisher_id %>"
            class="btn btn-outline-primary btn-sm">
            Edit
        </a>
    </td>

    <td>
        <a
            href="/deletePublisher/<%= publisher.publisher_id %>"
            onclick="return confirm('Are you sure?')"
            class="btn btn-outline-danger btn-sm">
            Delete
        </a>
    </td>

<% } %>
```

This ensures that publisher management controls are only presented to users with the appropriate administrative role.

---

## 🗄️ Database Schema — Simplified

```sql
CREATE TABLE books (
    bookId INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    author VARCHAR(255),
    genre VARCHAR(100),
    quantity INT,
    coverImage VARCHAR(255)
);

CREATE TABLE publishers (
    publisher_id INT AUTO_INCREMENT PRIMARY KEY,
    publisher_name VARCHAR(255),
    publisher_contact VARCHAR(100),
    publisher_country VARCHAR(100),
    address TEXT
);

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255),
    email VARCHAR(255) UNIQUE,
    password VARCHAR(255),
    role ENUM('user','admin') DEFAULT 'user'
);
```

---

## 🚀 Installation

### 1. Install Dependencies

Clone the repository and install the required Node.js packages:

```bash
npm install
```

### 2. Set Up MySQL Database

Create the required MySQL database:

```text
librarydb
```

Run the provided SQL schema to create the required tables.

### 3. Configure Database Connection

Update the MySQL connection configuration in `app.js` with your own database credentials:

```javascript
const pool = mysql.createPool({
    host: 'YOUR_HOST',
    port: YOUR_PORT,
    user: 'YOUR_USER',
    password: 'YOUR_PASSWORD',
    database: 'YOUR_DATABASE',
    waitForConnections: true,
    connectionLimit: 10,
    queueLimit: 0
});
```

> ⚠️ **Security Note:** Never commit database credentials to a public repository. Use environment variables for production deployments.

### 4. Run the Application

```bash
npx nodemon app.js
```

### 5. Access Locally

```text
http://localhost:3000
```

---

## 🌐 Live Demo

- 🌐 **Live Application:** [View Live Demo](YOUR_RENDER_LINK)
- 🎥 **Demo Video:** [Watch Project Demo](YOUR_GOOGLE_DRIVE_LINK)
- 💻 **GitHub Repository:** [View Source Code](https://github.com/MargaretPabustan/Library-Management-System)

> **Note:** The live deployment may take some time to start. Running the application locally is recommended for faster testing.

### 🔐 Sample Login Credentials

#### Admin Account

```text
Email: admin6@gmail.com
Password: margaret
```

#### User Account

```text
Email: user@email.com
Password: user1234
```

> **Note:** These credentials are provided for demonstration purposes only.

---

## 📦 Dependencies

```json
{
    "express": "^5.1.0",
    "ejs": "^3.1.10",
    "mysql2": "^3.14.2",
    "express-session": "^1.18.2",
    "multer": "^2.0.2",
    "body-parser": "^2.2.0"
}
```

---

## ⚠️ Challenges & Solutions

### 1. Learning GitHub Workflow

**Challenge:**  
Initially, I was unfamiliar with GitHub workflows such as committing, pushing changes, and working within a shared repository.

**Solution:**  
I practised Git commands such as `git add`, `git commit`, and `git push`, while learning how to work with branches and keep my module synchronised with the team repository.

**Learning Outcome:**  
Improved confidence in version control and collaborative software development.

---

### 2. Routing Syntax and Logic

**Challenge:**  
Some Express.js routes initially failed due to incorrect syntax, middleware ordering, or misplaced logic.

**Solution:**  
I debugged the routes by reviewing route definitions, middleware order, request parameters, and database queries. I also used Postman to test endpoints and verify CRUD operations.

**Learning Outcome:**  
Developed a stronger understanding of Express.js routing, middleware, request handling, and backend debugging.

---

### 3. Module Integration

**Challenge:**  
Integrating the Publisher module with modules developed by **Sahana, Yi Xi, and Samuel** required consistent database fields, session information, and access-control logic.

**Solution:**  
We coordinated the database schema and authentication requirements across modules. I integrated the existing authentication and RBAC mechanisms developed by **Yi Xi** into my Publisher module to maintain consistent access control.

**Learning Outcome:**  
Strengthened my understanding of integrating independently developed modules within a shared full-stack application.

---

## 🎯 Key Learning Outcomes

Through this project, I strengthened my practical understanding of:

- Full-stack web application development
- Node.js and Express.js backend development
- CRUD operations and database integration
- MySQL relational databases
- Server-side rendering with EJS
- Authentication and **role-based access control (RBAC)**
- Express middleware
- Frontend and backend integration
- Git and GitHub collaboration
- API testing and debugging
- Collaborative software development



