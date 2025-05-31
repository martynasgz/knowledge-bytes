# Authorization Code Flow

## Visualization

![authorization code flow visualization](/assets/auth-code.png)

## OpenID Connect

In Authorization Code Flow, when using OIDC, nothing changes apart from the exchange of authorization code for tokens - an ID token is added:

![authorization code flow visualization](/assets/auth-code-oidc.png)

The ID token is a JWT. The client can decode the payload of the JWT to get information about the user.