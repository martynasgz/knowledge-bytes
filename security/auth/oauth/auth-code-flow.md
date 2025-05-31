# Authorization Code Flow

## Visualization

![authorization code flow visualization](/assets/auth-code.png)

## OpenID Connect

In Authorization Code Flow, when using OIDC, nothing changes apart from the exchange of authorization code for tokens - an ID token is added:

![authorization code flow oidc visualization](/assets/auth-code-oidc.png)

The ID token is a JWT. The client can decode the payload of the JWT to get information about the user.

## CSRF Prevention

State is used to prevent an attacker from making the victim authorize with the attacker's captured authorization code (CSRF). Before requesting for the auth code, the client generates a state value which is validated after receiving the auth code. If the state is different or it wasn't provided, that means the authorization was intercepted/forged.[^1]

[^1]: [Stack Overflow answer](https://stackoverflow.com/a/35988614)

If auth code flow with PKCE is used state is not needed, as before authorization, code_verifier and code_challenge are generated. During the request for auth code, the code_challenge is sent to the auth server with the generation algorithm and stored there. The client then requests for the token with the auth code and code_verifier. Inside the auth server, the code_verifier is hashed using the provided algorithm and compared with the code_challenge which was stored before. If the values match, it means that the client didn't change and nothing was intercepted.