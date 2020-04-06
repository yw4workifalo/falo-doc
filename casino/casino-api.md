# Casino API Document
  
## CASINO API數量：**40**
-------
1. [註冊帳號](#註冊帳號)(*1)
2. [取得玩家登入網址](#取得玩家登入網址)(*2)
3. [查詢玩家](#查詢玩家)(*3)
4. [修改玩家暱稱](#修改玩家暱稱)
5. [設定玩家帳號模式](#設定玩家帳號模式)(*4)
6. [設定玩家是否啟用遊戲](#設定玩家是否啟用遊戲)(*5)
7. [設定玩家限輸](#設定玩家限輸)(*6)
8. [設定玩家限贏](#設定玩家限贏)(*7)
9. [玩家限注查詢](#玩家限注查詢)(*8)
10. [玩家限注回復](#玩家限注回復)(*9)
11. [玩家額度轉出入](#玩家額度轉出入)(*10)
12. [玩家轉帳狀態查詢](#玩家轉帳狀態查詢)
13. [玩家下注查詢](#玩家下注查詢)
14. [玩家下注簡報查詢](#玩家下注簡報查詢)
15. [踢玩家](#踢玩家)
16. [踢多玩家](#踢多玩家)
17. [設定信用玩家額度](#設定信用玩家額度)(*11)
18. [查詢信用玩家額度](#查詢信用玩家額度)(*12)
19. [重設信用玩家額度](#重設信用玩家額度)(*13)
20. [重設指定信用玩家額度](#重設指定信用玩家額度)(*14)
21. [設定信用額度重設群組](#設定信用額度重設群組)(*15)
22. [查詢信用額度重設群組](#查詢信用額度重設群組)(*16)
23. [查詢注區範本](#查詢注區範本)
24. [玩家注區範本設定查詢](#玩家注區範本設定查詢)(*17)
25. [玩家注區範本設定](#玩家注區範本設定)(*18)
26. [手機API串接](#手機API串接)
27. [手機API串接-CN](#手機API串接-CN)
28. [修改玩家佔成](#修改玩家佔成)
29. [查詢玩家佔成](#查詢玩家佔成)
30. [新增遊戲公告](#新增遊戲公告)
31. [查詢遊戲公告](#查詢遊戲公告)
32. [修改遊戲公告](#修改遊戲公告)
33. [刪除遊戲公告](#刪除遊戲公告)
34. [修改玩家退水](#修改玩家退水)(*19)
35. [查詢玩家退水](#查詢玩家退水)(*20)
36. [查詢玩家下注區間總額](#查詢玩家下注區間總額)
37. [新增維護](#新增維護)
38. [查詢維護](#查詢維護)
39. [修改維護](#修改維護)
40. [刪除維護](#刪除維護)

## API For Web 數量：**8**
-------
1. [下注記錄網頁](#下注紀錄網頁)
2. [規則網頁](#規則網頁)
3. [jackpot網頁](#jackpot網頁)
4. [版本檢查](#版本檢查)
5. [取得server狀態](#取得server狀態)
6. [登入 (取得server需要的登入token)](#登入(取得server需要的登入token))
7. [app直接登入 (dToken交換)](#app直接登入)
8. [體驗帳號(url和Token)](#體驗帳號及url)
9. [單桌未結算注單](#單桌未結算注單)
10. [單桌注單](#單桌注單)
11. [即時注單頁面修改玩家佔成](#即時注單頁面修改玩家佔成)
12. [取得所有玩家](#取得所有玩家)
13. [Monitor使用者登入取得Token](#Monitor使用者登入取得Token)


## *登入流程*
-------
1. 玩家透過平台登入
2. 平台呼叫[註冊帳號](#註冊帳號)註冊帳號
3. 平台呼叫[玩家額度轉出入](#玩家額度轉出入)(現金玩家)、[設定信用玩家額度](#設定信用玩家額度)(信用玩家)，來設定額度
4. 平台呼叫[玩家注區範本設定](#玩家注區範本設定)來設定玩家注區範本
5. 平台呼叫[取得玩家登入網址](#取得玩家登入網址)取得遊戲登入網址
6. 使用[取得玩家登入網址](#取得玩家登入網址)回傳內容 `LoginUrl` 登入遊戲

### [流程圖連結](https://cacoo.com/diagrams/xgypN4flYVaJlTcU#28811)

![流程圖](https://cacoo.com/diagrams/xgypN4flYVaJlTcU-28811.png "API 登入 流程圖")

---

### <span id="GameType">遊戲類別</span>
| 遊戲名稱  | gameType                 | 
|---------- |-------------------------  |
| 百家樂         | 1   | 
| Jackpot Bonus         | 98   |
| Jackpot BigWin         | 99   |

### <span id="mode">玩家帳號模式</span>
| 代碼  | mode 說明                | 
|---------- |-------------------------  |
| 0         | 正常   | 
| 1        | 鎖單(不能下注)   | 
| 2        | 停用(不能進入遊戲)，修改狀態為 2時會執行踢除玩家   | 

### <span id="enable">玩家啟用狀態</span>
| 代碼  | enable 說明                 | 
|---------- |-------------------------  |
| 0         | 停用   | 
| 1        | 啟用   | 

### <span id="currency">支援幣別</span>
| 代碼  | 幣別                  | 
|---------- |-------------------------  |
| TWD        | 台幣   | 
| CNY       | 人民幣   | 

### <span id="language">支援語系</span>
| 代碼  | 說明                  | 
|---------- |-------------------------  |
| zh_TW     | 繁體中文   | 
| zh_CN     | 簡體中文   | 

### <span id="platformType">支援裝置</span>

| 代碼  | 說明                  |
|---------- |-------------------------  |
| Web     | 網頁電腦版  |
| Android    | App   |
| IOS     | App   |

### <span id="TableType">注區範本遊戲類別</span>

| 遊戲名稱  | gameType                 |
|---------- |-------------------------  |
| 百家樂         | 1   |

### <span id="TableType">退水遊戲類別</span>

| 遊戲名稱  | gameType                 |
|---------- |-------------------------  |
| 百家樂         | 1   |

### <span id="resetGroup">重設群組類型</span>
| 類型  | groupId                 |
|---------- |-------------------------  |
| 不回復         | 0   |
| 日回復         | 1   |
| 週回復         | 2   |

### <span id="marqueeUseLocation">公告播放地點代碼</span>
| 地點  | location                 |
|---------- |-------------------------  |
| Lobby        | 99   |
| 百家樂 A 桌        | 1   |
| 百家樂 B 桌         | 2   |
| 百家樂 C 桌        | 3   |
| 百家樂 D 桌         | 4   |
| 百家樂 E 桌        | 5  |
| 百家樂 F 桌    | 6 |
| 百家樂 G 桌    | 7 |
| 百家樂 H 桌    | 8 |
| 百家樂 X 桌 | 24 |
| 百家樂 Y 桌 | 25 |
| 百家樂 Z 桌 | 26 |

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
| 9  | player is online   | 玩家在線中，無法轉出入額度       |
| 10 | service not available   | 無法使用遊戲服務       |
| 11 | {parameter} is invalid   | 參數不合法       |
| 12 | gameType:{gameType} not found  | 無此遊戲存在       |
| 13 | account length between 4 - 20  |  帳號至少4位元以上      |
| 14 | nickname length between 1- 20  | 暱稱至少1位元以上       |
| 15 | enable setting is invalid  | 玩家啟用設定值不合法       |
| 16 | mode setting is invalid  | 玩家帳號模式設定值不合法       |
| 17 | transfer in error  | 額度轉入失敗       |
| 18 | transfer out error  | 額度轉出失敗       |
| 19 | player credit is not enough  | 玩家籌碼不足       |
| 20 | account:{account} has been used| 此帳號已被使用 |
| 21 | player have not yet checkout bet | 玩家有下注尚未結算注單，無法轉出入額度       |
| 22 | {param} must be a unsigned integer  | {param}設定值必須為正整數       |
| 23 | [startAt or  EndAt] value must be datetime Example：2017-01-01 00:00:00   | 查詢玩家注單，時間參數必須使用規定格式       |
| 24 |{transferId} is not exist   | 此筆交易單不存在       |
| 25 | transfer id can not be empty  | 交易的ID不可為空       |
| 26 | transferid:（平台方TransferID）has been used  | 此交易單已被使用       |
| 27 | transfer in or out can not be 0  | 額度轉出入設定值不可為0       |
|28|transfer id:{transfer id} transaction processing| 交易單處理中   |
|29|{params} must be entirely alpha-numeric characters| {param} 只允許英數   |
|30|{params} must be a unsigned decimal| {param} 只允許正數的 decimal   |
|31|{params} must be a integer| {param} 只允許整數   |
|32|player mode is been disabled   | 此帳號模式已被停用 |
|33|player enable is been disabled   | 此帳號已設定為不啟用 |
|34|tableType:{tableType} not found|您所設定的[注區範本遊戲類別](#注區範本遊戲類別)不支援|
|35|player stake limit setting max five count| 注區範本最大為5筆 |
|36|player stake limit setting value [{invalid stakeLimitValue}] is invalid| 注區範本設定值有不合法內容 |
|37|platform type {platformType} is invalid | 支援裝置不存在 |
|38|The cashtype is invalid | 此 api 操作不合法 |
|39|The credit reset action is pending | 回復設定尚在進行中 |
|40|{param} must be a unsigned integer, and only numeric characters | 只允許正數的 integer，且不允許正負記號 |
|41|{param} must between 1 and 0| 設定的數值只允許 1 到 0，且小位數最多 2 位數 |
|42|{param} must be a unsigned decimal, and only numeric characters | 只允許正數的 decimal，且不允許正負記號 |
| 43  | reset group id not found player | 此回復群組沒有任何玩家     |
| 44  | player credit reset is pending|重設信用玩家額度功能尚在進行中  |
| 45 | setting limitId:{limitId} level is {level}, platform limit level is {level}| 設定的注區範本等級超過允許設定的等級  |
|46|multi credit player reset do not allow call zero|重設指定信用玩家額度不允許執行不回復操作|
|47|marqueeId:{marqueeId} is not found|查無此遊戲公告|
|48| location:{location} is invalid |播放地點代碼不存在|
|49|marquee date range startAt:{startAt} ~ endAt:{endAt} is invalid|公告播放時間不合法|
| 50 | refund set value 0 ~ 150, setting refund is:{refund}|退水設定值 0 ~ 150|
| 51 | recordId:{recordId} is not exist|維護紀錄Id不存在|
| 52 | maintain time range is invalid startAt:{startAt} ~ endAt:{endAt} is invalid, At least half an hour|維護時間區間有問題，不允許過往時間且維護時間需至少半小時|
| 53 | get maintain record is need recordId or startAt and endAt|查詢維護紀錄，recordId 或 時間區間需擇一輸入|
| 102|The currency:{currency} is not supported|您所設定的貨幣類型不支援|
| 103 | The language is not supported| 您所設定的語系類型不支援 |

## *API 使用*
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
    |  currency   | 玩家貨幣類型 |  設定玩家貨幣類型，設定之後不可更改  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + nickname + currency + secret)`**
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  nickname   | 玩家暱稱 |  string  |
    |  currency   | 玩家設定幣別 |  string  |
    |  playerId   | 遊戲方玩家編號 |  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "nickname":"a99asdf",
            "currency":"TWD",
            "playerId":63
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
    | 7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 13 | account length between 4 - 20  |
    | 14 | nickname length between 1- 20  |
   | 20 | account:{account} has been used  |
   | 102 | The currency:{currency} is not supported |
   
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
    |  gameType | [遊戲類別](#遊戲類別)  |  smallint  | 選填，預設為：1(百家樂)    |
    |  language | 語系代碼 |  string  | 選填[支援語系](#支援語系)，預設為：zh_TW    |
    |  platformType | 裝置代碼|string| 選填[支援裝置](#支援裝置)，預設為：Web    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  loginUrl | 遊戲登入網址|  string  | 
    | token | 登入用 token | string |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "loginUrl":"http://test.app/casino-api/player/login?token=......M1NyJ9",
            "token":"......M1NyJ9"}
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
    | 12 | gameType:{gameType}  not found|
    |22 |{param} must be a unsigned integer |
    |32|player mode is been disabled   | 
    |33|player enable is been disabled   |
    |37|platform type {platformType} is invalid |
    | 103 | The language is not supported |
   
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
    |  credit | 額度|  decimal(19,4)  | 
    |  limitWin | 限贏|  decimal(19,4)  | 
    |  limitLose | 限輸|  decimal(19,4)  | 

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
            "currency":"TWD",
            "enable":1,
            "mode":0,
            "isOnline":0,
            "credit":4101.0000,
            "limitWin":1099.0000,
            "limitLose":1.0000
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
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 14 | nickname length between 1- 20  |
    
5. ## <span id="player-mode">設定玩家帳號模式</span>

    ```
    PUT /casino-api/player/mode?
        key=<key>&
        accounts=<accounts>&
        mode=<mode>&
        hash=<hash>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts | 玩家帳號 |  string  |     必填    |
    |  mode   | 帳號模式 |  smallint  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    
    #### <span id="mode">玩家帳號模式</span>
    | 代碼  | mode                  | 
    |---------- |-------------------------  |
    | 0         | 正常   | 
    | 1        | 鎖單(不能下注)   | 
    | 2        | 停用(不能進入遊戲)，修改狀態為 2時會執行踢除玩家   | 
    
    #### **`hash = md5(accounts + mode + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  mode   | 帳號模式 |  smallint  |
    |  result | 多玩家處理狀況 |  string  |

    #### result 多玩家處理狀況
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  |
    | status| [玩家設定結果](#玩家設定結果)|smallint  |

    #### <span id="set-mode-status">玩家設定結果</span>
    | 狀態代碼 | status                  |
    |---------- |-------------------------  |
    | 0 | success |
    | -1 | player not found |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "mode":1,
            "result":[
                {"account":"test1","status":-1},
                {"account":"test666","status":0},
                {"account":"test2","status":-1}
            ]
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
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 16 | mode setting is invalid  |
    | 22 | {param} must be a unsigned integer  |
    |40|{param} must be a unsigned integer, and only numeric characters |

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
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 15 | enable setting is invalid  |
    | 22 | {param} must be a unsigned integer  |
    |40|{param} must be a unsigned integer, and only numeric characters |
    
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
    |  limit   | 限輸值 |  decimal(19,4) |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + limit + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  limit   | 限輸值 |  decimal(19,4)  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "limitLose":10.1239
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":30,
            "message":"limit must be a unsigned decimal"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    |30|{params} must be a unsigned decimal|
    |42|{param} must be a unsigned decimal, and only numeric characters |
    
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
    |  limit   | 限贏值 |  decimal(19,4) |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(account + limit + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  limit   | 限贏值 |  decimal(19,4)  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "limitWin":10.1239
        }
    }
    ```

    失敗

   ```javascript
    {
        "status":"error",
        "error":{
            "code":30,
            "message":"limit must be a unsigned decimal"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    |30|{params} must be a unsigned decimal|
    |42|{param} must be a unsigned decimal, and only numeric characters |

3. ## <span id="player-info">玩家限注查詢</span>

    ```
    GET /casino-api/player/limit?
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
    |  winCredit | 目前損益|  decimal(19,4)  | 
    |  limitWin | 限贏|  decimal(19,4)  | 
    |  limitLose | 限輸|  decimal(19,4)  | 

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "winCredit":20113.6,
            "limitWin":25000.0000,
            "limitLose":0.0000
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
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    

10. ### <spin id="player-transfer">玩家額度轉出入</spin>
   
    ```
    PUT /casino-api/player/credit/transfer?
        key=<key>&
        account=<account>&
        credit=<credit>&
        transferId=<transferId>&
        hash=<hash>
    ```
    
    #### *限現金平台使用
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  credit | 轉出入額度 |integer  |必填，正值為轉入，負值為轉出。(目前不開放小數籌碼，請仍以整數串送)    |
    |transferId |平台方交易Id |string| 必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    
    #### **`hash = md5(account + credit + transferId + secret)`**
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  originCredit   | 交易前額度 |  decimal(19,4)  |
    |  addCredit   | 增加額度 |  integer  |
    |  finalCredit   | 交易後額度 |  decimal(19,4)  |
    |  transferId   | 平台方交易Id(只允許英數) |  string  |
    |  orderId   | 此筆交易單號 |  integer  |
    |  status   | 交易狀態 |  smallint  |

    #### <span id="enable">交易狀態</span>
    | 代碼  | status                  | 
    |---------- |-------------------------  |
    | 0         | 成功   | 
    
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
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    |  10  |service not available |
    | 11 | {parameter} is invalid   |
    | 19 | player credit is not enough  |
    | 25 | transfer id can not be empty  |
    | 26 | transferid:（平台方TransferID）has been used  |
    | 27 | transfer in or out can not be 0  | 
    |29|{params} must be entirely alpha-numeric characters| {param} 只允許英數   |
    |31|{params} must be a integer|
    |38|The cashtype is invalid | 此 api 操作不合法 |
    
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
    |  originCredit   | 交易前額度 |  decimal(19,4)  |
    |  addCredit   | 增加額度 |  integer  |
    |  finalCredit   | 交易後額度 |  decimal(19,4)  |
    |  transferId   | 平台方交易Id |  string  |
    |  orderId   | 此筆交易單號 |  integer  |
    |  status   | 交易狀態 |  smallint  |

    #### <span id="status">交易狀態</span>
    | 代碼  | status                  | 
    |---------- |-------------------------  |
    | 0         | 成功   | 
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
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    |  10  |service not available |
    | 11 | {parameter} is invalid   |
    | 24 |{transferId} is not exist   |
    |29|{params} must be entirely alpha-numeric characters| 


12. ### <span id="bet-report">玩家下注查詢</span>

    ```
    GET /casino-api/bet/report?
        key=<key>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```
    
    ##### 目前使用分頁式，一頁1000筆

    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  startAt | 起始時間 |  string|必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |必填，格式 2017-01-01 13:00:10    |
    |   page   | 分頁 |  int  |     選填，預設：1   |
    | pageSize| 分頁筆數 |  int  | 選填，預設：1000，若有輸入錯誤，會自動填入1000   |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(startAt + endAt + secret)`**
    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  total | 總筆數 |  integer  | 
    |  perPage| 各分頁含有筆數 |integer  |
    |  currentPage| 目前所在頁數 |integer  |
    |  lastPage|下一頁頁數 |integer  |
    |  previousPageUrl|上一頁 (null 表示為第一頁)  | string or null |
    |  nextPageUrl|下一頁 (null 表示無下一頁) | string or null  |
    |  betResult| [下注明細](#betResult 說明) | array |
    
    #### <span id="betResult">betResult 說明</span>
    
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  betFormId| 注單編號 | integer |
    |  gameType| [遊戲類別](#遊戲類別) ，`98`為額外彩金，`99`為JP彩金| smallint  |
    |gameMethod|[遊戲玩法](#遊戲玩法)，`gameType` 為 `98` or `99`，此欄位 0|smallint |
    |tableType|[桌台類型](#桌台類型)，`gameType` 為 `98` or `99`，此欄位 0| smallint |
    | tableId | 遊戲局桌檯Id，`gameType` 為 `98` or `99`，此欄位為空白 | string |
    | round | 局號，`gameType` 為 `98` or `99`，此欄位 0 | integer |
    | run | 輪號，`gameType` 為 `98` or `99`，此欄位 0| smallint |
    | gameResult |[開牌結果](#百家樂遊戲開牌代碼說明)，順序：[閒1,莊1,閒2,莊2,閒補,莊補]，若空的表示 無補牌，若 `gameType` 為 `98` or `99` 分別顯示對應的 Jackpot 獎項名稱 | string |
    | playerId | 遊戲方玩家Id | integer |
    | account | 玩家帳號 | string |
    | amount | 總下注額度，`gameType` 為 `98` or `99`，此欄位 0 | integer |
    | validAmount | 總有效下注額度，`gameType` 為 `98` or `99`，此欄位 0 | integer |
    | betList | 下注注區列表，`gameType` 為 `98` or `99`，此欄位為空白 | string(json) |
    | refund| 退水設定值 0 ~ 150 | integer|
    | refundFee| 退水費用 | decimal(10,2)|
    | checkoutAmount|結帳金額，`gameType` 為 `98` or `99`相對應 Jackpot獎金 |decimal(19,4)|
    | jackpotBonus|`gameType` 為 `98` or `99` 相對應 Jackpot獎金|decimal(19,4)|
    | percent | 佔成 | decimal(19,2) |
    |betTime|當局最後一筆下注時間，`gameType` 為 `98` or `99`，此欄位為中獎時間|string |
    | billingDate | 結帳日 | string |
    | modifiedStatus | [注單狀態](#注單狀態) | smallint |
    | updatedAt | 更新時間，`gameType` 為 `98` or `99`，此欄位為中獎時間 | string |
    | createdAt | 結帳時間，`gameType` 為 `98` or `99`，此欄位為中獎時間 | string |
    
    ---
    
    ### <span id="gameMethods">遊戲玩法</span>
    | 名稱  | gameMethods                  | 
    |---------- |-------------------------  |
    | 百家樂標準         | 1   | 
    | 百家樂西洋         | 2   |
    | 百家樂免水         | 3   |
    
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
        "status":"success",
        "data":{
            "total":1,
            "perPage":1000,
            "currentPage":1,
            "lastPage":1,
            "previousPageUrl":null,
            "nextPageUrl":null,
            "betResult":[{
                "betFormId":299,
                "gameType":1,
                "gameMethod":1,
                "tableType":3,
                "tableId":"A",
                "round":1658620520,
                "run":4,
                "gameResult":"C1,CJ,DJ,H1,C7,C7",
                "playerId":26,
                "account":"EddyCount",
                "amount":1000,
                "validAmount":1000,
                "betList":[{"spotId":18,"spotName":"BankerJQKA","betAmount":1000,"loseWinAmount":14000,"odds":14}],
                "refund": 35,
                "refundFee": 49,
                "checkoutAmount":"14000.0000",
                "jackpotBonus":0,
                "percent":0.87,
                "betTime":"2017-08-25 16:30:09",
                "billingDate":"2017-08-26",
                "modifiedStatus":0,
                "updatedAt":"2017-08-25 16:31:24",
                "createdAt":"2017-08-25 16:31:24"}]
        },
        {
	        "betFormId": 1578,
	        "gameType": 98,
	        "gameMethod": 0,
	        "tableType": 0,
	        "tableId": "",
	        "round": 0,
	        "run": 0,
	        "gameResult": "JackpotBonus",
	        "playerId": 4,
	        "account": "rick",
	        "amount": 0,
	        "validAmount": 0,
	        "betList": "",
	        "refund": 0,
	        "refundFee": 0,
	        "checkoutAmount": 0,
	        "jackpotBonus": 1200,
	        "percent": 0.88,
	        "betTime": "2017-12-11 16:41:00",
	        "billingDate": "2017-12-12",
	        "modifiedStatus": 0,
	        "updatedAt": "2017-12-11 16:41:00",
	        "createdAt": "2017-12-11 16:41:00"
	      },
	      {
	        "betFormId": 1584,
	        "gameType": 99,
	        "gameMethod": 0,
	        "tableType": 0,
	        "tableId": "",
	        "round": 0,
	        "run": 0,
	        "gameResult": "JackpotBigWin",
	        "playerId": 4,
	        "account": "rick",
	        "amount": 0,
	        "validAmount": 0,
	        "betList": "",
	        "refund": 0,
	        "refundFee": 0,
	        "checkoutAmount": 0,
	        "jackpotBonus": 771520,
	        "percent": 0.88,
	        "betTime": "2017-12-11 16:44:58",
	        "billingDate": "2017-12-12",
	        "modifiedStatus": 0,
	        "updatedAt": "2017-12-11 16:44:58",
	        "createdAt": "2017-12-11 16:44:58"
	      }
    }  
              
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":23,
            "message":"[startAt or endAt] value must be datetime Example：2017-01-01 00:00:00"
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
    | 23 | [startAt or  EndAt] value must be datetime Example：2017-01-01 00:00:00   | 查詢玩家注單，時間參數必須使用規定格式       |

13. #### <span id="bet-report-multiple">玩家下注簡報查詢</span>

    ```
    GET /casino-api/bet/report-multiple?
        key=<key>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(startAt + endAt + secret)`**
    
    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  | 
    |  betFormCount | 注單數量 |integer  |
    |  totalAmount | 總下注額度 |integer  |
    |  totalLoseWinAmount | 總輸贏 | decimal(19,4)  |
        
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
                "totalLoseWinAmount":1000.88,
            },
            {
                "account":"a129995b",
                "betFormCount":67,
                "totalAmount":563900,
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
            "code":23,
            "message":"[startAt or endAt] value must be datetime Example：2017-01-01 00:00:00"
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
    | 23 | [startAt or  EndAt] value must be datetime Example：2017-01-01 00:00:00   | 查詢玩家注單，時間參數必須使用規定格式       |

12. #### <span id="player-kick">踢玩家</span>

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
    |  10  |service not available |
    | 11 | {parameter} is invalid   |
   
15. #### <span id="player-kick-multiple">踢多玩家</span>

    ```
    DELETE /casino-api/player/kick-multiple?
        key=<key>&
        accounts=<accounts>&
        reason=<reason>&
        hash=<hash>
    ```
    
    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts | 遊戲帳號 |  string  |     必填，多玩家使用 `,` 號隔開   |
    |  reason   | 原因 |  string  |     非必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(accounts + secret)`**
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
    |  10  |service not available |
    | 11 | {parameter} is invalid   |

3. ## <span id="player-credit-set">設定信用玩家額度</span>

    ```
    PUT /casino-api/player/credit?
        key=<key>&
        accounts=<accounts>&
        credit=<credit>&
        hash=<hash>
    ```

    #### *限信用平台使用

    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts | 玩家帳號 |  string  |     必填    |
    |  credit | 額度 |  decimal(19,4)  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(accounts + credit + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  credit | 額度|  decimal(19,4)  |
    |  account | 玩家帳號|  string  |
 
    #### 玩家設定結果
    | 狀態代碼 | status| 
    |:--------:|:--------:|
    |  0 | Success | 
    |  -1 | Player Not Found |
    |  -2 | Not Enough Credit |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "credit":"65000",
        "data":{
            {"account":"a1234","status":0}
            {"account":"test1","status":-1}
            {"account":"a1234","status":0}
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":10,
            "message":"service not available"
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
    |30|{params} must be a unsigned decimal| {param} 只允許正數的 decimal   |
    |38|The cashtype is invalid | 此 api 操作不合法 |
    |42|{param} must be a unsigned decimal, and only numeric characters |
    | 44  | player credit reset is pending|

3. ## <span id="player-credit-info">查詢信用玩家額度</span>

    ```
    GET /casino-api/player/credit?
        key=<key>&
        account=<account>&
        hash=<hash>
    ```
    
    #### *限信用平台使用
    
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
    |  credit | 額度|  decimal(19,4)  |
    |  currentCredit | 額度|  decimal(19,4)  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "credit":5000.0000,
            "currentCredit":4101.0000
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
    |38|The cashtype is invalid | 此 api 操作不合法 |

3. ## <span id="player-credit-reset">重設信用玩家額度</span>

    ```
    PUT /casino-api/player/credit/reset?
        key=<key>&
        callback=<callback>&
        hash=<hash>
    ```

    #### *限信用平台使用
    
    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  callback | 執行完後通知平台 url |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(callback + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  recordId | 重設紀錄 id 流水號|  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "recordId":12
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":3,
            "message":"key is invalid"
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
    |38|The cashtype is invalid | 此 api 操作不合法 |
    |39|The credit reset action is pending |
    

3. ## <span id="player-credit-reset">重設指定信用玩家額度</span>

    ```
    PUT /casino-api/player/credit/multi-reset?
        key=<key>&
        groupId=<groupId>&
        callback=<callback>&
        hash=<hash>
    ```

    #### *限信用平台使用

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  groupId | [重設群組類型](#重設群組類型)，注意只允許日及週回復 |integer |必填|
    |  callback | 執行完後通知平台 url |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(groupId + callback + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  recordId | 重設紀錄 id 流水號|  integer  |
    |  groupId | [重設群組類型](#重設群組類型)|  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "recordId":12,
            "groupId":1
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":3,
            "message":"key is invalid"
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
    |38|The cashtype is invalid | 此 api 操作不合法 |
    |39|The credit reset action is pending |
    | 43  | reset group id not found player |
    |46|multi credit player reset do not allow call zero|

3. ## <span id="credit-reset-group">設定信用額度重設群組</span>

    ```
    PUT /casino-api/credit-reset-group?
        key=<key>&
        account=<account>&
        groupId=<groupId>&
        hash=<hash>
    ```

    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  groupId | [重設群組類型](#重設群組類型) |  integer  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(account + groupId + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號|  string  |
    |  groupId |重設群組類型 |  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "groupId":2
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

3. ## <span id="query-credit-reset-group">查詢信用額度重設群組</span>

    ```
    GET /casino-api/credit-reset-group?
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
    |  groupId |[重設群組類型](#重設群組類型) |  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "groupId":2
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

3. ## <span id="stake-limit-query">查詢注區範本</span>

    ```
    GET /casino-api/stake-limit-list?
        key=<key>&
        currency=<currency>&
        tableType=<tableType>&
        hash=<hash>
    ```

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  currency | [支援幣別](#支援幣別) |  string  |     必填    |
    |  tableType | [注區範本遊戲類別](#注區範本遊戲類別) |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(currency + tableType + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  limitId | 範本Id |  integer  |
    |  cashLevel | 現金對應範本等級 A~E|  integer  |
    |  creditLevel | 信用對應範本等級|  integer  |
    |  min | 最小下注值|  integer  |
    |  max | 最大下注值|  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
		"status":"success",
		"data":[
			{"limitId":1,"cashLevel":"A","creditLevel":1,"min":50,"max":1000},
			{"limitId":2,"cashLevel":"A","creditLevel":2,"min":50,"max":2000},
			{"limitId":3,"cashLevel":"B","creditLevel":3,"min":100,"max":5000},
			{"limitId":4,"cashLevel":"B","creditLevel":4,"min":200,"max":10000},
			{"limitId":5,"cashLevel":"C","creditLevel":5,"min":200,"max":20000},
			{"limitId":6,"cashLevel":"C","creditLevel":6,"min":200,"max":30000},
			{"limitId":7,"cashLevel":"D","creditLevel":7,"min":500,"max":50000},
			{"limitId":8,"cashLevel":"D","creditLevel":8,"min":500,"max":100000},
			{"limitId":9,"cashLevel":"E","creditLevel":9,"min":500,"max":200000},
			{"limitId":10,"cashLevel":"E","creditLevel":10,"min":1000,"max":400000}
		]
	 }
   ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":3,
            "message":"key is invalid"
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
    | 34 | tableType:{tableType} not found|
    | 102 | The currency:{currency} is not supported|

3. ## <span id="player-stake-limit-setting-query">玩家注區範本設定查詢</span>

    ```
    GET /casino-api/player/stake-limit?
        key=<key>&
        account=<account>&
        tableType=<tableType>&
        hash=<hash>
    ```

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  tableType | [注區範本遊戲類別](#注區範本遊戲類別) |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(account + tableType + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號|  string  |
    |  tableType | [注區範本遊戲類別](#注區範本遊戲類別)  |  string  |
    |  limitValue | 玩家所設定範本內容 |  array  |

    #### <span id="limitValue">limitValue 說明</span>
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  limitId| 範本對應 Id | integer |
    |  enable| 是否有設定此範本，0=>不使用，1=>使用  | boolean|
    |  cashLevel | 現金對應範本等級 A~E|  integer  |
    |  creditLevel | 信用對應範本等級|  integer  |
    |  min | 最小下注值|  integer  |
    |  max | 最大下注值|  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
	{
		"status": "success",
		"data": {
			"account": "supreme",
			"tableType": 1,
			"limitValue": [
			      {
			        "limitId": 1,
			        "enable": 1,
			        "cashLevel": "A",
			        "creditLevel": 1,
			        "min": 50,
			        "max": 1000,
			      },
			      {
			        "limitId": 2,
			        "enable": 1,
			        "cashLevel": "A",
			        "creditLevel": 2,
			        "min": 50,
			        "max": 2000,
			      },
			      {
			        "limitId": 3,
			        "enable": 1,
			        "cashLevel": "B",
			        "creditLevel": 3,
			        "min": 100,
			        "max": 5000
			      },
			      {
			        "limitId": 4,
			        "enable": 1,
			        "cashLevel": "B",
			        "creditLevel": 4,
			        "min": 200,
			        "max": 10000
			      },
			      {
			        "limitId": 5,
			        "enable": 0,
			        "min": 200,
			        "max": 20000,
			        "cashLevel": "C",
			        "creditLevel": 5
			      },
			      {
			        "limitId": 6,
			        "enable": 0,
			        "cashLevel": "C",
			        "creditLevel": 6,
			        "min": 200,
			        "max": 30000
			      },
			      {
			        "limitId": 7,
			        "enable": 1,
			        "cashLevel": "D",
			        "creditLevel": 7
			        "min": 500,
			        "max": 50000
			      },
			      {
			        "limitId": 8,
			        "enable": 1,
			        "cashLevel": "D",
			        "creditLevel": 8,
			        "min": 500,
			        "max": 100000
			      },
			      {
			        "limitId": 9,
			        "enable": 1,
			        "cashLevel": "E",
			        "creditLevel": 9,
			        "min": 500,
			        "max": 200000
			      },
			      {
			        "limitId": 10,
			        "enable": 1,
			        "cashLevel": "E",
			        "creditLevel": 10,
			        "min": 1000,
			        "max": 400000
			      }
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

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found          |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 34 | tableType:{tableType} not found|

3. ## <span id="player-stake-limit-setting-query">玩家注區範本設定</span>

    ```
    PUT /casino-api/player/stake-limit?
        key=<key>&
        account=<account>&
        tableType=<tableType>&
        stakeLimitValue=<stakeLimitValue>&
        hash=<hash>
    ```

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  tableType | [注區範本遊戲類別](#注區範本遊戲類別) |  string  |     必填    |
    |  stakeLimitValue | 設定玩家幣別能使用的範本Id，多筆請用`,` 號 | string | 必填  |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(account + tableType + stakeLimitValue + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號|  string  |
    |  tableType | [注區範本遊戲類別](#注區範本遊戲類別)  |  string  |
    |  stakeLimitValue | 玩家所設定範本 |  string  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"test001",
            "tableType":4,
            "stakeLimitValue":"110,112,113,115"
        }
    }
    ```

    失敗

   ```javascript
    {
        "status":"error",
        "error":{
            "code":36,
            "message":"player stake limit setting value [133] is invalid"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found          |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 34 | tableType:{tableType} not found|
    | 36 | player stake limit setting value [{invalid stakeLimitValue}] is invalid|
    |40|{param} must be a unsigned integer, and only numeric characters | 

3. ## <span id="app-api">手機API串接</span>

    ###### scheme url - tw
    
    ```
    pharao-casino-mobile://launch-game?
        token=eyJpdiI6Im9kYTk1WVFlaVRyZm5..&
        lang=zh_TW&
        platformURL=pharao-platform-mobile://launch-game?...
    ```

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |   必填    |
    |:--------:|:--------:|:--------:|:-----------:|:-----------:|
    |token| 從[取得玩家登入網址](#取得玩家登入網址)取得的token |string| 用來驗證玩家 |Y|
    |  lang | [支援語系](#支援語系) |  string  |     遊戲內使用語系    |Y|
    |platformURL| 返回平台 schemal url |  string  | 遊戲app返回平台APP使用  |Y|

3. ## <span id="app-api-cn">手機API串接-CN</span>

    ###### scheme url - cn

    ```
    pharao-casino-mobile-cn://launch-game?
        token=eyJpdiI6Im9kYTk1WVFlaVRyZm5..&
        lang=zh_TW&
        platformURL=pharao-platform-mobile-cn://launch-game?...
    ```

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |   必填    |
    |:--------:|:--------:|:--------:|:-----------:|:-----------:|
    |token| 從[取得玩家登入網址](#取得玩家登入網址)取得的token |string| 用來驗證玩家 |Y|
    |  lang | [支援語系](#支援語系) |  string  |     遊戲內使用語系    |Y|
    |platformURL| 返回平台 schemal url |  string  | 遊戲app返回平台APP使用  |Y|

7. ## <span id="playper-percent">修改玩家佔成</span>

    ```
    PUT /casino-api/player/profit-percent?
        key=<key>&
        account=<account>&
        percent=<percent>&
        hash=<hash>
    ```

    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  percent   | 佔成比例 |  decimal(19,2) | 必填，只能0~1之間，且只能小數點第 2 位|
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(account + percent + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號 |  string  |
    |  percent   | 佔成比例 |  decimal(19,2)  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "percent":0.85
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":30,
            "message":"percent must be a unsigned decimal"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    |30|{params} must be a unsigned decimal|
    |41|{param} must between 1 and 0|
    |42|{param} must be a unsigned decimal, and only numeric characters |

7. ## <span id="get-player-percent">查詢玩家佔成</span>

    ```
    GET /casino-api/player/profit-percent?
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
    |  percent   | 佔成比例 |  decimal(19,2)  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "percent":0.85
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
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |

7. ## <span id="add-marquee">新增遊戲公告</span>

    ```
    POST /casino-api/marquee?
        key=<key>&
        message=<message>&
        location=<location>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  message | 公告訊息|  string  |     必填    |
    | location| [公告播放地點代碼](#公告播放地點代碼) | string | 必填，多筆請用`,` 號|
    |  startAt   | 播放起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 播放結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(message + location + startAt + endAt + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  marqueeId | 遊戲公告Id |  integer  |
    |  message   | 公告訊息|  string  |
    |  location   | [公告播放地點代碼](#公告播放地點代碼)|  string  |
    |  startAt   | 播放起始時間|  string  |
    |  endAt   | 播放結束時間|  string  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "marqueeId":88,
            "message":"test A,B,C,D",
            "location":"1,2,3,4",
            "startAt":"2017-12-26 16:00:00",
            "endAt":"2017-12-28 00:00:00"
        }
    }
    ```

    失敗

   ```javascript
   {
       "status":"error",
       "error":{
           "code":48,
           "message":"location:100 is invalid"
       }
   }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |    
    |48|location:{location} is invalid|
    |49|marquee date range startAt:{startAt} ~ endAt:{endAt} is invalid|

7. ## <span id="add-marquee">修改遊戲公告</span>

    ```
    PUT /casino-api/marquee?
        key=<key>&
        marqueeId=<marqueeId>&
        message=<message>&
        location=<location>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  marqueeId | 遊戲公告Id|  integer  |     必填    |
    |  message | 公告訊息|  string  |     必填    |
    | location| [公告播放地點代碼](#公告播放地點代碼) | string | 必填，多筆請用`,` 號|
    |  startAt   | 播放起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 播放結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(marqueeId + message + location + startAt + endAt + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  marqueeId | 遊戲公告Id |  integer  |
    |  message   | 公告訊息|  string  |
    |  location   | [公告播放地點代碼](#公告播放地點代碼)|  string  |
    |  startAt   | 播放起始時間|  string  |
    |  endAt   | 播放結束時間|  string  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
       "status":"success",
       "data":{
           "marqueeId":"85",
           "message":"test A,B,C,D",
           "location":"1,2,3,4",
           "startAt":"2017-12-26 16:00:00",
           "endAt":"2017-12-28 00:00:00"
       }
    }
    ```

    失敗

   ```javascript
   {
       "status":"error",
       "error":{
           "code":47,
           "message":"marqueeId:100 is not found"
       }
   }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |    
    |47|marqueeId:{marqueeId} is not found|
    |49|marquee date range startAt:{startAt} ~ endAt:{endAt} is invalid|

7. ## <span id="add-marquee">查詢遊戲公告</span>

    ```
    GET /casino-api/marquee?
        key=<key>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  startAt   | 播放起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 播放結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(startAt + endAt + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  marqueeId | 遊戲公告Id |  integer  |
    |  status   | 公告是否啟用|  boolean  |
    |  message   | 公告訊息|  string  |
    |  location   | [公告播放地點代碼](#公告播放地點代碼)|  string  |
    |  startAt   | 播放起始時間|  string  |
    |  endAt   | 播放結束時間|  string  |
    |  updateTime   | 公告修改時間|  string  |

    ---

    ### Response 結果
    成功

    ```javascript
	 {
        "status": "success",
        "data": [
            {
                 "marqueeId": 85,
                 "status": 1,
                 "message": "test A,B,C,D",
                 "useLocation": "1,2,3,4,5",
                 "startAt": "2017-12-26 16:00:00",
                 "endAt": "2017-12-28 00:00:00",
                 "updateTime": "2017-12-26 16:00:00"
             },
             {
                 "marqueeId": 86,
                 "status": 0,
                 "message": "hello moto 1122",
                 "useLocation": "1,3,99",
                 "startAt": "2017-12-26 16:00:00",
                 "endAt": "2017-12-27 00:00:00"
                 "updateTime": "2017-12-26 16:00:00"
             }
        ]
	}
   ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":47,
            "message":"marqueeId:100 is not found"
        }
   }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |    

7. ## <span id="add-marquee">刪除遊戲公告</span>

    ```
    DELETE /casino-api/marquee?
        key=<key>&
        marqueeId=<marqueeId>&
        hash=<hash>
    ```

    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  marqueeId | 遊戲公告Id|  integer  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(marqueeId + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  marqueeId | 遊戲公告Id |  integer  |

    ---

    ### Response 結果
    成功

   ```javascript
	 {
       "status":"success",
       "data":{
           "marqueeId":"85"
       }
   }   
	```

    失敗

   ```javascript
   {
       "status":"error",
       "error":{
           "code":47,
           "message":"marqueeId:100 is not found"
       }
   }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found         |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |    
    |47|marqueeId:{marqueeId} is not found|

3. ## <span id="set-refund">修改玩家退水</span>

    ```
    PUT /casino-api/player/refund?
        key=<key>&
        accounts=<accounts>&
        tableType=<tableType>&
        refund=<refund>&
        hash=<hash>
    ```

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts | 玩家帳號 |  string  |     必填    |
    |  tableType | [退水遊戲類別](#退水遊戲類別) |  smallint  |     必填    |
    |  refund | 退水設定值 0 ~ 150 |  integer  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(accounts + tableType + refund + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  tableType | [退水遊戲類別](#退水遊戲類別)  |  smallint  |
    |  refund | 退水設定值 |  integer  |
    #### result 多玩家處理狀況
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號|  string  |
    |  status | 玩家設定結果|  smallint  |
    #### 玩家設定結果
    | 狀態代碼 | status| 
    |:--------:|:--------:|
    |  0 | success | 
    |  -1 | player not found|
    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "tableType":1,
            "refund":120,
            "result":[
            	{"account":"test1","status":-1},
            	{"account":"test666","status":0},
            	{"account":"test2","status":-1}
            ]
        }
    }
    ```

    失敗

   ```javascript
    {
        "status":"error",
        "error":{
            "code":34,
            "message":"tableType:2 not found"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found          |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 22 | {param} must be a unsigned integer  |
    | 34 | tableType:{tableType} not found|
    |40 |{param} must be a unsigned integer, and only numeric characters |
    | 50 | refund set value 0 ~ 150, setting refund is:{refund}|

3. ## <span id="get-refund">查詢玩家退水</span>

    ```
    GET /casino-api/player/refund?
        key=<key>&
        account=<account>&
        tableType=<tableType>&
        hash=<hash>
    ```

    ### Request 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  tableType | [退水遊戲類別](#退水遊戲類別) |  smallint  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(account + tableType + secret)`**
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號|  string  |
    |  tableType | [退水遊戲類別](#退水遊戲類別)  |  smallint  |
    |  refund | 退水設定值 |  integer  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"test001",
            "tableType":1,
            "refund":120
        }
    }
    ```

    失敗

   ```javascript
    {
        "status":"error",
        "error":{
            "code":34,
            "message":"tableType:2 not found"
        }
    }
   ```

    #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |
    |:--------:|:--------:|
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 4  | player not found          |
    | 5  | {method} is not allowed   |
    |  7  | internal server error |
    | 11 | {parameter} is invalid   |
    | 22 | {param} must be a unsigned integer  |
    | 34 | tableType:{tableType} not found|

13. #### <span id="single-player-bet-report">查詢玩家下注區間總額</span>

    ```
    GET /casino-api/player/bet-report?
        key=<key>&
        account=<account>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    #### Describe API說明
	 
	 統計會包含已取消之牌局、查詢條件從"牌局修改時間"(BaccaratHistory.ModifiedTime)改為"注單成立時間"(BetForm.betTime 當局最後一筆下注時間)


    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |    account   | 玩家帳號 |  string  |  選填，若無 account 回傳所屬玩家區間總額總計 |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(startAt + endAt + secret)`**

    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    |  account | 玩家帳號（若無 account 此欄位不會出現) |  string  |
    |  totalBetCount | 注單數量 |integer  |
    |  totalBetAmount | 總下注額度 |integer  |
    |  totalBetCheckoutAmount | 總輸贏 | decimal(19,4)  |

    ---

    #### Response 結果
    成功

    **`查詢單一玩家結果`**

    ```javascript
    {
        "status":"success",
        "data":{
            "account":"a1234",
            "totalBetCount":14,
            "totalBetAmount":208000,
            "totalBetCheckoutAmount":-128250,
        }
    }
    ```

    **`查詢多位玩家結果`**

    ```javascript
    {
        "status":"success",
        "data":{
            "totalBetCount":14,
            "totalBetAmount":208000,
            "totalBetCheckoutAmount":-128250,
        }
    }
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":23,
            "message":"[startAt or endAt] value must be datetime Example：2017-01-01 00:00:00"
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
    | 23 | [startAt or  EndAt] value must be datetime Example：2017-01-01 00:00:00   | 查詢玩家注單，時間參數必須使用規定格式       |

13. #### <span id="create-maintain">新增維護</span>

    ```
    POST /casino-api/maintain?
        key=<key>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(startAt + endAt + secret)`**

    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    | recordId | 維護紀錄Id| int |
    |  startAt  | 起始時間| string |
    |  endAt  | 結束時間| string |
    ---

    #### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "recordId":65,
            "startAt":"2018-03-01 15:00:00",
            "endAt":"2018-03-01 16:00:00"
        }
    }
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":23,
            "message":"[startAt or endAt] value must be datetime Example：2017-01-01 00:00:00"
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
    | 23 | [startAt or EndAt] value must be datetime Example：2017-01-01 00:00:00   | 查詢玩家注單，時間參數必須使用規定格式       |

13. #### <span id="get-maintain">查詢維護</span>

    ```
    GET /casino-api/maintain?
        key=<key>&
        recordId=<recordId>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  recordId   | 維護紀錄Id |  int  | 選填，維護紀錄Id |
    |  startAt   | 起始時間 |  string  |     選填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     選填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(secret)`**

    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    | recordId | 維護紀錄Id| int |
    |  active  | 是否有啟動維護| boolean |
    |  startAt  | 起始時間| string |
    |  endAt  | 結束時間| string |
    |  modifyTime  | 修改時間 | string |
    |  createTime  | 建立時間 | string |
    ---

    #### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":[
            {
                "recordId":66,
                "active":0,
                "startAt":"2018-03-01 13:00:00",
                "endAt":"2018-03-01 13:30:00",
                "modifyTime":"2018-03-01 12:00:00",
                "createTime":"2018-03-01 12:00:00",
            },
            {
                "recordId":67,
                "active":1,
                "startAt":"2018-03-02 14:00:00",
                "endAt":"2018-03-02 14:30:00",
                "modifyTime":"2018-03-02 12:20:00",
                "createTime":"2018-03-02 12:00:00",
            },
        ]
    }
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":23,
            "message":"[startAt or endAt] value must be datetime Example：2017-01-01 00:00:00"
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
    | 23 | [startAt or EndAt] value must be datetime Example：2017-01-01 00:00:00| 
    | 51 | record id:{recordI} is not exist|  

13. #### <span id="modify-maintain">修改維護</span>

    ```
    PUT /casino-api/maintain?
        key=<key>&
        recordId=<recordId>&
        startAt=<startAt>&
        endAt=<endAt>&
        hash=<hash>
    ```

    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  recordId   | 維護紀錄Id |  int  | 必填，維護紀錄Id |
    |  startAt   | 起始時間 |  string  |     必填，格式 2017-01-01 12:00:10    |
    |  endAt   | 結束時間 |  string  |     必填，格式 2017-01-01 13:00:10    |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(recordId + startAt + endAt + secret)`**

    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    | recordId | 維護紀錄Id| int |
    |  startAt  | 起始時間| string |
    |  endAt  | 結束時間| string |
    ---

    #### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
           "recordId":66,
           "startAt":"2018-03-01 13:00:00",
           "endAt":"2018-03-01 13:30:00",
        }
    }
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":23,
            "message":"[startAt or endAt] value must be datetime Example：2017-01-01 00:00:00"
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
    | 23 | [startAt or EndAt] value must be datetime Example：2017-01-01 00:00:00| 
    | 51 | record id:{recordI} is not exist|
    | 52 |maintain time range is invalid startAt:{startAt} ~ endAt:{endAt} is invalid|

13. #### <span id="delete-maintain">刪除維護</span>

    ```
    DELETE /casino-api/maintain?
        key=<key>&
        recordId=<recordId>&
        hash=<hash>
    ```

    #### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  recordId   | 維護紀錄Id |  int  | 必填，維護紀錄Id |
    |   hash   | 驗證參數 |  string  |     必填    |

    #### **`hash = md5(recordId + secret)`**

    ---
    #### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|
    | recordId | 維護紀錄Id| int |
    ---

    #### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
           "recordId":66,
        }
    }
    ```

    失敗

    ```javascript
    {
        "status":"error",
        "error":{
            "code":51,
            "message":"record id:66 is not exist"
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
    | 51 | record id:{recordI} is not exist|

----


## API FOR WEB

1. #### <span id="player-report">下注紀錄網頁</span>

	## 使用說明
	### api: 
	/player/report
	
	### 參數:
	
	|name |description |
	|----|----|
	|token |dToken (手機專用持久性的token，可於登入後取得。)|
	
	### 例: 
	
	````
	http://casino-api-stage.greatvirtue.com.tw/player/report?token = skfabfha....
	````

2. #### <span id="game-rule">規則網頁</span>
	## 使用說明
	### api: 
	/game/rule
	
	### 參數:
	
	|name	|description |
	|----|----|
	|cashType |1 = 信用, 2 = 現金|
	
	### 例: 
	````
	http://casino-api-stage.greatvirtue.com.tw/game/rule?cashType=1
	````

3. #### <span id="jackpot">jackpot網頁</span>
	快速查看JP的規則
	## 使用說明
	### api: 
	/game/jackpot
	
	### 參數:
	
	|name |description |
	|----|----|
	|appBack |回到app的schreme URL，例如：pharao-casino-mobile:// |
	
	### 例: 
	````
	http://casino-api-stage.greatvirtue.com.tw/game/jackpot
	````

4. #### <span id="version-check">版本檢查</span>
	判斷是否更新App
	##使用說明
	###api:
	/app-api/version-check
	
	### 參數:
	參數需加密
	
	|name |description |
	|----|----|
	|gametype |CASINO = 1|
	|sourcetype | ANDROID = 1, IOS = 2 |
	|version |目前版本號 |
	
	### 回傳值
	````
	{
	    "ReturnCode": 代碼,
	    "ReturnMessage": 回傳訊息
	}
	
	例.成功
	{
	    "ReturnCode": 2,
	    "ReturnMessage": "查詢成功，不須更新"
	}
	
	````
	### ReturnCode
	
	|code |description |
	|----|----| 
	|2 |查詢成功，不須更新| 
	|1 |必須更新|

5. #### <span id="maintain-check">取得server狀態</span>
	## 使用說明
	玩家的規則的網址 
	### api: 
	/app-api/maintain-check
	
	### 參數:
	
	|name |description |
	|----|----|
	|version | 版本號，例如:2.1.2 |
	
	### 回傳值
	
	````
	{
	    "ReturnCode": 代碼,
	    "ReturnMessage":  server 目前的狀態描述
	}
	````
	
	ReturnCode
	
	|code |description |
	|----|----|
	|0 |Server Up(伺服器運行中)| 
	|-4 |Server Down(伺服器維護中)|

6. #### <span id="app-login">登入(取得server需要的登入token)</span>
	取得 dToken 及 登入 game server 所需的token
	## 使用說明
	
	### api: 
	/casino-api/app-login
	
	### 參數:
	
	|name |description |
	|----|----|
	| /game/jackpot |cashType |
	
	### 回傳值
	
	````
	參數說明
	{
	    "username": 玩家名稱,
	    "token": 可登入Game Server的Token,
	    "PlatFormId": PlatFormId,
	    "dToken": 手機專用持久性的token。
	}
	
	例:成功
	{
	    "username": "rick",
	    "token": "e1bcd68b7f7604847e386ec69f43b856",
	    "PlatFormId": 1,
	    "dToken": "eyJpdiI6InI3RVwvNVlkaDBCSmZ0Qjg1bVBRMU5nPT0iLCJ2YWx1ZSI6ImhvMGtHdHhkYXFaaWtHbjM5NW04Qmp6Ukw1Zjh4YkJZclhWQ0NVSk5MQmdEWTUyTjZwb1M1THc4aE9TVTJka1J4YVd6d1daTHFuWk9KVU9ZNG1mamFUMm5SXC94TEtaNVwvUGpUNHl2RXZ2MGs9IiwibWFjIjoiZmE2YWMwNzVkMWU0ZmRhZjA3OTQwMTk2Y2Q0ZDMxOTIzNTRhYmFjMzc1YTA1ZmM3NGQ5ZTRlNzUzODRlMzhhNiJ9"
	}
	 
	例:失敗
	{
	    "ReturnCode": -2,
	    "ReturnMessage": "查詢失敗"
	}
	````
	
	ReturnCode
	
	|code |description |
	|----|----|
	|-4 |token 錯誤 (新)|
	|-2 |查詢失敗|
	|2 |POST is not allowed|


7. #### <span id="app-direct-login-game">app直接登入</span>
	用 dToken 交換 登入 game server 所需的token
	## 使用說明
	### api: 
	/app-api/direct-login-game
	
	### 參數:
	
	|name |description |
	|----|----|
	|dToken |cashType |
	
	### 回傳值
	
	````
	參數說明
	{
	    "username": 玩家名稱,
	    "token": 可登入Game Server的Token,
	    "PlatFormId": ?,
	    "dToken": 手機專用持久性的token。
	}
	
	例:成功
	{
	    "username": "rick",
	    "token": "e1bcd68b7f7604847e386ec69f43b856",
	    "PlatFormId": 1,
	    "dToken": "eyJpdiI6InI3RVwvNVlkaDBCSmZ0Qjg1bVBRMU5nPT0iLCJ2YWx1ZSI6ImhvMGtHdHhkYXFaaWtHbjM5NW04Qmp6Ukw1Zjh4YkJZclhWQ0NVSk5MQmdEWTUyTjZwb1M1THc4aE9TVTJka1J4YVd6d1daTHFuWk9KVU9ZNG1mamFUMm5SXC94TEtaNVwvUGpUNHl2RXZ2MGs9IiwibWFjIjoiZmE2YWMwNzVkMWU0ZmRhZjA3OTQwMTk2Y2Q0ZDMxOTIzNTRhYmFjMzc1YTA1ZmM3NGQ5ZTRlNzUzODRlMzhhNiJ9"
	}
	
	例:失敗
	{
	    "ReturnCode": -2,
	    "ReturnMessage": "查詢失敗"
	}
	````
	
	ReturnCode
	
	|code |description |
	|----|----|
	|-3 |dToken 錯誤 (新)|
	|-2 |查詢失敗|
	|2 |POST is not allowed|
	
	---
	

8. #### <span id="test-account">體驗帳號及url</span>
	每次呼叫api會產生一個一次性帳號，並回傳登入url和token
	## 使用說明
	### api: 
	```
	GET /app-api/visitor
	```
	
	### 回傳值
	
	````
	參數說明
	{
	    "account": 體驗帳號,
	    "loginUrl": 遊戲大廳網址,
	    "token": 可登入Game Server的Token
	}
	
	例:成功
	{
		"status":"success",
	    "account": "xxxxxyyyyyVisitor2",
	    "loginUrl": "http://casino-api-stage.greatvirtue.com.tw/casino-api/player/login?token=eyJpdiI6IjZLVEFCOXl4ekp6c0YyZjF4SjVMWHc9P",
	    "token": "eyJpdiI6IjZLVEFCOXl4ekp6c0YyZjF4SjVMWHc9P"
	}
	
	例:失敗
	{
	    "ReturnCode": -2,
	    "ReturnMessage": "查詢失敗"
	}
	````
	
	
	
9. #### <span id="單桌未結算注單">單桌未結算注單</span>
	取得單桌目前未結算注單
	## 使用說明
	### api: 
	```
	GET /realtime/monitor/betForm/nonCheckout
	```
	
	### 參數:
	
	|name |description |
	|----|----|
	|tableId |桌號 |
	
	### 回傳值
	
	````
	參數說明
	{
	    "BetListId": 注單編號
	}
	
	例:成功
	{
	    "status": "success",
    "data": [
        {
            "Account": "michael2",
            "CompanyId": "1",
            "BetListId": 28956,
            "TableId": null,
            "SpotId": 11001,
            "Round": null,
            "Run": null,
            "GameMethods": "Standard",
            "PlayerId": 15,
            "Amount": 2000,
            "Credit": "9090908.0000",
            "UsedCredit": "6100.0000",
            "RealCredit": "9084808.0000",
            "PlayerIp": "",
            "Refund": "0.0000",
            "IsRevoke": 0,
            "BetTime": "2019-08-27 14:56:43",
            "CheckoutDate": "2019-08-27"            
        }
    ]
	}
	
	例:失敗
	{
	    
	}
	````
	
10. #### <span id="單桌注單">單桌注單</span>
	取得單桌最近X局的注單
	## 使用說明
	### api: 
	```
	GET /realtime/monitor/betForm/table
	```
	
	### 參數:
	
	|name |description |
	|----|----|
	|tableId |桌號 |
	|number |局數 |
	
	
	### 回傳值
	
	````
	參數說明
	{
	    "BetFormId": 注單編號,
	    "GameTypes" : 遊戲類型(目前只有百家樂),
	    "GameMethods" : Standard(標準)、Western(西洋)、NoRefund(免水),
	    "TableTypes"：1：Single(單桌), 2：Multiple(多桌)
	}
	
	例:成功
	{
		    "status": "success",
    "data": [
        {
        		 "Account": "michael2",
            "BetFormId": 7574,
            "GameTypes": "Baccarat",
            "GameMethods": "Standard",
            "TableTypes": 1,
            "HistoryId": 2561451,
            "CompanyId": 2,
            "PlayerId": 15,
            "UsePlatform": "Web",
            "CheckoutList": "[{\"spot\":11001,\"amt\":2000.0,\"ewa\":1900.00,\"lwa\":1900.00}]",
            "Amount": 2000,
            "ValidAmount": 2000,
            "NoRefundCheckoutAmount": "1900.0000",
            "CheckoutAmount": "1900.0000",
            "Percent": "0.00",
            "ActualsAmount": "1900.0000",
            "PlayerIP": "3.0.110.185",
            "RefundSetting": "0.0000",
            "Refund": "0.0000",
            "ModifiedStatus": "Normal",
            "BetTime": "2019-08-27 14:56:43",
            "CheckoutDate": "2019-08-28 00:00:00",
            "JpContribute": "0.000",
            "BetFormTypes": 0,
            "TableId": "A",
            "Round": 1566895939,
            "Run": 38
        }
    ]
	}
	
	例:失敗
	{
	    
	}
	````
	
11. #### <span id="即時注單頁面修改玩家佔成">即時注單頁面 修改/取得玩家佔成</span>
	修改/取得玩家佔成
	
	## 使用說明
	### Token
	header Token: 'Authorization' => 'Bearer $token'
	
	### api: 
	```
	PUT /realtime/monitor/player-percent
	GET /realtime/monitor/player-percent
	```
	
	### 參數:
	
	|name  |  description |
	|------|--------------|
	|account | 必填，帳號   |
	|companyId | 必填，平台ID |
	|percent | PUT必填，佔成 Ex.0.5 |
	
	
	### 回傳值
	
	````
	
	例:成功
	{
	  "status": "success",
	  "data": {
  		 "PlayerId": "224",
      "CompanyId": 1,
      "Account": "michael2",
      "Percent": "0.5",
      "update_at": "2019-12-02 03:17:31",
      "created_at": "2019-12-02 03:17:34"
	  }
	}
	
	例:失敗
	{
    "status": "error",
    "error": {
        "code": "10",
        "message": "service not available"
    }
   }
	````

12. #### <span id="取得所有玩家">取得所有玩家</span>
	取得所有玩家/取得有設定佔成的玩家
	
	## 使用說明
	### Token
	header Token: 'Authorization' => 'Bearer $token'
	
	### api: 
	```
	GET /realtime/monitor/players
	```
	
	### 參數:
	
	|name  |  description |
	|------|--------------|
	|setPercent |必填，1:有設定佔成 0:所有玩家  |
	|setTime    |選填，指定時間點後設定佔成的玩家 |
	|perPage  |選填，分頁數量 |
	|filter  |選填，null或空字串無條件回傳玩家，傳入字串的話會做搜尋Account欄位 |
	
	
	### 回傳值
	
	````
	參數說明：
	"Percent" : 遊戲佔成
	"MonitorPercent" 即時注單佔成
	
	
	例:成功
	{
	  {
    "status": "success",
    "data": {
        "current_page": 1,
        "data": [
            {
                "PlayerId": 3700,
                "CompanyId": 432,
                "Account": "Mohamed",
                "Nickname": "LightPink",
                "Currency": 1,
                "ResetGroupId": 0,
                "Token": null,
                "AppToken": null,
                "Password": null,
                "Enable": 1,
                "Mode": 1,
                "IsOnline": 0,
                "LoginCheckoutDate": "2019-12-02",
                "Credit": "7297.0000",
                "UsedCredit": "0.0000",
                "RealCredit": "120.0000",
                "CurrentLoseWinCredit": "0.0000",
                "LimitWin": "0.0000",
                "LimitLose": "0.0000",
                "Percent": "0.00",
                "StatusId": 0,
                "Attention": 0,
                "LoginLocation": null,
                "LastLoginTime": "2019-12-02 03:44:38",
                "LoginIp": "7.171.115.10",
                "RowVersion": 28,
                "MonitorPercent": "0.99"
            },
            {
                "PlayerId": 3701,
                "CompanyId": 432,
                "Account": "Kaya",
                "Nickname": "PaleVioletRed",
                "Currency": 1,
                "ResetGroupId": 0,
                "Token": null,
                "AppToken": null,
                "Password": null,
                "Enable": 1,
                "Mode": 1,
                "IsOnline": 0,
                "LoginCheckoutDate": "2019-12-02",
                "Credit": "2707.0000",
                "UsedCredit": "0.0000",
                "RealCredit": "628.0000",
                "CurrentLoseWinCredit": "0.0000",
                "LimitWin": "0.0000",
                "LimitLose": "0.0000",
                "Percent": "0.00",
                "StatusId": 0,
                "Attention": 0,
                "LoginLocation": null,
                "LastLoginTime": "2019-12-02 03:44:38",
                "LoginIp": "207.217.100.36",
                "RowVersion": 207,
                "MonitorPercent": "0.99"
            },
            {
                "PlayerId": 3702,
                "CompanyId": 432,
                "Account": "Angelita",
                "Nickname": "Gray",
                "Currency": 1,
                "ResetGroupId": 0,
                "Token": null,
                "AppToken": null,
                "Password": null,
                "Enable": 1,
                "Mode": 1,
                "IsOnline": 0,
                "LoginCheckoutDate": "2019-12-02",
                "Credit": "1217.0000",
                "UsedCredit": "0.0000",
                "RealCredit": "836.0000",
                "CurrentLoseWinCredit": "0.0000",
                "LimitWin": "0.0000",
                "LimitLose": "0.0000",
                "Percent": "0.00",
                "StatusId": 0,
                "Attention": 0,
                "LoginLocation": null,
                "LastLoginTime": "2019-12-02 03:44:38",
                "LoginIp": "188.216.22.255",
                "RowVersion": 271,
                "MonitorPercent": "0.99"
            }
        ],
        "from": 1,
        "last_page": 1,
        "next_page_url": null,
        "path": "http://localhost/realtime/monitor/players",
        "per_page": 15,
        "prev_page_url": null,
        "to": 10,
        "total": 10
    }
}
	
	
	例:失敗
	{
    "status": "error",
    "error": {
        "code": "10",
        "message": "service not available"
    }
   }
	````

13. #### <span id="Monitor使用者登入取得Token">Monitor使用者登入取得Token</span>
	Monitor使用者登入取得Token
	
	## 使用說明
	### Token
	header Token: 'Authorization' => 'Bearer $token'
	
	### api: 
	```
	POST /realtime/monitor/user/login
	```
	
	### 參數:
	
	|name  |  description |
	|------|--------------|
	|account |companyAdmin管理帳號  |
	|password|密碼 |
	
	
	### 回傳值
	
	````
	參數說明：
	token: JWT token
	exp: JWT token 過期時間
	
	例:成功
	{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOnsiYWNjb3VudCI6InFhc3RhZ2UyIiwicGFzc3dvcmQiOiIxMjM0NTYifSwiZXhwIjoxNTc1MzQ1Mzk4LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0L3JlYWx0aW1lL21vbml0b3IvdXNlci9sb2dpbiIsImlhdCI6MTU3NTI1ODk5OCwibmJmIjoxNTc1MjU4OTk4LCJqdGkiOiJVZWM0OGVaMHlYOXlXc0RxIn0.9KEZmtVrs_Fz2zLNwwSC6shseYOuaeMkUTyVb161sLk",
    "exp": 1575429071
   }
	
	
	例:失敗
	{
    'account' => '帳號或密碼不正確，請注意大小寫是否有誤',
   }
	````


## 歷程紀錄

| 日期     | 版本       | 人員               | 備註   |
| ------- | ---------- | ----------------- | ----- |
| 2019/04/17 | 1.0.0  | Kevin | 初版                    | 
| 2019/04/17 | 1.0.1  | Kevin | 新增公告播放地點代碼，新增F,X,Y,Z桌  |
| 2019/05/06 | 1.0.14  | Kevin | 1.修改API [查詢玩家下注區間總額](#查詢玩家下注區間總額)，統計會包含已取消之牌局、查詢條件從"牌局修改時間"(BaccaratHistory.ModifiedTime)改為"注單成立時間"(BetForm.betTime 當局最後一筆下注時間) 2.新增公告播放地點代碼，新增G,H桌 |
| 2019/08/27 | 1.0.25  | Kevin | 1. 新增API 9. [單桌未結算注單](#單桌未結算注單) 2. 新增API 10. [單桌注單](#單桌注單) |
| 2019/12/02 | 1.0.26  | Kevin | 1. 新增API 11. [即時注單修改玩家佔成](#即時注單修改玩家佔成) 2. 新增API 12. [取得所有玩家](#取得所有玩家) 3. 新增API 13. [Monitor使用者登入取得Token](#Monitor使用者登入取得Token) |

