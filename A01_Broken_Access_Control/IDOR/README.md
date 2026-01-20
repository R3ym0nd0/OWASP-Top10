# IDOR (Insecure Direct Object Reference)

## Definition

IDOR (Insecure Direct Object Reference) is a vulnerability that happens when an application lets users access objects (like user IDs, files, or records) **without checking if they are allowed to access them**.

---

## Root Cause

- No proper authorization check on the server
- Application trusts user input
- Application assumes users will only access their own data
- Authorization done only on the frontend

---

## Common Object References

- user_id
- id
- account_id
- order_id
- file
- document_id
- invoice_id

---

## Basic Example

Request:

    GET /api/profile?id=1001


Modified request:
  
    GET /api/profile?id=1002


If the application returns another user’s data, it is vulnerable to IDOR.

---

## IDOR and Privilege Escalation

### Horizontal Privilege Escalation

- Accessing another user’s data with the same role
- This is the most common IDOR case

Example:  

    User A can view User B’s profile by changing `user_id`.

---

### Vertical Privilege Escalation

- Accessing data or features of a higher role (like admin)
- Happens when role checks are missing

Example:

    GET /admin/users?id=5


If a normal user can access this, IDOR causes vertical privilege escalation.

---

## Common IDOR Scenarios

### Read Access
- View other users’ profiles
- Read private documents
- Download invoices or reports

### Write Access
- Edit other users’ data
- Change passwords
- Modify orders

### Delete Access
- Delete user accounts
- Delete files or records

---

## Testing Methodology

1. Capture requests using Burp Suite
2. Look for object references in:
   - URL parameters
   - JSON request bodies
   - Headers
3. Change the ID values
4. Check the server response
5. Test:
   - Read access
   - Write or delete access
6. Test with different user roles if available

---

## False Sense of Security

- UUIDs do NOT fix IDOR by themselves
- Hiding IDs in frontend does NOT fix IDOR
- Client-side checks are NOT enough

**Authorization** must always be checked on the server.

---

## Prevention and Mitigation

### Server-Side Authorization
- Check if the user owns the object
- Check permissions on every request

### Indirect Object References
- Do not expose real IDs directly
- Map public values to real objects on the server

### Role-Based Access Control (RBAC)
- Enforce user and admin roles
- Protect admin endpoints

### Deny by Default
- Do not allow access unless it is explicitly allowed

### Logging and Monitoring
- Log object access
- Detect ID guessing or enumeration

---

## Key Takeaways

- IDOR happens because of missing authorization
- Login does not mean permission
- IDOR can leak sensitive data
- Horizontal and vertical privilege escalation are results of IDOR
- Always enforce access control on the server
