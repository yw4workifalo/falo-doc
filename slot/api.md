# API 說明文件
## 目錄
1. [註冊帳號](#register)
1. [取得登入連結](#auth)
1. [登入](#login)
1. [查詢玩家](#player-info)   
2. [取得玩家遊戲紀錄](#bet-report-session)
3. [玩家離線通知](#offline_notification)
4. [玩家額度轉出入](#credit_transfer)
5. [玩家轉帳狀態查詢](#transfer-status)
6. [踢玩家](#kick)
7. [踢多玩家](#kick-multiple)
8. [限輸](#lose-limit)
9. [限贏](#win-limit)
10. [限注回復](#limit-recover)
11. [設定玩家帳號模式](#ban-mode)
12. [注單回補](#recover-logs)
13. [限注查詢](#limit-query)
14. [查詢玩家帳號模式](#get-ban-mode)
15. [查詢玩家是否啟用遊戲](#get-activation)
16. [設定玩家是否啟用遊戲](#set-activation)
17. [LOG查詢](#query-logs)
18. [玩家下注簡報查詢](#bet-report)
19. [玩家多人下注簡報區間總額查詢](#bet-report-multiple)
20. [玩家JP紀錄查詢](#jp-logs)
21. [玩家多人JP紀錄查詢](#jp-multiple)
22. [JP中獎紀錄](#jp-status)
23. [玩家JP核銷](#jp-vertification)



## 登入流程
1. 玩家透過平台登入
2. 平台透過[註冊帳號](#register)新增帳號
3. 平台透過[取得玩家登入網址](#auth)取得帳號密碼
4. 透過表單直接將取得的帳號密碼送到[登入](#login)驗證
5. API直接將玩家導向至遊戲網頁

[流程圖連結](https://cacoo.com/diagrams/UQAlsLczvUqmOvl6)

![流程圖](https://cacoo.com/diagrams/UQAlsLczvUqmOvl6-28811.png?t=1455870225441 "API 登入 流程圖")

## <span id="GameType">Slot Game Type</span>
| 遊戲名稱  | GameType                  | 
|---------- |-------------------------  |
| 武士道         | 1   | 
| 金錢貓         | 2   | 

## 錯誤代碼

| 錯誤代碼  | 錯誤訊息                  | 錯誤說明              |
|---------- |-------------------------  |---------------------- |
| 1         | \{parameter\} is required   | 缺少必填參數            |
| 2         | key is invalid            | 提供的金鑰是不合法的    |
| 3         | hash is invalid           | 驗證碼是錯誤的           |
| 4         | user not found            | 找不到合法的使用者     |
| 5         | \{method\} is not allowed   | http method 不允許       |
| 6         | query time range out of limit   | 查詢時間範圍超出限制       |
| 7         | internal server error   | 伺服器內部錯誤       |
| 8         | user offline   | 使用者離線       |
| 9         | add chips error   | 加錢錯誤       |
| 10        | service not available   | 無法使用遊戲服務       |
| 11        | user chips not enough   | 玩家籌碼不足       |
| 12        | \{parameter\} is invalid   | 參數不合法       |
| 13        | log not found   | 無法查詢到紀錄       |
| 14        | nicknames should mapping accounts   | 參數 nickname 與 accounts 需相對應    |
| 15        |duplicate transfer id|轉帳id重複|
| 16        |transfer log not found| 轉帳 LOG不存在|

## API
1. ### <span id="register">註冊帳號 </span>

    ```
    POST player/register?
        key=<key>&
        account=<account>&
        nickname=<nickname>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   nickname   | 玩家暱稱 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    |account| string | 帳號|
    |nickname| string | 玩家暱稱 |

2. ### <span id="auth">取得玩家登入網址</span>

    ```
    GET player/auth?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |    
    |   hash   | 驗證參數 |  string  |     必填    |

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

3. ### <span id="login">登入</span>


    ```
    GET 由[取得玩家登入網址](#auth)回傳的loginUrl
    ```

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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    
5. ### <span id="bet-report-session">取得玩家遊戲紀錄</span>

    ```
    GET player/bet/report/session?
        key=<key>&
        account=<account>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱  | 參數說明  | 參數型態  | 說明                            |
    |---------- |---------- |---------- |------------------------------ |
    | key       | 服務金鑰  | string    | 由API端提供                   |
    | account   | 玩家帳號  | string    | 必填                            |
    | startAt  | 開始時間  | string    | 固定格式Y-m-d,Y-m-d H:i:s或者0，0代表不限制，多查詢七天內的資料  |
    | endAt    | 結束時間  | string    | 固定格式Y-m-d,Y-m-d H:i:s或者0 ，代表不限制，最多查詢七天內的資料 |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果
    成功

    ```javascript
    [  
       {  
          "id":30,
          "user_id":1,
          "initial_chips":100720,
          "sum_of_bet":0,
          "sum_of_win_chips":0,
          "final_chips":100720,
          "ip":"192.168.10.1",
          "login_at":"2016-04-12 18:29:49",
          "logout_at":"2016-04-12 18:32:23",
          "created_at":"2016-04-12 18:29:49",
          "updated_at":"2016-04-12 18:32:23"
       }
    ]
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
    | initial_chips|string|本局初始金額|
    | sum_of_bet |int|下注|
    | sum_of_win_chips |int|損益|
    | final_chips |int|最後籌碼|            
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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  credit   | 增加的籌碼 |  int  |     必填, 負號代表轉出    |
    |  transferId   | 交易編號 |  int  |     必填    |    
    |   hash   | 驗證參數 |  string  |     必填    |

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
    {"status":"error","error":{"code":4,"message":"user not found"}}
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
      
7. ### <spin id="transfer-status">玩家轉帳狀態查詢</spin>

    ```
    GET player/credit/transfer-status?
        key=<key>&
        transferId =<transferId>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  transferId   | 交易編號 |  int  |     必填    |    
    |   hash   | 驗證參數 |  string  |     必填    |

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
          "transferId":"fsefs",
          "status":0
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
    |account|string|玩家帳號|  
    |originCredit|int|原始籌碼|
    |addCredit|int|新增籌碼|
    |finalCredit|int|最後籌碼|
    |transferId|string(30)|平台交易編號，unique|
    |orderId|int|遊戲方交易編號, unique|
    |status|int|狀態 0:success, 1:餘額不足, 2:伺服器錯誤, 3:訂單交易中|
    

8. ### <spin id="kick">踢玩家</spin>

    呼叫之後會在10秒之後將在線的玩家踢出遊戲
    ```
    DELETE player/kick?
        key=<key>&
        account=<account>&
        reason=<reason>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填   |
    |  reason  | 剔除原因 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + reason + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "status":"kicked",
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
    |status|string|狀態user_offline,kicked,user_not_found|
        
8. ### <spin id="kick-multiple">踢多玩家</spin>
    呼叫之後會在10秒之後將在線的玩家踢出遊戲
    ```
    DELETE player/kick-multiple?
        key=<key>&
        accounts=<accounts>&
        reason=<reason>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts| 玩家帳號 |  string  |     必填，可填多組用`,` 分割   |
    |  reason  | 剔除原因 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + reason + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "account":"kk",
             "status":"user_not_found"
          },
          {  
             "account":"aa",
             "status":"user_not_found"
          },
          {  
             "status":"user_offline",
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
    |status|string|狀態user_offline,kicked,user_not_found|

9. ### <span id="lose-limit">限輸</span>

    設定玩家限輸

    ```
    PUT player/limit/lose?
        key=<key>&
        account=<account>&
        limit=<limit>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  limit   | 限制金額 |  int  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    |account|string|玩家帳號|
    |limitLose|int|限輸金額|
    |limitWin|int|限贏金額|    
10. ### <span id="win-limit">限贏</span>

    設定玩家限贏

    ```
    PUT player/limit/win?
        key=<key>&
        account=<account>&
        limit=<limit>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  limit   | 限制金額 |  int  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    |account|string|玩家帳號|
    |limitLose|int|限輸金額|
    |limitWin|int|限贏金額|  

11. ### <span id="limit-recover">限注回復</span>

    限注回復

    ```
    PUT player/limit/recover?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account+secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "account":"mm@gmail.com",
          "winChips":0
       }
    }
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    |參數|型態|說明|
    |:---:|:---:|:---:|
    |account|string|玩家帳號|
    | winChips |int|回復後損益|
    
12. ### <span id="ban-mode">設定玩家帳號模式</span>

    玩家封鎖模式

    ```
    PUT player/mode?
        key=<key>&
        account=<account>&
        mode=<mode>&        
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填  |
    |   mode | 模式 |  int    |     必填(0:normal,1:locked, 2:banned) <ul><li>`normal` 是無封鎖</li><li>`banned` 是停用</li><li>`locked`是鎖單</li><li>`no_permission`是無權限</li></ul>
    |   hash   | 驗證參數 |  string  |     必填    |

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
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

13. ### <span id="recover-logs">注單回補</span>

    注單回補，注單有可能太多，造成執行過久，所以是利用queue的方式，因此response之後注單不會立刻回補。

    ```
    GET player/logs/recover?
        key=<key>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "success":true
       }
    }
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

14. ### <span id="limit-query">限注查詢</span>

    查詢玩家限注狀態

    ```
    GET player/limit?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":{  
          "account":"mm@gmail.com",
          "winChips":0,
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
    |account|string|玩家帳號|
    |limitLose|int|限輸金額|
    |limitWin|int|限贏金額|    
    | winChips |int|損益|     

15. ### <span id="get-ban-mode">查詢玩家帳號模式</span>

    查詢玩家鎖單/停用狀態

    ```
    GET player/mode?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填，支援多組帳號可用`,`分割   |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + secret)**

    ##### 回傳結果
    成功

    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "account":"test-api01",
             "status":"normal"
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
    |status|string|模式|    

16. ### <span id="get-activation">查詢玩家是否啟用遊戲</span>

    查詢玩家是否啟用遊戲

    ```
    GET player/enable?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填，支援多組帳號可用`,`分割   |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + secret)**

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
    |account|string|玩家帳號|
    | activation |string|是否啟動 1:是, 0:否|    

17. ### <span id="set-activation">設定玩家是否啟用遊戲</span>

    設定玩家是否啟用遊戲

    ```
    PUT player/enable?
        key=<key>&
        account=<account>&
        enable=<0|1>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填，支援多組帳號可用`,`分割   |
    |enable| 驗證參數 |  int    |     必填，是否啟動 1:是, 0:否 |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    |account|string|玩家帳號|
    | activation |int|是否啟動 1:是, 0:否| 

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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填 |
    |  gameType | 遊戲代稱 |  int  |     選填 [GameType](#GameType) |    
    |startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果
    成功
    ```javascript
	{  
	   "status":"success",
	   "data":[  
	      {  
	         "id":423,
	         "machine_no":1,
	         "bet":50,
	         "bet_lines":9,
	         "total_bet":450,
	         "win_chips":500,
	         "scatter":0,
	         "bonus":0,
	         "created_at":"2017-04-17 10:04:12",
	         "game_result":[  
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
	         "machine_no":1,
	         "bet":50,
	         "bet_lines":9,
	         "total_bet":450,
	         "win_chips":300,
	         "scatter":0,
	         "bonus":0,
	         "created_at":"2017-04-17 10:53:22",
	         "game_result":[  
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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填 |
    |startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

       **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果

    成功
    ```javascript
    {  
       "status":"success",
       "data":{  
          "total_bet":"540",
          "win_chips":"40"
       }
    }
    ```

    失敗
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  gameType | 遊戲代稱 |  int  |     必填 [GameType](#GameType) |
    | startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    | endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

       **hash = md5(account + startAt + endAt + secret)**

    ##### 回傳結果

    成功
    ```javascript
    {  
       "status":"success",
       "data":[  
          {  
             "account":"leochen",
             "win_chips":null,
             "total_bet":40,
             "created_at":"2016-12-16 14:00:11"
          },
          {  
             "account":"wei01",
             "win_chips":"28935",
             "total_bet":40,
             "created_at":"2017-03-23 09:36:08"
          }
       ]
    }
    ```

    失敗
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填 |
    |startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    | jackpot |string|遊戲jackpot編號|
    | createdAt |string|jackpot時間|
    | paidAt |string|核銷時間|
    
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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  gameType | 遊戲代稱 |  int  |     必填 [GameType](#GameType) |
    |startAt  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |endAt    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    | jackpot |string|遊戲jackpot編號|
    | created_at |string|jackpot時間|
    | paid_at |string|核銷時間|    

21. ### <span id="jp-status">JP中獎紀錄</span>


    ```
    GET jackpot-status?
        key=<key>&
        status=<status>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  status| 狀態 |  int  |  必填 0:不限,1未核銷,2：已核銷 |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    | jackpot |string|遊戲jackpot編號|
    | created_at |string|jackpot時間|
    | paid_at |string|核銷時間|
             
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

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填 |
    |jp_id  | jp紀錄的id |  int  |     jp紀錄的id，離線通知會提供 |
    |verified_at| 核銷日期 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

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
    | jackpot |string|遊戲jackpot編號|
    | createdAt |string|jackpot時間|
    | paidAt |string|核銷時間|
