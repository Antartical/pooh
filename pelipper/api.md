## Pelipper API

<details>
<summary>Users</summary>
<p>

- **/emails/users/verify**:
  - POST: send user verification email
    - Headers: no headers required
    - Payload:
      ```json
      {
          "from": "test@test.com",
          "to": "test@test.com",
          "subject": "Verifica tu usuario",
          "Name": "Test",
          "verification_link": "http://test"
      }
      ```
    - Result: no response
    - Codes:
- **/emails/users/change_password**: 
  - POST: send user change password email
    - Headers: no headers required
    - Payload:
      ```json
      {
          "from": "test@test.com",
          "to": "test@test.com",
          "subject": "Verifica tu usuario",
          "Name": "Test",
          "change_password_link": "http://test"
      }
      ```
    - Result: no response
    - Codes:

</p>
</details> 
