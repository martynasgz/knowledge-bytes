# oauth2-login-authentication-filter

## Process

1. Gets the authorization code from the response.
2. Passes it to the authentication manager to authenticate.
3. Authentication manager exchanges authorization code for a token.
4. Loads the user using the `UserService`.
5. Upon a successful authentication, an `OAuth2AuthenticationToken` is created (representing the End-User Principal) and associated to the Authorized Client using the `OAuth2AuthorizedClientRepository`.
6. Finally, the `OAuth2AuthenticationToken` is returned and ultimately stored in the `SecurityContextRepository` to complete the authentication processing[^1].

[^1]: [Spring documentation](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/oauth2/client/web/OAuth2LoginAuthenticationFilter.html).