# pmd api

- BaseUrl: http://xxx.xxx.xxx/api
- Response Format
  - api normal : {"code":0}
  - api normal with data:  {"code":0, "data":{}}
  - api error: {"code":-1}  (code<0 means error)
- Description
  - In order to distinguish between the app and the desktop, it is determined by the second parameter of the url, such as: /api/desk/login, /api/mobile/login. Representing the desktop and mobile versions respectively.
  - all the api request require  "version" paremeter in header.
  - desk app request require "disk_id" parameter in header.



#### 1、Send SMS Code

- GET /sendVerify?email=xxx@123.com

- Response: 

	```json
	{"code":0}
	```
 
#### 2、Register

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
  
#### 3、Login

- POST /login

- Body  

	```json
	{
		"email": "xxx@123.com", 
		"pwd":"12345"
	}
	```

- Response

  ```json
  {
  	"code":0,
  	"data":{
      	"x-token":"aaaaaaaa",
      	"uid":99999,
      	"isNew":0
      }
  }
  ```

#### 4、Modify payment password

- GET /reset/pin
	
- body
	
	```json
	{
	"old_trade_pwd":"xxx",
	"trade_pwd":"xxx",
	"trade_pwd_repeat":"xxx"
	}
	```

- response
	
	```json
	{
		"code":0
	}
	```

#### 5、Modify the login password

- GET /reset/password
	
- body
	
	```json
	{
	"old_pwd":"xxx",
	"pwd":"xxx",
	"pwd_repeat":"xxx"
	}
	```

- response
	
	```json
	{
		"code":0
	}
	```
		
#### 6、Get a list of messages

- GET /msg/list?page=1

- response  ​

	```json
  {
  "code":0,
  "data":
  		{
	      "all_cnt":1,
	      "list":[
	          {
	           	"title":"system infoxxxx",
	              "content": "abcdefg",
	              "category": 0, // 0-system info ,1-transfer info,2- withdrawl info
	              "create_at": 12345
	          }
	      ]
	  }
  }
  ```
  
#### 6、Get a list of messages (by category, no paging)

- GET /msgs/:type

- param
	- :type
		* 0 - system notification
		* 1 = transfer notification 
		* 2 - coin notification

- response  ​

	```json
  {
  "code":0,
  "data":
  		{
	      "all_cnt":1,
	      "list":[
	          {
	           	"title":"system infoxxxx",
	              "content": "abcdefg",
	              "category": 0, // 0-system info ,1-transfer info,2- withdrawl info
	              "create_at": 12345
	          }
	      ]
	  }
  }
  ```
  
#### 7、Get exchange rate information

- GET /exchange/rate

- response

	```json
  {
  "code":0,
  "data":{
      "list":[
		{"name":"usdt", "rate":1.222},
		{"name":"cny", "rate":1.322},
      ]
	}
  }
  ```

#### 8、Get the exchange price

- GET /exchange/info

- response

	```json
	{
   "code":0, 
   "data":{
   		"name": "Fcoin", 
   		"price": 0.00001
   	}
  }
  	```

#### 9、Home overview information

- GET /overview

	- response
	
		* desk
		
		```json
		{
	   "code":0, 
	   "data":
	      {
	          "pmd_all": 20180107,  // pmd
	          "other_incoming": 25, // otherIncoming
	          "mining_profit": 75,
	          "exchangeIndex":[
	              {
	                  "name":"Fcoin",
	                  "price":"99.05"
	              }
	          ]
	      }
	  }
		```

		* mobile 
		 
		```json
		{
	   "code":0, 
	   "data":
	      {
	          "pmd_all": 20180107,  // pmd
	          "other_incoming": 25, // otherIncoming
	          "mining_profit": 75,
	          "exchangeIndex":[
	              {
	                  "name":"Fcoin",
	                  "price":"99.05"
	              }
	          ],
	          "machine_list":[
	          	{
	          		"wallet_addr":"xxx",
						"balance":0.00,
						"machine_nick":"xxx",
						"status":0/1
	          	}
	          ]
	      }
	  }
		``` 


#### 10、machine detail

- GET /machine/detail/:wallet_addr

- Response

	```json
	{
		"code":0,
		"data":{
			"status":1,
			"margin":"xxx",
			"elapsed:"xxx"
		}
	}
	```	

#### 11、Get my /balance

- GET /balance

- response

	```json
	{
	"code":0,
	"data":{
		"balance_by_miner":0.00,
		"balance_by_other":0.00
		}
	}
	```

#### 12、Get historical withdrawal address

- GET /withdraw/history/addrs

- response
	
	```json
	{
		"code":0,
		"data":{
			"list":[
				{"addr":"xxx"},
				{"addr":"xxx"}
			]
	}
	```

#### 13、outer transaction

- POST /transaction/outer

- Body

	```json
	{
	"to_addr":"xxx",
	"amount":xxxx,
	"trade_pwd":"xxx"
	}
	```

- Reponse
	
	```json
	{
		"code":0
	}
	```
## 2018-10-08

#### 1、Scan QR code

- POST /mobile/qrcode

- Body

	```json
	{
   	"mac_info":"mac:A1-20180917\rdisk:A1-20180917"
	}
	```

- Reponse
	
	```json
	{
    "code": "0",
    "data": {
    	"is_bind":0/1,   ##1-表示已绑定到当前用户，0-表示未绑定
        "wallet_addr": "0x863D0C461818D74D7012443E362DC21B7E49505C"
    },
    "msg": ""
	}
	```
#### 2、Binding mining machine

- GET /bind/:wallet_addr

- Reponse
	
	```json
	{
    "code": "0"
    }
	```
### 3、set machine alias

- POST /mobile/machine/set

- Body

	```json
	{
   "wallet_addr":"0x863D0C461818D74D7012443E362DC21B7E49505C",
   "machine_name":"haha-no.1"
	}
	```

- Reponse
	
	```json
	{
		"code":0
	}
	```

### 4、扫描二维码，桌面版自动登录
> 1.请使用长轮询访问此接口

> 2.桌面版打开二维码，即访问此接口，

> 3.当手机扫描成功，并完成绑定时，才会有返回，

> 4.当玩家绑定机器后，通过disk_id查询用户信息，然后再完成自动登录，并返回token

- POST /desk/autologin

- Body

- Response
	```json
		{
		    "code": "0",
		    "data": {
		        "uid": 10009,
		        "isNew": 0,
		        "token": "0f04b388129f74b12dbbadc7f2c2bce6"
		    },
		    "msg": ""
		}
	```
## 2018-10-15

#### 1、mobile unbind my machine

- GET /mobile/unbind/:wallet_addr

- Reponse
	
	```json
	{
    "code": "0"
    }
	```
- Error
	```
		-15 : find machine error
		-10202 : Machine has balance
	
	```

## 2018-10-25

#### 1、update mobile bonus/info api

- GET /mobile/bonus/info/:wallet_addr?q=xxx

-Param:
   q : 0/1/2/3

- Reponse
	
	```json
	{
	    "code": "0",
	    "data": {
		"q": 0,
		"all_bonus": 0,
		"chart": [
		    {
			"bonus": 0,
			"date": "2018-10-23",
			"time": 1540224000
		    },
		    {
			"bonus": 0,
			"date": "2018-10-24",
			"time": 1540310400
		    },
		    {
			"bonus": 0,
			"date": "2018-10-25",
			"time": 1540396800
		    }
		]
	    },
	    "msg": ""
	}
	```
- Error
	```
		-18 : not found machine  or  machine is not mine
	
	```











		




  ​

