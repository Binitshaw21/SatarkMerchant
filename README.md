# 🛡️ SatarkMerchant // Zero-Hardware UPI Fraud Defense

**An event-driven AWS serverless application built for the First Commit Hackathon (WeMakeDevs × AWS Bharat Builds Tour).**

SatarkMerchant empowers India's micro-merchants to instantly audit their physical storefronts against "Rogue QR Sticker Overlays" using a military-grade, real-time cloud backend.

---

## 🚨 The Problem: The "Paper Blindspot"
Over 50 million street vendors and small merchants in India rely on printed paper QR stands to receive daily payments. **The Threat:** Scammers physically paste fake, identically sized QR stickers over the merchant's authentic code during peak hours. 
* Customers scan the code.
* The payment succeeds instantly.
* **The money goes directly to the scammer's bank account.** 

Currently, merchants have zero technical tools to verify their physical countertop stand hasn't been tampered with. They only find out they've been scammed hours later when checking their bank balance.

## 💡 The Solution
SatarkMerchant flips the camera around. Instead of customers scanning to pay, **merchants scan to audit.**
By utilizing a simple web dashboard on their smartphone, a merchant can scan their own counter QR code. The app extracts the target UPI ID (VPA) and cross-references it against an immutable **Amazon DynamoDB** database via **AWS Lambda**. 
* If the VPA matches, the UI flashes Green. 
* If a rogue overlay is detected, the UI flashes Red, and **Amazon SNS** instantly dispatches an automated threat alert.

---

## 🏗️ AWS Serverless Architecture
This project completely bypasses traditional servers, utilizing a synchronous, event-driven AWS pipeline for single-digit millisecond latency. 

**How AWS fits into this project:**
1. **AWS Amplify**: Hosts the secure, responsive frontend (`index.html` landing page & `app.html` scanner dashboard) with continuous edge deployment.
2. **Amazon API Gateway**: Provides the RESTful HTTP endpoints (`/auth` and `/scan`) utilizing direct **Lambda Proxy Integration** and strict CORS controls.
3. **AWS Lambda (Python 3.10)**: Acts as the decision engine. Two distinct microservices (`Satark-MerchantAuth` and `Satark-VerifyVPA`) parse the incoming web payloads, sanitize the UPI query strings, and execute the cryptographic logic.
4. **Amazon DynamoDB**: A NoSQL key-value store (`Satark_Merchants` table) that securely holds the baseline Merchant IDs, passwords, and authentic VPAs.
5. **Amazon SNS**: The automated incident response system. If a Lambda function detects a VPA mismatch, it immediately publishes to an SNS topic to dispatch an email/SMS fraud alert to the merchant's device.

---

## 🚀 Key Features
* **Enterprise Custom Auth:** Merchants can dynamically register their business and genuine UPI ID to generate a unique `merchant_id`. 
* **Optical Threat Scanner:** Built with `html5-qrcode` utilizing raw Web Assembly to force the environment/rear camera for high-speed edge detection.
* **Live Telemetry Terminal:** The frontend UI features a mock-terminal that logs the synchronous AWS pipeline execution in real-time.
* **Zero-Hardware Cost:** Completely web-based. Requires no IoT scanners or biometric hardware.

---

## 📂 Repository Structure
```text
satarkmerchant/
├── frontend/
│   ├── index.html       # Cyber-themed product landing page
│   └── app.html         # SPA Dashboard (Auth + Optical Scanner)
├── backend/
│   ├── lambda_auth.py   # Code for Satark-MerchantAuth Lambda
│   └── lambda_verify.py # Code for Satark-VerifyVPA Lambda
└── README.md
