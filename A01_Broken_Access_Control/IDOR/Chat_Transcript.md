# Chat Transcript Disclosure

## Lab Overview

This lab demonstrates an **Insecure Direct Object Reference (IDOR)** vulnerability where chat transcripts are stored as static files and retrieved using predictable URLs.

Because chat messages are saved as files and proper access checks are missing, it is possible to view messages that belong to other users.

<img width="1366" height="768" alt="labScreennshot" src="https://github.com/user-attachments/assets/552bdd94-b7e8-4645-b76c-6b4a9f6b2994" />

You can read more about IDOR and related labs here: 
[PortSwigger – IDOR](https://portswigger.net/web-security/access-control/idor)

---

## What I observed during testing

### Unauthenticated Access to Live Chat

During testing, the Live Chat feature was accessible without logging in.  
Any messages sent were stored on the server as chat transcripts, which could later be viewed through the transcript functionality.

This behavior increases the risk of unauthorized access if proper access controls are not enforced.

<img width="1366" height="768" alt="1stStep" src="https://github.com/user-attachments/assets/928557d0-608c-4b05-9c0d-4fa74f1222a7" />

### Downloadable Chat Transcripts

When using the `view transcript` feature, the application provided a downloadable text file containing the chat messages.
The filename followed a numeric pattern, suggesting that transcripts may be stored and referenced in a sequential manner.

This behavior indicates that chat transcripts could potentially be accessed directly if proper authorization checks are not enforced.

<img width="1366" height="768" alt="2ndStep" src="https://github.com/user-attachments/assets/8f5add3b-32bd-4c8d-9b7e-e6498d21e3f5" />

### Chat Transcripts Stored as Files

During testing using burpsuite, an HTTP GET request revealed a URL path related to chat transcript retrieval.
The structure of this path suggests that transcripts are stored as individual files on the server.

If access to these files is not properly restricted, this design may allow unauthorized users to retrieve previously stored chat transcripts.

<img width="1366" height="401" alt="3rd" src="https://github.com/user-attachments/assets/32dec337-2950-4168-a7a0-6921f5d4c5d9" />
