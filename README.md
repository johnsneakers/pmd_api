# pmd api

- BaseUrl: http://xxx.xxx.xxx/api
- Response Format
  - api normal : {"code":0}
  - api normal with data:  {"code":0, "data":{}}
  - api error: {"code":-1}  (code<0 means error)
- Description
  - In order to distinguish between the app and the desktop, it is determined by the second parameter of the url, such as: /api/desk/login, /api/mobile/login. Representing the desktop and mobile versions respectively.
  - all the api request header need: version paremete
  - desk app need disk_id parameter



#### Register

> password require 6-15 letter and number

- POST /register

- Body

  ```json
  {
   "email": "xxx@123.com",
   "verify_code":12345, 
   "pwd":"12345",
   "pwd_repeat":"12345",
   "trade_pwd":"12345",
   "trade_pwd_repeat":"12345"
  }
  ```

- Response

  ```json
  //succ:
   {"code":0}
  //wrong password format
   {"code":-4}
  //wrong verity code
   {"code":-5}
  // trade password is empty
   {"code":-9}
  // machine not found
   {"code":-18}
  ```

  ​

  ​

#### Send SMS Code

- GET /sendVerify?email=xxx@123.com
- Response: {"code":0}



#### Login

- POST /login

- Body  {"email": "xxx@123.com", "pwd":"12345"}

- Response

  ```json
  {
      "x-token":"aaaaaaaa",
      "uid":99999,
      "isNew":0
  }
  ```

  ​



  ​





  ​

