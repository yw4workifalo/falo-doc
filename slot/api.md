# API 說明文件
## 目錄
1. [註冊帳號](#註冊帳號)
1. [取得登入連結](#取得玩家登入網址)
2. [修改暱稱](#修改暱稱)
1. [查詢玩家](#查詢玩家)   
2. [取得玩家遊戲紀錄](#取得玩家遊戲紀錄)
4. [玩家額度轉出入](#玩家額度轉出入)
5. [玩家轉帳狀態查詢](#玩家轉帳狀態查詢)
6. [踢玩家](#踢玩家)
7. [踢多玩家](#踢多玩家)
8. [限輸](#限輸)
9. [限贏](#限贏)
10. [限注回復](#限注回復)
11. [設定玩家帳號模式](#設定玩家帳號模式)
13. [限注查詢](#限注查詢)
14. [查詢玩家帳號模式](#查詢玩家帳號模式)
15. [查詢玩家是否啟用遊戲](#查詢玩家是否啟用遊戲)
16. [設定玩家是否啟用遊戲](#設定玩家是否啟用遊戲)
17. [玩家下注記錄查詢](#玩家下注記錄查詢)
18. [玩家下注簡報查詢](#玩家下注簡報查詢)
19. [玩家多人下注簡報區間總額查詢](#玩家多人下注簡報區間總額查詢)
20. [玩家JP紀錄查詢](#玩家jp紀錄查詢)
21. [玩家多人JP紀錄查詢](#玩家多人jp紀錄查詢)
22. [JP中獎紀錄](#jp中獎紀錄)
23. [玩家JP核銷](#玩家jp核銷)
24. [手機API串接](#手機api串接)
25. [設定信用玩家額度](#設定信用玩家額度)
26. [查詢信用玩家額度](#查詢信用玩家額度)
27. [重設信用玩家額度](#重設信用玩家額度)
28. [APP下載連結](#app下載連結)
29. [設定玩家佔成](#設定玩家佔成)

## CHANGE LOG
[CHANGE LOG](CHANGELOG.md)

## 登入流程
1. 玩家透過平台登入
2. 平台透過[註冊帳號](#註冊帳號)新增帳號
3. 平台透過[取得玩家登入網址](#取得玩家登入網址)取得登入網址
5. 透過登入網址直接進入遊戲

[流程圖連結](https://cacoo.com/diagrams/WaDYppQPnrwiv0ot/simple#28811)

![流程圖](https://cacoo.com/diagrams/WaDYppQPnrwiv0ot-28811.png?t=1501038793649 "API 登入 流程圖")

## GameType
| 遊戲名稱  | GameType                  | 
|---------- |-------------------------  |
| 武士道  (Spirit Of Samurai)       | 1   | 
| 金錢貓  (Golden Cat)       | 2   | 
| 百鬼夜行  (Demon)  (未上線)     | 3   | 
| 美人魚  (Mermaid)  (未上線)     | 4   | 

## 玩家模式說明

|模式|說明|
|:---:|:---:|
|0|正常|
|1|鎖單無法下注|
|2|封鎖無法登入|

## 支援貨幣

 |代碼|說明|
 |:-:|:-:|
 |TWD|台幣|
 |CNY|人民幣|

## 支援語系

 |代碼|說明|
 |:-:|:-:|
 | zh_TW |繁體中文|
 | zh_CN |簡體中文|
 
## 支援裝置

 |代碼|說明|
 |:-:|:-:|
 | Web | 網頁電腦版 |
 | Android | Android App | 
 | IOS | IOS App |  

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
| 20 | account:{account} has been used | 此帳號已被使用 | 
| 21 | bet accounts processing | 帳務結算中不可額度轉出入 | 
| 22 | {param} must be a unsigned int | {param}設定值必須為正整數 | 
| 23 | [startAt or endAt] value must be datetime Example:2016-01-01 00:00:00 | 查詢玩家注單,時間參數必須使用規定格式 | 
| 24 | {transferId} is not exist | 此筆交易單不存在 | 
| 25 | transfer id credit can not be empty | 交易的ID不可為空 | 
| 26 | transfer id:（平台方TransferID）has been used | 此交易單已被使用 |
| 27 | transfer in or out can not be 0 | 額度轉出入設定值不可為0 |
| 28 | transfer pending | 訂單交易中 |
| 29 | transfer over ratelimit {count} times/per munite | 超過每分鐘請求次數 |
| 30 | the cash type is invalid | 只適用信用用戶 |
| 31 | \{var\} must between \{min\} and \{max\}| 設定數值超出範圍 |
| 101 | jackpot log not found | 找不到 jackpot 記錄 |
| 102 | The currency what you set is not supported | 您所設定的貨幣類型不支援 |
| 103 | The language is not supported | 您所設定的語系類型不支援 |
| 104 | The platform type is not supported | 您指定登入的平台不支援 |


## API
1. ### 註冊帳號

    ```
    POST player/register?
        key=<key>&
        account=<account>&
        nickname=<nickname>&
        currency=<currency>&
        hash=<hash>
    ```

    ##### Request 範例
    
	bash
	
    ```bash
    CURL -X POST -d account=test02 -d nickname=test -d currency=TWD -d key=57d0bc61dffff -d hash=28676265eba1bf74a397887e5b7df167 \ 
    -G http://poker.app/api/v2/slot/player/register
    ```
    
    php
    
    ```php
    $key = '57d0bc61dffff';
    $secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
    $url = 'http://poker.app/api/v2/slot/player/register';
    $data = [
        'account'=>'test02',
        'nickname'=>'test',
        'currency'=>'TWD'
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
    | currency | 玩家貨幣類型 | string(10) | 設定玩家貨幣類型，設定之後不可更改 [支援貨幣](#支援貨幣)| Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + nickname + secret)**

    ##### 回傳結果
    成功
    
    ```javascript
    {  
       "status":"success",
       "data":{  
          "currency":"TWD",
          "account":"test02",
          "nickname":"test",
          "id":36
       }
    }
    ```
    
    失敗
    
    ```javascript
   {  
       "status":"error",
       "error":{  
          "code":102,
          "message":"The currency what you set is not supported"
       }
    }
    ```
    
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account| string(20) | 帳號|
    |nickname| string(20) | 玩家暱稱 |
    |currency| string(10) | 玩家設定貨幣 |
    |id| integer | 遊戲方玩家編號 | 
    
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
    | 20 | account:{account} has been used | 此帳號已被使用 | 
    | 102 | The currency what you set is not supported |

1. ### 修改暱稱

    ```
    PUT player/nickname?
        key=<key>&
        account=<account>&
        nickname=<nickname>&
        hash=<hash>
    ```

    ##### Request 範例
    
	bash
	
    ```bash
    CURL -X POST -d account=test -d nickname=測試 -d key=57d0bc61dffff -d hash=106c9b6891a82e9af6d404f4a9707341 \
      -G http://poker.app/api/v2/slot/player/nickname
    ```
    
    php
    
    ```php
    <?php
    $key = '57d0bc61dffff';
    $secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
    $url = 'http://poker.app/api/v2/slot/player/nickname';
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
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
    curl_setopt($ch, CURLOPT_POSTFIELDS,http_build_query($data));

    $response = curl_exec($ch);

    echo $response;
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
    |:--------:|:--------:|:--------:|:-----------:|:-----------:|
    |    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
    |  account | 玩家帳號 |  string(20)  |     必填    | Y |
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
          "nickname":"qoo"
       }
    }
    ```
    
    失敗
    
    ```javascript
	{  
	   "status":"error",
	   "error":{  
	      "code":14,
	      "message":"nickname length between 1 - 20"
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
    | 4  | player not found          |     
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 14 | nickname length between 1- 20  |


2. ### 取得玩家登入網址

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
    |  gameType | 遊戲代稱 |  int  |     選填 [GameType](#gametype) 直接進入遊戲| N |
    |  language | 語系代碼 |  string  |     選填 [支援語系](#支援語系) | N |
    |   platformType   | 裝置代碼 |  string  |     選填 [支援裝置](#支援裝置)    |N|    
    |   hash   | 驗證參數 |  string  |     必填    |Y|

    **hash = md5(account + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "loginUrl":"http:\/\/feature_api.dev\/api\/v2\/slot\/player\/login?token=eyJpdiI6IlU4bmFUOEJiNnpneFdVd3RkQjNtd3c9PSIsInZhbHVlIjoiUWhDUXZwRVduRHdIK25NK2kzYjdCZ2FFKzUwclRwd1BYcEszOTdxOHBRTHNVc2E5OVwvcDE5TmRYMjNxTzZ2dHVldzRUSFhLVHhkVEJOSHgzNVAwXC80cUNjeHQzME05clp2MG9JeVRMQTNPbzV0SWNvZGRyeGVCaWhOQWp2V0prRUlLSXgydkMyTkdCV1lWQ3ArMWxqNmk3WG1BOE83S1o4SVwveWxjeXMwbDl1amR6VXJiMU1TN1wvaVBKZXdLSzllbHE5eitnaGFRV3BWdFwvd25YYVZUUWljbWRnbWpJdVVxM1pnZk5qNUlXSXVYcWU0UTJjZVpwNGthWXRVUGdBVnZ6dlNoczVIem9HZThCNUl6WUJ2QzN6QT09IiwibWFjIjoiMTIxMmNjMDY2ZmJjYzg3NjFlMzQzZmFiN2VmNjc3ZDcyNGI5NTZkNmZhZDY0YzZhYmZjYWNhYjk1YzM2MGM2NCJ9",
          "token":"eyJpdiI6IlU4bmFUOEJiNnpneFdVd3RkQjNtd3c9PSIsInZhbHVlIjoiUWhDUXZwRVduRHdIK25NK2kzYjdCZ2FFKzUwclRwd1BYcEszOTdxOHBRTHNVc2E5OVwvcDE5TmRYMjNxTzZ2dHVldzRUSFhLVHhkVEJOSHgzNVAwXC80cUNjeHQzME05clp2MG9JeVRMQTNPbzV0SWNvZGRyeGVCaWhOQWp2V0prRUlLSXgydkMyTkdCV1lWQ3ArMWxqNmk3WG1BOE83S1o4SVwveWxjeXMwbDl1amR6VXJiMU1TN1wvaVBKZXdLSzllbHE5eitnaGFRV3BWdFwvd25YYVZUUWljbWRnbWpJdVVxM1pnZk5qNUlXSXVYcWU0UTJjZVpwNGthWXRVUGdBVnZ6dlNoczVIem9HZThCNUl6WUJ2QzN6QT09IiwibWFjIjoiMTIxMmNjMDY2ZmJjYzg3NjFlMzQzZmFiN2VmNjc3ZDcyNGI5NTZkNmZhZDY0YzZhYmZjYWNhYjk1YzM2MGM2NCJ9"
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
    |token|string|登入用token|
    
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
	| 103 | The language is not supported |   
	| 104 | The platform type is not supported | 	 

4. ### 查詢玩家

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
    $url = 'http://poker.app/api/v2/slot/player/info';
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
          "credit":"100.0000",
          "loginAt":null,
          "logoutAt":null,
          "mode":0,
          "enable":1,
          "limitWin":"100000.0000",
          "limitLose":"100000.0000",
          "isOnline":false,
          "currency":"TWD"
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
    |id|int|序號|
    |account|string|玩家帳號|
    |nickname|string|玩家暱稱|
    |credit|decimal(19, 4)|玩家目前額度|
    |mode|int|玩家目前模式 0:正常 1:鎖單無法下注 2:封鎖無法登入|
    |enable|int|是否啟用 0:停用 1:啟用|
    |limitWin|decimal(19, 4)|限贏 0為無限制|
    |limitLose|decimal(19, 4)|限輸 0為無限制|
    |isOnline|boolean|玩家是否在上線|
    |currency|string| 玩家設定貨幣 |
    
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

    
5. ### 取得玩家遊戲紀錄

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
		   "id":8,
		   "userId":53,
		   "initialCredit":"985519.0000",
		   "sumOfBet":"0.0000",
		   "sumOfWinCredit":"0.0000",
		   "finalCredit":"985519.0000",
		   "ip":"192.168.11.1",
		   "loginAt":"2017-07-13 11:16:13",
		   "logoutAt":"2017-07-13 11:18:16"
		},
		{  
		   "id":9,
		   "userId":53,
		   "initialCredit":"985519.0000",
		   "sumOfBet":"0.0000",
		   "sumOfWinCredit":"0.0000",
		   "finalCredit":"985519.0000",
		   "ip":"192.168.11.1",
		   "loginAt":"2017-07-13 11:21:33",
		   "logoutAt":"2017-07-13 11:23:36"
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
    | initialCredit |decimal(19, 4)|初始金額|
    | sumOfBet |decimal(19, 4)|下注金額|
    | sumOfWinCredit |decimal(19, 4)|贏取金額，損益為 sumOfWinCredit - sumOfBet|
    | finalCredit |decimal(19, 4)|最後籌碼|    
    
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
    
        	
7. ### 玩家額度轉出入

    ```
    PUT player/credit/transfer?
        key=<key>&
        account=<account>&
        credit =<credit>&
        transferId=<transferId>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X PUT -d account=test -d credit=1 -d transferId=GC0001 -d key=57d0bc61dffff -d hash=f28a85dd168db1089dc2ae2b1c033424 \
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
    |  credit   | 增加的籌碼 |  decimal(19, 4)  |     必填, 負號代表轉出。(目前不開放小數籌碼，請仍以整數串送)    | Y |
    |  transferId   | 交易編號 |  string(30)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + credit + transfer_id + secret)**

    ##### 回傳結果
    成功

    ```javascript
	{  
	   "status":"success",
	   "data":{  
	      "originalCredit":"1010810.0000",
	      "addedCredit":"9999.0000",
	      "finalCredit":"1020809.0000",
	      "account":"57b_wei",
	      "orderId":11,
	      "transferId":"PDNN",
	      "status":0,
	      "currency":"TWD"
	   }
	}
    ```
    失敗

    ```javascript
    {  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"player not found"
	   }
	}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string|玩家帳號|  
    |originalCredit|decimal(19, 4)|原始籌碼|    
    |addedCredit|decimal(19, 4)|新增籌碼|
    |finalCredit|decimal(19, 4)|最後籌碼|
    |transferId|string(30)|平台交易編號，unique|
    |orderId|int|遊戲方交易編號, unique|
    |status|int|狀態 0:success|  
    |currency|string| 玩家設定貨幣 |    
    
    
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
    | 17 |transfer in error|
    | 18 |transfer out error|    
    |19 | player credit is not enough|
    | 26 | transfer id:（平台方TransferID）has been used |


7. ### 玩家轉帳狀態查詢

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
	      "addedCredit":"9999.0000",
	      "originalCredit":"1010810.0000",
	      "finalCredit":"1020809.0000",
	      "account":"57b_wei",
	      "orderId":11,
	      "transferId":"PDNN",
	      "status":0,
	      "currency":"TWD"
	   }
	}
    ```
    失敗

    ```javascript
    {  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"player not found"
	   }
	}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|  
    |originalCredit|decimal(19, 4)|原始籌碼|
    |addedCredit|decimal(19, 4)|新增籌碼|
    |finalCredit|decimal(19, 4)|最後籌碼|
    |transferId|string(30)|平台交易編號，unique|
    |orderId|int|遊戲方交易編號, unique|
    |status|int|狀態 0:success|
    |currency|string| 玩家設定貨幣 |        
    
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
	| 28 | transfer pending | 
    

8. ### 踢玩家

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
    |  reason  | 剔除原因 |  string(50)  |     選填    | Y |
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
    {"status":"error","error":{"code":4,"message":"player not found"}}
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

        
8. ### 踢多玩家
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
    |  reason  | 剔除原因 |  string(50)  |     玩家    | Y |
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


9. ### 限輸

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
    |  limit   | 限制金額 |  decimal(19, 4)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account+limit+secret)**

    ##### 回傳結果
    成功

    ```javascript
	{  
	   "status":"success",
	   "data":{  
	      "account":"57b_wei",
	      "limitWin":"100000.0000",
	      "limitLose":"9998.0000",
	      "winCredit":"0.0000"
	   }
	}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"player not found"}}
    ```
    回傳參數說明
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    |limitLose|decimal(19, 4)|限輸金額|
    |limitWin|decimal(19, 4)|限贏金額|
    | winCredit | decimal(19, 4) | 當前損益 |
    
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


10. ### 限贏

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
    |  limit   | 限制金額 |  decimal(19, 4)  |     必填    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account+limit+secret)**

    ##### 回傳結果
    成功

    ```javascript
	{  
	   "status":"success",
	   "data":{  
	      "account":"57b_wei",
	      "limitWin":"100000.0000",
	      "limitLose":"9998.0000",
	      "winCredit":"0.0000"
	   }
	}
    ```
    失敗

    ```javascript
	{  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"player not found"
	   }
	}
    ```
    
    回傳參數說明    
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    |limitLose|decimal(19, 4)|限輸金額|
    |limitWin|decimal(19, 4)|限贏金額| 
    | winCredit | decimal(19, 4) | 當前損益 |    
    
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
 

11. ### 限注回復

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
	      "account":"57b_wei",
	      "winCredit":"0.0000"
	   }
	}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"player not found"}}
    ```
    
    回傳參數說明
    
    | 參數 | 型態 | 說明 |
    |:---:|:---:|:---:|
    | account |string(20)| 玩家帳號 |
    | winCredit |decimal(19, 4)| 回復後損益 |
    
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

    
12. ### 設定玩家帳號模式

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
             "mode":1
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
	      "message":"player not found"
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


14. ### 限注查詢

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
	      "account":"57b_wei",
	      "winCredit":"0.0000",
	      "limitWin":"8888.9900",
	      "limitLose":"9998.0000",
	      "updatedAt":"2017-07-19 11:31:56"
	   }
	}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"player not found"}}
    ```
    回傳參數說明    
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    |limitLose|decimal(19, 4)|限輸金額|
    |limitWin|decimal(19, 4)|限贏金額|    
    | winCredit |decimal(19, 4)|損益|     
    
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

15. ### 查詢玩家帳號模式

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
	{"status":"error","error":{"code":4,"message":"player not found"}}
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

16. ### 查詢玩家是否啟用遊戲

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
    {"status":"error","error":{"code":4,"message":"player not found"}}
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

17. ### 設定玩家是否啟用遊戲

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
    {"status":"error","error":{"code":4,"message":"player not found"}}
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

18. ### 玩家下注記錄查詢

    查詢玩家下注LOG，查詢區間依注單更新時間

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
    |  account| 玩家帳號 |  string(20)  |     選填 | N |
    |  gameType | 遊戲代稱 |  int  |     選填 [GameType](#gametype) | N |  
    |startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
    |endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 | Y |
    |page    | 分頁 |  int  |     預設:1 | N |    
    |pageSize    | 分頁筆數 |  int  |     預設每頁1000筆 | N |        
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果
    
    成功
    
    ```javascript
	{  
       "status":"success",
       "data":{  
          "total":25890,
          "perPage":1000,
          "currentPage":1,
          "lastPage":26,
          "previousPageUrl":null,
          "nextPageUrl":"http:\/\/poker.app\/api\/v2\/slot\/bet\/report\/detail?account=test-api01&startAt=2017-06-01&endAt=2017-06-30&key=57d0bc61dffff&hash=bf7e365f2731d57951ac6b912692367b&page=2",
          "datas":[  //改單的情況
				{  
					"id":2010,
					"account":"test-api01",
					"gameType":2,
					"bet":"1.0000",
					"betLines":20,
					"totalBet":"20.0000",
					"winCredit":"0.0000",
					"scatter":0,
					"percent": 0.5,
					"bonus":"0.0000",
					"createdAt":"2017-05-15 13:47:57",
					"updatedAt":"2017-06-06 14:30:12",
					"billingDate":"2017-05-15",
					"platformType":"Android"
         		},
				{   	//未改單的情況
					"id":3011,
					"account":"test-api01",
					"gameType":2,
					"bet":"1.0000",
					"betLines":20,
					"totalBet":"20.0000",
					"winCredit":"0.0000",
					"scatter":0,
					"percent": 0.5,
					"bonus":"0.0000",
					"createdAt":"2017-06-06 14:30:12",
					"updatedAt":"2017-06-06 14:30:12",
					"billingDate":"2017-06-06",
					"platformType":"Android"
				},
             ...............
          ]
       }
    }
    ```

    失敗
    
    ```javascript
    {  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
    ```
	
	 ##### 回傳參數說明
    
    |參數名稱|參數型態|說明|
    |:---:|:---:|:---:| 
    | currentPage | int | 當前頁數 |
    | lastPage | int | 最後一頁的頁數 |        
    | total | int | 總筆數 |   
    | perPage | int | 一頁的筆數 |
    | previousPageUrl | string | 存取上一頁資料的url，沒有上一頁時為null |
    | nextPageUrl | string | 存取下一頁資料的url，沒有下一頁時為null |
    | datas | array\<object\> | server回送的資料陣列 |   
    
    資料說明

    |參數名稱|參數型態|說明|
    |:---:|:---:|:---:|
    | id | int | 流水號 |
    | account | String | 玩家帳號 |
    | gameType | int | 遊戲代稱 |        
    | machineNo | int | 機器編號 |
    | bet | decimal(19, 4) | 押注金額 |
    | betLines | int | 下注線數 |
    | totalBet | decimal(19, 4) | 總下注金額 |
    | winCredit | decimal(19, 4) | 贏得金額 |
    | scatter | int | 是否是在scatter 狀態下，scatter時總下注金額不需計算 |
    | percent | float | 玩家注單佔成 |
    | bonus | decimal(19, 4) | bonus 贏得金額 |
    | createdAt | string | 建立時間 |
    | updatedAt | string | 更新時間 |    
    | billingDate | string | 結帳日 | 
    | platformType  | string | 裝置代碼 |
    
	
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
	| 23 |[start_at or end_at] value must be datetime Example：2016-01-01 00:00:00 |
    

19. ### 玩家下注簡報查詢

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
	      "totalBet":"56440.0000",
	      "winCredit":"41960.0000"
	   }
	}
	```

    失敗
    
	```javascript
	{  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
	```
    
	##### 回傳參數說明
    
	|參數名稱|參數型態|說明|
	|:---:|:---:|:---:|
	| winCredit | decimal(19, 4) | 贏得的金額 |
	| totalBet | decimal(19, 4) | 下注金額 |
	
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
	| 23 |[start_at or end_at] value must be datetime Example：2016-01-01 00:00:00 |	
    
19. ### 玩家多人下注簡報區間總額查詢

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
	|  gameType | 遊戲代稱 |  int  |     必填 [GameType](#gametype) | Y |
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
	         "account":"57b_wei",
	         "winCredit":"41960.0000",
	         "totalBet":"40.0000",
	         "createdAt":"2017-07-06 14:30:11"
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
          "message":"player not found"
       }
    }
	```
    
    ##### 回傳參數說明
    
	| 參數名稱 | 參數型態 | 說明 |
	|:---:|:---:|:---:|
	| data | array\<object\> | server回送的資料陣列，請看下表 |

    data 內 object 說明
    
	| 參數名稱 | 參數型態 | 說明 |
	|:---:|:---:|:---:|
	| account | string(20) | 玩家帳號 |
	| winCredit | decimal(19, 4) | 贏得的金額 |
	| totalBet | decimal(19, 4) | 下注金額 |
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
	| 23 |[start_at or end_at] value must be datetime Example：2016-01-01 00:00:00 |	

20. ### 玩家JP紀錄查詢

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
	{  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
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
    
20. ### 玩家多人JP紀錄查詢

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
	|  gameType | 遊戲代稱 |  int  |     必填 [GameType](#gametype) | Y |
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
	{  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
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

21. ### JP中獎紀錄

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
	{  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
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
             
21. ### 玩家JP核銷

	玩家JP核銷

	```
	PUT jackpot/write-off?
		key=<key>&
		account=<account>&
		jpId=<jpId>&
		verifiedAt=<verifiedAt>&
		hash=<hash>
	```
    
    ##### Request 範例
    
    bash
    
    ```bash
    CURL -X PUT -d account=test -d jpId=1 -d verifiedAt=2017-04-10 23:23:23 -d key=57d0bc61dffff -d hash=bcc2ea7bcd3ea6b479c4a493ff041d56 \
 		-G http://poker.app/api/v2/slot/jackpot/write-off
 	```
 	
 	php
 	
 	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/jackpot/write-off';
	$data = [
		'account'=>'test',
		'jpId'=>'1',
		'verifiedAt'=>'2017-04-10 23:23:23'
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
	
       	**hash = md5(account + jpId + verifiedAt + secret)**

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
	{  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
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
	
25. ### 手機API串接
	#### 大地圖app
	###### scheme url 
		
	```
	pharaoh-gamecity://launch-game?
		token=<token>&
		lang=<lang>&
		platformURL=<schemaUrl>&
		gameType=<gameType>
	```
	
	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	| 	token | 從[取得玩家登入網址](#取得玩家登入網址)api取得的token | String | 用來驗證玩家 | Y |
	| 	lang | 設定玩家遊戲語系 | String | 使用  ISO 639 ex `zh_TW` 詳細請查看 [支援語系](#支援語系) | N |
	|	platformURL 	|	設定返回平台的 callback url	| 	string 	|	可以使用schema url 或者 http 網址 |	N |
	|	gameType	|	可以直接開啟相對應遊戲，未設定進大廳	| 	int	| 	請查看 [gameType](#gametype)	| 	N	|
	
	
20. ### 設定信用玩家額度

	設定信用玩家額度
    
	```
	PUT player/credit?
		key=<key>&
		account=<account>&
		credit=<credit>&
		hash=<hash>
	```
	##### Request 範例
	
	bash
	
	```bash
	CURL -X PUT -d account=test01 -d credit=100000 -d key=57d0bc61dffff -d hash=b04cc896b399f3ec69454c1c48d30a69 \
 		-G http://poker.app/api/v2/slot/player/credit
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/credit';
	$data = [
		'account'=>'test01',
		'credit'=>'100000',
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
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	$response = curl_exec($ch);
	echo $response;
	```
	
    ##### 參數說明

	| 參數名稱 | 參數說明 | 參數型態 |     說明    | 必填 |
	|:--------:|:--------:|:--------:|:-----------:|:---:|
	|    key   | 服務金鑰 |  string(20)  | 由API端提供 | Y |
	|  account | 玩家帳號 |  string(20)  |     必填，4-20個字元    | Y |
	|credit  | 玩家額度|  decimal(19, 4)  |   要設定的信用額度 | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

	**hash = md5(account + credit + secret)**

	##### 回傳結果

     成功

      ```javascript
      {  
      "status":"success",
      "data":{  
           "account":"test01",
           "credit":100000,
        }
      }
      ```

    失敗
    
	```javascript
	{  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
	```
	
    回傳參數說明    
    
	|參數|型態|說明|
	|:---:|:---:|:---:|
	|  account|  string(20)  | 玩家帳號 |
	| credit | decimal(19, 4) |目前設定的信用額度 |

	
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
	| 30 | the cash type is invalid | 只適用信用用戶 |

20. ### 查詢信用玩家額度

	查詢信用玩家額度
    
	```
	GET player/credit?
		key=<key>&
		account=<account>&
		credit=<credit>&
		hash=<hash>
	```
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d account=test01 -d key=57d0bc61dffff -d hash=b04cc896b399f3ec69454c1c48d30a69 \
 		-G http://poker.app/api/v2/slot/player/credit
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/credit';
	$data = [
		'account'=>'test01',
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
	|  account | 玩家帳號 |  string(20)  |     必填，4-20個字元    | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

	**hash = md5(account + secret)**

	##### 回傳結果

     成功

      ```javascript
      {  
      "status":"success",
      "data":{  
           "account":"test01",
           "credit":100000,
           "currentCredit": 1000
        }
      }
      ```

    失敗
    
	```javascript
	{  
       "status":"error",
       "error":{  
          "code":4,
          "message":"player not found"
       }
    }
	```
	
    回傳參數說明    
    
	|參數|型態|說明|
	|:---:|:---:|:---:|
	|  account|  string(20)  | 玩家帳號 |
	| credit | decimal(19, 4) |目前設定的信用額度 |
	| currentCredit | decimal(19, 4) | 玩家目前可用額度 |

	
	錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	|   錯誤代碼 |   錯誤說明 |     
	|:---------:|:--------:|
	| 	1  | {parameter} is required   |
	|  	2  | key is invalid            |
	|  	3  | hash is invalid           |
	|  	4 | player not found  |    		
	|  	5  | {method} is not allowed   |
	|  	7 | internal server error |
	|  	11 | {parameter} is invalid   |
	|  	30 | the cash type is invalid | 只適用信用用戶 |


20. ### 重設信用玩家額度

	重設信用玩家額度
    
	```
	GET player/credit/reset?
		key=<key>&
		callback=<callback>&
		hash=<hash>
	```
	##### Request 範例
	
	bash
	
	```bash
	CURL -X GET -d callback=http://callback.url/reset -d key=57d0bc61dffff -d hash=b04cc896b399f3ec69454c1c48d30a69 \
 		-G http://poker.app/api/v2/slot/player/credit/reset
	```
	
	php
	
	```php
	$key = '57d0bc61dffff';
	$secret = 'bf4b77c4965b3ee0b185f5caa81827e6';
	$url = 'http://poker.app/api/v2/slot/player/credit/reset';
	$data = [
		'callback'=>'http://callback.url/reset',
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
	|  callback | 額度回覆完成callback url |  string(100)  |     必填，完成額度回覆後的 callback    | Y |
	|   hash   | 驗證參數 |  string  |     必填    | Y |

	**hash = md5(callback + secret)**

	##### 回傳結果

     成功

      ```javascript
	{
		"status": "success",
		"data": {
			"logId": 3
		}
	}
      ```

    失敗
    
	```javascript
	{  
       "status":"error",
       "error":{  
          "code":30,
          "message":"the cash type is invalid"
       }
    }
	```
	
    回傳參數說明    
    
	|  參數 | 型態 | 說明 |
	|:---:|:---:|:---:|
	|  logId|  int  | 重設紀錄編號 |

	#錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
	    
	|  錯誤代碼 |  錯誤說明 |     
	|:--------:|:--------:|
	| 	1 |  \{parameter\} is required   |
	| 	2  | key is invalid            |
	| 	3  | hash is invalid           |	
	| 	5  | \{method\} is not allowed   |
	|  	7  | internal server error |
	| 	11 | {parameter} is invalid   |
	| 	30 | the cash type is invalid | 只適用信用用戶 |
	
21. ### APP下載連結

	 下載 app
	 
	 ```
	 GET app-download/
	 ```
	 
	##### 參數說明

	none
		
	#### 回傳
	
		302 redirect to app download link
		
10. ### 設定玩家佔成

    設定玩家佔成

    ```
    PUT player/profit-percent
        key=<key>&
        account=<account>&
        percent=<percent>&
        hash=<hash>
    ```
    
    ##### Request 範例
    
    bash
    
    ```bash
	CURL -X PUT -d account=poker \
	-d percent=0.5 \
	-d key=5a16601d7f7fb \
	-d hash=6571ba1dad29d8964a6182a849922574 \
	-G http://localhost/api/v2/slot/player/profit-percent
	```
	 
	 php
	 
	 ```php
	$key = '5a16601d7f7fb';
	$secret = 'a8e00e8d699c91fc6056dfb2e438149b';
	$url = 'http://localhost/api/v2/slot/player/profit-percent';
	$data = [
		'account'=>'poker',
		'percent'=>'0.5'
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
    |  percent   | 佔成比例 |  float  |     必填，只能到小數點第二位且0~1之間    | Y |
    |   hash   | 驗證參數 |  string  |     必填    | Y |

    **hash = md5(account+limit+secret)**

    ##### 回傳結果
    成功

    ```javascript
	{
		"status": "success",
		"data": {
			"account": "poker",
			"percent": 0.5
		}
	}
    ```
    失敗

    ```javascript
	{  
	   "status":"error",
	   "error":{  
	      "code":4,
	      "message":"player not found"
	   }
	}
    ```
    
    回傳參數說明    
    
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string(20)|玩家帳號|
    | percent |float|佔成|
    
    錯誤列表(詳細說明請查看[錯誤代碼](#錯誤代碼))
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 	1  	| {parameter} is required   |
    | 	2  	| key is invalid            |
    | 	3  	| hash is invalid           |
    | 	4 	| player not found  |    
    | 	5  	| {method} is not allowed   |
    |  	7  	| internal server error |
    | 	11 	| {parameter} is invalid   |
    | 	31 	| {parameter} must between 1 and 0   |

	

