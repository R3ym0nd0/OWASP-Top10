# Chat Transcript Disclosure

## Lab Overview

This lab demonstrates an **Insecure Direct Object Reference (IDOR)** vulnerability where chat transcripts are stored as static files and retrieved using predictable URLs.

The goal of the lab is to **obtain the password** of user `carlos` by accessing an unauthorized chat transcript.

<img width="1366" height="768" alt="labScreennshot" src="https://github.com/user-attachments/assets/552bdd94-b7e8-4645-b76c-6b4a9f6b2994" />

You can read more about IDOR and related labs here: 
[PortSwigger – IDOR](https://portswigger.net/web-security/access-control/idor)

---

## How I Solved the Lab

### Step 1: Access Live Chat Without Login
I accessed the Live Chat feature without logging in and sent a message to create a chat transcript.  
This indicates that the Live Chat functionality is accessible without authentication, which increases the overall risk.

<img width="1366" height="768" alt="1stStep" src="https://github.com/user-attachments/assets/928557d0-608c-4b05-9c0d-4fa74f1222a7" />

---
