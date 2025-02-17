# CloudBank-V2
Uses new Core2

[Key file format](README.md#key-file-format)

[PRINT WELCOME SERVICE](README.md#print-welcome-service)

[ECHO RAIDA SERVICE](README.md#echo-raida-service)

[DEPOSIT SERVICE](README.md#deposit-service)

[DEPOSIT SERVICE WITH CHANGE](README.md#deposit-service-with-change)

[RECEIVE FROM RAIDA](README.md#receive-from-raida)

[Retrieve Pay Forward Service](README.md#retrieve-pay-forward-service)

[New Account](README.md#new-account)



# CLOUDCOIN CONSORTIUM'S CLOUDSERVER VERSION 2 June-22-2018 MIT LICENCE

This Software is provided as is with all faults, defects 
and errors, and without warranty of any kind.
Free from the CloudCoin Consortium. You can see a working sample here:

https://bank.cloudcoin.global/
                                                 
CloudBank offers many services:

1. Banking Services
2. Exchange Services
3. Bill Pay
4. Application Support



# Banking Services
The purpose is to allow software to pown coins. 
Some examples of use:

You sell products from your website and want to accept CloudCoins as payment. 

You want to sell virtule goods in your minecraft server without worrying about charge backs.

You create an accounting system and want to recieve and export CloudCoins.



NOTES: To Stop Replay attacks and other sercurity concenrs, HTTPS.

For receiving CloudCoins, you may only need a few services. The other services may be shut down to reduce the security surface. Services that are neccessary include: Echo, (Deposite or Import One Stack), Get Receipt. The Print Welcome, Show Coins, Expot One Stack services can be shut off if you will collect your CloudCoins from the harddrive of the web server that runs CloudBank. 

Note that this fist phaze service uses file-based storage and not a database. 



Many services are available:
1. Print Welcome
2. Echo
3. Show Coins
4. Deposit One Stack
5. Withdraw One Stack
6. Get Receipt
7. Bill Pay
8. Write and Send Check
9. Cash Check

Exchange Services Available
1. Show Coins for Sale
2. Mark Coins for Sale


There are also standards for how the transactions will go:
1. Merchant / Buyer Collaboration
2. Buyer Initiated
3. Merchant Dominates
Note that these are still under development

------------------------------------------------------------------
General Rules:

The name of the servers is the URL:

bank_server =  https://bank.CloudCoin.com/ (Use the name of the local host)

The time will always be in ISO 8601 zulu time. 

For security, the system admin must setup SSL and limit the servers that can connect to this web server. 
----------------------------------------------------------------


## Key file format
The CloudBank requires a key and will be in the following format
```http
{
    "url":"bank.CloudCoin.Global",
    "privatekey":"6e2b96d6204a4212ae57ab84260e747f",
    "account":"CloudCoin@Protonmail.com"
}
```


 Services
------------------------


## PRINT WELCOME SERVICE

Get's the bank's welcome information. Note that the web server must be configured to use extentionless urls.

Sample request
```http
https://bank.cloudcoin.global/service/print_welcome

```
Response if success:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"welcome",
"version":"2.0",
"message":"CloudCoin Bank. Used to Authenticate, Store and Payout CloudCoins.This Software is provided as is with all faults, defects and errors, and without warranty of any kind.Free from the CloudCoin Consortium.",
"time":"2018-06-23T05:18:34.3025868Z"
}
```

## ECHO RAIDA SERVICE

Sample GET Request:
```http
https://bank.cloudcoin.global/service/echo?account=CloudCoin@Protonmail.com&pk=640322f6d30c45328914b441ac0f4e5b
```

Echo Response for good
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"ready",
"version":"2.0",
"message":"The RAIDA is ready for counterfeit detection.",
"time":"2018-06-23T05:18:34.3025868Z",
"readyCount":25,
"notReadyCount":0
}
```

Echo Response for bad
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Not enough RAIDA servers can be contacted to import new coins.",
"time":"2018-06-23T05:18:34.3025868Z",
"readyCount":3,
"notReadyCount":22
}
```

Not enough RAIDA servers can be contacted to import new coins.

## DEPOSIT SERVICE

The program must put a stack file in a folder that is accessible via the web to cors on the CloudBank Server. The request is a post request but may include the GET parameter "rn" (receipt number). If the rn parameter is included it must be a GUID without hyphends. The service will then use the customer's rn as the receipt number instead of generating its own. 


Sample POST Request:
```http
https://bank.cloudcoin.global/service/deposit_one_stack
account=CloudCoin@Protonmail.com
stack=
{
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"1112240", 
		"an": ["f5a52ee881daaae548c24a8eaff7176c", "415c2375a6fa48c4661f5af8d7c95541", "73e067b7b47c1556deebdca33f9a09fb", "9b90d265d102a565a702813fa2211f54", "e3e191ca987c8010a3adc49c6fc18417",
			"baa7578e207b7cfaa0b8336d7ed4a4f8", "6d8a5c66a589532fe9e5dc3932650cfa", "1170b354e097f2d90132869631409bd3", "b7bc83e8ee7529ff9f874866b901cf15", "a37f6c4af8fbcfbc4d77880fc29ddfbc",
			"277668208e9bafd9393aebd36945a2c3", "ef50088c8218afe53ce2ecd655c2c786", "b7bbb01fbe6c3a830a17bd9a842b46c0", "737360e18596d74d784f563ca729aaea", "e054a34f2790fd3353ea26e5d92d9d2f",
			"7051afef36dc388e65e982bc853be417", "ea22cbae0394f6c6918691f2e2f2e267", "95d1278f54b5daca5898c62f267b6364", "b98560e11b7142d1addf5b9cf32898da", "e325f615f93ed682c7aadf6b2d77c17a",
			"3e8f9d74290fe31d416b90db3a0d2ab1", "c92d1656ded0a4f68e5171c8331e0aea", "7a9cee66544934965bca0c0cb582ba73", "7a55437fa98c1b10d7f47d84f9accdf0", "c3577cced2d428f205355522bc1119b6"],
		"ed":"7-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]
}
```


Sample Response if good:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"importing",
 "message":"The stack file has been imported and detection will begin automatically so long as they are not already in bank. Please check your reciept.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if bad file bad:
```http
{
 "bank_server":"bank.cloudcoin.global",
  "account":"CloudCoin@Protonmail.com",
 "status":"error",
 "message":"JSON: Your stack file was corrupted. Please check JSON validation.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "time":"2016-49-21 7:49:PM"
}
```
Sample Response if nothing attached :
```http
{
 "bank_server":"bank.cloudcoin.global",
  "account":"CloudCoin@Protonmail.com",
 "status":"error",
 "message":"LoadFile: The stack file was empty.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if receipt number already in use :
```http
{
 "bank_server":"bank.cloudcoin.global",
  "account":"CloudCoin@Protonmail.com",
 "status":"error",
 "message":"Duplicate: The receipt number is already in use.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "time":"2016-49-21 7:49:PM"
}
```


## DEPOSIT SERVICE WITH CHANGE

Allows a stack of CloudCoins to be uploaded and powned on the server. This service does not tell the caller about the status of the pown but gives a receipt that the client can look at later. The request is a post request but may include the GET parameter "rn" (receipt number). If the rn parameter is included it must be a GUID without hyphends. The service will then use the customer's rn as the receipt number instead of generating its own. 


Sample POST Request:
```http
https://bank.cloudcoin.global/service/deposit_one_stack_with_change
account=CloudCoin@Protonmail.com&
amount=137&
stack=
{
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"16112240", 
		"an": ["f5a52ee881daaae548c24a8eaff7176c", "415c2375a6fa48c4661f5af8d7c95541", "73e067b7b47c1556deebdca33f9a09fb", "9b90d265d102a565a702813fa2211f54", "e3e191ca987c8010a3adc49c6fc18417",
			"baa7578e207b7cfaa0b8336d7ed4a4f8", "6d8a5c66a589532fe9e5dc3932650cfa", "1170b354e097f2d90132869631409bd3", "b7bc83e8ee7529ff9f874866b901cf15", "a37f6c4af8fbcfbc4d77880fc29ddfbc",
			"277668208e9bafd9393aebd36945a2c3", "ef50088c8218afe53ce2ecd655c2c786", "b7bbb01fbe6c3a830a17bd9a842b46c0", "737360e18596d74d784f563ca729aaea", "e054a34f2790fd3353ea26e5d92d9d2f",
			"7051afef36dc388e65e982bc853be417", "ea22cbae0394f6c6918691f2e2f2e267", "95d1278f54b5daca5898c62f267b6364", "b98560e11b7142d1addf5b9cf32898da", "e325f615f93ed682c7aadf6b2d77c17a",
			"3e8f9d74290fe31d416b90db3a0d2ab1", "c92d1656ded0a4f68e5171c8331e0aea", "7a9cee66544934965bca0c0cb582ba73", "7a55437fa98c1b10d7f47d84f9accdf0", "c3577cced2d428f205355522bc1119b6"],
		"ed":"7-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]
}
```


Sample Response if good:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"importing",
 "message":"The stack file has been imported and detection will begin automatically so long as they are not already in bank. Please check your reciept.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "change_url":"https://ccc.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232&receive=download",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if good and the user sent the perfect amount was sent and there does not need to be change:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"importing",
 "message":"The stack file has been imported and detection will begin automatically so long as they are not already in bank. Please check your reciept.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "change_url":"none",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if the server cannot make change:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"cannot_make_change",
 "message":"Sorry, we cannot make change for your order. The deposite was canceled.",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if the account does not exist on the server or if the password was not provided:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"fail",
 "message":"Request Error: Private key or Account not found.",
 "time":"2016-49-21 7:49:PM"
}
```



Sample Response if bad file bad:
```http
{
 "bank_server":"bank.cloudcoin.global",
  "account":"CloudCoin@Protonmail.com",
 "status":"error",
 "message":"JSON: Your stack file was corrupted. Please check JSON validation.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "time":"2016-49-21 7:49:PM"
}
```
Sample Response if nothing attached :
```http
{
 "bank_server":"bank.cloudcoin.global",
  "account":"CloudCoin@Protonmail.com",
 "status":"error",
 "message":"LoadFile: The stack file was empty.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if receipt number already in use :
```http
{
 "bank_server":"bank.cloudcoin.global",
  "account":"CloudCoin@Protonmail.com",
 "status":"error",
 "message":"Duplicate: The receipt number is already in use.",
 "reciept":"640322f6d30c45328914b441ac0f4e5b",
 "time":"2016-49-21 7:49:PM"
}
```




## GET RECEIPT SERVICE

The get receipt service returns a receipt based on the receipt id. 

### Sample Request
```http
https://bitshares.CloudCoin.global/bank/get_receipt.aspx?rn=ef50088c8218afe53ce2ecd655c2c786&account=CloudCoin@Protonmail.com

```
rn= Receipt Number.

Account is the account ID within the bank

### Sample Response
If powning process has not been started
```http
{
	"receipt_id": "e054a34f2790fd3353ea26e5d92d9d2f",
	"time": "2016-49-21 7:49:PM",
	"timezone": "UTC-7",
	"bank_server": "bank.CloudCoin.Global",
	"total_authentic": 5,
	"total_fracked": 7,
	"total_counterfeit": 1,
	"total_lost": 0,
	"receipt_detail": [{
			"nn.sn": "1.16777216",
			"status": "suspect",
			"pown": "uuuuuuuuuuuuuuuuuuuuuuuuu",
			"note": "Waiting"
		},
		{
			"nn.sn": "1:1425632",
			"status": "suspect",
			"pown": "uuuuuuuuuuuuuuuuuuuuuuuuu",
			"note": "Waiting"
		},
		{
			"nn.sn": "1.956258",
			"status": "suspect",
			"pown": "uuuuuuuuuuuuuuuuuuuuuuuuu",
			"note": "Waiting"
		},
		{
			"nn.sn": "1.15666214",
			"status": "suspect",
			"pown": "uuuuuuuuuuuuuuuuuuuuuuuuu",
			"note": "Waiting"
		},
		{
			"nn.sn": "1.15265894",
			"status": "suspect",
			"pown": "uuuuuuuuuuuuuuuuuuuuuuuuu",
			"note": "Waiting"
		}

	]

}
```

If powning process is complete:
```http
{
        "receipt_id":"e054a34f2790fd3353ea26e5d92d9d2f",
	"time": "2016-49-21 7:49:PM",
	"timezone": "UTC-7",
	"bank_server": "bank.CloudCoin.Global",
	"total_authentic": 5,
	"total_fracked": 7,
	"total_counterfeit": 1,
	"total_lost": 0,
	"receipt_detail": [{
			"nn.sn": "1.16777216",
			"status": "authentic",
			"pown": "ppppppppepppppppppppeppp",
			"note": "Moved to Bank"
		},
		{
			"nn.sn": "1:1425632",
			"status": "counterfeit",
			"pown": "fffffffffpfffffffffffffff",
			"note": "Sent to trash"
		},
		{
			"nn.sn": "1.956258",
			"status": "authentic",
			"pown": "ppppppppppppppppppppppppf",
			"note": "Moved to Fracked"
		},
		{
			"nn.sn": "1.15666214",
			"status": "lost",
			"pown": "pfpfpfpfpfpfpfpfpfpfpfpfp",
			"note": "Moved to Lost"
		},
		{
			"nn.sn": "1.15265894",
			"status": "lost",
			"pown": "ppppffpeepfpppfpfffpfffpf",
			"note": "STRINGS ATTACHED!"
		}

	]

}
```
## SHOW COINS SERVICE 

Gets the totals of CloudCoins in the bank.

Sample GET Request:

```http
https://bank.cloudcoin.global/service/show_coins?pk=baa7578e207b7cfaa0b8336d7ed4a4f8&account=CloudCoin@Protonmail.com
```
Sample Response if good:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"account":"CloudCoin@Protonmail.com",
"status":"coins_shown",
"message":"Coin totals returned.",
"ones":3,
"fives":0,
"twentyfives":0,
"hundreds":0,
"twohundredfifties":1,
"time":"2018-06-23T05:53:39.4155794Z",
"version":"2.0"
}
```

Sample Response if fail:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"fail",
 "message":"Private key incorrect",
 "ones":0,
 "fives":0,
 "twentyfives":0,
 "hundreds":0,
 "twohundredfifties":0,
 "time":"2018-06-23T05:53:39.4155794Z",
 "version":"2.0"
}
```


## WITHDRAW ONE STACK SERVICE

Sample GET Request:

```http
https://bank.CloudCoin.global/service/withdraw_one_stack?amount=254&pk=ef50088c8218afe53ce2ecd655c2c786&account=CloudCoin@Protonmail.com

```
sample response if good

```http
{
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"1112240", 
		"an": ["f5a52ee881daaae548c24a8eaff7176c", "415c2375a6fa48c4661f5af8d7c95541", "73e067b7b47c1556deebdca33f9a09fb", "9b90d265d102a565a702813fa2211f54", "e3e191ca987c8010a3adc49c6fc18417",
			"baa7578e207b7cfaa0b8336d7ed4a4f8", "6d8a5c66a589532fe9e5dc3932650cfa", "1170b354e097f2d90132869631409bd3", "b7bc83e8ee7529ff9f874866b901cf15", "a37f6c4af8fbcfbc4d77880fc29ddfbc",
			"277668208e9bafd9393aebd36945a2c3", "ef50088c8218afe53ce2ecd655c2c786", "b7bbb01fbe6c3a830a17bd9a842b46c0", "737360e18596d74d784f563ca729aaea", "e054a34f2790fd3353ea26e5d92d9d2f",
			"7051afef36dc388e65e982bc853be417", "ea22cbae0394f6c6918691f2e2f2e267", "95d1278f54b5daca5898c62f267b6364", "b98560e11b7142d1addf5b9cf32898da", "e325f615f93ed682c7aadf6b2d77c17a",
			"3e8f9d74290fe31d416b90db3a0d2ab1", "c92d1656ded0a4f68e5171c8331e0aea", "7a9cee66544934965bca0c0cb582ba73", "7a55437fa98c1b10d7f47d84f9accdf0", "c3577cced2d428f205355522bc1119b6"],
		"ed":"7-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]
}
```

Sample Response if fail:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"fail",
 "message":"Private key incorrect",
 "time":"2016-49-21 7:49:PM"
}
```

# Exchange Services

These services allow you to exchange your CloudCoins for other money or even goods and services. Exchange services include:

* Show Coins For Sale
* Place Order using Green.money ach
* Check Green Money order
* Place order using PayPal and Fulfill
* Place order using Stripe and Fulfill
* Fulfil Order with custom text
* Advertsie on Exchange






## Show Coins For Sale
Gets the totals of CloudCoins in the bank.The price in different currencies, the payment methods that are accepted.

Sample GET Request:

```http
https://bank.cloudcoin.global/service/show_coins_forsale?account=CloudCoin@Protonmail.com

```

Sample Response if good:
```http
{
 "exchange_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"coins_for_sale",
 "currencies":["dollars","bitcoin"],
 "prices":[.03,.0098],
 "methods":["green.money","stripe","paypal"],
 "ones":205,
 "fives":10,
 "twentyfives":105,
 "hundreds":1050,
 "twohundredfifties":98,
 "time":"2018-06-23T05:53:39.4155794Z",
 "version":"2.0"
}
```

Sample Response if fail:
```http
{
 "exchange_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"fail",
 "message":"Some error happened",
 "time":"2018-06-23T05:53:39.4155794Z",
 "version":"2.0"
}
```





## Mark Coins For Sale
This allows the account owner to mark some of their coins for sale so that others will be able to buy them. 

Sample GET Request:

```http
https://bank.cloudcoin.global/service/mark_coins_forsale?account=CloudCoin@Protonmail.com&pk=c92d1656ded0a4f68e5171c8331e0aea&ones=3&fives=23&twentyfives=2&hundreds=20&twohundredfifties=0

```

Sample Response if good:
```http
{
 "exchange_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"success",
 "message":"Coins marked for sale",
 "currencies":["dollars","bitcoin"],
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if fail:
```http
{
 "exchange_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"fail",
 "message":"Some error happened",
 "time":"2018-06-23T05:53:39.4155794Z",
 "version":"2.0"
}
```


## Place Order With Green.money ACH

This allows an order to be placed using the Green.money ACH payment system. Note that you will need an account with Green.money. You will need a web form that posts specific into and a default Green Pay button. 

Sample POST Request:

```http
https://bank.cloudcoin.global/service/place_order_with_green_money?
account=CloudCoin@Protonmail.com
GreenButton_id=11087
Amount=11.43
ItemName=Cloud Ed Pack se
TransactionID=2018.3.28.23.20.b0b7dc
Affiliate=Bill Jenkins

```

Sample Response if good:
```http
{
 "exchange_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"success",
 "message":"Green Payment Accepted. Check the URL to collect your CloudCoins",
 "green_pay_status_url":"https://bank.cloudcoin.global/service/green_pay_status?id=2018.3.28.23.20.b0b7dc",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if fail:
NOTE: The programmer should add the error that caused the fail instead of the "Error detail here" text. 
```http
{
 "exchange_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"fail",
 "message":"Invalid Request - Error detail here.",
 "time":"2016-49-21 7:49:PM"
}
```

## Green Pay Status
Allows the user to get a check from the CloudBank if the Green Pay status is paid. The user can click on this over and over until they get a check


Sample POST Request:

```http
https://bank.cloudcoin.global/service/green_pay_status?id=2018.3.28.23.20.b0b7dc

```

Sample Response if good: 
NOTE: This is the same response as the Write Check service.

```http
{
 "bank_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"url",
 "message":"https://bank.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232",
 "time":"2016-49-21 7:49:PM"
}
```


Sample Response if fail:
NOTE: The programmer should add the exact reason the customer was not able to recieve a check
```http
{
 "exchange_server":"bank.cloudcoin.global",
 "account":"CloudCoin@Protonmail.com",
 "status":"fail",
 "message":"Your Check has not cleared yet. Please try this URL later.",
 "time":"2016-49-21 7:49:PM"
}
```



# BILL PAY SERVICE

This is a task that is called every day. The program checks the Excel spreadsheet to see if bills need to be paid.The Excell spread sheet is a standardized spreadsheet.

FileName: BillPay
Sheets within: Reoccurring, Onetime, Pending, Complete, Requested and  History (one for each month)


Column Headers:

### Reoccurring 
Used to mark payments that should be paid automatically each month. These records are not deleted. They will be checked everyday and payments assigned to that day of the month will be made and copied to the Pending folder. 
1. Status: Active or Deactive.
2. PAY TO THE ORDER OF ( Payee Name )
3. SEND TO EMAIL ( Payee Email )
4. ACCOUNT NUMBER OR MEMO 
5. DAY OF THE MONTH TO PAY ( Day of the month that checks will be sent out )
6. AMOUNT (Amount of CloudCoin to be sent )
7. SIGNED BY ( Who is sending the CloudCoins )
8. YOUR EMAIL ( Senders Email )
9. YOUR OTHER CONTACT INFO ( Other information that can be included to for contact )
10. DAYS EXPIRES AFTER (Number of days the check will expire after it is written) 

### Onetime 
The system checks this list once each day. The Bill pay will make the payment and send to pending. Then the payment is deleted from this list)
1. Status: Active or Deactive.
2. PAY TO THE ORDER OF ( Payee Name )
3. SEND TO EMAIL ( Payee Email )
4. ACCOUNT NUMBER OR MEMO 
5. DAY OF THE MONTH TO PAY ( Day of the month that checks will be sent out )
6. AMOUNT (Amount of CloudCoin to be sent )
7. SIGNED BY ( Who is sending the CloudCoins )
8. YOUR EMAIL ( Senders Email )
9. YOUR OTHER CONTACT INFO ( Other information that can be included to for contact )
10. DAYS EXPIRES AFTER (Number of days the check will expire after it is written) 

### Pending
This holds all checks that have been sent but have not been cashed yet. Once they are cashed, they are deleted from pending and moved to  Paid. Payments in pending can be marked Cancel. If they are marked Cancel the money will be put back in the bank. Records will be checked for Cancel once each day. If they are marked as Hold. The check cashing service will not be allowed to give the user money until the status is changed to Pending. 
1. STATUS (Pending, Hold or Cancel)
2. PAY TO THE ORDER OF ( Payee Name )
3. SEND TO EMAIL ( Payee Email )
4. ACCOUNT NUMBER OR MEMO 
5. DATE MOVED TO PENDING ( Day that the check was moved to pending )
6. AMOUNT (Amount of CloudCoin to be sent )
7. SIGNED BY ( Who is sending the CloudCoins )
8. YOUR EMAIL ( Senders Email )
9. YOUR OTHER CONTACT INFO ( Other information that can be included to for contact )
10. DAYS EXPIRES AFTER (Number of days the check will expire after it is written) 

### Completed
1. STATUS (Paid, Canceled, Expired)
2. PAY TO THE ORDER OF ( Payee Name )
3. SEND TO EMAIL ( Payee Email )
4. ACCOUNT NUMBER OR MEMO 
5. DATE MOVED TO PENDING ( Day that the check was moved to pending )
6. AMOUNT (Amount of CloudCoin to be sent )
7. SIGNED BY ( Who is sending the CloudCoins )
8. YOUR EMAIL ( Senders Email )
9. YOUR OTHER CONTACT INFO ( Other information that can be included to for contact )
10. DATE COMPLETED

### Archive Month Year ( e.g. Archive December 2017 )
Creates a sheet with all the payments from a month and year for historical purposes. 
1. STATUS (Paid, Canceled, Expired)
2. PAY TO THE ORDER OF ( Payee Name )
3. SEND TO EMAIL ( Payee Email )
4. ACCOUNT NUMBER OR MEMO 
5. DATE MOVED TO PENDING ( Day that the check was moved to pending )
6. AMOUNT (Amount of CloudCoin to be sent )
7. SIGNED BY ( Who is sending the CloudCoins )
8. YOUR EMAIL ( Senders Email )
9. YOUR OTHER CONTACT INFO ( Other information that can be included to for contact )
10. DATE COMPLETED


## Daily Bil Pay Actions:
The following actions will take place one or more times each day according to the configuration: 
### Reoccuring:
1 Reoccurring: Check to see if a bill is to be paid. If yes, calls on the check making service to write a check.
2 Reoccurring: Creates new check in Pending
3 Reoccurring standard not finished.
### Onetime
1 Checks on the PayOnce to see if there is anything there. If yes, calls on the check making service and deletes the record from Onetime.
2 Creates new check in Pending
3 v standard not finished
### Pending
1. Bill Pay looks at the Pending to see if and are canceled. Put the canceled stack back into bank and deletes from pending.Writes Canceled to Completed worksheet. 
2. Do not allow checks to be cashed that are canceled or on hold. 
Pending protocol not finished
### Complete
1. If first of the month, archive all completed from last month into archived sheet with standard naming convention: Archive December 2017
Complete protocol not finished.  


## WRITE & SEND CHECK SERVICE
1. Make a check: Creates a stack file with a GUID and saves it in the Check folder
In CloudBank, a Check is a url that point to a stack file that is located in the Check folder.

Gets the totals of CloudCoins in the bank

Sample POST Request:

Parameters:

pk (private key) The user's secret info to allow the person to make the check

action How to send the check: email, url (just show the url), sms (maybe others to be supported later)

amount amount of CloudCoins to put in stac file

checkid (The check's unique identifier

emailto (recievers contact info)

payto (Person who is suppose to get the check)

from (person or organization check is from)

by (Who signed the check)

memo (some account info or memo)

```http
https://ccc.CloudCoin.Global/write_check
pk=a4b5e66f4b51418e81e8dc93e9db6503
action=email
amount=25
checkid=Billy12450
&eamilto=Bill@yardwork.com
payto=Billy Jenkins
from=CC@CloudCoin.global
signby=Sean Worthington
Memo=For Yard Work

```
Sample Request for URL
```http
https://ccc.CloudCoin.Global/write_check
pk=a4b5e66f4b51418e81e8dc93e9db6503
action=url
amount=25
checkid=Billy12450
emailto=Bill@yardwork.com
payto=Billy Jenkins
from=CC@CloudCoin.global
signby=Sean Worthington
Memo=For Yard Work

```

Sample Response if Success for email action:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "status":"emailed",
 "message":"Check sent to Bill@yardwork.com for 250 CloudCoins",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if Success for url action:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "status":"url",
 "message":"https://bank.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232",
 "time":"2016-49-21 7:49:PM"
}
```


Sample Response if Fail:
```http
{
 "bank_server":"bank.cloudcoin.global",
 "status":"error",
 "message":"Not enough funds to write Check for 250 CloudCoins to Bill@yardwork.com",
 "time":"2016-49-21 7:49:PM"
}
```


Example of a check:
```html
<!-- use html and css to make a form that looks like a standard check 
Maybe use background image that is of a check. 
-->
<html>
<body>
	<h1>Sean H. Wothington</h1>
	<address>1445 Heritage Oak Drive, Chico Ca, 95928</address>
	<email>CloudCoin@Protonmail.com</email>
	
	<h2>PAYTO THE ORDER OF: Larry's Landscaping</h2>
	<h2>AMOUNT: 59 CloudCoins</h2>
	
	<a href="https://Sean.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232">Cash Check Now</a>
	
</body>
<html>

https://bank.cloudcoin.global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232
```
Sample link to check graphical representation:
```html
https://bank.cloudcoin.global/checks/c3c3ab7b75ab4d089d2d4a287c1ef232.html
```
Sample link to cash check:
```html
https://bank.cloudcoin.global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232
```
1. Gathers CloudCoins into a stack and puts the stack file into the "check" folder.
2. Emails a link to the check to the payee.
3. Writes the Check Id (GUID) to the excel spread sheet.
4. Writes the send date to the excel spread sheet.
5. Send email:
Subject: Check for 2440 CloudCoins
Contains link to check:

## CHECK CASHING SERVICE
Allows user to download CloudCoins based on a check number. 
1. Checks to see if the excel spread sheet has the check on hold. 
2. Gives the stack file that the users wants to the user.
3. Updates the spreadsheet to show the date cashed. 

The request includes a receive parameter
receive=email 
receive=sms
receive=download
receive=json


Sample GET Request for a raw json stack file that can be imported into a program:
```http
https://ccc.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232&receive=json
```

Sample GET Request for a email stack file that can be sent to a person:
```http
https://ccc.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232&receive=email&contact=Billy@gmail.com
```

Sample GET Request for a json response to be sent to an SMS address:
```http
https://ccc.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232&receive=sms&contact=5305942578
```
Sample GET Request that downloads a file to a harddrive:
```http
https://ccc.CloudCoin.Global/checks?id=c3c3ab7b75ab4d089d2d4a287c1ef232&receive=download
```

Sample Response if Success:
```http
IF download is used, the browser Will download a stack file. Make sure your web server's mime type for .stack is set to download. 

otherwise: 

{
 "bank_server":"ccc.CloudCoin.global",
 "status":"success",
 "message":"CloudCoin stack file sent via email and has been deleted from this server.",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if on Hold:
```http
{
 "bank_server":"ccc.CloudCoin.global",
 "status":"hold",
 "message":"The check has been placed on hold status. You will not be able to cash this check until the status has been changed",
 "time":"2016-49-21 7:49:PM"
}
```

Sample Response if it does not exist in the Pending folder:
```http
NOTE: This standard is not finished. We may want to tell them it has already been cashed, if it was canceled and relavant info about the date it was canceled or cashed. 
{
 "bank_server":"ccc.CloudCoin.global",
 "status":"nonexistent",
 "message":"The check you requested was not found on the server. It may have been cashed, canceled or you have provided an ind that is incorrect. Did you type the write number?",
 "time":"2016-49-21 7:49:PM"
}
```


# ##################
## Change_Maker Service 
# ##################
Tells the Bank to break a CloudCoin note into several smaller notes.
Note that there are many (but a finte) way of making chage for each denomination. Each denomination will have a list (or matrix) of possible breaks with an id for Method for each possible method. 

NOTE: This standard is bad and not finished. We must create a way for them to upload a single note in a stack file and then download a stack file with change. This standard in the current form fails to do this. 
 
*CHANGE_MAKER REQUEST STRING*

```http
https://bank.cloudcoin.global/service/make_change?
method=100D
stack=
{
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"1112240", 
		"an": ["f5a52ee881daaae548c24a8eaff7176c", "415c2375a6fa48c4661f5af8d7c95541", "73e067b7b47c1556deebdca33f9a09fb", "9b90d265d102a565a702813fa2211f54", "e3e191ca987c8010a3adc49c6fc18417",
			"baa7578e207b7cfaa0b8336d7ed4a4f8", "6d8a5c66a589532fe9e5dc3932650cfa", "1170b354e097f2d90132869631409bd3", "b7bc83e8ee7529ff9f874866b901cf15", "a37f6c4af8fbcfbc4d77880fc29ddfbc",
			"277668208e9bafd9393aebd36945a2c3", "ef50088c8218afe53ce2ecd655c2c786", "b7bbb01fbe6c3a830a17bd9a842b46c0", "737360e18596d74d784f563ca729aaea", "e054a34f2790fd3353ea26e5d92d9d2f",
			"7051afef36dc388e65e982bc853be417", "ea22cbae0394f6c6918691f2e2f2e267", "95d1278f54b5daca5898c62f267b6364", "b98560e11b7142d1addf5b9cf32898da", "e325f615f93ed682c7aadf6b2d77c17a",
			"3e8f9d74290fe31d416b90db3a0d2ab1", "c92d1656ded0a4f68e5171c8331e0aea", "7a9cee66544934965bca0c0cb582ba73", "7a55437fa98c1b10d7f47d84f9accdf0", "c3577cced2d428f205355522bc1119b6"],
		"ed":"7-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]
}
```

if good:

*CHANGE_MAKER RESPONSE STRING*
```http
{
	"cloudcoin": 
	[
		{ 
		"nn":"1", 
		"sn":"1373099", 
		"an": ["c24010825f39c279596743a857fe11ca", "56531ce8de5fde250befd2739708e283", "a5960366fcd76396dffd95841922dc67", "4c7cacfee1fdad425e61eb3b82b8e3ae", "e1109e47ede4f0c0d6072ff94dede9fe",
			"ba54a6228b01c1b6ced4737439ab7928", "ff81207f096946d3f1068cd8e00035a5", "8960c63234da374b9eeccdc93c6335cd", "d7c0171807a1b430ebd1b8ea6fa002de", "78c4244ba2506a74ac08a5e18791db23",
			"159e963d210a457f43e108ae029f723b", "63032ed515bfdcc6ca9c3add62cb6024", "1ee4eae111f2b822eff492ba32af6048", "9cb6ed1da3db7ea296da40fa59825763", "79daa7da1a2c934e00816dcf9dec32a7",
			"920010c6b5490d0a46105c7a88af7e45", "dfed96dd6bfdf66f8d022b011d6e3607", "d4339f821c9b5aae9374225f6a825d0f", "6584be529ac36f399a608c1d3753a885", "7e579263a36d5b77ed07be48a07f7abc",
			"13877c03fad969896b7915cfc85a86dd", "4bbe4db9f557832368d2405b8d8e67a0", "12f0116251be8c261259f1ff4eec257e", "188c5bd2a01f3bad73978f904038c8f3", "50cd028d714a8b4cab9b30fa6e97a502"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}
,
		{ 
		"nn":"1", 
		"sn":"1373100", 
		"an": ["547907111f717da1b0c6069ce1fd4041", "e760ab538476344a29ba27cc345a2b3f", "ca2343891a20d05b35bf6d706b60b1ca", "8697127a6f2635e58da9051384d3cce5", "db24d970f26df109efa77f86a2a1fc91",
			"40ca4a165e3547fbff456d252534e4e5", "1dcdf80eacffc2059b5e6d9847c16245", "c53a4db06024b5369b56ba7d8574f0f9", "d3d31338158b2680cff747ca4a6ab360", "733214c079648dcc6e117a4f34b63167",
			"b89473121a4b1becfc1b7e9613920305", "8f1db113fed1501c05ae504d02a94703", "f9ae3005bb692b54d02f22598e98a228", "f371acef262599d243f9d1c507b2cc14", "539e499edd226942712b36be26b8bf3d",
			"d1f396e09194d5b4b5e9b6bb6e80d75a", "22f532f4ef54cbc569e4182b9de433f1", "295979d1680b41f86003e462e810112d", "b57fc449acec879482d2add0e1e9076b", "3fa566f4581239d99b8be80f242f33aa",
			"a7f6b60533bc5be9864aac8dd7d4ac9a", "22c37a78a7bcfb362e23d1243e126b8a", "35880ee199bf9c3f34daef52ba49c3f0", "70001f929717a3aea13b775fed3b9dfa", "bbf600f0e2eccabfe13a0779888c97c6"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}
,
		{ 
		"nn":"1", 
		"sn":"1373101", 
		"an": ["629f05f6d54b3b63ed0e9dd28f725735", "1ccf6ab3283a44ada4f6f1c562314e9c", "aabd78818f6eabe812529119d22c0150", "67575dcb350f228115f071d93ef34f44", "d0e75ab70387b5f9190ad2fcb4b9201e",
			"4740f6a6aff8376202c119e3bd9abb9b", "e2fa9e4fe4384c58bed9fe2be94eab43", "e8c5c364bc6e7962892352dab2cb59dc", "fa248a74ccd58078178a9d81915e4d61", "a117bc357d521b59e1518ff642ef6c8a",
			"877f86a4ac3c477dc47cd15c5ed6f890", "65e69d5eb8cbe4ec1b44f9081ffb2e9c", "09d6d01bbcae34588103deacb0e3cd4e", "6eaf4904cbcebace6655b04d5e8c7510", "70a78fb2121baad1adab7110ab90a603",
			"7ab99a31ba7dd1b90e5b62dafb58b0fb", "095ae42f6f529a3b24b287f2e7603aba", "ef1fef9c44b8527f731480b8ff4242de", "6da51c02df0b4799b2902925b06273a3", "f80b13f8956d775880e4adc41ba6c0b9",
			"f65d63c5147338932db00c3d06ba6710", "7775bbb934f5b4d76163c95aca4e9b8d", "ab9881583a1e8d5fd7cd701e089b9201", "2a05a81165c04829cac11cb9331fc289", "65dcddd78a8566208dbb2ce386b5629a"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}
,
		{ 
		"nn":"1", 
		"sn":"1373102", 
		"an": ["d38cfc99716fe74e8ac55b5baabde67a", "37efb9c2cea8fae0c83f1d556cabf10c", "248776d1e9e1f6d9fdf8a1d748baaf68", "b76342abbbc28673b3de3a4506c4aaa1", "2b43f87c9b1a94c5e5d6917f489a37ef",
			"8e4fb64562a067a81d9dbb21c7111822", "f2a76ab1d51bf9716c09be39aef92d25", "fb96fe546ab1731c111d03a1a0c6612c", "451ee5cfb1a29183e3ba8d203813229c", "0388cf348dcae17347ecd7f035a1a2a8",
			"73113babe70369ce8bc2d17e2c919045", "6d823990f4131478105ad97d0810e7d2", "7f2969c5a62e68d20ed8f4625ed64ee0", "edfb4c1d960721b1ed5daeadbe71dd49", "15b652fbc7dba0a7936aaa2a5d19abe4",
			"305eddcf75ea5baaea41cd92765e24a3", "29b4c604435f6d2e49f9f4a52b03d23e", "b904d664b1b1836d571ac9502c99de69", "da483c78247b9e360fcf7c617cab39b7", "3060849eca4c2b8a7a596c76ea590238",
			"b60da375b030609631c778572e6d4ffc", "b1db6e2a1fb1abc38584160c995a50b0", "c8bc597964034d76b7f298a02d4f14fe", "2ec7596c382ecd693fa595ed371b8921", "e41b286504c76d6f8d9179a818a568c6"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}
,
		{ 
		"nn":"1", 
		"sn":"1373103", 
		"an": ["d513fff400c24a3c9cb54aa75b1e4097", "47d8bff528f036b5484d8e3338024c32", "e8c2991b2fd4390a5366decd8eb1e50a", "3736613d473e797650b944adad4c8ecf", "ba07b460088ad282eb981b9059b8b488",
			"a4bc03bb2120bb6550bf17722583d101", "9bdcfe9181f05120d36d1e5829aed61d", "ccdb59e0dcfdd63a3f2ab124ba19388b", "bc774f62625a38cf6f629a9953fd886e", "fd2b87d841536cec85cb1b3ee490f76b",
			"c4dde8714b297df584ff1cb57bdd6063", "4b728d06c44438f6fc05596dd34a8cbc", "c635e4b698f2160a95fbd19148c4499f", "571b753f8f3a5686fc8cb9f76f02bdba", "2c2e1f1cf33d90b2ffa29398898dd81a",
			"7a9c8c09e671c44ae584a6152ad1c228", "2ab9416ce8ecd604d82283efe57b1bfe", "dea9bd54249b9aac37aaae11f5461a78", "323693b8b72f7b13a9454ff5f712e22b", "b9ba65c66cede3ca34846cba6ab475fb",
			"b96f00ecc33cb4082a54b3722e0a3759", "17a1da5874e69e1cc9be6cd59e80b3f3", "c1c402488b8b8d9f3047ee083fb3fa6a", "56d2cf33b824d723888af8a9362244b2", "1d83a153dd5c2055fe9564410a9de088"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}
	] 
}
```
if bad:
```http
{
	"server": "www.myBank.com",
	"status": "fail",
	"nn":"1",
	"sn":"5111558",
	"message": "CloudCoin note was not broken into smaller units. Coin was counterfeit",
	"time": "2016-40-21 10:40:PM"
}

```

Make Change
*CHANGE_MAKER METHODS:*

Denominaton 1
+ 1 (Cannot break)

Denaomination 5
+ Method 5A: 1,1,1,1,1

Denomination 25
+ Method 25A: 5,5,5,5,5 (Min)
+ Method 25B: 5,5,5,5,5A
+ Method 25C: 5,5,5A,5A,5A
+ Method 25D: 5A,5A,5A,5A  (Max)

Denomination 100
+ Method 100A: 25,25,25,25 (min)
+ Method 100B: 25,25,25,25A
+ Method 100C: 25,25,25A,25B
+ Method 100D: 25D,25D,25D,25D (Max)


Denomination 250
+ Method 250A: 100,100,25,25 (Min)
+ Method 250B: 100A,100A,25A,25B
+ Method 250C: 100B,100B,25B,25C
+ Method 250D: 100D,100D,25D,25D (Max)


# Shared secret Service:

Shared secret service allows an application to generate a secret that can be shared amount senders and receivers. This just saves the user from having to think about it. It also allows the application to make a suggestion. 

Sample Requests: 
```html
https://raida.tech/service/get_shared?type=string&complexity=1&lang=en-us
https://raida.tech/service/get_shared?type=string&complexity=2&lang=ru
https://raida.tech/service/get_shared?type=number&complexity=4
https://raida.tech/service/get_shared?type=string&complexity=20&lang=en-uk
https://raida.tech/service/get_shared?type=guid&complexity=3

```
GET Variables supplied:

type 

The data type. Either String, number or guid (more may be added later)
Strings are picked from lists of words, Numbers and guids are generated randomly. 

Complexity

For strings, this is the grade level of the word. K-12 or up to all words (20)
For numbers this is the number of numberals to be included. 4 will return a number like 3984. 6 will return a number like 381092
For Guids, this will return hexadecimal numbers up to the amount. 3 will return a number like: 4A7. 5 will return a number like 8FF24. Note that GUIDs are not case sensitive. 

lang 

Used only for strings. Can be any language using a two character code using the HTML ISO Country codes. First two are language and second two are country seperated by a hyphen. 

Sample Response if successful:
```http
{
"secret":"ball",
"iso8601":"2018-06-09T14:45:15Z"
}

{
"secret":"splendour",
"iso8601":"2018-06-09T14:45:15Z"
}

{
"secret":"4562",
"iso8601":"2018-06-09T14:45:15Z"
}

{
"secret":"сказать",
"iso8601":"2007-03-01T13:00:00Z"
}

{
"secret":"4AF",
"iso8601":"2018-06-09T14:45:15Z"
}
```
Secret

This will always be returned as a string. You may need to convert it to a number, or hex if that is what you want.

iso8601: This is the time in Coordinated Universal Time (UTC). The "T" seperates the date from the time. The "Z" is always at the end to show that it is universal. Z is the time designator for zero also called   (zulu). 


https://www.w3schools.com/TAgs/ref_language_codes.asp




# Trusted Transfer with Change Service

Unlike most services, data is Sent to Trusted Transfer Over Websockets.

Trusted transfer is designed to be a connection-orientated way to send CloudCoins from receiver to sender and allowing a trusted third-party server to authenticate the coins in between. 


Type:
1. authenticate_and_forward. Checks the coins to see if they are authentic and then sends them to the reciever.
2. just_change. Breaks the notes into smaller denominatoins. 
3. scramble. Exchanges a note for another note of the same denomination (just different serial number)

Internal modes of operation

The transfer can operate under four modes:

1. Both parties Anonymous. No one is logged in. (N) N for No trust. (Shared secret must be generated)
2. Sender is logged into server, receiver not trusted by Sender. (S) (Receiver can send a request for payment to the sender) 
3. Receiver logged in, Sender not trusted by Reciever.  (R) (Receiver gives sender the public address)
4. Both parties logged in.  (B) (Receiver can send request for payment with address of sender. No shared secret required because sender can send to public address)

The sender and receiver will need a  

```http
{
	"type":"authenticate_and_forward"
	"shared_secret":"magic",
	"trust_model":"n",
	"amount": 138,
	"payto":"Billy Jenkins",
	"sentby"="Sean Worthington",
	"fromaccountid":"c3c3ab7b75ab4d089d2d4a287c1ef232",
	"toaccountid":"daa1d6676f0ad818589138b6709cefae",
	"Memo"="For Yard Work",
	"iso8601":"2018-06-09T14:45:15Z",
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"14865168", 
		"an": ["39c054098b135bc31c1fc77c0f8b76bc", "6f3709e7533f9290a370ea53d57d1859", "e308b7bf5f12cb75b5f5bca7e5d2f5c4", "983b364ea602bd2be483664d0a83ba64", "ed510ce2b93980cf701a9514be5403da",
			"00e36769f732be5304c09c560b751d30", "f0479dd7e21248c976ac5bdfe489ae4f", "020d3f0a72647f021bbde13c0aeff55c", "054d806b15ea0c002fc07b2d019b9ab6", "4823f53b50b44c52cfba5dafb81db1fb",
			"cbe1bdf6971f78d333f828d593adae07", "007ee6f4e390b4551d2bc4bff3358c80", "43d091959de81a73edec5eb897095a16", "396f9f072689357254a91f0c17fd8f45", "ff46b0ed3ca3bb9679214164e035efe6",
			"86283df34c4d46ca8b16c456c63f2739", "278aa3025d9c464e99f718cadf3ce3a2", "f05618fe527b0eedc31547361cd0fdbb", "7ff63a99ff813d370dac6742b71342f8", "592ce3e049f651f2763d9b85b95c8a23",
			"5c1759904c63aceaf3b4023d494885f4", "264bf535577be1116bd14a7dce5dc3ea", "605ece6240c8892b638ecf50804e368f", "af718e6748598d321cbd276646b1b797", "3909c57b173f545566df124a22789294"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppepppppp",
		"aoid": []
		},
		{ 
		"nn":"1", 
		"sn":"12787478", 
		"an": ["818042c3595aa6f43c3dfe0242c378d7", "968fbd69c68fb07e663bdf8fcd56fef0", "8a9801667a8f5dce2c04800579c7e374", "4a0a60268c590378652435952c69cd86", "cb060d07e0bb6b249b78235b202ac742",
			"da2821f57dd59b779b851723c7e3495e", "daa1d6676f0ad818589138b6709cefae", "57e7a0b946c98879a0bc20a3d7f5db8e", "c3a9599496ed2ccfeecc21f2e12dd259", "7093c88e8c0a8a986a7a575e00933218",
			"5db00d5dbb5a1d6f2abc3bcf67e01515", "f0595cd3b62df316d70a17cf3cd96f1b", "bcc497a38df7574a31c48117668bb7f6", "de8c189bab949c9f0d7e77f4c0d090c6", "f5637e15e4a35dfc15d2462155a37dda",
			"1697658f59a6d6cd26d19b2a65897e4b", "a2265d79d3499012d1848d807c0a9ed3", "14e300371b7123bbe9400f65930c8806", "65f08b3b571b74685158840e768f7f40", "81c2c15923d0608b02742c37060a1f74",
			"18ac8cf467e483ff84ac76cd18f99a29", "d2d5abb8d03749a97d4eb19dbbbd2394", "5cf6112588aff427a95cb8ff9286b635", "ccbefaf705d223eb610eb6f59a7b9c8d", "1ed42144c91178330fc17fa0858609c3"],
		"ed":"11-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]
}	  
```
Message sent from Trusted Transfer Service to Receiver over websockets

If Authentication is good. Note that the only difference is an added "status"

```http
{
	"shared_secret":"magic",
	"status":"good",
	"trust_model":"n",
	"amount": 138,
	"payto":"Billy Jenkins",
	"sentby":"Sean Worthington",
	"fromaccountid":"c3c3ab7b75ab4d089d2d4a287c1ef232",
	"toaccountid":"daa1d6676f0ad818589138b6709cefae",
	"Memo":"For Yard Work",
	"iso8601":"2018-06-09T14:45:15Z",
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"14865168", 
		"an": ["39c054098b135bc31c1fc77c0f8b76bc", "6f3709e7533f9290a370ea53d57d1859", "e308b7bf5f12cb75b5f5bca7e5d2f5c4", "983b364ea602bd2be483664d0a83ba64", "ed510ce2b93980cf701a9514be5403da",
			"00e36769f732be5304c09c560b751d30", "f0479dd7e21248c976ac5bdfe489ae4f", "020d3f0a72647f021bbde13c0aeff55c", "054d806b15ea0c002fc07b2d019b9ab6", "4823f53b50b44c52cfba5dafb81db1fb",
			"cbe1bdf6971f78d333f828d593adae07", "007ee6f4e390b4551d2bc4bff3358c80", "43d091959de81a73edec5eb897095a16", "396f9f072689357254a91f0c17fd8f45", "ff46b0ed3ca3bb9679214164e035efe6",
			"86283df34c4d46ca8b16c456c63f2739", "278aa3025d9c464e99f718cadf3ce3a2", "f05618fe527b0eedc31547361cd0fdbb", "7ff63a99ff813d370dac6742b71342f8", "592ce3e049f651f2763d9b85b95c8a23",
			"5c1759904c63aceaf3b4023d494885f4", "264bf535577be1116bd14a7dce5dc3ea", "605ece6240c8892b638ecf50804e368f", "af718e6748598d321cbd276646b1b797", "3909c57b173f545566df124a22789294"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppepppppp",
		"aoid": []
		},
		{ 
		"nn":"1", 
		"sn":"12787478", 
		"an": ["818042c3595aa6f43c3dfe0242c378d7", "968fbd69c68fb07e663bdf8fcd56fef0", "8a9801667a8f5dce2c04800579c7e374", "4a0a60268c590378652435952c69cd86", "cb060d07e0bb6b249b78235b202ac742",
			"da2821f57dd59b779b851723c7e3495e", "daa1d6676f0ad818589138b6709cefae", "57e7a0b946c98879a0bc20a3d7f5db8e", "c3a9599496ed2ccfeecc21f2e12dd259", "7093c88e8c0a8a986a7a575e00933218",
			"5db00d5dbb5a1d6f2abc3bcf67e01515", "f0595cd3b62df316d70a17cf3cd96f1b", "bcc497a38df7574a31c48117668bb7f6", "de8c189bab949c9f0d7e77f4c0d090c6", "f5637e15e4a35dfc15d2462155a37dda",
			"1697658f59a6d6cd26d19b2a65897e4b", "a2265d79d3499012d1848d807c0a9ed3", "14e300371b7123bbe9400f65930c8806", "65f08b3b571b74685158840e768f7f40", "81c2c15923d0608b02742c37060a1f74",
			"18ac8cf467e483ff84ac76cd18f99a29", "d2d5abb8d03749a97d4eb19dbbbd2394", "5cf6112588aff427a95cb8ff9286b635", "ccbefaf705d223eb610eb6f59a7b9c8d", "1ed42144c91178330fc17fa0858609c3"],
		"ed":"11-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]
}	 
```
if counterfeit coins were sent or some other problem:

```http
{
	"shared_secret":"magic",
	"status":"fail",
	"trust_model":"n",
	"amount": 138,
	"payto":"Billy Jenkins",
	"sentby":"Sean Worthington",
	"fromaccountid":"c3c3ab7b75ab4d089d2d4a287c1ef232",
	"toaccountid":"daa1d6676f0ad818589138b6709cefae",
	"Memo":"For Yard Work",
	"iso8601":"2018-06-09T14:45:15Z",
	"cloudcoin": []
}	 
```

Message sent sender if the payment was sent successfully. Note that an array of CloudCoins may be attached assuming there is change due to the sender.

```http
{
	"receipt_id": "e054a34f2790fd3353ea26e5d92d9d2f",
	"shared_secret":"magic",
	"status":"success",
	"trust_model":"n",
	"amount": 138,
	"payto":"Billy Jenkins",
	"sentby":"Sean Worthington",
	"fromaccountid":"c3c3ab7b75ab4d089d2d4a287c1ef232",
	"toaccountid":"daa1d6676f0ad818589138b6709cefae",
	"Memo":"For Yard Work",
	"iso8601":"2018-06-09T14:45:15Z",
	"bank_server": "bank.CloudCoin.Global",
	"total_authentic": 5,
	"total_fracked": 7,
	"total_counterfeit": 1,
	"total_lost": 0,
	"receipt_detail": [{
			"nn.sn": "1.16777216",
			"status": "authentic",
			"pown": "ppppppppepppppppppppeppp",
			"note": "Moved to Bank"
		},
		{
			"nn.sn": "1:1425632",
			"status": "counterfeit",
			"pown": "fffffffffpfffffffffffffff",
			"note": "Sent to trash"
		},
		{
			"nn.sn": "1.956258",
			"status": "authentic",
			"pown": "ppppppppppppppppppppppppf",
			"note": "Moved to Fracked"
		},
		{
			"nn.sn": "1.15666214",
			"status": "lost",
			"pown": "pfpfpfpfpfpfpfpfpfpfpfpfp",
			"note": "Moved to Lost"
		},
		{
			"nn.sn": "1.15265894",
			"status": "lost",
			"pown": "ppppffpeepfpppfpfffpfffpf",
			"note": "STRINGS ATTACHED!"
		}

	]
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"14865168", 
		"an": ["39c054098b135bc31c1fc77c0f8b76bc", "6f3709e7533f9290a370ea53d57d1859", "e308b7bf5f12cb75b5f5bca7e5d2f5c4", "983b364ea602bd2be483664d0a83ba64", "ed510ce2b93980cf701a9514be5403da",
			"00e36769f732be5304c09c560b751d30", "f0479dd7e21248c976ac5bdfe489ae4f", "020d3f0a72647f021bbde13c0aeff55c", "054d806b15ea0c002fc07b2d019b9ab6", "4823f53b50b44c52cfba5dafb81db1fb",
			"cbe1bdf6971f78d333f828d593adae07", "007ee6f4e390b4551d2bc4bff3358c80", "43d091959de81a73edec5eb897095a16", "396f9f072689357254a91f0c17fd8f45", "ff46b0ed3ca3bb9679214164e035efe6",
			"86283df34c4d46ca8b16c456c63f2739", "278aa3025d9c464e99f718cadf3ce3a2", "f05618fe527b0eedc31547361cd0fdbb", "7ff63a99ff813d370dac6742b71342f8", "592ce3e049f651f2763d9b85b95c8a23",
			"5c1759904c63aceaf3b4023d494885f4", "264bf535577be1116bd14a7dce5dc3ea", "605ece6240c8892b638ecf50804e368f", "af718e6748598d321cbd276646b1b797", "3909c57b173f545566df124a22789294"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppepppppp",
		"aoid": []
		},
		{ 
		"nn":"1", 
		"sn":"12787478", 
		"an": ["818042c3595aa6f43c3dfe0242c378d7", "968fbd69c68fb07e663bdf8fcd56fef0", "8a9801667a8f5dce2c04800579c7e374", "4a0a60268c590378652435952c69cd86", "cb060d07e0bb6b249b78235b202ac742",
			"da2821f57dd59b779b851723c7e3495e", "daa1d6676f0ad818589138b6709cefae", "57e7a0b946c98879a0bc20a3d7f5db8e", "c3a9599496ed2ccfeecc21f2e12dd259", "7093c88e8c0a8a986a7a575e00933218",
			"5db00d5dbb5a1d6f2abc3bcf67e01515", "f0595cd3b62df316d70a17cf3cd96f1b", "bcc497a38df7574a31c48117668bb7f6", "de8c189bab949c9f0d7e77f4c0d090c6", "f5637e15e4a35dfc15d2462155a37dda",
			"1697658f59a6d6cd26d19b2a65897e4b", "a2265d79d3499012d1848d807c0a9ed3", "14e300371b7123bbe9400f65930c8806", "65f08b3b571b74685158840e768f7f40", "81c2c15923d0608b02742c37060a1f74",
			"18ac8cf467e483ff84ac76cd18f99a29", "d2d5abb8d03749a97d4eb19dbbbd2394", "5cf6112588aff427a95cb8ff9286b635", "ccbefaf705d223eb610eb6f59a7b9c8d", "1ed42144c91178330fc17fa0858609c3"],
		"ed":"11-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]

}
```

If the payment failed because of:
1. Not enough authentic CloudCoins were included.
2. Not enough RAIDA were contacted to complete the transaction
3. Receiver did not connect or lost contact. 
Note that there still may be some change or CloudCoins that were not sent to the receiver being returned. 

```http
{
	"receipt_id": "e054a34f2790fd3353ea26e5d92d9d2f",
	"shared_secret":"magic",
	"status":"fail",
	"message":"Not enough authentic CloudCoins were included.",
	"trust_model":"n",
	"amount": 138,
	"payto":"Billy Jenkins",
	"sentby":"Sean Worthington",
	"fromaccountid":"c3c3ab7b75ab4d089d2d4a287c1ef232",
	"toaccountid":"daa1d6676f0ad818589138b6709cefae",
	"Memo":"For Yard Work",
	"iso8601":"2018-06-09T14:45:15Z",
	"bank_server": "bank.CloudCoin.Global",
	"total_authentic": 5,
	"total_fracked": 7,
	"total_counterfeit": 1,
	"total_lost": 0,
	"receipt_detail": [{
			"nn.sn": "1.16777216",
			"status": "authentic",
			"pown": "ppppppppepppppppppppeppp",
			"note": "Moved to Bank"
		},
		{
			"nn.sn": "1:1425632",
			"status": "counterfeit",
			"pown": "fffffffffpfffffffffffffff",
			"note": "Sent to trash"
		},
		{
			"nn.sn": "1.956258",
			"status": "authentic",
			"pown": "ppppppppppppppppppppppppf",
			"note": "Moved to Fracked"
		},
		{
			"nn.sn": "1.15666214",
			"status": "lost",
			"pown": "pfpfpfpfpfpfpfpfpfpfpfpfp",
			"note": "Moved to Lost"
		},
		{
			"nn.sn": "1.15265894",
			"status": "lost",
			"pown": "ppppffpeepfpppfpfffpfffpf",
			"note": "STRINGS ATTACHED!"
		}

	]
	"cloudcoin": [
		{ 
		"nn":"1", 
		"sn":"14865168", 
		"an": ["39c054098b135bc31c1fc77c0f8b76bc", "6f3709e7533f9290a370ea53d57d1859", "e308b7bf5f12cb75b5f5bca7e5d2f5c4", "983b364ea602bd2be483664d0a83ba64", "ed510ce2b93980cf701a9514be5403da",
			"00e36769f732be5304c09c560b751d30", "f0479dd7e21248c976ac5bdfe489ae4f", "020d3f0a72647f021bbde13c0aeff55c", "054d806b15ea0c002fc07b2d019b9ab6", "4823f53b50b44c52cfba5dafb81db1fb",
			"cbe1bdf6971f78d333f828d593adae07", "007ee6f4e390b4551d2bc4bff3358c80", "43d091959de81a73edec5eb897095a16", "396f9f072689357254a91f0c17fd8f45", "ff46b0ed3ca3bb9679214164e035efe6",
			"86283df34c4d46ca8b16c456c63f2739", "278aa3025d9c464e99f718cadf3ce3a2", "f05618fe527b0eedc31547361cd0fdbb", "7ff63a99ff813d370dac6742b71342f8", "592ce3e049f651f2763d9b85b95c8a23",
			"5c1759904c63aceaf3b4023d494885f4", "264bf535577be1116bd14a7dce5dc3ea", "605ece6240c8892b638ecf50804e368f", "af718e6748598d321cbd276646b1b797", "3909c57b173f545566df124a22789294"],
		"ed":"6-2020",
		"pown":"ppppppppppppppppppepppppp",
		"aoid": []
		},
		{ 
		"nn":"1", 
		"sn":"12787478", 
		"an": ["818042c3595aa6f43c3dfe0242c378d7", "968fbd69c68fb07e663bdf8fcd56fef0", "8a9801667a8f5dce2c04800579c7e374", "4a0a60268c590378652435952c69cd86", "cb060d07e0bb6b249b78235b202ac742",
			"da2821f57dd59b779b851723c7e3495e", "daa1d6676f0ad818589138b6709cefae", "57e7a0b946c98879a0bc20a3d7f5db8e", "c3a9599496ed2ccfeecc21f2e12dd259", "7093c88e8c0a8a986a7a575e00933218",
			"5db00d5dbb5a1d6f2abc3bcf67e01515", "f0595cd3b62df316d70a17cf3cd96f1b", "bcc497a38df7574a31c48117668bb7f6", "de8c189bab949c9f0d7e77f4c0d090c6", "f5637e15e4a35dfc15d2462155a37dda",
			"1697658f59a6d6cd26d19b2a65897e4b", "a2265d79d3499012d1848d807c0a9ed3", "14e300371b7123bbe9400f65930c8806", "65f08b3b571b74685158840e768f7f40", "81c2c15923d0608b02742c37060a1f74",
			"18ac8cf467e483ff84ac76cd18f99a29", "d2d5abb8d03749a97d4eb19dbbbd2394", "5cf6112588aff427a95cb8ff9286b635", "ccbefaf705d223eb610eb6f59a7b9c8d", "1ed42144c91178330fc17fa0858609c3"],
		"ed":"11-2019",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}

	]

}
```


## Receive From Raida

Allows RAIDA to send Authenticity Numbers that can be combined and placed into accounts.The steps are: 

1. The Receiver Service validates the POST Parameters.
2. The Receiver Service sends a multi hints request to the Multi Hints service of the RAIDA to see if ticket is good. 
3. The Receiver Service stores the entire request in “Incoming” folder in and creates a order number subfolder there for the entire request. Within that order's folder, a folder is created for every coin serial number. The title of the names of the files will be The raida number, AN number and a ".txt" extension. 

Sample Files in the Imported/8772992/16777216/ (imported, OrderNumber, Serial Number)

```html
0.6fc740a03462463a8a417287a6996567.txt
1.29789830cc244322a702ebe9e0ce652a.txt
3.e33a71be3cff4f789389d59575877680.txt
.
.
.
23.dadd9adcbc524661ab7527a77029f0f3.txt
24.352a0355cd0141c1bd7c9e596991f53f.txt

```

The parameter "to_account_name_or_number" will either be an account name that is on the CloudServer or a serial number of a cloudcoin. The serial number is a serial number that the person who will claim the coin so that they can identifiy that the coins is meant for them. The idea is that anonymouse transactions can happen if the serial number is used. 

change_to_account_name_or_number could be "keep" as in keep the change. 

This request comes from the RAIDA. Note that the RAIDA generates a ticket so that the bank can check to see if it is authentic if the bank wants to. 



Sample POST request that includes 3 CloudCoins. 
```http
https://bank.cloudcoin.global/service/receive_from_raida?
order_number=99884&
raida=1&
to_account_name_or_number=8877676&
change_to_account_name_or_number=depository&
method=bank&
from_email=Sean@Worthington.net&
total_to_send=385&
memo=We love Pay Forward!

nn[]=1&
nn[]=1&
nn[]=1&

sn[]=16773897&
sn[]=16773898&
sn[]=16773899&

an[]=b25fc7a548c341c98cefbac35689aff1&
an[]=c6856721b1234696825156a7187f6256&
an[]=0157f12707cd4318aaa60e150514a839&

ticket[]=5AB9FA9D23324E52B8BAE707826870DA8760BD4F97ED&
ticket[]=CC8C977B96584DA3B8DCAEE4C018FB3DB96584DA3B8D&
ticket[]=B96584DA3B8D3F747E5EC78747CAA1E0A0EFF20A3916&

```
Response if success:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"paid",
"version":"2.0",
"message":"The CloudCoins were placed in the specified account name or number.",
"time":"2018-06-23T05:18:34.3025868Z"
}
```

Response if fail due to missing paramters:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Missing parameters. This request requires, NN, SN, AN, to_account_name_or_number,
change_to_account_name_or_number,method,from_email,total_to_send,memo",
"time":"2018-06-23T05:18:34.3025868Z"
}
```

Response if fail if account name does not exist:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Account: Account name does not exist on communication broker.",
"time":"2018-06-23T05:18:34.3025868Z"
}

Response if fail for any other reason:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Failed because put reason here",
"time":"2018-06-23T05:18:34.3025868Z"
}
```



## Retrieve Pay Forward Service

Allows a client to get money from a Communication Broker.  

The client must know the account name or number that they are using.

The client must first get a ticket to prove that they own that box. 

Actio is either Download or "bank" which means move it to thei bank. 


Sample GET request
```http
https://bank.cloudcoin.global/service/retrieve?
account_name_or_number=8877676&
from_email=Sean@Worthington.net&
ticket=5AB9FA9D23324E52B8BAE707826870DA8760BD4F97ED&
action=download
```
Response if success:
```http
{
	"cloudcoin": 
	[
		{ 
		"nn":"1", 
		"sn":"1353489", 
		"an": ["b78c68929dea65558e167976fcc31f60", "13985cb480d45ddb41f1b4601de359a3", "fcd842d59b8f526b720647196ef1d698", "5cde2c80ca0738181cac13dc9e07669c", "85f5a97c5deb6fab7b193ca89e4b4a27",
			"1401293332c26d29e703dcff1d9c8574", "f30e88c56e2ce873bc1b7566c4710291", "9a0fb5219971d3678b0ea46aff6d6b50", "73b515652b2d6d2cf10a084c51799e59", "865e234567f25feadb36d2bb1d33a641",
			"a7cde58b1e915beb4a96565fc01de8cc", "ae74d014288c4292a492158571b18e6c", "a6ef3f5e19af241bf3608553ddd06c3e", "157225a614f345630979fd77a4c7e425", "cab958c341d4406f42c6b40c5e1ad747",
			"dde3ac4586b572650de258b238dc3ff3", "8fb07919d15f572f7b7ce518a1cdd12c", "7068cea69910599c6b9f8e97d7422156", "8eba83834514b7c9e26cf44c5c81acb8", "70f3bf09ab4d49a2538e0d5a42fc3471",
			"906aef802ef9e98cc3048f1971d85ae1", "843c9b961687dd2b5bd5199581fc9a67", "a2df053780776cc4ccf46676d626c7b5", "0c54a99ac2eb5e9e3624580100aa33d2", "1289f5a6c6c4372d3de51b12372d524d"],
		"ed":"11-2020",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}
,
		{ 
		"nn":"1", 
		"sn":"1353490", 
		"an": ["b60c9f334d5073c4317eab5bd14e2055", "7752ab10ea43518c958b2c3c51bfbcd2", "f566c5637998c694e1270bd18fec802c", "fda1a04284c4ee2e937d2d55a8d7d245", "2d903b1f830061c0afc1e005ad855a81",
			"6a560829b5cf4868898d314ec6bde8b0", "6282c3d654c1fc06a7a9fdbdcd79c41b", "be31582115444e53a662020c934df5d8", "7007cfa54750fa889a055efd24aef195", "d6d5271085545dbe86e685bdcef031fa",
			"a74deccee27c15fcf50945c8b38fe23e", "40a2abcf692ec9c0674673921d10c4d7", "cfc97536b804dcc4a27913478874a4e0", "4c06bb21dd88f307c1b4e92b2d0dc0b9", "a837615c120de70b4ccf33478cd10177",
			"c144d923dd7d319a9eecdd7595a6ed84", "94a7ceb99d2c24bda65835991c563e5b", "744714bdb6ba06a8f58204916af9952b", "950396d59e7307625928d14530f01659", "4bd9ad35de4c345c59902adf8c557d94",
			"7fa42b7e25724c3de6b54dd5d054b066", "11c0cbefc3ffddb7c9cd7a4509f654e6", "195ade35b846570be01df9b99960b19b", "f5b3e678ae7b1d65347972b89fd1ea02", "78f3f866b544b6896466ad8037bc62fd"],
		"ed":"11-2020",
		"pown":"ppppppppppppppppppppppppp",
		"aoid": []
		}
	] 
}
```

Response if fail due to missing paramters:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Missing parameters. This request requires, NN, SN, AN, to_account_name_or_number,
change_to_account_name_or_number,method,from_email,total_to_send,memo",
"time":"2018-06-23T05:18:34.3025868Z"
}
```

Response if fail because ticket is bad:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Ticket: Ticket was bad.",
"time":"2018-06-23T05:18:34.3025868Z"
}


Response if fail if account name does not exist:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Account: Account name does not exist on communication broker.",
"time":"2018-06-23T05:18:34.3025868Z"
}

Response if fail for any other reason:
```http
{
"bank_server":"Bank.CloudCoin.Global",
"status":"fail",
"version":"2.0",
"message":"Failed because put reason here",
"time":"2018-06-23T05:18:34.3025868Z"
}
```





# END OF CLOUDBANK VERSION 1




# ADVANCED SERVICE THAT ARE PROPOSED

# TRANSFER TO OTHER BANK
Will transfer money from this bank to another. 

# REQUEST PAYMENT SERVICE
Requests payment from another bank

# ACCEPT REQUEST PAYMENT SERIVCE
Accepts a request for Payment from another bank




# TRANSACTION METHODS

## Merchant / Buyer Collaboration

1. Merchant Creates receipt

   The Merchant totals the amount due and generates a random receipt number. Then the Merchant creates a URL and gives it to the Buyer. 
   
   Sample URL Sent to Buyer from Merchant: https://bank.mydomain.com/pay/deposite?rn=ea22cbae0394f6c6918691f2e2f2e267
   
2. Buyer Deposites in bank

   Buyer Uses URL and attaches stack file: https://bank.mydomain.com/pay/deposite?rn=ea22cbae0394f6c6918691f2e2f2e267

3. Buyer shows Merchant Proof of Purchase

4. Merchant Confirms Purchase with CloudBank.

Sample: https://bank.mydomain.com/pay/get_receipt?rn=ea22cbae0394f6c6918691f2e2f2e267

## Buyer Initiated
1. Buyer Estimates Receipt
2. Buyer Deposites in bank
3. Buyer shows Merchant Proof of Purchase
4. Merchant Confirms Purchase with CloudBank.

## Merchant Dominates
1. Merchant Creates receipt
2. Buyer give Merchant CloudCoins
3. Merchant Deposits CC in CloudBank
4. Merchant Confirms Purchase with CloudBank.




# ###################
## Account Service
# ##################

*ACCOUNT REQUEST STRING*

This service will do many things:

1. Create an account for a new user including their username, password,  email and folders for the user if none exists.
2. Change the password (if a new password is provided)
3. Change the email address (if a new email address is provided)
4. Semd email to user saying that the account has been created.
5. Send email to user saying that the password and or email has been changed.
6. Send username and password if only the email is sent. 

This service also will change the password of an existing account, change the email of a user and send the username and password to the user.  This will create a folder names after the account and populate it with the following subfolders:

* Bank
* Broke
* Counterfeit
* Export
* Fracked
* Import
* Imported
* Logs
* Lost
* Suspect
* Templates
* Trash
* Waiting

**rules for user account names**
Your users must have a unique identifier that allows your system to identify the accounts within the CloudBank. We call this unique identifer the "Account ID." This number will become the name of folders within the CloudBank. This folder could be a number (like a customer number) or a GUID or any thing that you use to uniquly identify your users in your system. 
However, there are some rules: 
Your Account ID must work as folder names for both Windows and Linux Operating sytems.
Account IDs cannot contain any of the following characters:
```
    \ / : * ? " ' < > | 
```
Your CloudBank may treat your Account identifiers as either case sensitive or case insensitive. This depends on if your CloudBank is hosted on a Linux (PHP) or Windows (C#) System. 


## New Account

Allows the administrator of the bank to create a new account

POST:
```http
https://bank.cloudcoin.global/service/new_account?admin_password=e24b3a755916472f8768e4e9992827a&account_name=newcompany&pw=74307d8442f54763ba6ffab7fdc9b610

```

Note: If the account has already been created the API just says success. "newcompany" will be replaced with the actual name of the account. 
Response if success:
```http
{
	"server": "www.myBank.com",
	"status": "added",
	"message": "Account was created for user newcompany.",
	"time": "2016-40-21 10:40:PM"
}
```

Response if fail because password is not strong enough:
 "newcompany" will be replaced with the actaul name of the account. 
```http
{
	"server": "www.myBank.com",
	"status": "notadded",
	"message": "Password not strong enough. Account was not created for user newcompany.",
	"time": "2016-40-21 10:40:PM"
}
```

Response if fail because account name exists:
 "newcompany" will be replaced with the actaul name of the account. 
```http
{
	"server": "www.myBank.com",
	"status": "notadded",
	"message": "Exists. Account was not created for user newcompany because the account already exists.",
	"time": "2016-40-21 10:40:PM"
}
```
Response if fail because account name does not meet requirements:
 "newcompany" will be replaced with the actaul name of the account. 
```http
{
	"server": "www.myBank.com",
	"status": "notadded",
	"message": "Nameing Violation. Account was not created for user newcompany because the name proposed did not meet requirements.",
	"time": "2016-40-21 10:40:PM"
}
```








# FOLDER STRUCTURE




Folder Structure
<pre>
UserAccounID
-Bank
-Broke
-Counterfeit
-Export
-Fracked
-Import
-Imported
-Logs
-Lost
-Suspect
-Templates
-Trash
-Waiting
</pre>



# ##################
## Change_Owner Account Service :
# ##################
Requests that a CloudCoin note change ownership from one account to another.

*CHANGE REQUEST STRING*

https://cloudcoin.global/bank/change_owner.php?nn=1&sn=15489521&newid=273C9DFA8061407AB8102C0A4E872CA3


*CHANGE RESPONSE STRING*

if success
```http
{
    "server": "www.myBank.com",
	"status": "change",
	"message": "Change Complete",
	"time": "2016-40-21 10:40:PM"
}
```
if fail
```http
{
    "server": "www.myBank.com",
	"status": "fail",
	"message": "Change Failed, No such account",
	"time": "2016-40-21 10:40:PM"
}
```






# ##################
## Issue Reference Service 
# ##################
The Issue Reference service issues a "check" that refers to a real cloudcoin in the bank. 
The purpose is to allow CloudCoins to circulate amoung the bank customers without having 
to authenticate with the RAIDA everytime they change hands instead they will authenticate 
with the bank.

CloudCoin check format:

Any file can store a CloudCoin check because the CloudCoin check is stored in the file name.
However, it is best to have a txt (text) file with a .check extension. The text file keep
he size of the file small.

Example of a CloudCoin check embedded in a file name:

```http
250.CloudCoin.1.16777216.cb5e46ce270545b39b5efa9d9e199d93.www.myBank.com.2017.05.17.13.45.Any user memo here less 
than 155 characters.check
```

Denomination: For human reading.
CloudCoin: Litteral string for human reading
SN: Serial Number
NN: Network Number
AN: Single authenticity number (GUID with no hyphens) 
Bank Server URL: A DNS name or IP address of an application that can can turn the check into a 
real CloudCoin
Year:Year
Month
Day
Hour
Minute
Memo: 
Inside you can place a memo too. 


*ISSUE_REFERENCE REQUEST STRING*

https://cloudcoin.global/bank/issue_reference.php?nn=1&sn=88772322&method=1

*ISSUE_REFERENCE RESPONSE STRING*
```http
{
	"server": "www.myBank.com",
	"status": "success",
	"name":"250.CloudCoin.1.16777216.cb5e46ce270545b39b5efa9d9e199d93.www.myBank.com",
	"message": "CloudCoin note issued as reference.",
	"time": "2016-40-21 10:40:PM"
}
```
if bad:
```http
{
	"server": "www.myBank.com",
	"status": "fail",
	"name":"",
	"message": "CloudCoin not found. No reference can be made.",
	"time": "2016-40-21 10:40:PM"
}
```

# ##################
## Deposit Reference Service 
# ##################

*DEPOSIT_REFERENCE REQUEST STRING*

https://cloudcoin.global/bank/deposit_reference.php?an=cb5e46ce270545b39b5efa9d9e199d93

*DEPOSIT_REFERENCE RESPONSE STRING*
```http
{
	"server": "www.myBank.com",
	"status": "success",
	"name":"250.CloudCoin.1.16777216.cb5e46ce270545b39b5efa9d9e199d93.www.myBank.com",
	"message": "CloudCoin note issued as reference.",
	"time": "2016-40-21 10:40:PM"
}
```
if bad:
```http
{
	"server": "www.myBank.com",
	"status": "fail",
	"name":"",
	"message": "CloudCoin not found. No reference can be made.",
	"time": "2016-40-21 10:40:PM"
}
```

# ##################
## Service Detect Reference
# ##################

https://RAIDA20.cloudcoin.global/bank/detect_reference?nn=1&sn=16777216&an=1836843d928347fb22c2142b49d772b5&pan=1836843d928347fb22c2142b49d772b5

*Detection Response Example If Passed:*
```http
{
  "server":"www.myBank.com",
  "status":"pass",
  "sn":"16777216",
  "nn":"1",
  "message":"Authentic:16777216 is an authentic 1-unit. Your Proposed Authenticity Number is now the new Authenticate Number. Update your file.",
  "time":"2016-44-19 7:44:PM"
}
```
Note that the 1 after the word Authentic: is the serial number of the unit that was tested.
 
*Detection Response Example If failed to authenticate:*
```http
{
  "server":"www.myBank.com",
  "status":"fail",
  "sn":"16777216",
  "nn":"1",
  "message":"Counterfeit: The unit failed to authenticate on this server. You may need to fix it on other servers.",
  "time":"20"
}
```


# Send Secure Message 
Allows the bank to recieve a secure message like a check payment

# Recieve Secure Message



END OF API




