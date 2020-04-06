<div id="top"></div>
# 帳務系統介接API範本
● [註冊玩家帳號](#註冊玩家帳號)
● [取得餘額](#取得餘額)
● [判斷帳號是否存在](#判斷帳號是否存在)
● [取得帳號資訊](#取得帳號資訊)
● [設定限贏限輸](#設定限贏限輸)
● [設定信用額度](#設定信用額度)
● [設定會員狀態](#設定會員狀態)
● [取得登入網址](#取得登入網址)
● [取得玩家信用額度](#取得玩家信用額度)
● [設定遊戲範本](#設定遊戲範本)
● [登出](#登出)
● [重設限輸限贏](#重設限輸限贏)
● [設定暱稱](#設定暱稱)
● [設定信用額度回復類型](#設定信用額度回復類型)
● [重設信用玩家額度](#重設信用玩家額度)
● [設定退水](#設定退水)
● [一次踢多個玩家](#一次踢多個玩家)
● [抓取會員下注注單](#抓取會員下注注單)
● [額度轉出入](#額度轉出入)
● [遊戲代號](#遊戲代號)
● [黃金期權information和content](#黃金期權information和content)
● [彩票information和content](#彩票information和content)
● [彩球information和content](#彩球information和content)
● [3D娛樂城information和content](#3D娛樂城information和content)
● [casino的information和content](#casino的information和content)

------
## <span>註冊玩家帳號</span>
   **API Name : account**</br>
   **Method : POST**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |

   ### 範例
   + 調用方法
     ```
     POST /api/keno-api/遊戲代號/account?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
------
## <span>取得餘額</span>
   **API Name : quota**</br>
   **Method : GET**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
| credit | 餘額  | int  | |

   ### 範例
   + 調用方法
     ```
     GET /api/keno-api/遊戲代號/quota?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>判斷帳號是否存在</span>
   **API Name : accountExist**</br>
   **Method : GET**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| true or false  | true存在false不存在  | boolean  | |

   ### 範例
   + 調用方法
     ```
     GET /api/keno-api/遊戲代號/accountExist?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": true,
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>取得帳號資訊</span>
   **API Name : accountDetails</br>
   **Method : GET**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
| credit | 餘額| int||
|winCredit| 輸贏統計| float||
|status|狀態3:正常,2:停止下注,-2停用| int | |



   ### 範例
   + 調用方法
     ```
     GET /api/keno-api/遊戲代號/accountDetails?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "credit":100,
            "winCredit":51.5,
            "status":3
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>設定限贏限輸</span>
   **API Name : winLose**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
|winValue |限贏| int|Y|數字|
|loseValue|限輸| int |Y|數字|


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
|winValue |限贏| int||
|loseValue|限輸| int ||

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/winLose?
          account=ifalo001&winValue=1000&loseValue=3000
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "winValue":1000,
            "loseValue":3000
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>設定信用額度</span>
   **API Name : credit**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
|value |信用額度| int|Y|數字|


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
|credit |信用額度| int||

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/credit?
          account=ifalo001&value=1000
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "credit":1000,
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>設定會員狀態</span>
   **API Name : playerBanned**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
|banned |狀態3:正常,2:停止下注,-2停用| int|Y|數字|


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
|banned |狀態3:正常,2:停止下注,-2停用| int||

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/playerBanned?
          account=ifalo001&banned=3
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "banned":3,
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>取得登入網址</span>
   **API Name : gameUrl**</br>
   **Method : GET**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| loginUrl | 登入網址  | string  | |
| token | token  | string  | |

   ### 範例
   + 調用方法
     ```
     GET /api/keno-api/遊戲代號/gameUrl?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "loginUrl":"http://tw.yahoo.com",
            "token":""
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>取得玩家信用額度</span>
   **API Name : credit**</br>
   **Method : GET**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 帳號  | string  | |
| credit | 信用額度  | int  | |

   ### 範例
   + 調用方法
     ```
     GET /api/keno-api/遊戲代號/credit?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "credit":15000
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>設定遊戲範本</span>
   **API Name : template**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
|template |範本| string|Y|字串,請登入代理後台查詢各遊戲範本變化,再設定|


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
|template |範本| string


   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/template?
          account=ifalo001&template=A
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "template":"A",
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>登出</span>
   **API Name : logout**</br>
   **Method : DELETE**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |

   ### 範例
   + 調用方法
     ```
     DELETE /api/keno-api/遊戲代號/logout?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>重設限輸限贏</span>
   **API Name : recoverWinLose**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/recoverWinLose?
          account=ifalo001
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>設定暱稱</span>
   **API Name : name**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
|name|暱稱|string|Y||


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
| name | 暱稱  | string  | |

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/name?
          account=ifalo001&name=鋼鐵人
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "name":"鋼鐵人"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>設定信用額度回復類型</span>
   **API Name : creditReset**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
|restType|回復類型|int|Y||


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
| restType | 回復類型  | int  | |

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/creditReset?
          account=ifalo001&restType=1
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "restType":1
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------

## <span>重設信用玩家額度</span>
   **API Name : resetCredit**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
|restType|回復類型|int|Y||


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| restType | 回復類型  | int  | |

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/resetCredit?
          restType=1
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "restType":1
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>設定退水</span>
   **API Name : commission**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
|account|帳號|string|Y||
|commission|退水|int|Y|彩票,黃金期權:0~150,3d-slot:0~500|


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
| commission=15 | 退水  | int  | |

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/commission?
          account=ifalo&commission=15
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo",
            "commission":15
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>一次踢多個玩家</span>
   **API Name : kickPlayers**</br>
   **Method : DELETE**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| accounts | 帳號 | string | Y | 用,號格開 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| accounts | 帳號  | string  | |

   ### 範例
   + 調用方法
     ```
     DELETE /api/keno-api/遊戲代號/kickPlayers?
          accounts=ifalo001,ifalo002
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "accounts":"ifalo001,ifalo002",
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>抓取會員下注注單</span>
   **API Name : reportDetail</br>
   **Method : GET**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| startAt | 開始時間 | string | Y | yyyy-mm-dd hh:ii:ss |
| endAt | 結束時間 | string | Y | yyyy-mm-dd hh:ii:ss 兩時間間格5分鐘內 |
| page | 頁數 | int | Y |  |
|account|會員帳號| string || 長度最短4，最長20 |


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| total | 總注單數  | int  | |
| lastPage |最後頁數| int||
|currentPage| 當前頁數| int||
|list|注單內容| array | |



   ### 範例
   + 調用方法
     ```
     GET /api/keno-api/遊戲代號/reportDetail?
          startAt=2020-02-01 12:34:00&endAt=2020-02-01 12:38:00&page=1
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "total":1,
            "lastPage":1,
            "currentPage":1,
            "list":[
             {
                    "gameName" : "遊戲代號",
                    "orderId" : "注單編號",
                    "account" : "玩家帳號",
                    "currency" : "幣別",
                    "gameType" : "主玩法",
                    "betTime" :"下注時間",
                    "betAmount" :"注單金額",
                    "validBetAmount" : "有效金額",
                    "wins" :"贏錢,不包含本金,輸是0",
                    "commission" : "退水",
                    "betStatus" :"注單狀態-3刪單,3已結算,0未結算" ,
                    "updateTime" :"異動日期和結算日期",
                    "information" :"開獎資訊array,詳見各遊戲information說明",
                    "content" :"下注全部資訊array,詳見各遊戲content說明"
             }
            ]
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------
## <span>額度轉出入</span>
   **API Name : transfer**</br>
   **Method : PUT**

Header

| 型態   | 參數  |  說明   |
| :----- | :---- | :-----: |
| string | token | API金鑰 |

   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
|value |轉出入金額|string|Y|+1000為轉入1000,-1000為轉出1000|
|orderNo|轉出入端驗證編號| string |Y|唯一值|


   ### 輸出參數

| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
|value |轉出入金額| string||
|orderNo|轉出入端驗證編號| string ||

   ### 範例
   + 調用方法
     ```
     PUT /api/keno-api/遊戲代號/transfer?
          account=ifalo001&value=+1000&orderNo=de56f5a4-61f6-11ea-8ccf-02427e732ecf
     ```
     
   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "value":"+1000",
            "orderNo":"de56f5a4-61f6-11ea-8ccf-02427e732ecf"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":8,
           "message":"account already exist"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>
------

### 遊戲代號

| 代號 | 名稱         |
| -------- | ---------------- |
| golden-option  | 黃金期權         |
| lottery    | 彩票       |
| balls  | 彩球 |
|3d-slot |3D娛樂城|
|casino | casino |

### 黃金期權information和content

| 代號 | 名稱         |
| -------- | ---------------- |
| expireTime | 到期時間 |
| settlePrice | 結算價 |
|kind |類別F：Forex(外匯),I：Index(指數),S：Stock(股票),C：Commodity(商品)|
|symbol|標的物|
| betType|下注型態1：買上漲,2：買下跌|
| targetPrice|目標市價|
| odds|賠率|
| extend|是否有使用展延0：無,1：有|
| extendFee|展延費用|
| refund|退水 0 ~ 150，使用提早結算與展延功能不退水|
| mode|狀態模式X：展延(尚未結算),I：作廢,S：提早結算,空：正常狀態(下單未結算),F：(完成結算)|








### 彩票information和content

| 代號 | 名稱         |
| -------- | ---------------- |
| drawingNo | 開獎期別 |
| drawingNumber | 開獎號碼 |
| groupName | 玩法群組名稱 |
| gameName |玩法名稱|
|betData|	下注號碼|
| betNumber	|注數|
|odd|	賠率|
|multiple	|倍數|
|defaultBet	|底注|
|winBonus	|中獎金額|

### 彩球information和content

| 代號 | 名稱         |
| -------- | ---------------- |
| schedule | 注單期數 |
| draw_at | 開獎時間 |
| draw_number | 開獎資訊，尚未開獎則回傳空陣列 |
| play_type |遊戲玩法|
|bet_amount	|每碰下注金額|
|bet_count	|碰數|
|odds	|賠率|
|info	|下注內容|
|check_status|結帳狀態|
|delete_status|注單狀態|

### 彩球遊戲玩法

| play_type |   說明    |
| :-------: | :-------: |
|     1     |  特別號   |
|     2     |   全車    |
|     3     |   二星    |
|     4     |   三星    |
|     5     |   四星    |
|     6     |  天碰二   |
|     7     |  天碰三   |
|     8     |   台號    |
|     9     |  特尾三   |
|    10     | 組合/球色 |
|    11     |   生肖    |

### 彩球下注內容

#### 參數說明

|   key   |         玩法          |      說明       |
| :-----: | :-------------------: | :-------------: |
|   bet   |         所有          |    投注號碼     |
| special |    天碰二、天碰三     |    投注特碼     |
|  ball   |    組合/球色、生肖    |    投注球號     |
|  type   | 234星、天碰二、天碰三 | 0:連碰 / 1:柱碰 |

#### 組合/球色、生肖 - 球號

| ball |  說明  |
| :--: | :----: |
|  0   | 特別號 |
|  1   | 第一球 |
|  2   | 第二球 |
|  3   | 第三球 |
|  4   | 第四球 |
|  5   | 第五球 |
|  6   | 第六球 |

#### 組合/球色

| bet  |  說明  |
| :--: | :----: |
|  1   |   大   |
|  2   |   小   |
|  3   |   單   |
|  4   |   雙   |
|  5   |  合大  |
|  6   |  合小  |
|  7   |  合單  |
|  8   |  合雙  |
|  9   |   紅   |
|  10  |   藍   |
|  11  |   綠   |
|  12  |  紅大  |
|  13  |  藍大  |
|  14  |  綠大  |
|  15  |  紅小  |
|  16  |  藍小  |
|  17  |  綠小  |
|  18  |  紅單  |
|  19  |  藍單  |
|  20  |  綠單  |
|  21  |  紅雙  |
|  22  |  藍雙  |
|  23  |  綠雙  |
|  24  | 紅大單 |
|  25  | 藍大單 |
|  26  | 綠大單 |
|  27  | 紅大雙 |
|  28  | 藍大雙 |
|  29  | 綠大雙 |
|  30  | 紅小單 |
|  31  | 藍小單 |
|  32  | 綠小單 |
|  33  | 紅小雙 |
|  34  | 藍小雙 |
|  35  | 綠小雙 |

#### 生肖

| bet  | 說明 |
| :--: | :--: |
|  1   |  鼠  |
|  2   |  牛  |
|  3   |  虎  |
|  4   |  兔  |
|  5   |  龍  |
|  6   |  蛇  |
|  7   |  馬  |
|  8   |  羊  |
|  9   |  猴  |
|  10  |  雞  |
|  11  |  狗  |
|  12  |  豬  |

# 範例

**特別號、全車、台號、特尾三**

```
{
    "bet": 3
}
```

**234星**

連碰

```
{
    "bet": ["01", "22", "23", "25"],
    "type": 0 
}
```

柱碰

```
{
    "bet": [
        ["01"], // 第一柱
        ["22", "23"], // 第二柱
        ["25"], // 第三柱
        ["08", "11", "42"] // 第四柱
    ],
    "type": 1
}
```

**天碰二、天碰三**

連碰

```
{
    "bet": ["01", "22", "23", "25"],
    "special": ["09", "33"],
    "type": 0 
}
```

柱碰

```
{
    "bet": [
        ["01"], // 第一柱
        ["22", "23"], // 第二柱
        ["25"], // 第三柱
        ["08", "11", "42"] // 第四柱
    ],
    "special": ["09", "33"],
    "type": 1
}
```

**組合/球色、生肖**

```
{
    "ball": 1,
    "bet": 1
}
```

### 3D娛樂城information和content

| 代號 | 名稱         |
| -------- | ---------------- |
| result_point | 會員結果(中獎 - 下注金額 + 退水) |

###  casino的information和content
| 代號 | 名稱         |
| -------- | ---------------- |
|gameMethod |遊戲玩法，gameType 為 98 or 99，此欄位 0 |
|tableType| 桌台類型，gameType 為 98 or 99，此欄位 0|
|tableId|遊戲局桌檯Id，gameType 為 98 or 99，此欄位為空白 |
|round| 局號，gameType 為 98 or 99，此欄位 0|
|run|輪號，gameType 為 98 or 99，此欄位 0 |
|gameResult|開牌結果，順序：[閒1,莊1,閒2,莊2,閒補,莊補]，若空的表示 無補牌，若 gameType 為 98 or 99 分別顯示對應的 Jackpot 獎項名稱 |
|betList|下注注區列表，gameType 為 98 or 99，此欄位為空白 |
|validAmount| 退水有效金額|
|refund| 退水設定值 0 ~ 150|
|checkoutAmount| 結帳金額，gameType 為 98 or 99相對應 Jackpot獎金|
|jackpotBonus| gameType 為 98 or 99 相對應 Jackpot獎金 |
|modifiedStatus|注單狀態 |
|createdAt| 結帳時間，gameType 為 98 or 99，此欄位為中獎時間|



### casino遊戲類別

| 遊戲名稱       | gameType |
| -------------- | -------- |
| 百家樂         | 1        |
| Jackpot Bonus  | 98       |
| Jackpot BigWin | 99       |

### casino遊戲玩法

| 名稱       | gameMethods |
| ---------- | ----------- |
| 百家樂標準 | 1           |
| 百家樂西洋 | 2           |
| 百家樂免水 | 3           |

### casino桌台類型

| 名稱       | tableType |
| ---------- | --------- |
| 標準百家樂 | 1         |
| 先發百家樂 | 2         |
| 極速百家樂 | 3         |
| 眯牌百家樂 | 4         |

### casino百家樂遊戲開牌代碼說明

| 牌例 | 對應    |
| ---- | ------- |
| S1   | 黑桃 A  |
| H0   | 紅心 10 |
| DJ   | 方塊 J  |
| C6   | 梅花 6  |

### casino注單狀態

| 狀態 | status |
| ---- | ------ |
| 0    | 正常   |
| 1    | 改單   |
| 2    | 取消單 |