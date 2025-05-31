# CSRF

**CSRF (Cross-Site Request Forgery)** is an attack which aims to perform an operation in a web application on behalf of a user without their consent.


## Case
Let's say a user logs into a website and receives a session cookie. This cookie is sent automatically with every subsequent request to the same server.

User lands on an attackers website with a different domain. Its goal is to use the browser's automatic inclusion of cookies in the requests to the same domain to impersonate the user and make a request as if it is coming from the authenticated user.

## How CSRF Solves This

To secure from this attack, the backend needs a way to determine if the HTTP request is coming from a legitimate user AND from the expected website.

This can be done by generating an Anti-CSRF token in the back-end, and explicitly sending it through each subsequent requests from the browser.