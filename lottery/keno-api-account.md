<div id="top"></div>

# 介接帳務系統API範本
● [遊戲總帳與產品所有的明細](#遊戲總帳與產品所有的明細)<br>
● [會員個人下注明細網址](#會員個人下注明細網址)<br>
● [遊戲開關查詢](#遊戲開關查詢)<br>
● [遊戲開關設定](#遊戲開關設定)<br>
● [會員等級範本查詢](#會員等級範本查詢)<br>
● [會員等級查詢](#會員等級查詢)<br>
● [額度轉出入明細](#額度轉出入明細)<br>

<div align="right"><a href="#top">Top</a></div>

------
## <span>遊戲總帳與產品所有的明細</span>
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
| detail\lottery | 主玩法 | string | 彩票可能是彩種編號,其他遊戲依自己的主玩法 |
| detail\gameGroup | 次玩法 | string | 彩票可能是玩法群組編號,其他遊戲如果沒有空白表示  |
| detail\tBetCount | 下注總筆數 | string |  |
| detail\aBet | 下注總額度 | string | 依幣別 |
| detail\tBet | 下注有效總額度 | string | 依幣別 |
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
                 "aBet":"2000",
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
                 "aBet":"2000",
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
## <span>會員個人下注明細網址</span>
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
| \detail\nowBalance |會員目前真實額度|string| |
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
            "Message":"",
            "nowBalance":"110000.0000"
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


