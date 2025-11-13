# Samarthan-Gateway
A serverless donation gateway (B.Sc. Field Project) to upgrade small NGOs from QR codes to a full management system.

Samarthan Gateway: A Digital Donation Management System

A serverless donation management system (B.Sc. Field Project) designed to help small non-profits and NGOs upgrade from simple QR code payments to a fully functional platform with a real-time admin dashboard.

This project was built as a case study for the Animal Welfare Association Gondia (AWA), a local NGO that relied on a static QR code, which created challenges in tracking donors and managing specific fundraising campaigns.

Features

Public Donation Page: A clean, mobile-friendly page where donors can see and donate to specific, active causes.

Secure Admin Dashboard: A password-protected portal (/admin_dashboard.html) for the NGO staff.

Real-Time Analytics: The admin dashboard shows real-time stats (Total Donations, Total Raised, Unique Donors) that update instantly.

Cause Management (CRUD): Admins can easily Add, Edit, and Delete fundraising causes directly from the dashboard.

Secure Payment Processing: Integrates with the Razorpay payment gateway to securely handle UPI, card, and other payments. No sensitive financial data ever touches the server.

Serverless Architecture: Built entirely on Google Firebase, making it extremely low-cost, secure, and scalable.

Screenshots

(Add your own screenshots here after you take them)

Public Donation Page

Admin Dashboard & Analytics



Technologies Used

Frontend: HTML5, Tailwind CSS, Vanilla JavaScript (ES6+)

Backend: Google Firebase

Database: Firestore (NoSQL)

Authentication: Firebase Authentication (Email/Password)

Hosting: Firebase Hosting

Payments: Razorpay Payment Gateway (Test Mode)

Setup and Installation

To run this project, you will need to set up your own Firebase and Razorpay accounts.

Prerequisites:

Node.js (which includes npm)

A Firebase Account (free "Spark" plan)

A Razorpay Account (free "Test Mode" access)

Step 1: Get the Code

Clone this repository to your local machine:

git clone [https://github.com/D-D-Aditya/samarthan-gateway.git](https://github.com/D-D-Aditya/samarthan-gateway.git)
cd samarthan-gateway


Step 2: Firebase Setup

Create a Project: Go to the Firebase Console and create a new project.

Get Config Keys: In your project settings, add a new "Web App". Firebase will give you a firebaseConfig object.

Paste Keys: Open index.html and admin_dashboard.html. Find the // TODO: PASTE YOUR FIREBASE CONFIGURATION HERE section and paste your firebaseConfig object.

Enable Firestore: In the Firebase console, go to "Firestore Database" and create a new database. Start in "test mode" for now.

Enable Authentication: Go to "Authentication," click "Sign-in method," and enable Email/Password.

Create Admin User: In the "Authentication" > "Users" tab, manually add your first admin user (your email and a password). This is how you will log in to the admin dashboard.

Step 3: Razorpay Setup

Log in to your Razorpay Dashboard.

Make sure you are in Test Mode.

Go to Settings > API Keys and generate a new key.

Copy the Key ID (it looks like rzp_test_...).

Open index.html, find the // TODO: PASTE YOUR RAZORPAY KEY ID HERE section, and paste your Key ID.

Step 4: Set Firestore Security Rules

This is the most important step for security. In your Firebase Firestore "Rules" tab, paste the following rules and Publish them.

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Rules for the 'causes' collection
    match /causes/{causeId} {
      // Anyone can read the causes.
      allow read: if true;

      // Only a logged-in admin can create, delete, or update a cause.
      allow create, delete, update: if request.auth != null;
    }

    // Rules for the 'donations' collection
    match /donations/{donationId} {
      // Anyone can create a new donation record.
      allow create: if true;

      // Only a logged-in admin can read the donation records.
      allow read: if request.auth != null;
      
      // Nobody can ever change or delete a donation record once it's made.
      allow update, delete: if false;
    }
  }
}



Step 5: Deploy the Website

This project is set up to be deployed on Firebase Hosting.

Install Firebase Tools:

npm install -g firebase-tools


Log in:

firebase login


Initialize Firebase (in your project folder):

firebase init


Select Hosting.

Choose "Use an existing project" and select the project you created.

Set your public directory to public. (This is important, as the code files are in a folder named public).

Configure as a single-page app? No.

Deploy:

firebase deploy


Firebase will give you a live URL (e.g., https://your-project.web.app). Your site is now live!

Project Context

This project was developed as part of the B.Sc. Computer Science (NEP) curriculum for a Field Project / Case Study. The "Case Study" was conducted with the Animal Welfare Association Gondia (AWA) to understand the real-world challenges faced by small NGOs in the digital age.
