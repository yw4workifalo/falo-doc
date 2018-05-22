<div id="top"></div>

# 彩票系統介接帳務系統API範本
● [新增平台商](#新增平台商)<br>
● [刪除平台商](#刪除平台商)<br>
● [更改密鑰 ](#更改密鑰)<br>
● [帳務報表](#帳務報表)<br><!--
● [產品總帳查詢](#產品總帳查詢)<br>-->
● [總會員出入明細](#總會員出入明細)<br>
● [會員注單查詢](#會員注單查詢)<br>
● [會員注單明細網址](#會員注單明細網址)<br>
● [遊戲開關查詢](#遊戲開關查詢)<br>
● [遊戲開關設定](#遊戲開關設定)<br>
● [會員等級範本查詢](#會員等級範本查詢)<br>
● [會員等級設定](#會員等級設定)<br>
● [會員等級查詢](#會員等級查詢)<br>
● [會員帳號模式設定](#會員帳號模式設定)<br>
● [會員帳號模式查詢](#會員帳號模式查詢)<br>
● [遊戲玩法查詢](#遊戲玩法查詢)<br>
● [在線人數](#在線人數)<br>

------
## <span>新增平台商</span>
   **API Name : platform**</br>
   **Method : POST**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 平台商帳號 | string | Y | 長度最短4，最長20 |
| name | 平台商名稱 | string | Y | 長度最短4，最長20 |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+name+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| publicKey | 平台公鑰	 | string  | 重新產生各代理商私鑰(遊戲系統產生) |
| privateKey | 平台私鑰 | string | 重新各代理商私鑰(遊戲系統產生) |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 8 | account already exist | 帳號已存在
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     POST /keno-api/agent/platform?
          account=ifaloAgent&
          name=法老代理商&
          key=3de5b29aac97c072f5823dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
             "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
             "privateKey":"3de5b29aac97c072f5822dc99c5637d6"
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
## <span>刪除平台商</span>
   **API Name : platform**</br>
   **Method : DELETE**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y | 依"＠"區隔多組key |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     DELETE /keno-api/agent/platform?
            publicKey=3de5b29aac97c072f5822dc99c5637d6＠4de5b29aac97c072f5822dc99c5637d6&
            key=3de5b29aac97c072f5823dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
## <span>更改密鑰</span>
   **API Name : modify-secret**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| privateKey | 平台商私鑰 | string | 重新產生平台商私鑰(遊戲系統產生) |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     PUT /keno-api/agent/modify-secret?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         key=3de5b29aac97c072f5823dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
           "privateKey":"0#deb&$$\*4c4843eb89&^4!b&683!b6c7"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
## <span>帳務報表</span>
   **API Name : platform-report-summary**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | N | 依"＠"區隔多組key |
| begin | 查詢區間起始 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| end | 查詢區間結束 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+begin+end+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| detail\ | 報表資訊 | array |  |
| detail\platformName | 平台帳號 | string |  |
| detail\publicKey | 平台筆公鑰 | string | 玩家依公鑰對應之平台 |
| detail\begin | 區間起始 | string | 格式：YYYY-MM-dd hh:mm:ss |
| detail\end | 區間結束 | string | 格式：YYYY-MM-dd hh:mm:ss |
| detail\currency | 幣別 | string |  |
| detail\lottery | 彩票編號 | string |  |
| detail\gameGroup | 玩法群組編號 | string |  |
| detail\tBetCount | 下注總筆數 | string |  |
| detail\tBet | 下注總額度 | string | 依幣別 |
| detail\tPL | 總輸贏 | string | "-"表示虧（虛貨） |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/platform-repor-summary?
         publicKey=3de5b29aac97c072f5822dc99c5637d6@3de5b29aac97c072f5822dc99c5637d6&
         begin=2017-10-31 00:00:00&
         end=2017-11-31 23:59:59&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
           "detail":[
               {
                 "platformName":"falo1",
                 "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
                 "begin":"2017-10-30 00:00:00",
                 "end":"2017-10-31 11:59:59",
                 "currency":"NTD",
                 "lottery":"10002",
                 "gameGroup":"104",
                 "tBetCount":"3000",
                 "tBet":"2000",
                 "tPL":"20000",
                 "uuid":"a6f5fe8f3cfa4abf8f12e903d27e9414"
               },
               {
                 "platformName":"falo2",
                 "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
                 "begin":"2017-10-29 00:00:00",
                 "end":"2017-10-30 11:59:59",
                 "currency":"USD",
                 "lottery":"10002",
                 "gameGroup":"104",
                 "tBetCount":"3000",
                 "tBet":"2000",
                 "tPL":"20000",
                 "uuid":"f56a62934dc44d6fa8ae5606923868e9"
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
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
<!--
## <span>產品總帳查詢</span>
   **API Name : platform-report**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y | 依"＠"區隔多組key |
| begin | 查詢區間起始 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| end | 查詢區間結束 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+begin+end+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| detail\ | 報表資訊 | array |  |
| detail\platformName | 平台帳號 | string |  |
| detail\publicKey | 平台筆公鑰 | string | 玩家依公鑰對應之平台 |
| detail\begin | 區間起始 | string | 格式：YYYY-MM-dd hh:mm:ss |
| detail\end | 區間結束 | string | 格式：YYYY-MM-dd hh:mm:ss |
| detail\currency | 幣別 | string |  |
| detail\tBetCount | 下注總筆數 | string |  |
| detail\tBet | 下注總額度 | string | 依幣別 |
| detail\tPL | 總輸贏 | string | "-"表示虧（虛貨） |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/platform-report?
         publicKey=3de5b29aac97c072f5822dc99c5637d6@3de5b29aac97c072f5822dc99c5637d6&
         begin=2017-10-31 00:00:00&
         end=2017-11-31 23:59:59&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
           "detail":[
               {
                 "platformName":"falo1",
                 "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
                 "begin":"2017-10-30 00:00:00",
                 "end":"2017-10-31 11:59:59",
                 "currency":"NTD",
                 "tBetCount":"3000",
                 "tBet":"2000",
                 "tPL":"20000",
                 "uuid":"a6f5fe8f3cfa4abf8f12e903d27e9414"
               },
               {
                 "platformName":"falo2",
                 "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
                 "begin":"2017-10-29 00:00:00",
                 "end":"2017-10-30 11:59:59",
                 "currency":"USD",
                 "tBetCount":"3000",
                 "tBet":"2000",
                 "tPL":"20000",
                 "uuid":"f56a62934dc44d6fa8ae5606923868e9"
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
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----
-->
## <span>總會員出入明細</span>
   **API Name : player-log-report**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y | 依"＠"區隔多組key |
| account | 玩家帳號 | string | Y | 空白表示查全部 |
| begin | 查詢區間起始 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| end | 查詢區間結束 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| page | 頁數 | string | Y | 空白代表第1頁 |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+begin+end+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| totalPage | 全部頁數 | string |  |
| totalCount | 總筆數 | string |  |
| totalCalc | 總盈虧 | string |  |
| detail | 出入明細 | string |  |
| detail\publicKey | 平台公鑰 | string | 玩家依公鑰對應之平台 |
| detail\playerAccount | 玩家帳號 | string |  |
| detail\nickname | 玩家暱稱 | string |  |
| detail\currency | 幣別 | string |  |
| detail\loginTime | 登入時間 | string | 格式：YYYY-MM-dd hh:mm:ss |
| detail\logoutTime | 登出時間 | string | 格式：YYYY-MM-dd hh:mm:ss |
| detail\ain | 登入後餘額 | string |  |
| detail\bout | 登出後餘 | string |  |
| detail\calc | 盈虧 | string | 負號（"-"）表示虧損 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/player-log-report?
         publicKey=3de5b29aac97c072f5822dc99c5637d6@3de5b29aac97c072f5822dc99c5637d6&
         begin=2017-10-31 00:00:00&
         end=2017-11-31 23:59:59&
         page=1&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
           "totalPage":"200",
           "totalCount":"10000",
           "totalCalc":"-20000"
           "detail":[
               {
                 "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
                 "playerAccount":"ifalo001",
                 "currency":"NTD",
                 "loginTime":"2017-10-31 13:27:46",
                 "logoutTime":"2017-11-31 13:27:46",
                 "ain":"2000",
                 "bout":"0",
                 "calc":"-2000"
               },
               {
                 "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
                 "playerAccount":"ifalo001",
                 "currency":"NTD",
                 "loginTime":"2017-11-31 13:27:46",
                 "logoutTime":"2017-12-31 13:27:46",
                 "ain":"1000",
                 "bout":"0",
                 "calc":"-1000"
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
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
## <span>會員注單查詢</span>
   **API Name : player-bet-report**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| player | 玩家帳號 | string | Y | 依","區隔多組，不填表示全部 |
| begin | 查詢區間開始 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| end | 查詢區間結束 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| page | 頁數 | string | N | 空白代表第1頁 |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+player+begin+end+page+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| totalPage | 總頁數 | string |  |
| detail\ | 報表資訊 | array |  |
| detail\publicKey | 平台公鑰 | string |  |
| detail\platformName | 平台帳號 | string |  |
| detail\player | 玩家帳號 | string |  |
| detail\currency | 幣別 | string |  |
| detail\tBetCount | 下注總筆數 | string |  |
| detail\tBet | 下注總額度 | string |  |
| detail\benefit | 輸贏 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/player-bet-report?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         player=ifalo001,ifalo002&
         begin=2017-10-31 00:00:00&
         end=2017-11-31 23:59:59&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
           "totalPage":"10",
           "detail":[
           {
              "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
              "platformName":"ifalo",
              "player":"ifalo001",
              "currency":"NTD",
              "tBetCount":"25",
              "tBet":"2500",
              "benefit":"-25"
          },
          {
              "publicKey":"3de5b29aac97c072f5822dc99c5637d6",
              "platformName":"ifalo",
              "player":"ifalo002",
              "currency":"USD",
              "tBetCount":"30",
              "tBet":"1500",
              "benefit":"250"
          },
          {
              "publicKey":"4de5b29aac97c072f5822dc88c5637d7",
              "platformName":"pha",
              "player":"pha002",
              "currency":"USD",
              "tBetCount":"40",
              "tBet":"3500",
              "benefit":"-1000"
          }
          ]
         }
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
## <span>會員注單明細網址</span>
   **API Name : player-report**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| player | 玩家帳號 | string | Y |  |
| begin | 查詢區間開始 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| end | 查詢區間結束 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+player+begin+end+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| url | 連結網址 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/player-report?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         player=ifalo001&
         begin=2017-10-31 00:00:00&
         end=2017-11-31 23:59:59&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
           "url":"http://xxx.xxx.xxx/xxx"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------

## <span>遊戲開關查詢</span>
   **API Name : game-mode**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| mode | [遊戲狀態](#遊戲狀態) | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/game-mode?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
       "status":"success",
       "data": {
         "mode":"1"
       },
       "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
## <span>遊戲開關設定</span>
   **API Name : game-mode**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| mode | [遊戲狀態](#遊戲狀態) | string | Y |  |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+mode+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| mode | [遊戲狀態](#遊戲狀態) | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     PUT /keno-api/agent/game-mode?
         mode=1&
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
           "mode":"1"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----
## <span>會員等級範本查詢</span>
   **API Name : stake-limit-list**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| \exampleType | 範本類別 | string |  |
| \defaultBet | 底注 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/stake-limit-list?
     hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": [
           {
               "exampleType":"A",
               "defaultBet":"2"
           },
           {
               "exampleType":"B",
               "defaultBet":"5"
           },
           {
               "exampleType":"C",
               "defaultBet":"10"
           }
         ],
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
## <span>會員等級設定</span>
   **API Name : stake-limit**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| account | 玩家帳號 | string | Y |  |
| exampleType | 注區範本 | string | Y |  |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+account+exampleType+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號 | string |  |
| exampleType | 範本類別 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     PUT /keno-api/agent/stake-limit?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         account=ifalo001&
         exampleType=A&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "exampleType":"A"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----

## <span>會員等級查詢</span>
   **API Name : stake-limit**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| account | 玩家帳號 | string | Y |  |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+account+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號 | string |  |
| exampleType | 範本類別 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/stake-limit?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         account=ifalo001&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "exampleType":"A"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----

## <span>會員帳號模式設定</span>
   **API Name : mode**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| mode | 模式 | string | Y | 0:正常，1:鎖單無法下注，2:封鎖無法登入，並踢除其餘玩家 |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+account+mode+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| \account | 玩家帳號 | string |  |
| \mode | 模式 | string | 0:正常，1:鎖單無法下注，2:封鎖無法登入，並踢除其餘玩家 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     PUT /keno-api/agent/mode?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         account=ifalo001,ifalo002&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": [
           {
              "account":"ifalo001",
              "mode":"0"
           },
           {
              "account":"ifalo002",
              "mode":"1"
           },
         ],
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----
## <span>會員帳號模式查詢</span>
   **API Name : mode**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+account+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| \account | 玩家帳號 | string |  |
| \mode | 模式 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/mode?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         account=ifalo001,ifalo002&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": [
           {
              "account":"ifalo001",
              "mode":"0"
           },
           {
              "account":"ifalo002",
              "mode":"1"
           },
         ],
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----
## <span>遊戲玩法查詢</span>
   **API Name : gameList**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| \ | 列表 | array |  |
| \lotteryNo | 彩種編號 | string |  |
| \lotteryName | 彩種名稱 | string |  |
| \lotteryGame | 玩法列表 | array |  |
| \lotteryGame\groupName | 玩法群組名稱 | array |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/gameList?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": [
           {
            "lotteryNo":"10001",
            "lotteryName":"重慶時時彩",
            "lotteryGame":{
              "100":"五星直選複式",
              "200":"三星直選複式"
             }
           },
           {
            "lotteryNo":"10002",
            "lotteryName":"北京PK10時時彩",
            "lotteryGame":{
              "100":"五星直選複式",
              "200":"三星直選複式"
             }
           }
         ],
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----
## <span>在線人數</span>
   **API Name : onlineUsers**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y | 依"＠"區隔多組key |
| now | 頁數 | string | Y |  |
| page| 單頁幾筆 | string | Y | 預設100 |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| \detail | 列表 | array |  |
| \detail\account | 帳號 | string |  |
| \detail\nickName | 暱稱 | string |  |
| \detail\PlatformName | 平台名稱 | string |  |
| \detail\gameName | 遊戲 | string | 空白表示不在任何遊戲上面 |
| \detail\nowChip  | 現在籌碼 | string |  |
| \detail\Machine  | 機台 | string | 空白表示無機台,如彩票,黃金期權應是空白|
| totalPage | 總頁數 | string |  |
| nowPage  | 現在頁數 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | {parameter} not found | 欄位參數值無效
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 15 | data format error  | 資料格式錯誤

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/onlineUsers?publicKey=3de5b29aac97c072f5822dc99c5637d6&    hash=26f6b1074e1c9e80e9b613bf79a923a6&key=3de5b29aac97c072f5822dc99c5637d6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": [
         detail[
           {
            "account":"linand",
            "nickName":"鋼鐵人",
            "PlatformName":"現金網",
            "gameName":"北京賽車",
            "nowChip":"1000000.0000",
            "Machine":""
           },
           {
            "account":"linandag",
            "nickName":"雷神索爾",
            "PlatformName":"現金網",
            "gameName":"重慶時時彩",
            "nowChip":"1100000.0000",
            "Machine":""
           }],
           totalPage : "1000",
           nowPage :"2"
         ],
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
        "status":"error",
         "error": {
           "code":7,
           "message":"internal server error"
         },
         "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

-----

## <span>附表</span>
### <span>支援貨幣</span>
| 代碼 | 說明 |
|--|----|
| TWD | 台幣 |
| CNY | 人民幣 |
| USD | 美金 |

### <span>遊戲狀態</span>
| 遊戲狀態 | 說明 |
|--|----|
| 0 | 關閉 |
| 1 | 開啟 |
| 2 | 維護 |
