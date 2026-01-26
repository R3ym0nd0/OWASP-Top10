# Header Access Control Bypass

## Definition

Header Access Control Bypass is a vulnerability that happens when an application **trusts specific HTTP headers for authorization**, allowing attackers to **spoof or modify headers to gain unauthorized access** to restricted features or endpoints.

This usually occurs when access control decisions are based on client-supplied headers instead of secure server-side checks.

## Root Cause

- Application trusts user-controlled headers
- Authorization based on headers instead of sessions or roles
- Missing server-side permission validation
- Hardcoded header checks (e.g. X-Admin: true)
- Misconfigured reverse proxies or load balancers
- Inconsistent access control between services

## Commonly Abused Headers
- X-Forwarded-For
- X-Real-IP
- X-Admin
- X-User-Role
- X-Original-URL
- X-Rewrite-URL
- X-HTTP-Method-Override
- Authorization (improper validation)
- Referer

## Basic Example

Normal request (denied):

    GET /admin/dashboard HTTP/1.1
    Host: example.com


Modified request:

    GET /admin/dashboard HTTP/1.1
    Host: example.com
    X-Admin: true


If the application grants access, it is vulnerable to Header Access Control Bypass.

## Common Header Access Control Bypass Scenarios

### IP-Based Access Control

- Bypass internal-only endpoints
- Access admin panels restricted by IP
- Access staging or debug routes
  
### Role-Based Access Control

- Gain admin privileges
- Access moderator or staff features
- Perform privileged actions
  
### Path-Based Restrictions

- Bypass /admin or /internal protections
- Access hidden backend routes
- Reach restricted APIs

## Testing Methodology

1. Capture requests using Burp Suite
2. Identify restricted pages or actions
3. Observe headers used in normal requests
4. Add or modify headers such as:

    - X-Forwarded-For
    - X-Admin
    - X-User-Role
    - X-Original-URL

5. Replay the request manually
6. Analyze the server response
7. Test with different user roles if available

## False Sense of Security

- IP-based checks can be spoofed
- Headers are fully user-controlled
- Reverse proxy misconfigs expose protected routes
- Trusting headers ≠ authorization
- Client headers are NOT security boundaries

**Never trust headers for authorization logic**.

## Prevention and Mitigation

### Server-Side Authorization

- Validate permissions using sessions or tokens
- Never rely on headers for access control

### Secure Proxy Configuration

- Strip untrusted headers
- Only allow trusted proxies to set forwarding headers

### Consistent Access Control

- Apply authorization checks at every layer
- Ensure backend APIs enforce permissions

### Logging and Monitoring

- Log suspicious header usage
- Detect abnormal access patterns
- Alert on unauthorized access attempts

## Key Takeaways

- Header Access Control Bypass targets trusted headers
- Headers can be spoofed by attackers
- IP and role checks via headers are unsafe
- Proxies can introduce security gaps
- Always enforce authorization on the backend
