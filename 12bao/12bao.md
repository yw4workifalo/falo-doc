# 12bao API Document
  
## 12bao API數量：**7**
1. [創帳號](創帳號)
2. [查詢會員帳戶資訊](查詢會員帳戶資訊)
3. [修改會員利率](修改會員利率)
4. [查詢會員結算注單](查詢會員結算注單)
5. [查詢各平台利率](查詢各平台利率)
6. [修改各平台利率](修改各平台利率)
7. [取得會員目前本金利息](取得會員目前本金利息)


## *API 使用*

1. ## <span id="create-account">創帳號</span>

    ```
    POST /v1/user?
        agent=<agent>&
        account=<account>&
        name=<name>&
        interest_rate=<interest_rate>
    ```
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  name   | 玩家名稱 |  string  |     必填    |
    |  interest_rate | 利率 | double  |     選填    |
    
    #### **`token = JWT::encode($request->toArray(), platform_private.key, RS256)`**
    
    ---
    
    ### Response 參數說明
    | 參數名稱 		| 參數說明 	| 參數型態 |     
    |:--------:	|:--------:|:--------:|
    |  agent_id 	| 平台ID		|  string  | 
    |  account 	| 玩家帳號 	| string |
    |  name   		| 玩家名稱 	|  string  |
    |  wallet   	| 結算餘額 	|  decimal(20,8)  |
    |  interest_rate | 利率 	|  double  |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
        	"agent_id : 1",
          	"account":"aa1234",
          	"name":"aa99asdf",
           	"wallet":"0.01345888",
           	"interest_rate":0.18
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
   
2. ## <span id="player-info">查詢會員帳戶資訊</span>

    ```
    GET /v1/user?
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
    |  loginUrl | 遊戲登入網址|  string  | 
    | token | 登入用 token | string |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
        	"agent_id : 1",
          	"account":"aa1234",
          	"name":"aa99asdf",
           	"wallet":"0.01345888",
           	"interest_rate":0.18
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
  
3. ## <span id="modified-interest">修改會員利率</span>

    ```
    POST /v1/user/interest?
        agent=<agent>&
        account=<account>&
        interest=<interest>
    ```
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |  必填    |
    |  interest_rate | 利率 | double  |   必填   |
    
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
  |  agent   | 平台ID |  string  | 
    |  account | 玩家帳號 |  string  | 
    |  interest_rate | 利率 | double  | 

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
        	"agent_id : 1",
          	"account":"aa1234",
           	"interest_rate":0.18
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


4. ## <span id="settlement-report">查詢會員結算注單</span>

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
    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account":"aa1234",
          	"wallet" : 50344,
           	"interest":54
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

5. ## <span id="platform-interest">查詢平台利率</span>

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
    |  interest_rate | 平台利率 | double  | 
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
            "code":4,
            "message":"player not found"
        }
    }
   ```

6. ## <span id="modify-platform-interest">修改平台利率</span>

    ```
    POST /v1/platform/interest?
        agent=<agent>
    ```
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台 |  string  | 由API端提供 |
    |  interest_rate | 欲修改利率 | double  | 必填 |
    
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  agent 	| 平台		|  string  | 
    |  interest_rate | 已修改平台利率 | double  | 
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
            "code":4,
            "message":"player not found"
        }
    }
   ```

7. ## <span id="get-account-realtime-interest">取得會員目前本金利息</span>

    ```
    get /v1/user/interest?
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
    |  agent 	| 平台		|  string  | 
    | account | 玩家帳號 | string |
    |  total_interest | 即時本金利率 | double  | 
    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"agent": "jfa_platform",
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
            "code":4,
            "message":"player not found"
        }
    }
   ```


