# FeelDX - Materials & Furniture Assistant

## 📋 Project Overview
**FeelDX - Materials & Furniture Assistant** is an intelligent design assistant built to streamline the interior design process. The system allows users to configure a space by selecting various room types, materials, and furniture pieces. Based on these selections, the application generates real-time AI design feedback, including cost estimates, design notes, and compatibility alerts, helping users visualize and refine their room concepts effortlessly.

---

## 🛠 Technologies Used
* **Framework:** Laravel 12
* **Frontend:** Vue.js 3
* **Bridge:** Inertia.js
* **Styling:** Tailwind CSS
* **Components:**
    * `vue3-select` (for searchable selection inputs)
    * `VueSweetalert2` (for interactive notifications)

---

## 💻 System Requirements
To run this project, ensure you have the following installed:
* **PHP:** ^8.2 (Required for Laravel 12)
* **Composer:** 2.8.10
* **Node.js:** 22.17.1
* **Version Control:** Git
* **Database:** MySQL

---

## 🚀 Setup & Installation
Follow these steps to set up the application locally:

### 1. Clone the Repository
git clone https://github.com/AnthonyRyan119/FeelDX_Project.git

### 2. Install Dependencies

composer install
npm install

### 3. Environment Configuration
Copy the example environment file:

cp .env.example .env

Generate the application key:

php artisan key:generate

### 4. Database Setup

Create a new database in your MySQL server.

Update your .env file with your database credentials:

DB_CONNECTION=mysql

DB_HOST=127.0.0.1

DB_PORT=3306

DB_DATABASE=your_database_name

DB_USERNAME=your_username

DB_PASSWORD=your_password

### 5. Run Migrations

php artisan migrate

---

## 🏃 How to Run Locally

Open two terminal windows to host the project:

Terminal 1 (Backend):

php artisan serve

Terminal 2 (Frontend):

npm run dev

The application will typically be accessible at http://127.0.0.1:8000.

---

## 🧪 Testing Instructions

User Access: * Run php artisan db:seed to generate a default user.

Alternatively, use the Register page to create a new account.

### Core Flow:

Log in and navigate to the Materials Assistant.

Select a Room Type to unlock the material configuration.

Choose materials and click "Generate AI Summary" to see live feedback.

Test the Save and Delete features for recent selections.

---

## 📝 Assumptions & Limitations
AI Logic: The "AI Response" is currently handled via a sophisticated computed logic layer that simulates design feedback based on material combinations.

Admin Side: Currently, selection values are static. A future update will include an admin panel to manage these values dynamically.

---

## ✨ Future Improvements
Admin Dashboard: Implement a full admin interface to manage the materials and furniture database.
