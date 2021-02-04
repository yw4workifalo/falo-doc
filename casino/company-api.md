# Company API Document
  
## CAMPANY API數量：**2**
-------
1. [取得代理](#取得代理)(*1)
2. [新增代理](#新增代理)(*2)

## *API 使用*
-------

1. ## <span id="get-company">取得代理</span>

    ```
    GET /company-api/{companyId}
    ```
    
    ### Header 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    API-KEY   | 服務金鑰 |  string  | 由API端提供並 |
    |  API-HASH | 驗證參數 |  string  |     必填    |
    
    #### **`hash = md5(json_encode(RequestBody) + secret + floor(time() / (24 * 60 * 60))`**
    
    ---
    
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |  name | 代理名稱 |  string  | 
    |  cashType   | 額度類型 |  int  |
    |  limitStakeLevel   | 限注等級 |  int  |
    |  refund   | 反水 |  decimal  |
    | quota | 額度 | decimal |
    | publicKey | 公鑰 | string |
    | privateKey | 私鑰 | string |

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
            "name":"falo-cash",
            "cashType":"1",
            "limitStakeLevel":"3",
            "refund":0,
            "quota": 0,
            "publicKey":"586ef75fd8bf9",
            "privateKey":"180967fda7c2abd4935a8ed4c469115b"
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":3,
            "message":"hash is invalid"
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
   
2. ## <span id="create-company">新增代理</span>

    ```
    POST /company-api/company
    ```
    
    ### Header 參數說明
    
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    API-KEY   | 服務金鑰 |  string  | 由API端提供並 |
    |  API-HASH | 驗證參數 |  string  |     必填    |
    
    ### Request Body 參數說明
        
    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    name   | 代理名稱 |  string  |     必填    |
    |  cashType | 額度類型 |  int  |     必填    |
    |  limitStakeLevel | 限注等級 |  int  |     選填    |
    |  refund | 反水 |  decimal  |     選填    |
    |  quota | 額度 |  decimal  |     選填    |
    |  key | 代理公鑰 |  string  |     選填    |
    |  secret | 代理私鑰 |  string  |     選填    |
    
    #### **`hash = md5(json_encode(RequestBody) + secret + floor(time() / (24 * 60 * 60))`**
        
    ---
    ### Response 參數說明
    | 參數名稱 | 參數說明 | 參數型態 |     
    |:--------:|:--------:|:--------:|
    |    name   | 代理名稱 |  string  |
    |  cashType | 額度類型 |  int  |
    |  limitStakeLevel | 限注等級 | int |
    |  refund | 反水 |  decimal  |
    |  quota | 額度 |  decimal  |
    |  key | 代理公鑰 |  string  |
    |  secret | 代理私鑰 |  string  | 

    ---

    ### Response 結果
    成功

    ```javascript
    {
        "status":"success",
        "data":{
           "name":"falo-cash",
           "cashType":1,
           "limitStakeLevel":0,
           "refund":0,
           "quota":0,
           "key":"586ef75fd8bf9",
           "secret":"180967fda7c2abd4935a8ed4c469115b"
        }
    }
    ```

    失敗

   ```javascript
   {
        "status":"error",
        "error":{
            "code":3,
            "message":"hash is invalid"
        }
    }
   ```
   
   ---
   
   #### 會出現的錯誤項目
   | 錯誤代碼 | 錯誤說明 |     
    |:--------:|:--------:|
    | 0 | db failed |
    | 1  | {parameter} is required   |
    | 2  | key is invalid            |
    | 3  | hash is invalid           |
    | 5  | {method} is not allowed   |
    | 61 | game server update failed |
   
## 歷程紀錄

| 日期     | 版本       | 人員               | 備註   |
| ------- | ---------- | ----------------- | ----- |
| 2021/02/04 | 1.0.0  | Darren | 初版                    | 