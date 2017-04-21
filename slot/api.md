# API 說明文件
## 目錄
1. [註冊帳號](#註冊帳號)
1. [取得登入連結](#取得玩家登入網址)
1. [登入](#登入)
1. [查詢玩家](#查詢玩家)   
2. [取得玩家遊戲紀錄](#取得玩家遊戲紀錄)
3. [玩家離線通知](#玩家離線通知)
4. [玩家額度轉出入](#玩家額度轉出入)
5. [玩家轉帳狀態查詢](#玩家轉帳狀態查詢)
6. [踢玩家](#踢玩家)
7. [踢多玩家](#踢多玩家)
8. [限輸](#限輸)
9. [限贏](#限贏)
10. [限注回復](#限注回復)
11. [設定玩家帳號模式](#設定玩家帳號模式)
12. [注單回補](#注單回補)
13. [限注查詢](#限注查詢)
14. [查詢玩家帳號模式](#查詢玩家帳號模式)
15. [查詢玩家是否啟用遊戲](#查詢玩家是否啟用遊戲)
16. [設定玩家是否啟用遊戲](#設定玩家是否啟用遊戲)
17. [LOG查詢](#LOG查詢)
18. [玩家下注簡報查詢](#玩家下注簡報查詢)
19. [玩家多人下注簡報區間總額查詢](#玩家多人下注簡報區間總額查詢)
20. [玩家JP紀錄查詢](#玩家JP紀錄查詢)
21. [玩家多人JP紀錄查詢](#玩家多人JP紀錄查詢)
22. [JP中獎紀錄](#JP中獎紀錄)
23. [玩家JP核銷](#玩家JP核銷)



## 登入流程
1. 玩家透過平台登入
2. 平台透過[註冊帳號](#register)新增帳號
3. 平台透過[取得玩家登入網址](#auth)取得帳號密碼
4. 透過表單直接將取得的帳號密碼送到[登入](#login)驗證
5. API直接將玩家導向至遊戲網頁

[流程圖連結](https://cacoo.com/diagrams/UQAlsLczvUqmOvl6)

![流程圖](https://cacoo.com/diagrams/UQAlsLczvUqmOvl6-28811.png?t=1455870225441 "API 登入 流程圖")

## GameType
| 遊戲名稱  | GameType                  | 
|---------- |-------------------------  |
| 武士道         | 1   | 
| 金錢貓         | 2   | 

## 玩家模式說明

|模式|說明|
|:---:|:---|
|0|正常|
|1|鎖單無法下注|
|2|封鎖無法登入|

## 錯誤代碼

| 錯誤代碼  | 錯誤訊息                  | 錯誤說明              |
|---------- |-------------------------  |---------------------- |
| 1         | \{parameter\} is required   | 缺少必填參數            |
| 2         | key is invalid            | 提供的金鑰是不合法的    |
| 3         | hash is invalid           | 驗證碼是錯誤的           |
| 4         | player not found            | 找不到合法的使用者     |
| 5         | \{method\} is not allowed   | http method 不允許       |
| 6         | query time range out of limit   | 查詢時間範圍超出限制   |
| 7         | internal server error   | 伺服器內部錯誤       |
| 8         | player is offline   | 使用者離線       |
| 9	         | player is online	| 使用者在線中 |
| 10        | service not available   | 無法使用遊戲服務       |
| 11        | \{parameter\} is invalid   | 參數不合法       |
| 12 		  | gameType not found | 無此遊戲存在 |
| 13 | account length between 4 - 20 | 帳號至少4位元以上 | 
| 14 | nickname length between 1- 20 | 暱稱至少1位元以上 | 
| 15 | enable setting is invalid | 玩家啟用設定值必須為整數 | 
| 16 | mode setting is invalid | 玩家帳號模式設定值必須為整數 | 
| 17 | transfer in error | 額度轉入失敗 | 
| 18 | transfer out error | 額度轉出失敗 | 
| 19 | player credit is not enough | 玩家籌碼不足 | 
| 20 | range :2017-01-01 00:00:00 to 2017-01-02 00:00:00 not found bet results | 查詢結果無注單 | 
| 21 | bet accounts processing | 帳務結算中不可額度轉出入 | 
| 22 | {param} must be a unsigned int | {param}設定值必須為正整數 | 
| 23 | [startAt or endAt] value must be datetime Example:2016-01-01 00:00:00 | 查詢玩家注單,時間參數必須使用規定格式 | 
| 24 | {transferId} is not exist | 此筆交易單不存在 | 
| 25 | transfer id credit can not be empty | 交易的ID不可為空 | 
| 26 | transfer id:（平台方TransferID）has been used | 此交易單已被使用 |
| 27 | transfer in or out can not be 0 | 額度轉出入設定值不可為0 |
| 101 | jackpot log not found | 找不到 jackpot 記錄 |


## API
1. ### <span id="register">註冊帳號</span>

    ```
    POST player/register?
        key=<key>&
        account=<account>&
        nickname=<nickname>&
        hash=<hash>
    ```

    ##### Request 範例
    
	bash
	
    ```bash
    CURL -X POST -d account=test -d nickname=測試 -d key=57d0bc61dffff -d hash=106c9b6891a82e9af6d404f4a9707341 \
      -G http://poker.app/api/v2/slot/player/register
    ```
    
    php
    
    ```php
    <?php
    $key = '57d0bc61dffff';
    $secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
    $url = 'http://poker.app/api/v2/slot/player/register';
    $data = [
        'account' => 'test',
        'nickname' => '測試',
    ];
    //產生hash
    $hash = '';
    foreach ($data as $k => $v) {
        $hash .= $v;
    }
    $hash .= $secret;

    $hash = md5($hash);
    $data['key'] = $key;
    $data['hash'] = $hash;
    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
    curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));

    $response = curl_exec($ch);

    echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:-----------:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string(20)  |     必填，4-20個字元    | Y |
    |   nickname   | 玩家暱稱 |  string(20)  |     必填，1-20個字元    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + nickname + secret)**

    ##### 回傳結果
    成功
    
    ```javascript
    {  
       "status":"success",
       "data":{  
          "account":"qoo@gmail.com",
          "nickname":"qoo",
          "id":34
       }
    }
    ```
    
    失敗
    
    ```javascript
    {  
       "status":"error",
       "error":{  
          "code":1,
          "message":"account is required"
       }
    }
    ```
    
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account| string(20) | 帳號|
    |nickname| string(20) | 玩家暱稱 |
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    | 14 | nickname length between 1- 20  |

2. ### <span id="auth">取得玩家登入網址</span>

    ```
    GET player/auth?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```

    ##### Request 範例

	bash
	
    ```bash
    CURL -X GET -d account=test -d key=57d0bc61dffff -d hash=e2c7671a5d6334fa358172b4f9144cf1 \
      -G http://poker.app/api/v2/slot/player/auth
    ```
    
    php
    
    ```php
    <?php
    $key = '57d0bc61dffff';
    $secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
    $url = 'http://poker.app/api/v2/slot/player/auth';
    $data = [
        'account' => 'test',
    ];
    //產生hash
    $hash = '';
    foreach ($data as $k => $v) {
        $hash .= $v;
    }
    $hash .= $secret;

    $hash = md5($hash);
    $data['key'] = $key;
    $data['hash'] = $hash;
    $ch = curl_init($url.'?'.http_build_query($data));
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");

    $response = curl_exec($ch);
    echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |Y|
    |  account | 玩家帳號 |  string  |     必填    | Y|   
    |   hash   | 驗證參數 |  string  |     必填    |Y|

    **hash = md5(account + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "loginUrl":"http:\/\/feature_api.dev\/api\/v2\/slot\/player\/login?token=eyJpdiI6IlU4bmFUOEJiNnpneFdVd3RkQjNtd3c9PSIsInZhbHVlIjoiUWhDUXZwRVduRHdIK25NK2kzYjdCZ2FFKzUwclRwd1BYcEszOTdxOHBRTHNVc2E5OVwvcDE5TmRYMjNxTzZ2dHVldzRUSFhLVHhkVEJOSHgzNVAwXC80cUNjeHQzME05clp2MG9JeVRMQTNPbzV0SWNvZGRyeGVCaWhOQWp2V0prRUlLSXgydkMyTkdCV1lWQ3ArMWxqNmk3WG1BOE83S1o4SVwveWxjeXMwbDl1amR6VXJiMU1TN1wvaVBKZXdLSzllbHE5eitnaGFRV3BWdFwvd25YYVZUUWljbWRnbWpJdVVxM1pnZk5qNUlXSXVYcWU0UTJjZVpwNGthWXRVUGdBVnZ6dlNoczVIem9HZThCNUl6WUJ2QzN6QT09IiwibWFjIjoiMTIxMmNjMDY2ZmJjYzg3NjFlMzQzZmFiN2VmNjc3ZDcyNGI5NTZkNmZhZDY0YzZhYmZjYWNhYjk1YzM2MGM2NCJ9"
       }
    }     

    ```
    
    失敗
    
    ```javascript
    {  
       "status":"error",
       "error":{  
          "code":1,
          "message":"account is required"
       }
    }
    ```
    
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |loginUrl|string|玩家登入用網址|
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found          |    
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |

3. ### <span id="login">登入</span>



    GET 由[取得玩家登入網址](#取得玩家登入網址)回傳的loginUrl

    ##### 回傳結果
    成功
    
    ```
    直接導向遊戲頁
    ```
    
    失敗
    
    ```
    導向登入失敗頁
    ```

4. ### <span id="user">查詢玩家</span>

    ```
    GET player/info?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    ##### Request 範例
	
	bash
	
    ```bash
    CURL -X GET -d account=test -d key=57d0bc61dffff -d hash=e2c7671a5d6334fa358172b4f9144cf1 \
      -G http://poker.app/api/v2/slot/player/info
    ```
    
    php

    ```php
    <?php
    $key = '57d0bc61dffff';
    $secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
    $url = 'http://poker.app/api/v2/slot/player/auth';
    $data = [
        'account' => 'test',
    ];
    //產生hash
    $hash = '';
    foreach ($data as $k => $v) {
        $hash .= $v;
    }
    $hash .= $secret;

    $hash = md5($hash);
    $data['key'] = $key;
    $data['hash'] = $hash;
    $ch = curl_init($url.'?'.http_build_query($data));
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");

    $response = curl_exec($ch);
    echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "id":30,
          "account":"mm@gmail.com",
          "nickname":"mm",
          "credit":0,
          "loginAt":null,
          "logoutAt":null,
          "mode":0,
          "enable":1,
          "limitWin":11111,
          "limitLose":30000,
          "isOnline":false
       }
    }
    ```
    失敗

    ```javascript
    {  
       "status":"error",
       "error":{  
          "code":1,
          "message":"account is required"
       }
    }
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string|玩家帳號|
    |nickname|string|玩家暱稱|
    |credit|int|玩家目前額度|
    |mode|int|玩家目前模式 0:正常 1:鎖單無法下注 2:封鎖無法登入|
    |enable|int|是否啟用 0:停用 1:啟用|
    |limitWin|int|限贏 0為無限制|
    |limitLose|int|限輸 0為無限制|
    |isOnline|boolean|玩家是否在上線|
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found          |        
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |

    
5. ### <span id="bet-report-session">取得玩家遊戲紀錄</span>

    ```
    GET player/bet/report/session?
        key=<key>&
        account=<account>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X GET -d account=test -d startAt=0 -d endAt=0 -d key=57d0bc61dffff -d hash=deabb032a98cf55352f4126221e00117 \
    	-G http://poker.app/api/v2/slot/player/bet/report/session
    ```
    
    php
    
    ```php
    $key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/bet/report/session';
	$data = [
		'account'=>'test',
		'startAt'=>'0',
		'endAt'=>'0'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```

    ##### 參數說明

    | 參數名稱  | 參數說明  | 參數型態  | 說明                            | 必填 |
    |---------- |---------- |---------- |------------------------------ |---|
    | key       | 服務金鑰  | string(20)    | 由API端提供                   | Y |
    | account   | 玩家帳號  | string(20)    | 必填                            | Y |
    | startAt  | 開始時間  | string    | 固定格式Y-m-d,Y-m-d H:i:s或者0，0代表不限制，多查詢七天內的資料  | Y |
    | endAt    | 結束時間  | string    | 固定格式Y-m-d,Y-m-d H:i:s或者0 ，代表不限制，最多查詢七天內的資料 | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
	   "status":"success",
	   "data":[  
	      {  
	         "id":145,
	         "userId":19,
	         "initialCredit":1,
	         "sumOfBet":0,
	         "sumOfWinCredit":0,
	         "finalCredit":1,
	         "ip":"192.168.11.1",
	         "loginAt":"2017-04-20 10:22:18",
	         "logoutAt":"2017-04-20 10:22:28",
	      }
	   ]
	}
    ```
    失敗

    ```javascript
    {  
       "status":"error",
       "error":{  
          "code":1,
          "message":"endAt is required"
       }
    }
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    | initialCredit |int|初始金額|
    | sumOfBet |int|下注金額|
    | sumOfWinCredit |int|贏取金額，損益為 sumOfWinCredit - sumOfBet|
    | finalCredit |int|最後籌碼|    
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found 			|
    | 5  | {method} is not allowed   |
    | 6  | query time range out of limit  |
    | 7  | internal server error |
    | 11 | {parameter} is invalid   |
    
        
6. ### <spin id="offline_notification">玩家離線通知</spin>

    玩家離線通知需要在後台設定callback url，離線通知會透過`POST`的方式傳送資料，目前會附帶以下資料。
    
    ```
    id=<id>&                            //紀錄id (int)
    ip=<ip>&                            //玩家ip (string)
    loginAt=<loginAt>&                 //玩家登入時間 (string)
    logoutAt=<logoutAt>&               //玩家登出時間 (string)
    account=<account>&                   //玩家帳號 (string)
    key=<key>&                          //服務金鑰 (string)
    account=<account>&                  //玩家帳號(string)
    token=<token>&                      //Session Token(string)
    paid=<paid>&                        //玩家淨賠(int)
    result=<result>&                    //玩家損益(int)
    hash=<hash>                         //驗證參數(string)
    ```
    您可以驗證callback參數，獲得更好的安全性。
    
    **hash = md5(account + token + id + paid + result + secret)**

    另外請您回傳一組 Json，格式如下：
    成功
     ```javascript
    {"status":"success","message":"<自訂訊息>"}
    ```           
    失敗
     ```javascript
    {"status":"error","message":"<自訂訊息>"}
    ```   
	
7. ### <spin id="credit_transfer">玩家額度轉出入</spin>

    ```
    PUT player/credit/transfer?
        key=<key>&
        account=<account>&
        credit =<credit>&
        transfer_id = <transfer_id>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X PUT -d account=test -d credit=1 -d transfer_id=GC0001 -d key=57d0bc61dffff -d hash=f28a85dd168db1089dc2ae2b1c033424 \
		-G http://poker.app/api/v2/slot/player/credit/transfer
    ```
    
    php
    
    ```php
    $key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/credit/transfer';
	$data = [
		'account'=>'test',
		'credit'=>'1',
		'transfer_id'=>'GC0001'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string(20)  |     必填    | Y |
    |  credit   | 增加的籌碼 |  int  |     必填, 負號代表轉出    | Y |
    |  transferId   | 交易編號 |  string(30)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + credit + transfer_id + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "originalCredit":0,
          "addedCredit":3000,
          "finalCredit":3000,
          "account":"mm@gmail.com",
          "orderId":2,
          "transferId":"fsefs",
          "status":0
       }
    }
    ```
    失敗

    ```javascript
    {  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"user not found"
	   }
	}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string|玩家帳號|  
    |originCredit|int|原始籌碼|
    |addCredit|int|新增籌碼|
    |finalCredit|int|最後籌碼|
    |transferId|string(30)|平台交易編號，unique|
    |orderId|int|遊戲方交易編號, unique|
    |status|int|狀態 0:success, 1:餘額不足, 2:伺服器錯誤, 3:訂單交易中|  
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found 			|    
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    |19 | player credit is not enough|


      
7. ### <spin id="transfer-status">玩家轉帳狀態查詢</spin>

    ```
    GET player/credit/transfer-status?
        key=<key>&
        transferId =<transferId>&
        hash=<hash>
    ```
	
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d transfer_id=GC0001 -d key=57d0bc61dffff -d hash=eb5adf3a54d19488c9d1b641cd582333 \
 		-G http://poker.app/api/v2/slot/player/credit/transfer-status
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/credit/transfer-status';
	$data = [
		'transfer_id'=>'GC0001'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  transferId   | 交易編號 |  string(30)  |     必填    | Y |   
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + transferId + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "originalCredit":0,
          "addedCredit":3000,
          "finalCredit":3000,
          "account":"mm@gmail.com",
          "orderId":2,
          "transferId":"testTransferId",
          "status":0
       }
    }
    ```
    失敗

    ```javascript
    {  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"user not found"
	   }
	}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|  
    |originCredit|int|原始籌碼|
    |addCredit|int|新增籌碼|
    |finalCredit|int|最後籌碼|
    |transferId|string(30)|平台交易編號，unique|
    |orderId|int|遊戲方交易編號, unique|
    |status|int|狀態 0:success, 1:餘額不足, 2:伺服器錯誤, 3:訂單交易中|
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
	| 24 | {transferId} is not exist |
    

8. ### <spin id="kick">踢玩家</spin>

    呼叫之後會在10秒之後將在線的玩家踢出遊戲
    
    ```
    DELETE player/kick?
        key=<key>&
        account=<account>&
        reason=<reason>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X DELETE -d account=test -d reason=test -d key=57d0bc61dffff -d hash=ca0ac30b539a6a55ecb5bc7c95b95c48 \
 		-G http://poker.app/api/v2/slot/player/kick
    ```
    
    php
    
    ```php
    $key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/kick';
	$data = [
		'account'=>'test',
		'reason'=>'test'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "DELETE");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account| 玩家帳號 |  string(20)  |     必填   | Y |
    |  reason  | 剔除原因 |  string(50)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + reason + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "status":0,
             "account":"haha0738@ifalo.com.tw"
          }
       ]
    }

    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string|玩家帳號|
    |status|int| 狀態0:kick success, 1:player is not online |
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found |
    | 5  | {method} is not allowed   |
    |  7  | internal server error | 
	| 10 | service not available |
    | 11 | {parameter} is invalid   |

        
8. ### <spin id="kick-multiple">踢多玩家</spin>
    呼叫之後會在10秒之後將在線的玩家踢出遊戲
    
    ```
    DELETE player/kick-multiple?
        key=<key>&
        accounts=<accounts>&
        reason=<reason>&
        hash=<hash>
    ```
	
	##### Request 範例
	
	bash
	
	```bash
	CURL -X DELETE -d accounts=test,test01 -d reason=test -d key=57d0bc61dffff -d hash=22f0d24fa07d3e73c0c23c2626fe052e \
 		-G http://poker.app/api/v2/slot/player/kick-multiple
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/kick-multiple';
	$data = [
		'accounts'=>'test,test01',
		'reason'=>'test'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "DELETE");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
	```
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  accounts| 玩家帳號 |  string  |     必填，可填多組用`,` 分割   | Y |
    |  reason  | 剔除原因 |  string(50)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + reason + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "account":"kk",
             "status":0
          },
          {  
             "account":"aa",
             "status":1
          },
          {  
             "status":2,
             "account":"haha0738@ifalo.com.tw"
          }
       ]
    }
    ```
    失敗

    ```javascript
	{  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"service not available"
	   }
	}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    |status|int|狀態0:kick success, 1:player is not online 2:player not found|
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error | 
	| 10 | service not available |
    | 11 | {parameter} is invalid   |


9. ### <span id="lose-limit">限輸</span>

    設定玩家限輸

    ```
    PUT player/limit/lose?
        key=<key>&
        account=<account>&
        limit=<limit>&
        hash=<hash>
    ```
	##### Request 範例
	
	bash
	
	```bash
	CURL -X PUT -d account=test -d limit=10000 -d key=57d0bc61dffff -d hash=26dd56166f6933f6699c7118231dcb73 \
 		-G http://poker.app/api/v2/slot/player/limit/lose
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/limit/lose';
	$data = [
		'account'=>'test',
		'limit'=>'10000'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
	```
	
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string(20)  |     必填    | Y |
    |  limit   | 限制金額 |  int  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account+limit+secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "account":"haha0738@ifalo.com.tw",
          "limitLose":500,
          "limitWin":"5000"
       }
    }
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    |limitLose|int|限輸金額|
    |limitWin|int|限贏金額|    
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |


10. ### <span id="win-limit">限贏</span>

    設定玩家限贏

    ```
    PUT player/limit/win?
        key=<key>&
        account=<account>&
        limit=<limit>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X PUT -d account=test -d limit=10000 -d key=57d0bc61dffff -d hash=26dd56166f6933f6699c7118231dcb73 \
 		-G http://poker.app/api/v2/slot/player/limit/win
	 ```
	 
	 php
	 
	 ```php
	 $key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/limit/win';
	$data = [
		'account'=>'test',
		'limit'=>'10000'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
	```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string(20)  |     必填    | Y |
    |  limit   | 限制金額 |  int  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account+limit+secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "account":"haha0738@ifalo.com.tw",
          "limitLose":500,
          "limitWin":"5000"
       }
    }
    ```
    失敗

    ```javascript
	{  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"user not found"
	   }
	}
    ```
    
    回傳參數說明    
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    |limitLose|int|限輸金額|
    |limitWin|int|限贏金額| 
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |    
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
 

11. ### <span id="limit-recover">限注回復</span>

    限注回復

    ```
    PUT player/limit/recover?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X PUT -d account=test -d key=57d0bc61dffff -d hash=e2c7671a5d6334fa358172b4f9144cf1 \
 		-G http://poker.app/api/v2/slot/player/limit/recover
    ```
    
    php
    
    ```php
    $key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/limit/recover';
	$data = [
		'account'=>'test'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string(20)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account+secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "account":"mm@gmail.com",
          "winCredit":0
       }
    }
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    
    回傳參數說明
    
    | 參數 | 型態 | 說明 |
    |:---:|:---:|:---:|
    | account |string(20)| 玩家帳號 |
    | winCredit |int| 回復後損益 |
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |    
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |

    
12. ### <span id="ban-mode">設定玩家帳號模式</span>

    玩家封鎖模式

    ```
    PUT player/mode?
        key=<key>&
        account=<account>&
        mode=<mode>&        
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash  
	CURL -X PUT -d account=test -d mode=1 -d key=57d0bc61dffff -d hash=4e7efef29cb3127cb97859bdc1826daa \
 		-G http://poker.app/api/v2/slot/player/mode
    ```
    
    php
    
    ```php
    $key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/mode';
	$data = [
		'account'=>'test',
		'mode'=>'1'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account| 玩家帳號 |  string(20)  |     必填  | Y |
    |   mode | 模式 |  int    |     必填([詳細說明](#玩家模式說明)) | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account+mode+secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "account":"haha0738@ifalo.com.tw",
             "mode":"banned"
          }
       ]
    }
    ```
    失敗

    ```javascript
	{  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"user not found"
	   }
	}
    ```
    
    
     回傳參數說明
    
    | 參數 | 型態 | 說明 |
    |:---:|:---:|:---:|
    | account |string(20)| 玩家帳號 |
    | mode |int| 玩家模式([詳細說明](#玩家模式說明)) |
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |        
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |


14. ### <span id="limit-query">限注查詢</span>

    查詢玩家限注狀態

    ```
    GET player/limit?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d account=test -d key=57d0bc61dffff -d hash=e2c7671a5d6334fa358172b4f9144cf1 \
 		-G http://poker.app/api/v2/slot/player/limit
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/limit';
	$data = [
		'account'=>'test'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```
	
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string(20)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "account":"mm@gmail.com",
          "winCredit":0,
          "limitWin":11111,
          "limitLose":30000,
          "updatedAt":"2017-04-13 11:03:44"
       }
    }
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    回傳參數說明    
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    |limitLose|int|限輸金額|
    |limitWin|int|限贏金額|    
    | winCredit |int|損益|     
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |        
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |

15. ### <span id="get-ban-mode">查詢玩家帳號模式</span>

    查詢玩家鎖單/停用狀態

    ```
    GET player/mode?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    ##### Request  範例
    
    bash
    
    ```bash
    CURL -X GET -d account=test -d key=57d0bc61dffff -d hash=e2c7671a5d6334fa358172b4f9144cf1 \
	 	-G http://poker.app/api/v2/slot/player/mode
 	```
 	
 	php
 	
 	```php
 	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/mode';
	$data = [
		'account'=>'test'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```

   ##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
	|  account| 玩家帳號 |  string(20)  |     必填，支援多組帳號可用`,`分割   | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

   	**hash = md5(account + secret)**

  	##### 回傳結果
    
  	成功

	```javascript
	{  
	   "status":"success",
	   "data":[  
	      {  
	         "account":"test-api01",
	         "mode":0
	      }
	   ]
	}
	```
    
   失敗
	
	```javascript
	{"status":"error","error":{"code":4,"message":"user not found"}}
	```
    
	回傳參數說明    
    
	|參數|型態|說明|
	|:---:|:---:|:---:|
	|account|string(20)|玩家帳號|
	|mode|int|模式([詳細說明](#玩家模式說明))|    
    
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
	| 錯誤代碼 | 錯誤說明 |     
	|:--------:|:--------:|
	| 1  | {parameter} is required   |
	| 2  | key is invalid            |
	| 3  | hash is invalid           |
    | 4 | player not found  |    	
	| 5  | {method} is not allowed   |
	|  7  | internal server error |
	| 11 | {parameter} is invalid   |

16. ### <span id="get-activation">查詢玩家是否啟用遊戲</span>

    查詢玩家是否啟用遊戲

    ```
    GET player/enable?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X GET -d account=test -d key=57d0bc61dffff -d hash=e2c7671a5d6334fa358172b4f9144cf1 \
 		-G http://poker.app/api/v2/slot/player/enable
    ```
    
    php
    
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/enable';
	$data = [
		'account'=>'test'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
		
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```
    

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account| 玩家帳號 |  string(20)  |     必填，支援多組帳號可用`,`分割   | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "account":"test-api01",
             "enable":1
          }
       ]
    }
    ```

    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    回傳參數說明    
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string|玩家帳號|
    |enable |int|是否啟動 1:是, 0:否|    
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |        
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |

17. ### <span id="set-activation">設定玩家是否啟用遊戲</span>

    設定玩家是否啟用遊戲

    ```
    PUT player/enable?
        key=<key>&
        account=<account>&
        enable=<0|1>&
        hash=<hash>
    ```
    
    ##### Reqeust 範例
    
    bash
    
    ```bash
    CURL -X PUT -d account=test -d enable=1 -d key=57d0bc61dffff -d hash=4e7efef29cb3127cb97859bdc1826daa \
 		-G http://poker.app/api/v2/slot/player/enable
    ```
    
    php
	    
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/enable';
	$data = [
		'account'=>'test',
		'enable'=>'1'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
		
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
	```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account| 玩家帳號 |  string(20)  |     必填，支援多組帳號可用`,`分割   | Y |
    |enable| 驗證參數 |  int    |     必填，是否啟動 1:是, 0:否 | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + enable + secret )**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "account":"test-api01",
             "activation":1
          }
       ]
    }
    ```

    失敗
    
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    
    回傳參數說明    
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    | enable |int|是否啟動 1:是, 0:否| 
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |        
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |

18. ### <span id="query-logs">玩家下注記錄查詢</span>

    查詢玩家下注LOG

    ```
    GET bet/report/detail?
        key=<key>&
        account=<account>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```
	
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d account=test -d startAt=0 -d endAt=0 -d key=57d0bc61dffff -d hash=deabb032a98cf55352f4126221e00117 \
 		-G http://poker.app/api/v2/slot/bet/report/detail
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/bet/report/detail';
	$data = [
		'account'=>'test',
		'startAt'=>'0',
		'endAt'=>'0'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```
	
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:---:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account| 玩家帳號 |  string(20)  |     必填 | Y |
    |  gameType | 遊戲代稱 |  int  |     選填 [GameType](#GameType) | Y |  
    |startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
    |endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果
    
    成功
    
    ```javascript
	{  
	   "status":"success",
	   "data":[  
	      {  
	         "id":423,
	         "machineNo":1,
	         "bet":50,
	         "betLines":9,
	         "totalBet":450,
	         "winCredt":500,
	         "scatter":0,
	         "bonus":0,
	         "createdAt":"2017-04-17 10:04:12",
	         "gameResult":[  
	            {  
	               "icon":3,
	               "filled":false
	            },
	            {  
	               "icon":3,
	               "filled":false
	            },
	            {  
	               "icon":10,
	               "filled":true
	            },
	            {  
	               "icon":2,
	               "filled":true
	            },
	            {  
	               "icon":5,
	               "filled":true
	            },
	            {  
	               "icon":3,
	               "filled":false
	            },
	            {  
	               "icon":11,
	               "filled":false
	            },
	            {  
	               "icon":3,
	               "filled":false
	            },
	            {  
	               "icon":9,
	               "filled":true
	            },
	            {  
	               "icon":8,
	               "filled":true
	            },
	            {  
	               "icon":13,
	               "filled":false
	            },
	            {  
	               "icon":3,
	               "filled":false
	            },
	            {  
	               "icon":3,
	               "filled":false
	            },
	            {  
	               "icon":6,
	               "filled":true
	            },
	            {  
	               "icon":5,
	               "filled":true
	            }
	         ]
	      },
	      {  
	         "id":434,
	         "machineNo":1,
	         "bet":50,
	         "betLines":9,
	         "totalBet":450,
	         "winCredit":300,
	         "scatter":0,
	         "bonus":0,
	         "createdAt":"2017-04-17 10:53:22",
	         "gameResult":[  
	            {  
	               "icon":2,
	               "filled":false
	            },
	            {  
	               "icon":4,
	               "filled":true
	            },
	            {  
	               "icon":1,
	               "filled":true
	            },
	            {  
	               "icon":5,
	               "filled":true
	            },
	            {  
	               "icon":11,
	               "filled":false
	            },
	            {  
	               "icon":7,
	               "filled":false
	            },
	            {  
	               "icon":9,
	               "filled":true
	            },
	            {  
	               "icon":13,
	               "filled":false
	            },
	            {  
	               "icon":2,
	               "filled":true
	            },
	            {  
	               "icon":4,
	               "filled":true
	            },
	            {  
	               "icon":8,
	               "filled":false
	            },
	            {  
	               "icon":8,
	               "filled":false
	            },
	            {  
	               "icon":10,
	               "filled":true
	            },
	            {  
	               "icon":13,
	               "filled":false
	            },
	            {  
	               "icon":9,
	               "filled":true
	            }
	         ]
	      }
	   ]
	}
    ```

    失敗
    
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
	
	 ##### 回傳參數說明
    
    |參數名稱|參數型態|說明|
    |:---:|:---:|:---:|:---:
    | data | array\<object\> | server回送的資料陣列 |
    | id | int | 流水號 |
    | machineNo | int | 機器編號 |
    | bet | int | 贏得的金額 |
    | betLines | int | 下注金額 |
    | totalBet | string | 建立時間 |
    | winCredit | int | 贏得金額 |
    | scatter | int | 是否是在scatter 狀態下 |
    | bonus | int | bonus 贏得金額 |
    | createdAt | string | 建立時間 |
    | gameResult | array\<object\> | 遊戲開牌結果，說明請看下表 |
    
    開牌結果說明
    
    |參數名稱|參數型態|說明|
    |:---:|:---:|:---:|:---:
    | icon | int | 轉到的 icon id |
	
	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4 | player not found  |        
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
	| 20 |range :{startAt} to 2017-01-02 00:00:00 not found bet results |
	| 23 |[start_at or end_at] value must be datetime Example：2016-01-01 00:00:00 |
    

19. ### <span id="bet-report">玩家下注簡報查詢</span>

    	玩家下注簡報查詢
    
	```
	GET bet/report?
		key=<key>&
		account=<account>&
		startAt=<startAt>&
		endAt=<endAt>&
		hash=<hash>
	```
	
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d account=test -d startAt=0 -d endAt=0 -d key=57d0bc61dffff -d hash=deabb032a98cf55352f4126221e00117 \
 		-G http://poker.app/api/v2/slot/bet/report
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/bet/report';
	$data = [
		'account'=>'test',
		'startAt'=>'0',
		'endAt'=>'0'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```
	
   	##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
	|  account| 玩家帳號 |  string(20)  |     必填 | Y |
	|startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

	**hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果

    成功
    
	```javascript
	{  
		"status":"success",
		"data":{  
			  "totalBet":"540",
			  "winCredit":"40"
		}
	}
	```

    失敗
    
	```javascript
	{"status":"error","error":{"code":4,"message":"user not found"}}
	```
    
	##### 回傳參數說明
    
	|參數名稱|參數型態|說明|
	|:---:|:---:|:---:|:---:
	| winCredit | int | 贏得的金額 |
	| totalBet | int | 下注金額 |
	
	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	| 錯誤代碼 | 錯誤說明 |     
	|:--------:|:--------:|
	| 1  | {parameter} is required   |
	| 2  | key is invalid            |
	| 3  | hash is invalid           |
    | 4 | player not found  |    	
	| 5  | {method} is not allowed   |
	|  7  | internal server error |
	| 11 | {parameter} is invalid   |
	| 20 |range :{startAt} to 2017-01-02 00:00:00 not found bet results |
	| 23 |[start_at or end_at] value must be datetime Example：2016-01-01 00:00:00 |	
    
19. ### <span id="bet-report-multiple">玩家多人下注簡報區間總額查詢</span>

    玩家下注簡報查詢
    
	```
	GET bet/report-multiple?
		key=<key>&
		gameType =<gameType>&
		startAt=<startAt>&
		endAt=<endAt>&
		hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
	CURL -X GET -d gameType=samurai -d startAt=0 -d endAt=0 -d key=57d0bc61dffff -d hash=b04cc896b399f3ec69454c1c48d30a69 \
 		-G http://poker.app/api/v2/slot/bet/report-multiple
 	```
 	
 	php
 	
 	```php
 	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/bet/report-multiple';
	$data = [
		'gameType'=>'samurai',
		'startAt'=>'0',
		'endAt'=>'0'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```

    ##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
	|  gameType | 遊戲代稱 |  int  |     必填 [GameType](#GameType) | Y |
	| startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	| endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果

    成功
    
	```javascript
	{  
		"status":"success",
		"data":[  
		  {  
		     "account":"leochen",
		     "winCredit":null,
		     "totalBet":40,
		     "createdAt":"2016-12-16 14:00:11"
		  },
		  {  
		     "account":"wei01",
		     "winCredit":"28935",
		     "totalBet":40,
		     "createdAtt":"2017-03-23 09:36:08"
		  }
		]
	}
	```

	失敗
    
	```javascript
	{"status":"error","error":{"code":4,"message":"user not found"}}
	```
    
    ##### 回傳參數說明
    
	| 參數名稱 | 參數型態 | 說明 |
	|:---:|:---:|:---:|
	| data | array\<object\> | server回送的資料陣列，請看下表 |

    data 內 object 說明
    
	| 參數名稱 | 參數型態 | 說明 |
	|:---:|:---:|:---:|
	| account | string(20) | 玩家帳號 |
	| winCredit | int | 贏得的金額 |
	| totalBet | int | 下注金額 |
	| createdAt | string | 建立時間 |
	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	| 錯誤代碼 | 錯誤說明 |     
	|:--------:|:--------:|
	| 1  | {parameter} is required   |
	| 2  | key is invalid            |
	| 3  | hash is invalid           |
	| 5  | {method} is not allowed   |
	|  7  | internal server error |
	| 11 | {parameter} is invalid   |
	| 12 | gameType  not found |
	| 20 |range :{startAt} to 2017-01-02 00:00:00 not found bet results |
	| 23 |[start_at or end_at] value must be datetime Example：2016-01-01 00:00:00 |	

20. ### <span id="jp-logs">玩家JP紀錄查詢</span>

    玩家JP紀錄查詢
    
	```
	GET jackpot?
		key=<key>&
		account=<account>&
		startAt=<startAt>&
		endAt=<endAt>&
		hash=<hash>
	```
	
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d account=test -d startAt=0 -d endAt=0 -d key=57d0bc61dffff -d hash=deabb032a98cf55352f4126221e00117 \
 		-G http://poker.app/api/v2/slot/jackpot
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/jackpot';
	$data = [
		'account'=>'test',
		'startAt'=>'0',
		'endAt'=>'0'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```
	
    ##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string  | 由API端提供 | Y |
	|  account| 玩家帳號 |  string  |     必填 | Y |
	|startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

	**hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果

	成功
	
	```javascript
	{  
		"status":"success",
		"data":[  
		  {  
		     "id":6,
		     "jackpot":8622,
		     "created_at":"2016-05-30 15:06:13",
		     "paid_at":"2016-05-30 15:21:08",
		     "rate":0.15
		  }
		]
	}
	```

	失敗
	
	```javascript
	{"status":"error","error":{"code":4,"message":"user not found"}}
	```
    
    回傳參數說明    
    
	|參數|型態|說明|
	|:---:|:---:|:---:|
	| jackpot |int|遊戲jackpot編號|
	| createdAt |string|jackpot時間|
	| paidAt |string|核銷時間|
	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	| 錯誤代碼 | 錯誤說明 |     
	|:--------:|:--------:|
	| 1  | {parameter} is required   |
	| 2  | key is invalid            |
	| 3  | hash is invalid           |
    | 4 | player not found  |    		
	| 5  | {method} is not allowed   |
	|  7  | internal server error |
	| 11 | {parameter} is invalid   |
    
20. ### <span id="jp-multiple">玩家多人JP紀錄查詢</span>

    玩家JP紀錄查詢
    
	```
	GET jackpot/multiple?
		key=<key>&
		gameType=<gameType>&
		startAt=<startAt>&
		endAt=<endAt>&
		hash=<hash>
	```
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d gameType=samurai -d startAt=0 -d endAt=0 -d key=57d0bc61dffff -d hash=b04cc896b399f3ec69454c1c48d30a69 \
 		-G http://poker.app/api/v2/slot/jackpot/multiple
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/jackpot/multiple';
	$data = [
		'gameType'=>'samurai',
		'startAt'=>'0',
		'endAt'=>'0'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```
	
    ##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
	|  gameType | 遊戲代稱 |  int  |     必填 [GameType](#GameType) | Y |
	|startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

	**hash = md5(gameType + startAt + endAt + secret)**

	##### 回傳結果

   成功
    
	```javascript
	{  
	"status":"success",
	"data":[  
	  {  
	     "account":"ooo@gmail.com",
	     "jackpot":1,
	     "paid_at":"2017-04-14 11:36:54",
	     "rate":1,
	     "game_id":1,
	     "created_at":"2017-04-13 14:31:50"
	  }
	]
	}
	```

    失敗
    
	```javascript
	{"status":"error","error":{"code":4,"message":"user not found"}}
	```
	
    回傳參數說明    
    
	|參數|型態|說明|
	|:---:|:---:|:---:|
	| jackpot |int|遊戲jackpot編號|
	| created_at |string|jackpot時間|
	| paid_at |string|核銷時間|    
	
	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	| 錯誤代碼 | 錯誤說明 |     
	|:--------:|:--------:|
	| 1  | {parameter} is required   |
	| 2  | key is invalid            |
	| 3  | hash is invalid           |
	| 5  | {method} is not allowed   |
	|  7  | internal server error |
	| 11 | {parameter} is invalid   |
	| 12 | gameType  not found |	

21. ### <span id="jp-status">JP中獎紀錄</span>


	```
	GET jackpot-status?
		key=<key>&
		status=<status>&
		hash=<hash>
	```
    
    ##### Request  範例
    
    bash
    
	```bash
	CURL -X GET -d status=1 -d key=57d0bc61dffff -d hash=61f7e46050c1a02277d473348f0f2ee7 \
		-G http://poker.app/api/v2/slot/jackpot-status
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/jackpot-status';
	$data = [
		'status'=>'1'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;
	
	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url.'?'.http_build_query($data));
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "GET");
	$response = curl_exec($ch);
	echo $response;
	```

	##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
	|  status| 狀態 |  int  |  必填 0:不限,1未核銷,2：已核銷 | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

	**hash = md5(status + secret)**

    ##### 回傳結果

    成功
    
	```javascript
	{  
		"status":"success",
		"data":[  
		  {  
		     "account":"ooo@gmail.com",
		     "jackpot":1,
		     "paid_at":"2017-04-14 11:36:54",
		     "rate":1,
		     "game_id":1,
		     "created_at":"2017-04-13 14:31:50"
		  }
		]
	}
	```

    	失敗
    
	```javascript
	{"status":"error","error":{"code":4,"message":"user not found"}}
	```
	
    	回傳參數說明    
    
	|參數|型態|說明|
	|:---:|:---:|:---:|
	| jackpot |int|遊戲jackpot編號|
	| created_at |string|jackpot時間|
	| paid_at |string|核銷時間|
	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	| 錯誤代碼 | 錯誤說明 |     
	|:--------:|:--------:|
	| 1  | {parameter} is required   |
	| 2  | key is invalid            |
	| 3  | hash is invalid           |
	| 5  | {method} is not allowed   |
	|  7  | internal server error |
	| 11 | {parameter} is invalid   |
             
21. ### <span id="jp-vertification">玩家JP核銷</span>

	玩家JP核銷

	```
	PUT jackpot/write-off?
		key=<key>&
		account=<account>&
		jp_id=<jp_id>&
		verified_at=<verified_at>&
		hash=<hash>
	```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X PUT -d account=test -d jp_id=1 -d verified_at=2017-04-10 23:23:23 -d key=57d0bc61dffff -d hash=bcc2ea7bcd3ea6b479c4a493ff041d56 \
 		-G http://poker.app/api/v2/slot/jackpot/write-off
 	```
 	
 	php
 	
 	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/jackpot/write-off';
	$data = [
		'account'=>'test',
		'jp_id'=>'1',
		'verified_at'=>'2017-04-10 23:23:23'
	];
	//產生hash
	$hash = '';
	foreach ($data as $k => $v) {
		$hash .= $v;
	}
	$hash .= $secret;

	$hash = md5($hash);
	$data['key'] = $key;
	$data['hash'] = $hash;
	$ch = curl_init($url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));
	$response = curl_exec($ch);
	echo $response;
 	```
	
   	##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
	|  account| 玩家帳號 |  string(20)  |     必填 | Y |
	| jpId  | jp紀錄的id |  int  |     jp紀錄的id，離線通知會提供 | Y |
	| verifiedAt | 核銷日期 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |
	
       	**hash = md5(account + jp_id + verified_at + secret)**

   	##### 回傳結果

	成功
    
	```javascript
	{  
	   "status":"success",
	   "data":{  
	      "id":1,
	      "jackpot":1,
	      "createdAt":"2017-04-13 14:31:50",
	      "paidAt":"2017-04-14 11:36:54",
	      "rate":1,
	      "gameId":1
	   }
	}
	```

   	失敗
    
	```javascript
	{"status":"error","error":{"code":4,"message":"user not found"}}
	```
	
	回傳參數說明    
    
	|參數|型態|說明|
	|:---:|:---:|:---:|
	| jackpot |int|遊戲jackpot編號|
	| createdAt |string|jackpot時間|
	| paidAt |string|核銷時間|
	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	| 錯誤代碼 | 錯誤說明 |     
	|:--------:|:--------:|
	| 1  | {parameter} is required   |
	| 2  | key is invalid            |
	| 3  | hash is invalid           |
    | 4 | player not found  |    		
	| 5  | {method} is not allowed   |
	|  7  | internal server error |
	| 11 | {parameter} is invalid   |
	| 101 | jackpot log not found   |
