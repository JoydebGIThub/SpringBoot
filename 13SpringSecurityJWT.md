# JWT Tokens
- Let assume a employee visit the office on deaily basic and the employee needs to `authenticated` when enter the office.
- So the employee show his id to the `security gaurd` and he will verity the id in the register and after checking the employee can enter the office
- Every day the security gard validate and let him enter. So each time the validation is needed so this is called `Basic authentication`.

- If the employee visit another office then the details can be missing from the registry so its difficult to maintain all the details in everywhere
- So, employee can provide an access card so that there is no need for the `security gard` and we can entry office if the `access card` has the permission.
- So, to validate the `access card` we need to send some response when we try to access. We can send the response in `XML` or `JSON` format. JSON is light weight so sending token in the form of JSON is easy. This is called `JSON Web Token`. So here we don't need to store anything in database so its a stateless process

## What is JWT
Decode, verify and generate JSON Web Tokens, which are an open, industry standard RFC 7519 method for representing claims securely between two parties
JWT has 3 parts
- 1st part is decoded header where we use `HS256` algorithem to encode the token (type of the alogorithm)
- 2nd part is decoded payload where has actual information about the client
- 3rd part is signature verification where the signature will be validated

## JWT Flow of Authentication
- User need to authenticate once and send the post request `/login` and send the username and password.
- Then we authenticated the request and create a `Web Token` and send back to the client.
- Now client has the `JWT Token`
- Client can store that token to some memory
- And when the requester try to access something he will send the `Autherization bearer token`, now application will `Extract JWT Token` from the `Header`
- Then we `Decode JWT Token`
- Then the `signature` should be `verify` if request failed then an `unauthorized` error will be shown in the client side
- If success then `Expiry of the token` is checked. If the token is expired then we send a expire token message to client
- All the verification done the request will be processed
![image](https://github.com/user-attachments/assets/6e44cd4f-c5fa-49db-861f-a542b57d4955)
![image](https://github.com/user-attachments/assets/9d2aa77e-5ec1-4cda-ae93-0d3a1410547a)

## JWT Authentication Implementation
- If we send the request to to get the authentication and the filter comes into the picture then it will try to authenticate the request and for each request it will doing that. So, we don't want the `BasicAuthenticationFilter` or `UsernamePasswordAuthenticationFilter`
- We want to directly call the `authentication API` without any authentication. So we skip the filter and directly call the controller
- Now when the client send the request we skip the `Filters` for only `authentication API` and send the request directly to `AuthController`.
- `AuthController` will create a JWT token for the user.
- So, now authenticate the request `AuthController` will send the username and password to `Authentication Manager` and the same flow will be follow
- So after the authentication done `AuthController` will generate the `JWT Token`
![image](https://github.com/user-attachments/assets/9ebcc1ee-ab4f-4525-b817-fa75843aa5e7)

### Steps:
- Create a controller

![image](https://github.com/user-attachments/assets/61db660c-3902-4a77-a0c6-da2bbaf6c03b)

- Create a entity

![image](https://github.com/user-attachments/assets/628dda86-446f-405e-be65-6c5da0fedbd5)

- Now bypass the `API` request from the filter

![image](https://github.com/user-attachments/assets/f7c22ac7-73b3-4d5e-8285-becc17700805)

- Now we need to authentication

![image](https://github.com/user-attachments/assets/4f689165-a614-44df-b401-773e105a29c0)

- Now we need to create the JWT Token.
- For creating the JWT token I need to create a `JWT utility` and for creating the utility we need some libraries and that will be provided by `jjwt`
- So we need to add 3 dependency
  1. jjwt-api
  2. jjwt-impl
  3. jjwt-jackson

![image](https://github.com/user-attachments/assets/d663b21a-3e54-43f3-aa53-0c5f496b5701)

- So in utility we need `Jwts.builder()` to build our token
- set the subject
- set the issued date
- set the expiration time
- sign the signature and algorithem. Before provide the signature string we need to convert it as key

![image](https://github.com/user-attachments/assets/82318348-1840-4444-98c4-0d11e42eaca8)

- Now in controller we can pass the username to generate the token

![image](https://github.com/user-attachments/assets/9e26ac31-59bd-4645-ba62-41d5ed9ee95e)

## 2nd part
- We need to remove the `Basic Authentication Filter` and add `JwtAuthFilter` before `UsernamePasswordAuthenticationFilter`
- Create the `JwtAuthFilter` and extends `OncePerRequestFilter`, this filter will come into picture onec per request
- Take the 'Token' from the header
- extract the Token

![image](https://github.com/user-attachments/assets/5a590a22-f198-4584-8766-5b22dc3072bb)

- extract all the details from the token

![image](https://github.com/user-attachments/assets/536863b7-884d-43b1-8830-c5c49cdb7e12)

![image](https://github.com/user-attachments/assets/41f6038b-708c-44d2-bfdc-d1c9feccb384)

- Validate the Token

![image](https://github.com/user-attachments/assets/eac4da8d-35bc-42b6-a866-79d5f2deed9c)

- set inside the security context holder

![image](https://github.com/user-attachments/assets/cf468cd2-d2d7-443e-899e-9de1f1cd7df4)

- Call the next filter

![image](https://github.com/user-attachments/assets/98a8451f-ea12-41e7-8316-95f4f362624e)

- Add the filter before usernamePasswordAuthenticationFilter

![image](https://github.com/user-attachments/assets/fee03917-4efc-4b17-bb35-6ccbd8614455)







