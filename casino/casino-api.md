# Casino API Document
  
### API數量：**15**
-------
1. [註冊帳號](#註冊帳號)
1. [取得玩家登入網址](#取得玩家登入網址)
1. [查詢玩家](#查詢玩家)
1. [修改玩家暱稱](#修改玩家暱稱)
1. [設定玩家帳號模式](#設定玩家帳號模式)
1. [設定玩家是否啟用遊戲](#設定玩家是否啟用遊戲)
1. [設定玩家限輸](#設定玩家限輸)
1. [設定玩家限贏](#設定玩家限贏)
1. [玩家限注回復](#玩家限注回復)
1. [玩家額度轉出入](#玩家額度轉出入)
1. [玩家轉帳狀態查詢](#玩家轉帳狀態查詢)
1. [玩家下注簡報查詢](#玩家下注簡報查詢)
1. [玩家多人下注簡報區間總額查詢](#玩家多人下注簡報區間總額查詢)
1. [踢玩家](#踢玩家)
1. [踢多玩家](#踢多玩家)

### *登入流程*
-------
1. 玩家透過平台登入
2. 平台透過[註冊帳號](#註冊帳號)註冊帳號
3. 平台透過[玩家額度轉出入](#玩家額度轉出入)
4. 平台透過[取得玩家登入網址](#取得玩家登入網址)
5. 使用[取得玩家登入網址](#取得玩家登入網址)回傳內容 `LoginUrl` 登入遊戲

### [流程圖連結](https://cacoo.com/diagrams/xgypN4flYVaJlTcU#28811)

![流程圖](https://cacoo.com/diagrams/xgypN4flYVaJlTcU-28811.png "API 登入 流程圖")


### *錯誤代碼*
-------
| 錯誤代碼  | 錯誤訊息                  | 錯誤說明              |
|---------- |-------------------------  |---------------------- |
| 1  | {parameter} is required   | 缺少必填參數            |
| 2  | key is invalid            | 提供的金鑰是不合法的    |
| 3  | hash is invalid           | 驗證碼是錯誤的           |
| 4  | player not found            | 找不到合法的使用者     |
| 5  | {method} is not allowed   | http method 不允許       |
| 6  | query time range out of limit   | 查詢時間範圍超出限制       |
| 7  | internal server error   | 伺服器內部錯誤       |
| 8  | player is offline   | 玩家離線       |
| 9  | player is online   | 玩家在線中       |
| 10 | service not available   | 無法使用遊戲服務       |
| 11 | {parameter} is invalid   | 參數不合法       |
| 12 | gameType  not found  | 無此遊戲存在       |
| 13 | account length between 4 - 20  |  帳號至少4位元以上      |
| 14 | nickname length between 1- 20  | 暱稱至少1位元以上       |
| 15 | enable setting is invalid  | 玩家啟用設定值不合法       |
| 16 | mode setting is invalid  | 玩家帳號模式設定值不合法       |
| 17 | transfer in error  | 額度轉入失敗       |
| 18 | transfer out error  | 額度轉出失敗       |
| 19 | player credit is not enough  | 玩家籌碼不足       |
| 20 | range :{startAt} to {EndAt} not found bet results|查詢結果無注單        |
| 21 | bet accounts processing  | 帳務結算中不可額度轉出入       |
| 22 | {param} must be a unsigned int  | {param}設定值必須為正整數       |
| 23 | [startAt or  EndAt] value must be datetime Example：2017-01-01 00:00:00   | 查詢玩家注單，時間參數必須使用規定格式       |
| 24 |{transferId} is not exist   | 此筆交易單不存在       |
| 25 | transfer id credit can not be empty  | 交易的ID不可為空       |
| 26 | transferid:（平台方TransferID）has been used  | 此交易單已被使用       |
| 27 | transfer in or out can not be 0  | 額度轉出入設定值不可為0       |

### <span id="GameType">Casino Game Type</span>
| 遊戲名稱  | gameType                  | 
|---------- |-------------------------  |
| 百家樂         | 1   | 


### *API 使用*
-------

1. ## <span id="player-register">註冊帳號</span>

    ```
    POST /casino-api/player/register?
        key=<key>&
        account=<account>&
        nickname=<nickname>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  nickname   | 玩家暱稱 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + nickname + secret)`**
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  nickname   | 玩家暱稱 |  string  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "nickname":"a99asdf"
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

   ---
   
   #### 會出現的錯誤項目
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
   
   
2. ## <span id="player-auth">取得玩家登入網址</span>

    ```
    GET /casino-api/player/auth?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  loginUrl | 遊戲登入網址|  string  | 

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "loginUrl":"http://test.app/casino-api/player/login?token=......M1NyJ9"}
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
   
   ---
   
   #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found            |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
   
   
3. ## <span id="player-info">查詢玩家</span>

    ```
    GET /casino-api/player/info?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號|  string  | 
    |  nickname | 玩家暱稱|  string  | 
    |  enable | 帳號啟用|  smallint  | 
    |  mode | 帳號模式|  smallint  | 
    |  isOnline | 玩家是否在線|  smallint  | 
    |  credit | 額度|  float  | 
    |  limitWin | 限贏|  integer  | 
    |  limitLose | 限輸|  integer  | 

    ---

    #### <span id="enable">玩家啟用狀態</span>
    | 代碼  | enable                  | 
    |---------- |-------------------------  |
    | 0         | 停用   | 
    | 1        | 啟用   | 
    
    #### <span id="mode">玩家帳號模式</span>
    | 代碼  | mode                  | 
    |---------- |-------------------------  |
    | 0         | 正常   | 
    | 1        | 鎖單(不能下注)   | 
    | 2        | 停用(不能進入遊戲)，修改狀態為 2時會執行踢除玩家   | 
        
    #### <span id="isOnline">玩家是否在線</span>
    | 代碼  | isOnline                  | 
    |---------- |-------------------------  |
    | 0        | 玩家離線  | 
    | 1        | 玩家在線  |

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "nickname":"a99asdf",
            "enable":1,
            "mode":0,
            "isOnline":0,
            "credit":4101,
            "limitWin":1099,
            "limitLose":1
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
   
    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found            |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |


4. ## <span id="player-nickname">修改玩家暱稱</span>

    ```
    PUT /casino-api/player/nickname?
        key=<key>&
        account=<account>&
        nickname=<nickname>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  nickname   | 玩家暱稱 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + nickname + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  nickname   | 玩家暱稱 |  string  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "nickname":"a99asdf"
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
   
    ---
   
   #### 會出現的錯誤項目
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
    
5. ## <span id="player-mode">設定玩家帳號模式</span>

    ```
    PUT /casino-api/player/mode?
        key=<key>&
        account=<account>&
        mode=<nickname>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  mode   | 帳號模式 |  smallint  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    
    #### <span id="mode">玩家帳號模式</span>
    | 代碼  | mode                  | 
    |---------- |-------------------------  |
    | 0         | 正常   | 
    | 1        | 鎖單(不能下注)   | 
    | 2        | 停用(不能進入遊戲)，修改狀態為 2時會執行踢除玩家   | 
    
    #### **`hash = md5(account + mode + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  mode   | 帳號模式 |  smallint  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "mode":1
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":16,"message":"mode setting is invalid"
        }
    }
   ```
   
   #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    | 16 | mode setting is invalid  |
    | 22 | {param} must be a unsigned int  |

6. ## <span id="player-enable">設定玩家是否啟用遊戲</span>

    ```
    PUT /casino-api/player/enable?
        key=<key>&
        account=<account>&
        enable=<enable>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  enable   | 玩家帳號啟用 |  smallint  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### <span id="enable">玩家啟用狀態</span>
    | 代碼  | enable                  | 
    |---------- |-------------------------  |
    | 0         | 停用   | 
    | 1        | 啟用   | 

    #### **`hash = md5(account + enable + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  enable   | 玩家帳號啟用 |  smallint  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "enable":1
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":15,
            "message":"enable setting is invalid"
        }
    }
   ```


    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    | 15 | enable setting is invalid  |
    | 22 | {param} must be a unsigned int  |
    
7. ## <span id="player-limit-lose">設定玩家限輸</span>

    ```
    PUT /casino-api/player/limit/lose?
        key=<key>&
        account=<account>&
        limit=<limit>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  limit   | 限輸值 |  integer |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + limit + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  limit   | 限輸值 |  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "limitLose":10
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":22,
            "message":"LimitLose must be a unsigned int"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    | 22 | {param} must be a unsigned int  |
    
8. ## <span id="player-limit-win">設定玩家限贏</span>

    ```
    PUT /casino-api/player/limit/win?
        key=<key>&
        account=<account>&
        limit=<limit>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  limit   | 限贏值 |  integer |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + limit + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  limit   | 限贏值 |  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "limitWin":"1099"
        }
    }
    ```

    失敗

   ```javascript
    {
        "status":"error",
        "error":{
            "code":22,
            "message":"LimitWin must be a unsigned int"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    | 22 | {param} must be a unsigned int  |
    

9. ## <span id="player-recover">玩家限注回復</span>

    ```
    PUT /casino-api/player/limit/recover?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234"
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

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    

10. ### <spin id="player-transfer">玩家額度轉出入</spin>
   
    ```
    PUT /casino-api/player/credit/transfer?
        key=<key>&
        account=<account>&
        credit=<credit>&
        transferId=<transferId>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  credit | 轉出入額度 |integer  |必填，正值為轉入，負值為轉出    |
    |transferId |平台方交易Id |string| 必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    
    #### **`hash = md5(account + credit + transferId + secret)`**
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  originCredit   | 交易前額度 |  float  |
    |  addCredit   | 增加額度 |  integer  |
    |  finalCredit   | 交易後額度 |  float  |
    |  transferId   | 平台方交易Id |  string  |
    |  orderId   | 此筆交易單號 |  integer  |
    |  status   | 交易狀態 |  smallint  |

    #### <span id="enable">交易狀態</span>
    | 代碼  | status                  | 
    |---------- |-------------------------  |
    | 0         | 成功   | 
    | 1        | 伺服器錯誤  | 
    
    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "originCredit":4201.6,
            "addCredit":500,
            "finalCredit":4701.6,
            "transferId":"AA1099",
            "orderId":28,
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
            "message":"account not found"
        }
    }
    ```

    #### 會出現的錯誤項目
    
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid    |
    | 3  | hash is invalid    |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    | 19 | player credit is not enough  |
    | 25 | transfer id credit can not be empty  |
    | 26 | transferid:（平台方TransferID）has been used  | 此交易單已被使用 |
    | 27 | transfer in or out can not be 0  | 額度轉出入設定值不可為0 |

    
11. ### <spin id="player-transfer-status">玩家轉帳狀態查詢</spin>
    
    ```
    GET /casino-api/player/credit/transfer-status?
        key=<key>&
        transferId=<transferId>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |transferId |平台方交易Id |string| 必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(transferId + secret)`**
    
    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  originCredit   | 交易前額度 |  float  |
    |  addCredit   | 增加額度 |  integer  |
    |  finalCredit   | 交易後額度 |  float  |
    |  transferId   | 平台方交易Id |  string  |
    |  orderId   | 此筆交易單號 |  integer  |
    |  status   | 交易狀態 |  smallint  |

    #### <span id="status">交易狀態</span>
    | 代碼  | status                  | 
    |---------- |-------------------------  |
    | 0         | 成功   | 
    | 1        | 餘額不足  | 
    | 2 | 伺服器錯誤 |
    | 3 | 交易單處理中 |
    ---

    #### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "originCredit":4201.6,
            "addCredit":500,
            "finalCredit":4701.6,
            "transferId":"AA99",
            "orderId":28,
            "status":0
        }
    }
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":1,
            "message":"transferId is required"
        }
    }
    ```

    #### 會出現的錯誤項目
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 25 | transfer id credit can not be empty  |


12. ### <span id="bet-report">玩家下注簡報查詢</span>

    ```
    GET /casino-api/bet/report?
        key=<key>&
        account=<account>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + startAt + endAt + secret)`**
    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  betFormId| 注單編號 |integer  |
    |  gameType| [遊戲類別](#遊戲類別) |smallint  |
    |  gameMethods|[遊戲玩法](#遊戲玩法)  |smallint  |
    |  tableId|桌台編號  | string |
    |  tableType|[桌台類型](#桌台類型)  | smallint  |
    |  round| 輪號 | integer |
    |  run| 局號 | integer |
    |  gameResult| 遊戲結果 | string  |
    |gameResultDescription | 遊戲開牌結果，[百家樂遊戲開牌代碼說明](#百家樂遊戲開牌代碼說明) | string |
    | gameRunStartTime | 遊戲局開始時間 | datetime |
    | gameRunEndTime | 遊戲局結束時間 | datetime |
    | amount | 總下注注額 | integer |
    | validAmount | 總有效下注注額 | integer |
    | betDetail | 下注注區，金額，賠率明細 | string |
    | loseWinAmount | 輸贏額度  | float |
    | betTime | 下注時間 | datetime |
    | status | [注單狀態](#注單狀態) | smallint  |
    ---

    ```
    百家樂 無風險下注說明
    同時下注莊閒、閒單閒雙、莊單莊雙、大小皆算無風險投注
    ```
    
    ### <span id="GameType">遊戲類別</span>
    | 遊戲名稱  | gameType                  | 
    |---------- |-------------------------  |
    | 百家樂         | 1   | 
    
    ### <span id="gameMethods">遊戲玩法</span>
    | 名稱  | gameMethods                  | 
    |---------- |-------------------------  |
    | 百家樂標準         | 1   | 
    | 百家樂免水         | 2   |
    | 百家樂西洋         | 3   |
    
    ### <span id="tableType">桌台類型</span>
    | 名稱  | tableType                  | 
    |---------- |-------------------------  |
    | 標準百家樂         | 1   | 
    | 先發百家樂         | 2   |
    | 極速百家樂         | 3   |
    | 眯牌百家樂         | 4   |
    
    ### <span id="gameResultDescription">百家樂遊戲開牌代碼說明</span>
    |  牌例 |  對應                 | 
    |---------- |-------------------------  |
    | S1   | 黑桃 A   | 
    | H0   | 紅心 10   |
    | DJ   | 方塊 J   |
    | C6   | 梅花 6  |
    
    ### <span id="status">注單狀態</span>
    |  狀態 |  status               | 
    |---------- |-------------------------  |
    | 0  | 正常   | 
    | 1  | 改單   |
    | 2   | 取消單   |
    
    #### Response 結果
    成功

    ```javascript
    {
        "status": "success",
        "data": [
            {
                "account": "aa1234",
                "betFormId": 716719,
                "gameType": 1,
                "gameMethods": 1,
                "tableId": "A",
                "tableType": 1,
                "round": 1870760,
                "run": 17,
                "gameResult": "banker win",
                "gameResultDescription": {
                    "banker": "D4,S4",
                    "player": "S6,HK"
                },
                "gameRunStartTime": "2017-01-02 15:00:00",
                "gameRunEndTime": "2017-01-02 15:02:10",
                "amount": 3000,
                "validAmount": 2000,
                "betDetail": [
                    {
                        "spot": "莊家",
                        "odds": 0.95,
                        "betAmount": 1000,
                        "betWin": 950
                    },
                    {
                        "spot": "閒家",
                        "odds": 1,
                        "betAmount": 1000,
                        "betWin": -1000
                    },
                    {
                        "spot": "大",
                        "odds": 0.53,
                        "betAmount": 1000,
                        "betWin": -1000
                    },
                    {
                        "spot": "莊雙",
                        "odds": 0.92,
                        "betAmount": 1000,
                        "betWin": 920
                    }
                ],
                "loseWinAmount": -130,
                "betTime": "2017-01-02 15:00:10",
                "status": 0
            },
            {
                "account": "aa1234",
                "betFormId": 716720,
                "gameType": 1,
                "gameMethods": 1,
                "tableId": "A",
                "tableType": 3,
                "round": 1889286,
                "run": 25,
                "gameResult": "player win",
                "gameResultDescription": {
                    "banker": "D8,C9",
                    "player": "C6,H3"
                },
                "gameRunStartTime": "2017-01-02 16:30:00",
                "gameRunEndTime": "2017-01-02 16:31:02",
                "amount": 6000,
                "validAmount": 2000,
                "betDetail": [
                    {
                        "spot": "莊家",
                        "odds": 0.95,
                        "betAmount": 1000,
                        "betWin": -1000
                    },
                    {
                        "spot": "閒家",
                        "odds": 1,
                        "betAmount": 1000,
                        "betWin": 1000
                    },
                    {
                        "spot": "大",
                        "odds": 0.53,
                        "betAmount": 1000,
                        "betWin": -1000
                    },
                    {
                        "spot": "小",
                        "odds": 1.45,
                        "betAmount": 1000,
                        "betWin": 1450
                    },
                    {
                        "spot": "莊雙",
                        "odds": 0.92,
                        "betAmount": 1000,
                        "betWin": -1000
                    }
                    {
                        "spot": "閒雙",
                        "odds": 0.88,
                        "betAmount": 1000,
                        "betWin": -1000
                    }
                ],
                "loseWinAmount": -1550,
                "betTime": "2017-01-02 16:30:11",
                "status": 0
            },
        ]
    }
    
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":20,
            "message":"range :2017-01-01 00:00:00 to 2017-01-02 00:00:00 not found bet results"
        }
    }
    ```
   
    #### 會出現的錯誤項目
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found            |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 20 | range :{startAt} to {EndAt} not found bet results|查詢結果無注單        |
    | 23 | [startAt or  EndAt] value must be datetime Example：2017-01-01 00:00:00   | 查詢玩家注單，時間參數必須使用規定格式       |
    | 25 | transfer id credit can not be empty  |

13. #### <span id="bet-report-multiple">玩家多人下注簡報區間總額查詢</span>

    ```
    GET /casino-api/bet/report?
        key=<key>&
        gameType=<gameType>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  gameType | [遊戲類別](#遊戲類別) |  string  |     必填    |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(gameType + startAt + endAt + secret)`**
    
    ### <span id="GameType">遊戲類別</span>
    | 遊戲名稱  | gameType                  | 
    |---------- |-------------------------  |
    | 百家樂         | 1   | 
    
    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  betFormCount | 注單數量 |integer  |
    |  totalAmount | 總下注額度 |integer  |
    |  totalValidAmount | 總有效下注額度 |integer  |
    |  totalLoseWinAmount | 總輸贏 | float(小數點第一位)  |
        
    ---

    #### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":[
            {
                "account":"a1234",
                "betFormCount":13,
                "totalAmount":2000,
                "totalValidAmount":2000,
                "totalLoseWinAmount":1000,
            },
            {
                "account":"a129995b",
                "betFormCount":67,
                "totalAmount":563900,
                "totalValidAmount"365000,
                "totalLoseWinAmount":-10647.6,
            },
        ]
    }
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":22,
            "message":"gameType must be a unsigned int"
        }
    }
    ```

14. #### <span id="player-kick">踢玩家</span>

    ```
    DELETE /casino-api/player/kick?
        key=<key>&
        account=<account>&
        reason=<reason>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 遊戲帳號 |  string  |     必填    |
    |  reason   | 原因 |  string  |     非必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + secret)`**
    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  status   | [帳號處理狀況](#帳號處理狀況)|  smallint  |
    ---

    ### <span id="status">帳號處理狀況</span>
    | 狀態代碼 | status                  | 
    |---------- |-------------------------  |
    | 0 | kick success  | 
    | 1 | player is not online | 


    #### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "status":1
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
    
    
    #### 會出現的錯誤項目
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found            |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
   
15. #### <span id="player-kick-multiple">踢多玩家</span>

    ```
    DELETE /casino-api/player/kick-multiple?
        key=<key>&
        account=<account>&
        reason=<reason>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 遊戲帳號 |  string  |     必填，多玩家使用 `,` 號隔開   |
    |  reason   | 原因 |  string  |     非必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + secret)`**
    ---
    #### Response 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  status   | [多玩家帳號處理狀況](#多玩家帳號處理狀況)|  smallint  |
    ---

    ### <span id="status">多玩家帳號處理狀況</span>
    | 狀態代碼 | status                  | 
    |---------- |-------------------------  |
    | 0 | kick success  | 
    | 1 | player is not online | 
    | 2 | player not found |
    
    #### Response 結果
    
    成功
    
    ```javascript
    {
        "status":"success",
        "data":[
            {
                "account":"a1234",
                "status":0
            },
            {
                "account":"abc1234",
                "status":1
            },
            {
                "account":"bb8899",
                "status":2
            },
        ]
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
    
    #### 會出現的錯誤項目
    | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |


