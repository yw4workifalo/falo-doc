<div id="top"></div>

# 介接帳務系統API範本
● [帳務報表](#帳務報表)<br>
● [總會員出入明細](#總會員出入明細)<br>
● [會員注單查詢](#會員注單查詢)<br>
● [會員注單明細網址](#會員注單明細網址)<br>
● [遊戲開關查詢](#遊戲開關查詢)<br>
● [遊戲開關設定](#遊戲開關設定)<br>
● [會員等級範本查詢](#會員等級範本查詢)<br>
● [會員等級設定](#會員等級設定)<br>
● [會員等級查詢](#會員等級查詢)<br>
● [額度轉出入明細](#額度轉出入明細)<br>
● [會員額度查詢](#會員額度查詢)<br>
● [查詢玩家](#查詢玩家)<br>
● [會員盤口退水限注查詢](#會員盤口退水限注查詢)<br>
● [會員盤口查詢](#會員盤口查詢)<br>
● [會員盤口設定](#會員盤口設定)<br>

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
## <span>額度轉出入明細</span>
   **API Name : amounttransferred**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y | 依"＠"區隔多組key |
| account   | 會員名稱 | string | N | 帶入查此會員,無帶入查publicKey全部會員 |
| begin | 查詢區間起始 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
| end | 查詢區間結束 | string | Y | 格式：YYYY-MM-dd hh:mm:ss |
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
| \detail\RechargeBalance | 充值提取前餘額 | string |  |
| \detail\Recharge  | 充值提取(正充值,負數提取) | string |  |
| \detail\finalCredit  | 充值提取後餘額 | string | |
| \detail\Status  | 充值狀態 | string |成功與否或是其他 |
| \detail\CreateTime |轉出入時間 | string | |
| \detail\Message |其他備注|string| 無則空白|
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
     GET /keno-api/agent/amounttransferred?publicKey=3de5b29aac97c072f5822dc99c5637d6&    hash=26f6b1074e1c9e80e9b613bf79a923a6&key=3de5b29aac97c072f5822dc99c5637d6
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
            "RechargeBalance":"100000.0000",
            "Recharge":"+10000.0000",
            "finalCredit":"110000.0000",
            "Status":"成功",
            "CreateTime":"2018-06-14 11:36:00",
            "Message":""
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
## <span>會員額度查詢</span>
   **API Name : memberquotaquery**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y | 依"＠"區隔多組key |
| account   | 會員名稱 | string | N | 帶入查此會員,無帶入查publicKey全部會員 |
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
| \detail\Balance  | 餘額 | string | |
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
     GET /keno-api/agent/memberquotaquery?publicKey=3de5b29aac97c072f5822dc99c5637d6&    hash=26f6b1074e1c9e80e9b613bf79a923a6&key=3de5b29aac97c072f5822dc99c5637d6
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
            "Balance":"100000.0000"
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
## <span>查詢玩家</span>
   **API Name : info**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 長度最短4，最長20 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |
| nickname | 玩家暱稱 | string |
| credit | 玩家餘額 | float | 位數格式:(19,4) ex:1000.0001 |
| enable | 是否啟用 | int | 凍結狀態(0:正常，1:鎖單無法下注，2:封鎖無法登入) |
| limitWin | 限贏 | float | 0表示無限制 |
| limitLose | 限輸 | float | 0表示無限制 |
| isOnline | 是否在線 | boolean | 玩家是否在線 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| --| ---- | ----------- |
| 1 | {parameter} is required  | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 4 | player not found | 玩家未找到 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |

   ### 範例
   + 調用方法
     ```
     GET /keno-api/player/info?
     account=ifalo001&
     key=3de5b29aac97c072f5222dc99c5637d6&
     hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```
   + 成功
     ```javascript
     {
        "status":"success",
        "data":
        {
          "account":"ifalo001",
          "nickname":"1號測試帳號",
          "credit":"1000000.0000",
          "enable":0,
          "limitWin":0,
          "limitLose":0,
	      "isOnline":"true"
        }
        "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
       "status":"error",
       "error": {
         "code":15,
         "message":"data format error"
       },
       "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------

## <span>會員盤口退水限注查詢</span>
   **API Name : classList**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| detail\ | 盤口退水與限注設定 | array |格式依各遊戲需求改變|
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| --| ---- | ----------- |
| 1 | {parameter} is required  | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |

   ### 範例
   + 調用方法
     ```
     GET /keno-api/agent/classList?key=3de5b29aac97c072f5222dc99c5637d6&
     hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```
   + 成功
     ```javascript
     {
        "status":"success",
        "data":
        {
         "detail":array,
        }
        "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```

   + 失敗
     ```javascript
     {
       "status":"error",
       "error": {
         "code":15,
         "message":"data format error"
       },
       "uuquid":"e6f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------
## <span>會員盤口設定</span>
   **API Name : class**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| publicKey | 平台公鑰 | string | Y |  |
| account | 玩家帳號 | string | Y |  |
| class | 盤口範本 | string | Y |  |
| key | 公鑰 | string | Y | 專屬公鑰 |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(publicKey+account+class+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號 | string |  |
| class | 盤口範本 | string |  |
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
     PUT /keno-api/player/class?
         publicKey=3de5b29aac97c072f5822dc99c5637d6&
         account=ifalo001&
         class=A&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "class":"A"
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

## <span>會員盤口查詢</span>
   **API Name : classMember**</br>
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
| ClassPath | 盤口範本 | string |  |
|ClassList | 盤口內容 | array|依各家格式|
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
     GET /keno-api/player/classMember?
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
            "ClassPath":"A",
            "ClassList":array,
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

### <span>盤口範本</span>
| 盤口 | 說明 |
|--|----|
| A | A盤口 |
| B | B盤口 |
| C | C盤口 |
| D | D盤口 |

