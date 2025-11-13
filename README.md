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

## Setup and Installation

To run this project, you will need to set up your own Firebase and Razorpay accounts.

### Prerequisites:

Node.js (which includes npm)

A Firebase Account (free "Spark" plan)

A Razorpay Account (free "Test Mode" access)

#### Step 1: Get the Code

Clone this repository to your local machine:

git clone [https://github.com/D-D-Aditya/samarthan-gateway.git](https://github.com/D-D-Aditya/samarthan-gateway.git)
cd samarthan-gateway


#### Step 2: Firebase Setup

Create a Project: Go to the Firebase Console and create a new project.

Get Config Keys: In your project settings, add a new "Web App". Firebase will give you a firebaseConfig object.

Paste Keys: Open index.html and admin_dashboard.html. Find the // TODO: PASTE YOUR FIREBASE CONFIGURATION HERE section and paste your firebaseConfig object.

Enable Firestore: In the Firebase console, go to "Firestore Database" and create a new database. Start in "test mode" for now.

Enable Authentication: Go to "Authentication," click "Sign-in method," and enable Email/Password.

Create Admin User: In the "Authentication" > "Users" tab, manually add your first admin user (your email and a password). This is how you will log in to the admin dashboard.

#### Step 3: Razorpay Setup

Log in to your Razorpay Dashboard.

Make sure you are in Test Mode.

Go to Settings > API Keys and generate a new key.

Copy the Key ID (it looks like rzp_test_...).

Open index.html, find the // TODO: PASTE YOUR RAZORPAY KEY ID HERE section, and paste your Key ID.

#### Step 4: Set Firestore Security Rules

This is the most important step for security. In your Firebase Firestore "Rules" tab, paste the following rules and Publish them.
`````
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
`````


#### Step 5: How to Deploy Your Website to Firebase Hosting

This guide will walk you through the entire process of taking your index.html and admin_dashboard.html files and putting them on a live, secure website.

**Step A: Install the Necessary Tools**

Before you can deploy, you need to install the tools that allow your computer to talk to Firebase.

Install Node.js: If you don't already have it, go to the official Node.js website and download the "LTS" (Long-Term Support) version. This package also includes npm (Node Package Manager), which you will need.

Install Firebase CLI: Once Node.js is installed, open your computer's command prompt (CMD or PowerShell on Windows, Terminal on Mac/Linux) and run the following command. This will install the Firebase Command Line Interface (CLI) globally on your machine.

`````
npm install -g firebase-tools
`````

**Step B: Organize Your Project Files**

This is the most important step to avoid the errors.

Create a new, empty folder on your computer. Name it something clear, like samarthan-gateway-project.

Inside this folder, create another folder and name it exactly public.

Move your index.html and admin_dashboard.html files inside the public folder.

Your final folder structure must look like this:

`````
samarthan-gateway-project/
└── public/
    ├── index.html
    └── admin_dashboard.html
`````

**Step C: Connect Your Project to Firebase**

Now, we'll use the command line to link your project folder to your Firebase account.

Open your command prompt or terminal.

Navigate to your main project folder. Use the cd (change directory) command. For example, if your folder is on your Desktop, you might type:

`````
cd Desktop/samarthan-gateway-project
`````

Log in to Firebase. Run this command. A browser window will open, asking you to log in to your Google account and grant permissions.

`````
firebase login
`````

Initialize the Firebase project. This is the main setup step. Run the following command inside your samarthan-gateway-project folder:

`````
firebase init
`````

You will be asked a series of questions. Answer them exactly like this:

"Are you ready to proceed? (Y/n)"

Type Y and press Enter.

"Which Firebase features do you want to set up?"

Use the arrow keys to move down to "Hosting: Configure files for Firebase Hosting..."

Press the Spacebar to select it.

Press Enter.

"Please select an option:"

Choose "Use an existing project" and press Enter.

"Select a default Firebase project:"

A list of your Firebase projects will appear. Use the arrow keys to select your "Samarthan Gateway" project and press Enter.

"What do you want to use as your public directory?"

This is the most critical question. Type public and press Enter. (This tells Firebase that your website files are in the "public" folder you created in Step 2).

"Configure as a single-page app (rewrite all urls to /index.html)?"

Type N and press Enter. (This is important, otherwise your admin_dashboard.html page won't work).

"Set up automatic builds and deploys with GitHub?"

Type N and press Enter.

Firebase will now create two new files in your samarthan-gateway-project folder (.firebaserc and firebase.json). Your project is now successfully initialized!

**Step D: Deploy Your Website**

You are now ready to put your website on the internet.

In your command prompt (make sure you are still in the samarthan-gateway-project folder), run this one simple command:

`````
firebase deploy
`````

Firebase will start uploading your files. After a few seconds, it will be complete. You will see a "Deploy complete!" message and, most importantly, a Hosting URL. It will look something like https://samarthangateway.web.app. (url maybe different for you)

Project Context

This project was developed as part of the B.Sc. Computer Science (NEP) curriculum for a Field Project / Case Study. The "Case Study" was conducted with the Animal Welfare Association Gondia (AWA) to understand the real-world challenges faced by small NGOs in the digital age.
