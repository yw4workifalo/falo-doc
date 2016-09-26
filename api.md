#　API 說明文件
## Change Log
1. 2016-04-13
    * 新增API
        * [註冊帳號](#register)
        * [取得玩家帳密](#auth)
        * [查詢玩家](#user)
        * [取得玩家遊戲紀錄](#logs)
        * [玩家離線通知](#offline_notification)
2. 2016-04-14
    * 修復BUG
        *  查詢玩家，查無玩家時會發生錯誤。
        *  測試面板，API使用錯誤的 HTTP method 顯示的訊息讓人誤解。
3. 2016-04-15
    * 新增API
        * [玩家加錢](#add_chips)
        * [踢人](#kick)
    * 修改
        *  修改[踢人](#kick)API支援踢多人
4. 2016-04-19
    * 查詢玩家新增參數`status`
5. 2016-04-22
    * 新增[限輸](#lose-limit)
    * 新增[限贏](#win-limit)
    * 新增[限注回復](#limit-recover)
    * 新增[玩家封鎖模式](#ban-mode)
    * 新增[注單回補](#recover-logs)
6. 2016-04-25
    * 新增[限注查詢](#limit-query)
7. 2016-04-29
    * [玩家封鎖模式](#ban-mode) 新增 `no_permission`
8. 2016-05-09
	* 新增[查詢玩家封鎖模式](#get-ban-mode)
	* 新增[查詢玩家是否啟用遊戲](#get-activation)
	* 新增[設定玩家是否啟用遊戲](#set-activation)
	* 新增`agent_name`到[註冊帳號](#register)
9. 2016-05-25
    * 新增[log查詢](#query-logs)
    * 新增[玩家下注簡報查詢](#summary-logs)
10. 2016-05-31
    * 新增[JP紀錄查詢](#jp-logs)
    * 新增[JP核銷](#jp-vertification)
11. 2016-08-29
    * 修改[登入](#login) 增加source, redirect_url參數
    * 修改[玩家離線通知](#offline_notification) 
        * 修改參數說明
        * 新增Hash驗證
        * 新增Json回傳格式
    * 修改[設定玩家是否啟用遊戲](#set-activation) Hash組合
12. 2016-09-26
    * 修改[JP核銷](#jp-vertification) Hash組合

## 登入流程
1. 玩家透過平台登入
2. 平台透過[註冊帳號](#register)新增帳號
3. 平台透過[取得玩家帳號密碼](#auth)取得帳號密碼
4. 透過表單直接將取得的帳號密碼送到[登入](#login)驗證
5. API直接將玩家導向至遊戲網頁

[流程圖連結](https://cacoo.com/diagrams/UQAlsLczvUqmOvl6)

![流程圖](https://cacoo.com/diagrams/UQAlsLczvUqmOvl6-28811.png?t=1455870225441 "API 登入 流程圖")

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

## API
1. ### <span id="register">註冊帳號 </span>

    ```
    POST /api/user/register?
        key=<key>&
        account=<account>&
        name=<nane>&
        agent_name=<agent_name>&
        hash=<hash>
    ```
    
    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |   name   | 玩家暱稱 |  string  |     必填    |
    |agent_name| 代理名稱 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(account + name + secret)**

    ##### 回傳結果
    成功
    ```javascript
    {"status":"success","data":{"account":"test","name":"test","id":7}}
    ```
    失敗
    ```javascript
    {"status":"error","error":{"code":1,"message":"account is required"}}
    ```


2. ### <span id="auth">取得玩家帳號密碼</span>

    ```
    POST /api/slot/user/auth?
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
     {"status":"success","data":{"id":7,"account":"test","name":"test","password":"570de762d3c35","loginUrl":"http://syte.app//api/slot/user/login"}}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":1,"message":"account is required"}}
    ```

3. ### <span id="login">登入</span>
    參數請使用經由[取得玩家帳密](#auth)取得的加密資料

    ```
    GET /api/slot/user/login?
        account=<account>&
        password=<password>＆
        source=<source>
    ```


    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |  account | 玩家帳號 |  string  |     必填    |
    |  passowd | 玩家帳號 |  string  |     必填    |
    |  source  | 登入識別 |  string  |     必填    |
    |  redirect_url | 登出或斷線重導向 | string | 選填 |

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
    GET /api/slot/user?
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
     {"status":"success","data":{"status":"user_offline","id":3,"account":"haha0738","name":"\u6e2c\u8a66","chips":5000000}}
     //status 有user_online, user_offline, service_not_available 三種狀態。
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":1,"message":"account is required"}}
    ```

5. ### <span id="logs">取得玩家遊戲紀錄</span>

    ```
    GET /api/slot/user/logs?
        key=<key>&
        account=<account>&
        start_at=<start_at>&
        end_at=<end_at>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱  | 參數說明  | 參數型態  | 說明                            |
    |---------- |---------- |---------- |------------------------------ |
    | key       | 服務金鑰  | string    | 由API端提供                   |
    | account   | 玩家帳號  | string    | 必填                            |
    | start_at  | 開始時間  | string    | 固定格式Y-m-d,Y-m-d H:i:s或者0，0代表不限制，多查詢七天內的資料  |
    | end_at    | 結束時間  | string    | 固定格式Y-m-d,Y-m-d H:i:s或者0 ，代表不限制，最多查詢七天內的資料 |
    |   hash   | 驗證參數 |  string  |     必填    |

    **hash = md5(account + start_at + end_at + secret)**

    ##### 回傳結果
    成功

    ```javascript
     [{"id":30,"user_id":1,"initial_chips":100720,"sum_of_bet":0,"sum_of_win_chips":0,"final_chips":100720,"ip":"192.168.10.1","login_at":"2016-04-12 18:29:49","logout_at":"2016-04-12 18:32:23","created_at":"2016-04-12 18:29:49","updated_at":"2016-04-12 18:32:23"}]}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":1,"message":"end_at is required"}}
    ```

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

7. ### <spin id="add_chips">玩家加錢</spin>

    ```
    PUT /api/slot/user/chips/add?
        key=<key>&
        account=<account>&
        chips=<chips>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account | 玩家帳號 |  string  |     必填    |
    |  chips   | 增加的籌碼 |  int  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(account + chips + secret)**

    ##### 回傳結果
    成功

    ```javascript
       {"status":"success","data":{"original_chips":135475,"added_chips":"10","final_chips":135485,"account":"haha0738@ifalo.com.tw"}}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

8. ### <spin id="kick">踢玩家</spin>
    
    呼叫之後會在10秒之後將在線的玩家踢出遊戲
    ```
    DELETE /api/slot/user/kick?
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
        {"status":"success","data":[{"account":"kk","status":"user_not_found"},{"account":"aa","status":"user_not_found"},{"status":"user_offline","account":"haha0738@ifalo.com.tw"}]}
        // user 的 status 有 user_offline, kicked, user_not_found三種
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

9. ### <span id="lose-limit">限輸</span>
    
    設定玩家限輸

    ```
    PUT /api/slot/user/limit/lose?
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
    |  limit   | 限制金額 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(account+limit+secret)**

    ##### 回傳結果
    成功

    ```javascript
       {"status":"success","data":{"account":"haha0738@ifalo.com.tw","win_limit":500,"lose_limit":"5000"}}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

10. ### <span id="win-limit">限贏</span>
    
    設定玩家限贏

    ```
    PUT /api/slot/user/limit/win?
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
    |  limit   | 限制金額 |  string  |     必填    |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(account+limit+secret)**

    ##### 回傳結果
    成功

    ```javascript
       {"status":"success","data":{"account":"haha0738@ifalo.com.tw","win_limit":500,"lose_limit":"5000"}}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

11. ### <span id="limit-recover">限注回復</span>

    限注回復

    ```
    PUT /api/slot/user/limit/recover?
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
       {"status":"success","data":{"account":"haha0738@ifalo.com.tw","win_chips":0}}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

12. ### <span id="ban-mode">玩家封鎖模式</span>

    玩家封鎖模式

    ```
    PUT /api/slot/user/mode?
        key=<key>&
        accounts=<accounts>&
        status=<status>&        
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts| 玩家帳號 |  string  |     必填，支援多組帳號可用`,`分割   |
    |   status | 封鎖模式 |  enum    |     必填(normal,banned,locked, no_permission) <ul><li>`normal` 是無封鎖</li><li>`banned` 是停用</li><li>`locked`是鎖單</li><li>`no_permission`是無權限</li></ul>
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(accounts+status+secret)**

    ##### 回傳結果
    成功

    ```javascript
      {"status":"success","data":[{"account":"aa","mode":"user_not_found"},{"account":"haha0738@ifalo.com.tw","mode":"banned"}]}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

13. ### <span id="recover-logs">注單回補</span>

    注單回補，注單有可能太多，造成執行過久，所以是利用queue的方式，因此response之後注單不會立刻回補。

    ```
    GET /api/slot/user/logs/recover?
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
      {"status":"success","data":{"success":true}}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

14. ### <span id="limit-query">限注查詢</span>

    查詢玩家限注狀態

    ```
    GET /api/slot/user/limit?
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
      {"status":"success","data":{"account":"haha0738@ifalo.com.tw","win_chips":-30,"win_limit":500,"lose_limit":5000,"updated_at":"2016-04-22 14:14:08"}}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

15. ### <span id="get-ban-mode">查詢玩家封鎖模式</span>

    查詢玩家鎖單/停用狀態

    ```
    GET /api/slot/user/mode?
        key=<key>&
        accounts=<accounts>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts| 玩家帳號 |  string  |     必填，支援多組帳號可用`,`分割   |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(accounts + secret)**

    ##### 回傳結果
    成功

    ```javascript
      {"status":"success","data":[{"account":"test-api01","status":"normal"}]}
    ```
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

16. ### <span id="get-activation">查詢玩家是否啟用遊戲</span>

    查詢玩家是否啟用遊戲

    ```
    GET /api/slot/user/active?
        key=<key>&
        accounts=<accounts>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts| 玩家帳號 |  string  |     必填，支援多組帳號可用`,`分割   |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(accounts + secret)**

    ##### 回傳結果
    成功

    ```javascript
      {"status":"success","data":[{"account":"test-api01","activation":1}]}
    ```
    
    失敗

    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```

17. ### <span id="set-activation">設定玩家是否啟用遊戲</span>

    設定玩家是否啟用遊戲

    ```
    PUT /api/slot/user/active?
        key=<key>&
        accounts=<accounts>&
        activation=<0|1>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  accounts| 玩家帳號 |  string  |     必填，支援多組帳號可用`,`分割   |
    |activation| 驗證參數 |  int  	|     必填，0或1 |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(accounts + activation + secret )**

    ##### 回傳結果
    成功

    ```javascript
      {"status":"success","data":[{"account":"test-api01","activation":1}]}
    ```
    
    失敗
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```


18. ### <span id="query-logs">LOG查詢</span>

    查詢玩家下注LOG

    ```
    GET /api/slot/user/logs/detail?
        key=<key>&
        account=<account>&
        start_at=<start_at>&
        end_at=<end_at>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填 |
    |start_at  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |end_at    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |
    
    **hash = md5(account + start_at + end_at + secret)**

    ##### 回傳結果
    成功
    ```javascript
      "status":"success","data":[{"id":61,"machine_no":25,"bet":10,"bet_lines":9,"total_bet":90,"win_chips":0,"scatter":0,"created_at":"2016-05-03 13:42:38"},{"id":62,"machine_no":35,"bet":10,"bet_lines":9,"total_bet":90,"win_chips":0,"scatter":0,"created_at":"2016-05-03 13:45:19"},{"id":63,"machine_no":36,"bet":10,"bet_lines":9,"total_bet":90,"win_chips":0,"scatter":0,"created_at":"2016-05-03 13:48:34"},{"id":64,"machine_no":36,"bet":10,"bet_lines":9,"total_bet":90,"win_chips":0,"scatter":1,"created_at":"2016-05-03 13:49:22"},{"id":65,"machine_no":36,"bet":10,"bet_lines":9,"total_bet":90,"win_chips":40,"scatter":1,"created_at":"2016-05-03 13:49:26"},{"id":66,"machine_no":46,"bet":10,"bet_lines":9,"total_bet":90,"win_chips":0,"scatter":0,"created_at":"2016-05-03 13:52:27"}]}
    ```
    
    失敗
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```


19. ### <span id="summary-logs">玩家下注簡報查詢</span>

    玩家下注簡報查詢
    ```
    GET /api/slot/user/log/summary?
        key=<key>&
        account=<account>&
        start_at=<start_at>&
        end_at=<end_at>&
        hash=<hash>
    ```

    ##### 參數說明

    | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填 |
    |start_at  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |end_at    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

       **hash = md5(account + start_at + end_at + secret)**

    ##### 回傳結果

    成功
    ```javascript
      {"status":"success","data":{"total_bet":"540","win_chips":"40"}}
    ```
    
    失敗
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```


20. ### <span id="jp-logs">玩家JP紀錄查詢</span>

    玩家JP紀錄查詢
    ```
    GET /api/slot/user/jp/logs?
        key=<key>&
        account=<account>&
        start_at=<start_at>&
        end_at=<end_at>&
        hash=<hash>
    ```

    ##### 參數說明

     | 參數名稱 | 參數說明 | 參數型態 |     說明    |
    |:--------:|:--------:|:--------:|:-----------:|
    |    key   | 服務金鑰 |  string  | 由API端提供 |
    |  account| 玩家帳號 |  string  |     必填 |
    |start_at  | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |end_at    | 驗證參數 |  string  |     固定格式Y-m-d H:i:s或者0 |
    |   hash   | 驗證參數 |  string  |     必填    |

       **hash = md5(account + start_at + end_at + secret)**

    ##### 回傳結果
    
    成功
    ```javascript
      {"status":"success","data":[{"id":6,"jackpot":8622,"created_at":"2016-05-30 15:06:13","paid_at":"2016-05-30 15:21:08","rate":0.15}]}
    ```
    
    失敗
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```


21. ### <span id="jp-vertification">玩家JP核銷</span>

    玩家JP核銷
    ```
    PUT /api/slot/user/jp/vertify?
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
      {"status":"success","data":{"id":6,"jackpot":8622,"created_at":"2016-05-30 15:06:13","paid_at":"2016-06-01 11:34:18","rate":0.15}}
    ```
    
    失敗
    ```javascript
    {"status":"error","error":{"code":4,"message":"user not found"}}
    ```
    