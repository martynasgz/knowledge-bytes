# Spring Security Internal Flow

![spring security flow diagram](/assets/spring-security-flow.png)

- (1) User sends request to the web application, which is secured by the **Spring Security Filters**. As soon as the request is received by the servlet container, since we have added spring security, the container will make the request go through the filters first before reaching the corresponding servlet. The filters will have logic to check if user is already authenticated / has a session or not. If not, it will enforce authentication.
- (2) Spring security filters convert the user provided credentials into an **Authentication** object.
- (3, 4, 5, 6) The filters hand over the request to the **Authentication Manager**. The manager checks the available **Authentication Providers**. Inside the authentication providers we define the actual logic for the authentication. The authentication manager will try authenticating with all the providers until it succeeds. If all of them fail, the manager will send a failure response.  
The authentication provider performs authentication. By default, it uses **UserDetails Manager/Service**. Since we don't want to store or perform authentication with plain text passwords, we use the **Password Encoder** interface and implementations. UserDetails manager/service and the password encoder will work together to determine whether the given credentials are valid or not.
- (7, 8) The authentication manager sends the authentication result back to the filters.
- (9) Spring security filters store the authentication object created in step 2 inside **Security Context**. The Authentication object inside the context will store information such as whether the authentication is successful or not, what is the session ID, etc.
- (10) Response is sent back to the user.