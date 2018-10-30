# 12bao API Document
  
## API列表

1. [創建帳號](創建帳號)
2. [查詢會員帳戶資訊](查詢會員帳戶資訊)
3. [查詢會員結算注單](查詢會員結算注單)
4. [查詢各平台利率](查詢各平台利率)
5. [修改各平台利率](修改各平台利率)
6. [取得會員目前本金利息](取得會員目前本金利息)
7. [轉帳](轉帳)

### *錯誤代碼*

-------

| 錯誤代碼  | 錯誤訊息                  | 錯誤說明              |
|---------|----------|---------------------- |
| 1  | open ssl error | Open SSL 錯誤 |
| 2  | server internal error| 伺服器內部錯誤  |
| 101  | agent_not_exist | 平台不存在 |
| 102  | agent_modify_interest_error| 平台修改利率錯誤 |
| 201  | user_existed | 此會員帳戶已存在  |
| 202  | user_not_exist | 此會員帳戶不存在 |
| 203  | user_already_remitted | 此會員已結算過 |
| 204  | user_update_interest_failed | 會員更新利率失敗 |
| 205  | user_wallet_not_enoutgh | 此會員帳戶餘額不足 |

## *API 使用*

1. ## <span id="create-account">創建帳號</span>

    ```
    POST /v1/user/create?
        agent=<agent>&
        account=<account>&
        name=<name>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  name   | 玩家名稱 |  string  |     必填    |
   
    
    #### **`token = JWT::encode($request->toArray(), platform_private.key, RS256)`**
    
    ---
    
    ### Response 參數說明
    | 參數名稱 		| 參數說明 	| 參數型態 |     
    |:--------:	|:--------:|:--------:|
    |  account 	| 玩家帳號 	| string |
    |  name   		| 玩家名稱 	|  string  |
    |  wallet   	| 結算餘額 	|  decimal(20,8)  |
    |  interest  | 本日產生利息 | decimal(20,8) |
    | interest_rate | 本日利率 | double |
    | created_at | 建立時間 | date |
    | updated_at | 更新時間 | date |
   

    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account":"aa1234",
          	"name":"aa1234",
           	"wallet":"50000.00000000",
           	"interest":"5.01345888",
           	"interest_rate":"0.88",
           	"created_at":"2018-10-25 14:39:00",
           	"updated_at":"2018-10-25 14:39:00"
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":201,
            "message":"The user aa1234 existed"
        }
    }
   ```
   
2. ## <span id="player-info">查詢帳戶明細</span>

    ```
    GET /v1/show-user?
        agent=<agent>&
        account=<account>
    ```
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    
    ---
    
    ### Response 參數說明
    
  	| 參數名稱 		| 參數說明 	| 參數型態 |     
    |:--------:	|:--------:|:--------:|
     |  account 	| 玩家帳號 	| string |
    |  name   		| 玩家名稱 	|  string  |
    |  wallet   	| 本金 	|  decimal(20,8)  |
    |  interest  | 本日產生利息 | decimal(20,8) |
    | interest_rate | 本日利率 | double |
    | created_at | 建立時間 | date |
    | updated_at | 更新時間 | date |

    ---

	### Response 結果

   
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account":"aa1234",
          	"name":"aa1234",
           	"wallet":"50000.00000000",
           	"interest":"5.01345888",
           	"interest_rate":"0.88",
           	"created_at":"2018-10-25 14:39:00",
           	"updated_at":"2018-10-25 14:39:00"
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":202,
            "message":"The user aa1234 not exists"
        }
    }
   ```
  
3. ## <span id="settlement-report">查詢會員結算注單</span>

    ```
    GET /v1/user/settlement?
        agent=<agent>&
        account=<account>&
        billing_date=<billing_date>
    ```
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |  必填    |
    |  billing_date | 結算日 | string  |   必填，格式 2017-01-01   |
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | account 	| 玩家帳號 | string |
    | wallet 	| 轉出本金 | string |
    | interest	| 利率 	 | integer  |
    | created_at | 建立時間 | string |
    | updated_at | 更新時間 | string |
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account":"aa1234",
          	"wallet" : 50344,
           	"interest":54,
           	"created_at": "2018-10-26 00:00:00",
            "updated_at": "2018-10-26 00:00:00"
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":202,
            "message":"The user aa1234 not exists"
        }
    }
   ```

4. ## <span id="platform-interest">查詢平台利率</span>

    ```
    GET /v1/platform/interest?
        agent=<agent>
    ```
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台 |  string  | 由API端提供 |
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  agent 	| 平台		|  string  | 
    |  interest_rate | 平台利率(格式為：0.18) | double  | 
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"agent": "jfa_platform",
           	"interest_rate": 0.18
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
        		"code": 101,
        		"message": "The agent jfa_platform not exists"
        }
    }
   ```

5. ## <span id="modify-platform-interest">修改平台利率</span>

    ```
    POST /v1/platform/interest?
        agent=<agent>
    ```
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台 |  string  | 由API端提供 |
    |  interest_rate | 欲修改利率(格式為：0.18) | double  | 必填 |
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  agent 	| 平台		|  string  | 
    |  interest_rate | 已修改平台利率(格式為：0.18) | double  | 
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"agent": "jfa_platform",
           	"interest_rate": 0.18
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
     			"code": 101,
        		"message": "The agent jfa_platform not exists"
        }
    }
   ```

6. ## <span id="get-account-realtime-interest">取得會員目前本金利息</span>

    ```
    get /v1/user/interest/amount?
        agent=<agent>&
        account=<account>
    ```
    
    ### Request 參數說明
   
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | account | 玩家帳號 | string |
    |  total_interest | 即時本金利息 | double  | 
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account": "aasdf99",
           	"total_interest": 54.03239322
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code": 202,
        	"message": "The user kevinfalo22 not exists"
        }
    }
   ```


7. ## <span id="user-transfer">轉帳</span>

    ```
    post /v1/user/transfer?
        agent=<agent>&
        account=<account>&
        value=<value>
    ```
    
    ### Request 參數說明
   
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  value | 轉出入金額 |  string  |     必填    |
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | account | 玩家帳號 | string |
    | value | 轉出入金額 | string |
    | wallet | 轉出入前本金金額| string |
    | interest | 本次交易產生利息| string |
    |  total_interest | 即時今日本金利息 | double  | 
    | starting_at | 計息開始時間(格式:2018-10-30 10:00:00) | string |
    | closing_at | 計息結束時間(格式:2018-10-30 10:00:00) | string |
    | billing_date | 歸帳日(格式:2018-10-30) | string |
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account": "aasdf99",
          	"amount": "80000",
          	"wallet": "320000.000000000",
          	"interest": "0.62506342",
           	"total_interest": "54.03239322",
           	"starting_at": "2018-10-30 13:52:31"
           	"closing_at": "2018-10-30 13:52:31"
           	"billing_date": "2018-10-30"
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code": 202,
        	"message": "The user kevinfalo22 not exists"
        }
    }
   ```

