## Gandalf API

<details>
<summary>Auth</summary>
<p>

- **/auth/login**:
  - POST: authenticates an user into the system
    - Headers: no headers required
    - Payload:
        ```json
        {
            "email": "test@test.com",
            "password": "testestestest",
            "scopes": [
                "user:read",
            ]
        }
        ```
    - Result:
      ```json
      {
          "type": "tokens",
          "data": {
              "access_token": "eyJhbGciOiJIUzI",
              "refresh_token": "eyJhbGciOiJIUzP"
          }
      }
      ```
    - Codes:
      - 200
      - 400
      - 403

- **/auth/refresh**:
  - POST: refresh the access token given in the `login` endpoint
    - Headers: no headers required
    - Payload:
      ```json
      {
          "access_token": "eyJhbGciOiJIUzI",
          "refresh_token": "eyJhbGciOiJIUzP"
      }
      ```
    - Result:
      ```json
      {
          "type": "tokens",
          "data": {
              "access_token": "eyJhbGciOiJIUzI",
              "refresh_token": "eyJhbGciOiJIUzP"
          }
      }
      ```:
    - Codes:
      - 200
      - 400

</p>
</details> 

<details>
<summary>Users</summary>
<p>

- **/users**:
  - POST: creates a new user
    - Headers: no headers required
    - Payload:
      ```json
      {
          "email": "test@test.com",
          "name": "Test",
          "surname": "Test test",
          "password": "testestestest",
          "birthday": "1997-12-21T00:00:00Z",
          "verification_url": "http://test",
          "phone": "+34666666666" // Optional
      }
      ```
    - Result:
      ```json
      {
          "type": "user",
          "data": {
              "uuid": "b6dcda6d-b149-4063-baa1-f7ac12508772",
              "email": "test@test.com",
              "name": "Test",
              "surname": "Test test",
              "birthday": "1997-12-21T00:00:00Z",
              "phone": "+34666666666"
          }
      }
      ```
    - Codes:
      - 200
      - 400
- **/users/me**:
  - GET: retrieves the user data who perform the request
    - Headers:
      - `Authorization: Bearer <token>`
    - Scopes:
      - `users:read`
    - Result:
      ```json
      {
          "type": "user",
          "data": {
              "uuid": "b6dcda6d-b149-4063-baa1-f7ac12508772",
              "email": "test@test.com",
              "name": "Test",
              "surname": "Test test",
              "birthday": "1997-12-21T00:00:00Z",
              "phone": "+34666666666"
          }
      }
      ```
    - Codes:
      - 200
      - 403
- **/users/verify**:
  - PATCH: verifies the user who perform the request
    - Headers:
      - `Authorization: Bearer <token>`
    - Scopes:
      - `user:verify`
    - Result: no response
    - Codes:
      - 200
      - 403
- **/users/verify/resend**:
  - POST: resend the user verification email
    - Headers: no headers required
    - Payload:
      ```json
      {
          "email": "test@test.com",
          "verification_url": "http://test",
      }
      ```
    - Result: no response
    - Codes:
      - 201
      - 400
- **/users/reset**:
  - PATCH: reset the password of the user who perform the request
    - Headers:
      - `Authorization: Bearer <token>`
    - Scopes:
      - `user:change-password`
    - Payload:
      ```json
      {
           "password": "testestestest"
      }
      ```
    - Result: no response
    - Codes:
      - 200
      - 403
- **/users/reset/resend**:
  - POST: resend the user reset password email
    - Headers: no headers required
    - Payload:
      ```json
      {
          "email": "test@test.com",
          "verification_url": "http://test",
      }
      ```
    - Result: no response
    - Codes:
      - 201
      - 400

</p>
</details> 
