# PKI-Based Two-Factor Authentication Microservice 

A production-ready authentication microservice implementing Public Key Infrastructure (PKI) and Time-based One-Time Password (TOTP) two-factor authentication. This service demonstrates enterprise-grade security practices including RSA 4096-bit encryption, background job scheduling using cron, and persistent storage using Docker volumes.

---

## Table of Contents
- Features
- Architecture Overview
- Tech Stack
- Project Structure
- API Endpoints
- Configuration
- Running the Project
- Docker & Persistence
- Background Cron Job
- Security Design
- Testing
- License

---

## Features

### PKI-Based Secure Seed Exchange
- RSA 4096-bit asymmetric encryption
- Encrypted seed exchange using instructor public key
- Secure decryption using student private key

### Two-Factor Authentication (TOTP)
- Time-based OTP generation
- OTP expiration enforcement
- OTP verification support

### Background Processing
- Cron-based background execution
- Periodic OTP generation
- Persistent OTP logging

### Containerized Architecture
- Docker multi-stage build
- Isolated runtime environment
- Persistent storage across restarts

---

## Architecture Overview



Client
|
| HTTP / JSON
v
FastAPI Application
|
| Seed Decryption (RSA)
| OTP Generation & Verification
v
Cron Job
|
| OTP Logging
v
Docker Volume


---

## Tech Stack

| Component | Technology |
|---------|------------|
| Backend Framework | FastAPI |
| Language | Python 3.11 |
| Encryption | RSA 4096-bit |
| OTP | TOTP |
| Background Jobs | Linux Cron |
| Containerization | Docker, Docker Compose |
| Server | Uvicorn |

---

## Project Structure



pki-2faa/
├── app/
│   └── main.py
│
├── scripts/
│   └── log_2fa_cron.py
│
├── cron/
│   └── 2fa-cron
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── request_seed.py
├── encrypted_seed.txt
├── student_private.pem
├── student_public.pem
├── instructor_public.pem
├── .gitignore
├── .gitattributes
└── README.md



---

## API Endpoints

### Security Files Used

student_private.pem

student_public.pem

instructor_public.pem

encrypted_seed.txt

## Running the Project

### Build and Start the Service

docker-compose build --no-cache
docker-compose up -d

### Verify Running Containers
docker ps

### API Base URL
http://localhost:8080

### Docker & Persistence
Volume	Purpose
seed-data	Decrypted seed storage
cron-output	OTP log storage

Background Cron Job
Executes every minute

Generates a valid OTP

Logs output to /cron/last_code.txt

View Cron Logs

docker exec pki-2fa sh -c "cat /cron/last_code.txt"

## Security Design
No plaintext secret exposure

Strong asymmetric encryption

OTP expiration enforcement

Isolated background execution

Container-level isolation

## Testing
Decrypt Seed

curl -X POST http://localhost:8080/decrypt-seed \
-H "Content-Type: application/json" \
-d '{"encrypted_seed":"<base64-string>"}'

## Generate OTP
curl http://localhost:8080/generate-2fa

## Verify OTP
curl -X POST http://localhost:8080/verify-2fa \
-H "Content-Type: application/json" \
-d '{"code":"123456"}'

curl -X POST http://localhost:8080/verify-2fa \
-H "Content-Type: application/json" \
-d '{"code":"123456"}'
