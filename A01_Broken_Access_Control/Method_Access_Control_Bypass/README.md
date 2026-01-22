# Method Access Control Bypass

## Definition

Method Access Control Bypass is a vulnerability that happens when an application lets users **call restricted methods, functions, or endpoints without checking if they are allowed to perform the action**.

This usually occurs when access control is enforced only on the **frontend**, but missing on the **server-side**.

---

## Root Cause

- No proper authorization check on the server
- Access control enforced only on frontend
- Application trusts HTTP methods (GET, POST, PUT, DELETE)
- Application assumes users will not access hidden endpoints
- Role or permission checks are missing or incomplete

## Common Restricted Methods

- createUser
- deleteUser
- updateRole
- approveRequest
- resetPassword
- cancelOrder
- refundPayment

---

## Basic Example

Request:

    POST /api/user/update-role
    Content-Type: application/json
    
    {
      "user_id": 1002,
      "role": "admin"
    }


Modified request:

    GET /api/user/update-role?user_id=1002&role=admin


If the application processes the request successfully, it is vulnerable to **Method Access Control Bypass**.

---

## Common Method Access Control Bypass Scenarios

### Read Access

- Access admin dashboards
- View restricted system data
- Read internal reports

### Write Access

- Modify user roles
- Change system settings
- Update other users’ data

### Delete Access

- Delete user accounts
- Remove system records
- Cancel or refund transactions
  
---

## Testing Methodology

1. Capture requests using Burp Suite
2. Identify sensitive or restricted actions
3. Look for methods and endpoints used by admins
4. Change:
   - HTTP methods (POST → GET, PUT, DELETE)
   - Request body → query parameters
5. Replay the request manually
6. Check the server response
7. Test with different user roles if available

---

## False Sense of Security

- Hiding buttons does NOT secure backend methods
- Disabled features in UI do NOT disable APIs
- HTTP method restrictions alone are NOT enough
- Client-side role checks are NOT enough

**Authorization must always be enforced on the server.**

---

## Prevention and Mitigation

### Server-Side Authorization

- Check user permissions on every request
- Enforce access control per method

### Allow Only Required HTTP Methods

- Explicitly define allowed methods per endpoint
- Reject unexpected HTTP methods

### Deny by Default

- Block access unless explicitly allowed
- Protect all sensitive endpoints

### Logging and Monitoring

- Log method access attempts
- Detect unauthorized action attempts

---

## Key Takeaways

- Method Access Control Bypass targets **methods and actions**
- Frontend restrictions are NOT security
- Hidden endpoints can still be accessed
- Changing HTTP methods can bypass controls
- Always enforce authorization on the backend
