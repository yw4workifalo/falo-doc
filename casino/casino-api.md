#Casino API Document
  
### API數量：**15**
-------
>####[註冊帳號](#player-register)
>####[取得玩家登入網址](#player-auth)
>####[查詢玩家](#player-info)
>####[修改玩家暱稱](#player-nickname)
>####[設定玩家帳號模式](#player-mode)
>####[設定玩家是否啟用遊戲](#player-enable)
>####[設定玩家限輸](#player-limit-lose)
>####[設定玩家限贏](#player-limit-win)
>####[玩家限注回復](#player-recover)
>####[玩家額度轉出入](#player-transfer)
>####[玩家轉帳狀態查詢](#player-transfer-status)
>####[玩家下注簡報查詢](#bet-report)
>####[玩家多人下注簡報區間總額查詢](#bet-report-multiple)
>####[踢玩家](#player-kick)
>####[踢多玩家](#player-kick-multiple)

### *登入流程*
-------
####1. 玩家透過平台登入
####2. 平台透過[註冊帳號](#player-register)註冊帳號
####3. 平台透過[玩家額度轉出入](#player-transfer)
####4. 平台透過[取得玩家登入網址](#player-auth)
####5. 使用[取得玩家登入網址](#player-auth)回傳內容 `LoginUrl` 登入遊戲

###[流程圖連結](https://cacoo.com/diagrams/xgypN4flYVaJlTcU#28811)

![流程圖](https://cacoo.com/diagrams/xgypN4flYVaJlTcU-28811.png "API 登入 流程圖")


### *錯誤代碼*
-------
| 錯誤代碼  | 錯誤訊息                  | 錯誤說明              |
|---------- |-------------------------  |---------------------- |
| 1         | \{parameter\} is required   | 缺少必填參數            |
| 2         | key is invalid            | 提供的金鑰是不合法的    |
| 3         | hash is invalid           | 驗證碼是錯誤的           |
| 4         | account not found            | 找不到合法的使用者     |
| 5         | \{method\} is not allowed   | http method 不允許       |
| 6         | query time range out of limit   | 查詢時間範圍超出限制       |
| 7         | internal server error   | 伺服器內部錯誤       |
| 8         | player is offline   | 玩家離線       |
| 9         | player is online   | 玩家在線中       |
| 10        | service not available   | 無法使用遊戲服務       |
| 11        | \{parameter\} is invalid   | 參數不合法       |
| 12        | [遊戲英文名稱](#gametype)  is not find  | 無此遊戲存在       |
| 13        | transfer in error  | 額度轉入失敗       |
| 14        | transfer out error  | 額度轉出失敗       |
| 15        | api server error  | api server 發生錯誤       |
| 16        | player is not valid  | 玩家需透過[取得玩家帳號密碼](#player-auth)才可進入遊戲       |
| 17        | credit is not enough  | 額度轉出時，娛樂場內額度不足       |
| 19        | bet accounts processing  | 帳務結算中不可額度轉出入       |
| 23  | [`start_at` or `end_at`] value must be datetime Example：2016-01-01 00:00:00| 查詢玩家遊戲紀錄，時間參數必須使用規定格式       |
| 24  | `2017-01-01 00:00:00` to `2017-01-02 00:00:00` not found bet results| 起始時間 to 結束時間 未查詢到注單資料       |
| 25  | transfer credit has some problem, please try again later| 洗分時發生暫停性錯誤，請稍等片刻後再重試       |
| 26  | check transferId:`單號` is not exist| 此筆交易單號不存在      |
| 27 | transferId value must be an numeric|交易序號必須為整數 |
| 28 | account must be english or numeric or account length between 4 - 20 | 帳號必須是英文和數字組合，至少4位元以上|
| 29 | nickname length between 4 - 20| 暱稱至少4位元以上 |
| 30 | player account is not enable| 玩家帳號已被設定為`不啟用`|
| 31 | player account mode is setting stop| 玩家帳號已被設定為`停用` |
| 33 | limit value must be an integer or limit value must be an numeric| 限輸 限贏的設定值 必須為整值|
| 34 | limit value must be an integer or limit value must be an numeric | 玩家啟用 設定值 必須為 整數|
| 35 |limit value must be an integer or limit value must be an numeric | 玩家帳號模式 設定值 必須為 整數|
| 36 |transfer_credit_can_not_zero | 交易單號不可為 0|
| 37 |transfer id：`(平台方TransferId)` is been used | 此交易單號已被使用|
| 38 |gametype：[遊戲英文名稱](#gametype)，range：`2017-01-01 00:00:00` to `2017-01-02 00:00:00` not found bet results | 查詢遊戲查無結果|

<span id="gametype">

###＊遊戲類別中英文對照
>####百家樂：Baccarat

</span>

### *API 使用*
-------

1. ### <span id="player-register">註冊帳號</span>

    ```
    POST /casino-api/player/register?
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
    |  nickname   | 玩家暱稱 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + nickname + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  nickname   | 玩家暱稱 |  string  |

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status": "success","account":"test123","nickname":"我是測試"}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

2. ### <span id="player-auth">取得玩家登入網址</span>

    ```
    GET /casino-api/player/auth?
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
    
    ####**`hash = md5(account + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  loginUrl | 遊戲登入網址|  string  | 

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"loginUrl":"http://test.app/casino-api/player/
    login?token=......M1NyJ9"}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```
   
   
3. ### <span id="player-info">查詢玩家</span>

    ```
    GET /casino-api/player/info?
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
    
    ####**`hash = md5(account + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號|  string  | 
    |  nickname | 玩家暱稱|  string  | 
    |  enable | 帳號啟用|  smallint  | 
    |  mode | 帳號模式|  smallint  | 
    |  isOnline | 玩家是否在線|  smallint  | 
    |  credit | 遊戲登入網址|  float  | 
    |  limitWin | 遊戲登入網址|  integer  | 
    |  limitLose | 遊戲登入網址|  integer  | 

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","nickname":"a99asdf","enable":1,"mode":0,"isOnline":0,"credit":4101,"limitWin":1099,"limitLose":1}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```
   
4. ### <span id="player-nickname">修改玩家暱稱</span>

    ```
    PUT /casino-api/player/nickname?
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
    |  nickname   | 玩家暱稱 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + nickname + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  nickname   | 玩家暱稱 |  string  |

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status": "success","account":"test123","nickname":"我是測試"}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

5. ### <span id="player-mode">設定玩家帳號模式</span>

    ```
    PUT /casino-api/player/mode?
        key=<key>&
        account=<account>&
        mode=<nickname>&
        hash=<hash>
    ```
    
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  mode   | 帳號模式 |  smallint  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    
    ```
        mode 說明
        0:normal
        1:鎖單(不能下注)
        2:停用(不能進遊戲)
            停用會立即踢除玩家
    ```

    ####**`hash = md5(account + mode + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  mode   | 帳號模式 |  smallint  |

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","mode":"1"}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

6. ### <span id="player-enable">設定玩家是否啟用遊戲</span>

    ```
    PUT /casino-api/player/enable?
        key=<key>&
        account=<account>&
        enable=<enable>&
        hash=<hash>
    ```
    
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  enable   | 玩家帳號啟用 |  smallint  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ```
        enable 說明
        0:停用
        1:啟用
    ```

    ####**`hash = md5(account + enable + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  enable   | 玩家帳號啟用 |  smallint  |

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","enable":"1"}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

7. ### <span id="player-limit-lose">設定玩家限輸</span>

    ```
    PUT /casino-api/player/limit/lose?
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
    |  limit   | 限輸值 |  integer |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + limit + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  limit   | 限輸值 |  integer  |

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","limitLose":"10"}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

8. ### <span id="player-limit-win">設定玩家限贏</span>

    ```
    PUT /casino-api/player/limit/win?
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
    |  limit   | 限贏值 |  integer |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + limit + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  limit   | 限贏值 |  integer  |

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","limitWin":"1099"}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

9. ### <span id="player-recover">玩家限注回復</span>

    ```
    PUT /casino-api/player/limit/recover?
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
    
    ####**`hash = md5(account + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 

    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234"}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```


10. ### <spin id="player-transfer">玩家額度轉出入</spin>
   
    ```
    PUT /casino-api/player/credit/transfer?
        key=<key>&
        credit=<credit>&
        transferId=<transferId>&
        hash=<hash>
    ```
    
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  credit | 轉出入額度 |  integer  |     必填，正值為轉入，負值為轉出   |
    |  transferId   | 平台方交易Id |  integer  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + credit + transferId + secret)`**
    
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  originCredit   | 交易前額度 |  float  |
    |  addCredit   | 增加額度 |  integer  |
    |  finalCredit   | 交易後額度 |  float  |
    |  transferId   | 平台方交易Id |  string  |
    |  orderId   | 此筆交易單號 |  integer  |
    |  status   | 交易狀態 |  smallint  |

    ```
        status 說明
        1:餘額不足
        2:伺服器錯誤
    ```
    
    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","originCredit":4201.6,"addCredit":500,"finalCredit":4701.6,"transferId":99,"orderId":28,"status":0}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

11. ### <spin id="player-transfer-status">玩家轉帳狀態查詢</spin>
    
    ```
    GET /casino-api/player/credit/transfer-status?
        key=<key>&
        transferId=<transferId>&
        hash=<hash>
    ```
    
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  transferId |平台方交易Id |  integer  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + transferId + secret)`**
    
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  originCredit   | 交易前額度 |  float  |
    |  addCredit   | 增加額度 |  integer  |
    |  finalCredit   | 交易後額度 |  float  |
    |  transferId   | 平台方交易Id |  string  |
    |  orderId   | 此筆交易單號 |  integer  |
    |  status   | 交易狀態 |  smallint  |

    ```
        status 說明
        1:餘額不足
        2:伺服器錯誤
        3:交易單處理中
    ```
    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","originCredit":4201.6,"addCredit":500,"finalCredit":4701.6,"transferId":99,"orderId":28,"status":0}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

12. ### <span id="bet-report">玩家下注簡報查詢</span>

    ```
    GET /casino-api/bet/report?
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
    |  account | 玩家帳號 |  string  |     必填    |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + startAt + endAt + secret)`**
    ---
    ##### 回傳參數說明
    
    ---

    ##### 回傳結果
    成功

    ```javascript
    
    ```

    失敗

   ```javascript
   
   ```
   
13. ### <span id="bet-report-multiple">玩家多人下注簡報區間總額查詢</span>

    ```
    GET /casino-api/bet/report?
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
    |  gameType | 遊戲類別 |  string  |     必填    |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(gameType + startAt + endAt + secret)`**
    ---
    ##### 回傳參數說明
    
    ---

    ##### 回傳結果
    成功

    ```javascript
    
    ```

    失敗

   ```javascript
   
   ```

14. ### <span id="player-kick">踢玩家</span>

    ```
    DELETE /casino-api/player/kick?
        key=<key>&
        account=<account>&
        reason=<reason>&
        hash=<hash>
    ```
    
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 遊戲帳號 |  string  |     必填    |
    |  reason   | 原因 |  string  |     非必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + secret)`**
    ---
    ##### 回傳參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  status   | 帳號處理狀況|  string  |
    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status":"success","data":{"account":"a1234","status":"player is not online"}}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```
   
15. ### <span id="player-kick-multiple">踢多玩家</span>

    ```
    DELETE /casino-api/player/kick-multiple?
        key=<key>&
        account=<account>&
        reason=<reason>&
        hash=<hash>
    ```
    
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 遊戲帳號 |  string  |     必填，多玩家使用 `,` 號隔開   |
    |  reason   | 原因 |  string  |     非必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    ####**`hash = md5(account + startAt + endAt + secret)`**
    ---
    ##### 回傳參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  status   | 帳號處理狀況|  string  |
    ---

    ##### 回傳結果
    成功

    ```javascript
    {"status": "success","account":"test123","nickname":"我是測試"}
    ```

    失敗

   ```javascript
   {"status":"error","error":{"code":4,"message":"account not found"}}
   ```

