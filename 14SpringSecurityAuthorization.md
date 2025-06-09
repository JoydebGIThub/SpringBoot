# Authorization
- In our application different user can perform different task all users are not giving the `permission` of `Read`, `write`, `Delete`.
- Depending upon the `role` the permission is given

![image](https://github.com/user-attachments/assets/5420f5bf-6c51-4b31-901a-4b75ccbd5dd0)

- In the `filter chain` the `AuthorizationFilter` is the Last by default
- Just like the `Authentication manager` the `AuthorizationFilter` will send the request to `Authorization manager`, it is a functional interface with one method `check()`
- When we use the `requestermatcher` the in `Authorization manger` the `RequestMatcherDelegatingAuthorizationManager` comes if the request is not authorized the `AccessDeniedException` will throw

![image](https://github.com/user-attachments/assets/84308a7f-3d02-413a-8021-df22cf6a4857)

- now we can add the `requestMatchers` to access any `url` of a perticuler role is present
![image](https://github.com/user-attachments/assets/f4942d35-81e2-43c7-8112-ae3f34205d0c)
![image](https://github.com/user-attachments/assets/7de3ee08-57cd-4d81-b5fe-3d04eb3505c1)

## Implementation
- Create a `enum` permission

![image](https://github.com/user-attachments/assets/fc8e0505-94d6-4c64-860c-f2e1b1150405)

- Create a `enum` for role

![image](https://github.com/user-attachments/assets/67fa5796-d30e-46eb-b420-f4b56da86ca0)

- Now we save the role inside database so we need to convert it to String

![image](https://github.com/user-attachments/assets/7b3a49ae-d37b-4bbe-a48a-07222ab3a497)

- Now also change the `getAuthorities()`

![image](https://github.com/user-attachments/assets/ef292a41-f554-402f-9af8-9cdfdecc2df3)

- give the access when the role or permission matches

![image](https://github.com/user-attachments/assets/f9508fc1-1901-44ed-a61d-c331fa61a236)

![image](https://github.com/user-attachments/assets/e293cea9-a370-48af-84d0-a8ac460a3bf5)

![image](https://github.com/user-attachments/assets/bac5df7a-51e4-4792-81d1-f51586670a45)

## Method Sercurity
- If we have 1000 or api then setting `requestMatchers` for all the request is hard
- So, here we are going to use methodSecurity
- So we can add `@PreAuthorize` so that the request will check the authorization before access the method
- So before calling the method there will be an `intercepter` (AuthorizationManagerBeforeMethodIntercepter).
- The `preauthorizationHandler` has something called `MethodSecurityExpressionHandler`

![image](https://github.com/user-attachments/assets/22a8fb9d-ae3e-48e4-abe1-ba600e1afc97)

- To use the method security we need to enable the method security fast

![image](https://github.com/user-attachments/assets/d86fa07d-01e5-4499-8906-2f65179861b3)

![image](https://github.com/user-attachments/assets/9f373136-3cff-4410-b447-5ada54c38d5f)

- Now we can add `@PreAuthorize("hasAuthority('WEATHER_READ')")` to check the authorization

![image](https://github.com/user-attachments/assets/c3b74e58-b86a-4999-aff9-eb3742e13910)

- We can also pass the role for checking

![image](https://github.com/user-attachments/assets/dbe853ce-f5cc-437e-8935-80d08e5aba9d)

- If you have multiple role then

![image](https://github.com/user-attachments/assets/de6e7c43-b32d-43c8-ab99-820254c83de2)









