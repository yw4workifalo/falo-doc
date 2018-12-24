# 12bao API Document
  
## API列表
1. [創建帳號](#創建帳號)
2. [查詢帳戶明細](#查詢帳戶明細)
3. [查詢會員結算注單](#查詢會員結算注單)
4. [查詢平台利率](#查詢平台利率)
5. [修改平台利率](#修改平台利率)
6. [取得會員目前本金利息](#取得會員目前本金利息)
7. [轉帳](#轉帳)
8. [轉帳明細](#轉帳明細)
9. [錢包明細](#錢包明細)
10. [利息明細](#利息明細)

## 餘額寶新舊API比照

舊版API => 新版API
1. 創帳號 => 1.[創建帳號](#創建帳號)
2. 帳戶明細 => 2.[查詢帳戶明細](#查詢帳戶明細)
3. 修改利率 => 5.[修改平台利率](#修改平台利率)
4. 充值 => 7.[轉帳](#轉帳)
5. 錢包明細 => 9. [錢包明細](#錢包明細)
6. 利息明細 => 10. [利息明細](#利息明細)
7. 取得目前本金利息 => 6.[取得會員目前本金利息](#取得會員目前本金利息)
8. 統計報表 => 3.[查詢會員結算注單](#查詢會員結算注單)
9. 默認利息 => 4.[查詢平台利率](#查詢平台利率)


## 流程

1.	玩家透過平台登入
2.	平台呼叫 [創建帳號](#創建帳號) 來註冊帳號
3.	平台呼叫 [轉帳](#轉帳) 來轉出入金額
4.	每日00:00進行利息結算，結算完畢後，餘額寶將會通知平台
5.	平台呼叫 [查詢會員結算注單](#查詢會員結算注單)，取得結算注單
6. 平台將玩家結算注單的轉出金額從玩家餘額寶錢包轉出至主錢包
	

## 錯誤代碼

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

## JWT Authentication 

**`token = JWT::encode($request->toArray(), platform_private.key, RS256)`**


## *API 使用*

1. ## <span id="create-account">創建帳號</span>

    ```
    POST /api/v1/user/create?
        agent=<agent>&
        account=<account>
    ```
    
    ### Request 舊版參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  *bchGuid   | 平台guid |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  *interest   | 利率 |  string  |   必填    |
    
    ### Request 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  *agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
  
    ---
    
    ### Response 舊版參數說明
     
    | 參數名稱 		| 參數說明 	|    
    |:--------:	|:--------:|
    | wallet |本金 | 
    | income |本日產生利息 | 
    | interest |本日利率 | 
    | created_timestamp |建立時間 |
    | timestamp |更新時間 | 
    | last_compute |最後交易時間 | 
    | income_second |每秒產生利息 | 
    | income_time |計息時間 | 
    | yesterday_income |昨日產生利息 | 

    
    ### Response 參數說明
    
    | 參數名稱 		| 參數說明 	| 參數型態 |     
    |:--------:	|:--------:|:--------:|
    | account 	| 玩家帳號 	| string |
    | wallet   	| 本金 	|  decimal(20,8)  |
    | income  | 本日產生利息 | decimal(20,8) |
    | interest | 本日利率 | double |
    | yesterday_income | 昨日產生利息 | double |
    | created_timestamp | 建立時間 | date |
    | timestamp | 更新時間 | date |
    | last_compute |最後一筆交易時間 | date |
    | income_second |每秒產生利息 | double |
    | income_time |計息時間 | double |

    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account":"aa1234",
           	"wallet":"50000.00000000",
           	"income":"5.01345888",
           	"interest":"0.88",
           	"yesterday_income" : 0,
            "created_timestamp": 1542086854,
	        "timestamp": 1542086854,
	        "last_compute": 1542086854,
	        "income_second": "0.00000000",
	        "income_time": "0"
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
    GET /api/v1/user/show?
        agent=<agent>&
        account=<account>
    ```
    
    ### Request 舊版參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  *bchGuid   | 平台GUID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  *agent   | 平台ID |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    
    ---
    
    ### Response 舊版參數說明
     
    | 參數名稱 		| 參數說明 	|    
    |:--------:	|:--------:|
    | wallet |本金 | 
    | income |本日產生利息 | 
    | interest |本日利率 | 
    | created_timestamp |建立時間 |
    | timestamp |更新時間 | 
    | *last_compute |最後交易時間 | 
    | *income_second |每秒產生利息 | 
    | *income_time |計息時間 | 
    | *yesterday_income |昨日產生利息 |
    
    ### Response 參數說明
    
  	| 參數名稱 		| 參數說明 	| 參數型態 |     
    |:--------:	|:--------:|:--------:|
    | account 	| 玩家帳號 	| string |
    | wallet   	| 本金 	|  decimal(20,8)  |
    | income  | 本日產生利息 | decimal(20,8) |
    | interest | 本日利率 | double |
    | yesterday_income | 昨日產生利息 | double |
    | created_timestamp | 建立時間 | date |
    | timestamp | 更新時間 | date |
    | last_compute |最後一筆交易時間 | date |
    | income_second |目前本金每秒可產生利息 | double |
    | income_time |計息時間(秒) | double |

    ---

	### Response 結果

   
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"account":"aa1234",
           "wallet": "30000.00000000",
	        "income": "0.00072298",
	        "interest": 18,
	        "yesterday_income": 15.15517503,
	        "created_timestamp": 1542086854,
	        "timestamp": 1542086854,
	        "last_compute": 1542086893,
	        "income_second": "0.00036149",
	        "income_time": "2"
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
    GET /api/v1/user/settlement?
        agent=<agent>&
        accounts=<accounts>&
        starting_at=<starting_at>&
        closing_at=<closing_at>&
        count=<count>&
        page=<page>
    ```
    ### Response 舊版參數說明
    
    | 參數名稱 | 參數說明 | 
    |:--------:|:--------:|
    |  bch_id   | 平台ID | 
    |  account | 玩家帳號 | 
    |  start | 開始時間 | 
    |  end | 結束時間 |
    |  perpage | 筆數 | 
    |  page | 頁數 |
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  accounts | 玩家帳號 |  string  | 必填:多帳號用逗號隔開，空白為搜尋該平台全部帳號  |
    |  starting_at | 開始時間(2018-10-01 00:00:00) |  string  | 必填    |
    |  closing_at | 結束時間(2018-11-30 00:00:00) |  string  | 必填    |
    |  count | 筆數 |  string  | 選填，筆數，預設20筆 |
    |  page | 頁數 |  string  | 選填，頁數：預設第1頁   |
    
    ---
    
    ### Response 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | account 	| 玩家帳號 | string |
    | recharge_in 	| 轉入本金 | string |
    | recharge_out	| 轉出本金 | integer  |
    | get_income_total | 利息收入 | double |
    | created_at | 建立時間 | string |
    | income | 前一日餘額 | string |
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
    "status": "success",
    "data": {
        "current_page": 1,
        "data": [
            {
                "account": "kevinfalo994",
                "recharge_in": "7222221.99574390",
                "recharge_out": 7224504,
                "get_income_total": "2283.99574388",
                "created_at": "2018-11-14 00:00:00",
                "income": "0.99574390"
            },
            {
                "account": "kevinfalo333",
                "recharge_in": "1666665.60712820",
                "recharge_out": 1666969,
                "get_income_total": "304.60712816",
                "created_at": "2018-11-14 00:00:00",
                "income": "0.60712820"
            }
        ],
        "first_page_url": "http://127.0.0.1:8000/api/v1/user/settlement?page=1",
        "from": 1,
        "last_page": 1,
        "last_page_url": "http://127.0.0.1:8000/api/v1/user/settlement?page=1",
        "next_page_url": null,
        "path": "http://127.0.0.1:8000/api/v1/user/settlement",
        "per_page": "20",
        "prev_page_url": null,
        "to": 2,
        "total": 2
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
    GET /api/v1/platform/interest?
        agent=<agent>
    ```
    
    ### 平台利率設定、查詢說明
    
    * 平台方呼叫API"修改平台利率"後，會在隔日00:00進行各平台利率更新，在隔日00:00之前查詢到的平台利率仍是原先的利率
    * 若有平台有優惠活動進行中，將會回傳平台的活動利率
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台 |  string  | 由API端提供 |
    
    ---
    
    ### Response 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | agent 	| 平台		|  string  | 
    | interest_rate | 生效平台利率(格式為：0.18) | double  | 
    | platform_interest_rate | 平台利率 | double|
    | next_interest_rate | 明日生效平台利率 | double |
    | event_interest_rate | 平台活動利率 | double |
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"agent": "jfa_platform",
           	"interest_rate": 0.99,
        	"platform_interest_rate": 1.11,
        	"next_interest_rate": 5.55,
        	"event_interest_rate": 0.99
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
    POST /api/v1/platform/interest?
        agent=<agent>
        interest_rate=<interest_rate>
    ```
    
    ### 平台利率設定、查詢說明
    
    * 平台方呼叫API"修改平台利率"後，會在隔日00:00進行各平台利率更新，在隔日00:00之前查詢到的平台利率仍是原先的利率
    * 若有平台有優惠活動進行中，將會回傳平台的活動利率
    
    ### Request 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台 |  string  | 由API端提供 |
    |  interest_rate | 欲修改利率(格式為：0.18) | double  | 必填 |
    | platform_interest_rate | 平台利率 | double|
    | next_interest_rate | 明日生效平台利率 | double |
    | event_interest_rate | 平台活動利率 | double |
    
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
           	"interest_rate": 0.99,
        	"platform_interest_rate": 1.11,
        	"next_interest_rate": 5.55,
        	"event_interest_rate": 0.99
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
    GET /api/v1/user/interest/amount?
        agent=<agent>&
        count=<count>&
        page=<page>

    ```
    
    ### Request 參數說明
   
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  count | 筆數 |  string  | 選填，筆數，預設50筆 |
    |  page | 頁數 |  string  | 選填，頁數：預設第1頁   |
    
    ---
    
    ### Response 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | account | 玩家帳號 | string |
    | billing_date | 歸帳日 | string |
    | income | 利息收入總和 | string |
    | wallet | 錢包本金 | string |
    | total  | 本金+利息 | string |
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status": "success",
    	"data": {
        	"current_page": 1,
        	"data": [
	            {
	                "account": "kevinfalo993",
	                "billing_date": "2018-11-13",
	                "income": "11.01598063",
	                "wallet": "19999998.00000000",
	                "total": "20000009.01598063"
	            },
	            {
	                "account": "kevinfalo994",
	                "billing_date": "2018-11-13",
	                "income": "0.00000000",
	                "wallet": "9999999.00000000",
	                "total": "9999999.00000000"
	            }
	        ],
        "first_page_url": "http://127.0.0.1:8000/api/v1/user/interest/amount?page=1",
        "from": 1,
        "last_page": 1,
        "last_page_url": "http://127.0.0.1:8000/api/v1/user/interest/amount?page=1",
        "next_page_url": null,
        "path": "http://127.0.0.1:8000/api/v1/user/interest/amount",
        "per_page": "20",
        "prev_page_url": null,
        "to": 2,
        "total": 2
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
    POST /api/v1/user/transfer?
        agent=<agent>&
        account=<account>&
        value=<value>&
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
    | id   | 交易ID | number |
    | type | 轉帳類型：1轉入、2轉出 | string |
    | account | 玩家帳號 | string |
    | value | 轉出入金額 | string |
    | before_value | 轉出入前本金金額| string |
    | after_value | 轉出入後本金金額| string |
    | created_timestamp | 交易時間 | string | 
    
    ---
    
    ### <span id="enable">轉帳類型</span>
    | 代碼  | 類型                  | 
    |----------|-------------------------  |
    | 1        | 轉入、存款   | 
    | 2        | 轉出、提款   | 
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
          	"id": 28,
	        "type": 2,
	        "account": "kevinfalo994",
	        "value": "-5555555",
	        "before_value": "12222221.00000000",
	        "after_value": "6666666.00000000",
	        "created_timestamp": 1542087778
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

8. ## <span id="transfer-report">轉帳明細</span>

    ```
    POST /api/v1/user/transfer-report?
        agent=<agent>&
        accounts=<accounts>&
        starting_at=<starting_at>&
        closing_at=<closing_at>&
        count=<count>&
        page=<page>
    ```
    
    ### Request 參數說明
   
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  accounts | 玩家帳號 |  string  |     必填    |
    |  starting_at | 開始時間 |  string  | 必填    |
    |  closing_at | 結束時間 |  string  | 必填    |
    |  count | 筆數 |  string  | 選填筆數，預設20筆 |
    |  page | 頁數 |  string  | 選填頁數：預設第1頁   |
    
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
    |transfer_type | 轉帳類型(一般轉帳Normal/系統轉出System) | string |
    |after_value| 本金餘額 | string |
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
        "status":"success",
        "data":{
     "current_page": 1,
            "data": [
                {
                    "user_id": 1,
                    "amount": "0.00000000",
                    "wallet": "0.33000000",
                    "interest": "0.00000000",
                    "starting_at": "2018-10-29 00:00:00",
                    "closing_at": "2018-10-29 00:00:00",
                    "billing_date": "2018-10-28",
                    "transfer_type": "System",
                    "after_value": "20000.0034532"
                }
            ],
            "first_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=1",
            "from": 1,
            "last_page": 1,
            "last_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=1",
            "next_page_url": null,
            "path": "http://127.0.0.1:8000/api/v1/user/transfer-report",
            "per_page": "20",
            "prev_page_url": null,
            "to": 1,
            "total": 1
        }        }
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

9. ## <span id="wallet-report">錢包明細</span>

    ```
    POST /api/v1/user/transfer/wallet-report?
        agent=<agent>&
        accounts=<accounts>&
        starting_at=<starting_at>&
        closing_at=<closing_at>&
        count=<count>&
        page=<page>
    ```
    
    ### Request 參數說明
   
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  accounts | 玩家帳號 |  string  | 必填:多帳號用逗號隔開，空白為搜尋該平台全部帳號 |
    |  starting_at | 開始時間 |  string  | 必填    |
    |  closing_at | 結束時間 |  string  | 必填    |
    |  count | 筆數 |  string  | 選填，筆數，預設20筆 |
    |  page | 頁數 |  string  | 選填，頁數：預設第1頁   |
    
    ---
    
    
    ### Response 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | id | 交易編號 | string |
    | account | 玩家帳號 | string |
    | type | 轉帳類型 | string |
    | value | 轉出入金額 | string |
    | before_value | 轉出入前本金金額| string |
    | after_value | 轉入後本金金額 | string |
    | created_timestamp | 計息開始時間(格式timestamp:1524007559) | string |
    | get_income | 利息餘額 | string |
    | income_time | 計時時間 | string |
    | &emsp;&emsp;income_data |         |       |
    | &emsp;&emsp;&emsp;income_id | 利息編號 | string |
    | &emsp;&emsp;&emsp;type | 類型1:利息 2:提款 | string |
    | &emsp;&emsp;&emsp;value| 異動金額| string |
    | &emsp;&emsp;&emsp;before_value| 異動前金額 |string|
    | &emsp;&emsp;&emsp;after_value| 異動後金額 |string|
    | &emsp;&emsp;&emsp;created_timestamp| 明細建立時間 unix-time|string|
    | &emsp;&emsp;&emsp;last_compute| 上一次算利息時間 unix-time|string|
    
    |舊版參數名稱| 分頁參數名稱 | 參數說明 | 參數型態 |
    |:--------:|:--------:|:--------:|:--------:|
    |error|是否成功| 0:success| string|
    |page| current_page | 目前頁數 | string |
    |count| per_page | 每頁筆數 | string |
    |total | total | 總筆數 | string |
    |total_page| last_page | 總頁數 | string |
    
    ---
    
    ### <span id="enable">交易類型</span>
    | 代碼  | 類型                  | 
    |----------|-------------------------  |
    | 0        | Normal 一般玩家轉出入   | 
    | 1        | System 系統轉帳   | 
    
    ---

    ### Response 結果
    
     成功

    ```javascript
    {
    "status": "success",
    "data": [
        {
            "current_page": 1,
            "data": [
                {
                    "id": 943,
                    "account": "test02",
                    "after_value": 10000,
                    "type": "1",
                    "value": "10000.00000000",
                    "before_value": "0.00000000",
                    "created_timestamp": "2018-12-20 16:22:11",
                    "get_income": "0.00000000",
                    "income_time": 0,
                    "income": 0,
                    "income_data": [
                        {
                            "income_id": 943,
                            "type": "1",
                            "value": 0,
                            "before_value": "0.00000000",
                            "after_value": 0,
                            "created_timestamp": "2018-12-20 16:22:11",
                            "last_compute": {
                                "date": "2018-12-24 10:09:24.027915",
                                "timezone_type": 3,
                                "timezone": "Asia/Taipei"
                            }
                        }
                    ]
                },
                {
                    "id": 943,
                    "account": "test02",
                    "after_value": 20000.0034532,
                    "type": "1",
                    "value": "10000.00000000",
                    "before_value": "10000.00000000",
                    "created_timestamp": "2018-12-20 16:22:11",
                    "get_income": "0.00345320",
                    "income_time": 11,
                    "income": 0.0034532,
                    "income_data": [
                        {
                            "income_id": 943,
                            "type": "1",
                            "value": 0,
                            "before_value": "0.00000000",
                            "after_value": 0.0034532,
                            "created_timestamp": "2018-12-20 16:22:11",
                            "last_compute": {
                                "date": "2018-12-24 10:09:24.034560",
                                "timezone_type": 3,
                                "timezone": "Asia/Taipei"
                            }
                        }
                    ]
                }
            ],
            "first_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=1",
            "from": 1,
            "last_page": 2,
            "last_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=2",
            "next_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=2",
            "path": "http://127.0.0.1:8000/api/v1/user/transfer-report",
            "per_page": "20",
            "prev_page_url": null,
            "to": 20,
            "total": 23
        },
        {
            "current_page": 1,
            "data": [
                {
                    "id": 101,
                    "account": "testAcc5",
                    "type": 1,
                    "value": "50000.00000000",
                    "before_value": "0.00000000",
                    "after_value": 50000,
                    "created_timestamp": "2018-10-25 00:30:00"
                }
            ],
            "first_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=1",
            "from": 1,
            "last_page": 2,
            "last_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=2",
            "next_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=2",
            "path": "http://127.0.0.1:8000/api/v1/user/transfer-report",
            "per_page": "20",
            "prev_page_url": null,
            "to": 20,
            "total": 23
        }
    ]
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
   
   ---

10. ## <span id="interest-report">利息明細</span>

    ```
    POST /api/v1/user/transfer/interest-report?
        agent=<agent>&
        accounts=<accounts>&
        starting_at=<starting_at>&
        closing_at=<closing_at>&
        count=<count>&
        page=<page>
    ```

    
    ### Request 參數說明
   
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  agent   | 平台ID |  string  | 由API端提供 |
    |  accounts | 玩家帳號 |  string  | 必填:多帳號用逗號隔開，空白為搜尋該平台全部帳號    |
    |  starting_at | 開始時間 |  string  | 必填    |
    |  closing_at | 結束時間 |  string  | 必填    |
    |  count | 筆數 |  string  | 選填筆數，預設20筆 |
    |  page | 頁數 |  string  | 選填頁數：預設第1頁   |
    
    ---
    
    ### Response 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    | account | 玩家帳號 | string |
    | type | 轉帳類型 | string |
    | value | 本次產生利息 | string |
    | before_value | 交易前利息總和 | string |
    | after_value | 交易後利息總和 | string |
    | created_timestamp | 計息開始時間(格式timestamp:1524007559) | string |
    | current_page | 目前頁數 | string |
    | per_page | 每頁筆數 | string |
    | total | 總筆數 | string |
    | last_page | 最後一頁頁數 | string |
    
    ---
    
    ### <span id="enable">交易類型</span>
    | 代碼  | 類型                  | 
    |----------|-------------------------  |
    | 0        | Normal 一般玩家轉出入   | 
    | 1        | System 系統轉帳   | 
    
    ---

    ### Response 結果
    
    成功

    ```javascript
    {
    "status": "success",
    "data": [
     		{
            "current_page": 1,
            "data": [],
            "first_page_url": "http://127.0.0.1:8000/api/v1/user/transfer/interest-report?page=1",
            "from": null,
            "last_page": 1,
            "last_page_url": "http://127.0.0.1:8000/api/v1/user/transfer/interest-report?page=1",
            "next_page_url": null,
            "path": "http://127.0.0.1:8000/api/v1/user/transfer/interest-report",
            "per_page": "20",
            "prev_page_url": null,
            "to": null,
            "total": 0
        },
        {
            "current_page": 1,
            "data": [
                {
                    "account": "testAcc6",
                    "type": "1",
                    "value": "0.00000000",
                    "before_value": "0.00000000",
                    "after_value": "0",
                    "created_timestamp": 1542007556
                },
                {
                    "account": "testAcc6",
                    "type": "1",
                    "value": "0.20928461",
                    "before_value": "0.00000000",
                    "after_value": "0.20928461",
                    "created_timestamp": 1542007556
                },
                {
                    "account": "testAcc6",
                    "type": "1",
                    "value": "0.41856922",
                    "before_value": "0.20928461",
                    "after_value": "0.62785383",
                    "created_timestamp": 1542007557
                }
            ],
            "first_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=1",
            "from": 1,
            "last_page": 2,
            "last_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=2",
            "next_page_url": "http://127.0.0.1:8000/api/v1/user/transfer-report?page=2",
            "path": "http://127.0.0.1:8000/api/v1/user/transfer-report",
            "per_page": "20",
            "prev_page_url": null,
            "to": 20,
            "total": 23
        }
    ]
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






