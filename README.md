# College Bus Route Management System

A full-stack DBMS mini project built with React.js, Tailwind CSS, Node.js, Express.js, and MySQL.

## Project Overview

This system includes:
- Admin module with dashboard, CRUD for buses, routes, drivers, students, stops, payments, and assignments.
- Student module to view assigned bus, route, stop times, driver details, and payment status.
- Driver module to view assigned route, bus details, student list, and update trip status.
- MySQL database with normalized tables, foreign keys, and prepared statements.
- JWT authentication and protected APIs.

## Folder Structure

- `backend/` - Node.js API server
- `frontend/` - React application
- `README.md` - Project instructions and documentation
- `backend/db.sql` - SQL schema and sample data

## Setup Instructions

### 1. Clone or Open Project

Open the `busmanagement` workspace in VS Code.

### 2. Configure MySQL

1. Install MySQL.
2. Create a database named `college_bus_management`.
3. Run the SQL script in `backend/db.sql` to create tables and sample data.

### 3. Backend Setup

```bash
cd backend
npm install
cp .env.example .env
```

Update `.env` with your MySQL credentials and a strong `JWT_SECRET`.

Start backend server:

```bash
npm run dev
```

The server will run on `http://localhost:5000`.

### 4. Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

Open the browser at the address shown by Vite, usually `http://localhost:3000`.

## Login Credentials

Use sample accounts or create your own in MySQL.

- Admin:
  - Email: `admin@example.com`
  - Password: `admin123`
- Driver:
  - Email: `driver1@example.com`
  - Password: `driver123`
- Student:
  - Email: `student1@example.com`
  - Password: `student123`

## API Documentation

### Authentication
- `POST /api/auth/login` - Login for admin, student, or driver. Request body: `{ email, password, role }`
- `POST /api/auth/register` - Create admin user (admin only).

### Admin Endpoints
- `GET /api/admin/dashboard` - Dashboard analytics
- `GET /api/admin/students` - List students
- `POST /api/admin/students` - Add student
- `PUT /api/admin/students/:id` - Update student
- `DELETE /api/admin/students/:id` - Remove student
- `GET /api/admin/drivers` - List drivers
- `POST /api/admin/drivers` - Add driver
- `GET /api/admin/buses` - List buses
- `GET /api/admin/routes` - List routes
- `GET /api/admin/payments` - List payments
- `GET /api/admin/assignments` - List assignments

### Student Endpoints
- `GET /api/students/profile` - View student profile and assigned bus
- `GET /api/students/payments` - View student payments

### Driver Endpoints
- `GET /api/drivers/profile` - View driver details
- `GET /api/drivers/students` - View assigned students
- `POST /api/drivers/trip-status` - Update trip status

### Reports
- `GET /api/reports/students-by-bus`
- `GET /api/reports/pending-fees`
- `GET /api/reports/bus-occupancy`
- `GET /api/reports/driver-assignments`

## ER Diagram Explanation

The database is normalized with the following relationships:
- `admins` manages the system.
- `students` are assigned to `buses` and `bus_stops`.
- `drivers` are assigned to `buses`.
- `buses` follow `routes`.
- `bus_stops` belong to `routes`.
- `payments` link `students` and `buses`.
- `assignments` record `student`, `bus`, and `driver` allocation.
- `trip_status` records daily bus trips with route and driver details.

## Example SQL Queries

```sql
SELECT b.bus_number, COUNT(s.id) AS student_count
FROM buses b
LEFT JOIN students s ON s.assigned_bus_id = b.id
GROUP BY b.id;

SELECT s.name, p.amount, p.status
FROM payments p
JOIN students s ON p.student_id = s.id
WHERE p.status = 'pending';

SELECT d.name, b.bus_number, r.route_name
FROM drivers d
LEFT JOIN buses b ON d.assigned_bus_id = b.id
LEFT JOIN routes r ON b.route_id = r.id;
```

## Viva Questions and Answers

1. **What is JWT and why do we use it?**
   - JWT is a JSON Web Token used for secure stateless authentication. It keeps API routes protected without storing session data on the server.
2. **How does MySQL prevent SQL injection in this project?**
   - The backend uses prepared statements via `mysql2` and `pool.execute(...)` with parameter bindings.
3. **What is the purpose of MVC architecture?**
   - MVC separates concerns: Models handle database logic, Views handle UI, and Controllers manage request routes and business flow.
4. **How is responsive design implemented?**
   - The frontend uses Tailwind CSS with responsive utility classes for mobile and desktop layouts.
5. **How do you secure password storage?**
   - Passwords are hashed with bcrypt before storing in the database.

## Notes

- This project is designed for college-level DBMS evaluation.
- Frontend and backend are separate, with clear REST API boundaries.
- The code is written for clarity and beginner-friendly understanding.
