<div id="top"></div>

# 彩票系統介接API範本
● [註冊玩家帳號](#註冊玩家帳號)<br>
● [取得登入網址](#取得登入網址)<br> <!--
● [取得測試帳號登入網址](#取得測試帳號登入網址)<br>-->
● [修改玩家帳號暱稱](#修改玩家帳號暱稱)<br>
● [查詢玩家](#查詢玩家)<br>
● [查詢玩家下注紀錄](#查詢玩家下注紀錄)<br>
● [查詢玩家下注簡報總額](#查詢玩家下注簡報總額)<br>
● [額度轉出入](#額度轉出入)<br>
● [額度轉出入狀態](#額度轉出入狀態)<br>
● [玩家踢線](#玩家踢線)<br>
● [設定玩家限輸](#設定玩家限輸)<br>
● [設定玩家限贏](#設定玩家限贏)<br>
● [玩家限注回復](#玩家限注回復)<br>
● [玩家限注查詢](#玩家限注查詢)<br>
● [玩家信用額度回復](#玩家信用額度回復)<br>
● [玩家信用額度設定](#玩家信用額度設定)<br>
● [玩家信用額度查詢](#玩家信用額度查詢)<br>
● [群組信用額度回復](#群組信用額度回復)<br>
● [群組信用額度設定](#群組信用額度設定)<br>
● [群組信用額度查詢](#群組信用額度查詢)<br>
● [設定玩家帳號模式](#設定玩家帳號模式)<br>
● [查詢玩家帳號模式](#查詢玩家帳號模式)<br>
● [設定玩家佔成](#設定玩家佔成)<br>
● [查詢玩家佔成](#查詢玩家佔成)<br>
● [查詢注區範本](#查詢注區範本)<br><!--
● [設定平台注區範本](#設定平台注區範本)<br>
● [查詢平台注區範本](#查詢平台注區範本)<br>-->
● [設定玩家注區範本](#設定玩家注區範本)<br>
● [查詢玩家注區範本](#查詢玩家注區範本)<br>
● [修改退水值](#修改退水值)<br>
● [查詢退水值](#查詢退水值)<br><!--
● [設定代理商賠率](#設定代理商賠率)<br>-->
● [設定玩家盤口](#設定玩家盤口)<br>
● [查詢平台盤口與各盤口退水及限注規範](#查詢平台盤口與各盤口退水及限注規範)<br>
● [手機串接](#手機串接)<br>

------
## <span>註冊玩家帳號</span>
   **API Name : register**</br>
   **Method : POST**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
|----|----|----|----|----|
| account | 帳號 | string | Y | 長度最短4，最長20 |
| nickname | 玩家暱稱 | string | Y | 長度最短4，最長20 |
| currency | 貨幣 | string | N | [玩家貨幣類型](#支援貨幣)，不指定以代理商為準 |
| limitwin | 限贏 | string | Y | 0表示無限制 |
| limitlose | 限輸 | string | Y | 0表示無限制 |
| memberType | 帳務類型 | string | N | 1:現金制、2:信用制 沒輸入預設是2:信用制|
| groupId | 回復類型 | string | N | 0:無、1:日回復、2:週回復 不指定預設是0:無|
| classpath|盤口設定 | string | N| [玩家盤口設定](#盤口設定) |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+nickname+limitwin+limitlose+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
|--|----|----------|--|
| account | 玩家帳號  | string  | |
| nickname | 玩家暱稱 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | -- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位
   | 2 | invalid key  | 金鑰無效
   | 4 | player not found | 玩家未找到
   | 5 | method is not allowed  | 使用之Http方法不允許
   | 6 | function not found  | API不存在
   | 7 | internal server error  | 服務器內部錯誤
   | 8 | account already exist | 帳號已存在
   | 15 | data format error  | 服務器內部錯誤
   | 22 | quota_is_error | 玩家註冊限額大於上層
   | 46 | Not_a_credit_member_set | 現金制無法設定回復機制

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/register?
          account=ifalo001&
          nickname=ifalo001&
          currency= NTD&
          maxbetlimit＝100000&
          limitwin＝0&
          limitlose=0&
          key=3de5b29aac97c072f5823dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data": {
            "account":"ifalo001",
            "nickname":"1號測試帳號"
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
   **API Name : auth**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 長度最短4，最長20 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |
   #### **`  hash = md5(account+privateKey) `**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| --| ---- | ----------- | -- |
| account | 玩家帳號 | string |
| login | 登入網址 | string |
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
| 15 | data format error  | 服務器內部錯誤 |
| 25 | locked_and_can_not_login | 帳號或上層被鎖 |
| 52 | Trial_account_expired | 試玩帳號限期已過 |

   ### 範例
   + 調用方法
     ```
     GET /keno-api/player/auth?
     account=ifalo001&
     key=3de5b29aac97c072f5823dc99c5637d6&
     hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
       "status":"success",
       "data": {
       "login":"https://keno.com/keno-game/player/login?account=ifalo001&key=K0hvSzZvbzlPbHoy"
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
## <span>取得測試帳號登入網址</span>
   **API Name : authtest**</br>
   **Method : POST**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |
   #### **`  hash = md5(privateKey) `**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| --| ---- | ----------- | -- |
| account | 會員帳號 | string |
| login | 登入網址 | string |
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
| 15 | data format error  | 服務器內部錯誤 |
| 25 | locked_and_can_not_login | 帳號或上層被鎖 |
| 52 | Trial_account_expired | 試玩帳號限期已過 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/authtest?
     key=3de5b29aac97c072f5823dc99c5637d6&
     hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
       "status":"success",
       "data": {
          "login":"https://keno.com/keno-game/player/login?account=ifalo001&key=K0hvSzZvbzFZGdwVDVmSGliSE5EMloxS3dEM3c9PQ=="
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
------
-->
## <span>修改玩家帳號暱稱</span>
   **API Name : nickname**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 長度最短4，最長20 |
| nickname | 玩家暱稱 | string | Y | 長度最短4，最長20 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+nickname+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | -- | ---- | ----------- | -- |
   | account | 玩家帳號 | string |
   | nickname | 玩家暱稱 | |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |


   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
| 1 | {parameter} is required  | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 4 | player not found | 玩家未找到 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |
| 31 | account_not_exist | 帳號不存在 |
| 32 | prohibited to modify their own members | 禁止修改非自己的玩家 |

  ###範例
  + 調用方法
    ```
    PUT /keno-api/player/nickname?
         account=ifalo001&
         nickname=ifaloxxx&
         key=3de5b29aac93c072f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```
  + 成功
    ```javascript
    {
        "status":"success",
        "data": {
           "account":"ifalo001",
           "nickname":"1號測試帳號"
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

## <span>查詢玩家下注紀錄</span>
   **API Name : report**</br>
   **Method : GET**
   * 一頁至多輸出1000筆
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | N | 傳遞空值,為查詢區間段所有下注玩家的注單 |
| startAt | 開始時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
| endAt | 結束時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
| now | 第?頁 | string | N | 空值代表第1頁 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+startAt+endAt+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| --| ---- | ----------- | -- |
| count | 下注筆數 | string | |
| totalPage | 總頁數 | string | |
| historyList | 歷史紀錄 | array | |
| historyList \ orderID | 下注單號 | string | |
| historyList \ memberUID | 玩家編號 | string | |
| historyList \ memberAccount | 玩家帳號 | string | |
| historyList \ agentUID | 代理商編號 | string | |
| historyList \ agentName | 代理商名稱 | string | |
| historyList \ lotteryName | 彩票名稱 | string | |
| historyList \ drawingNo | 開獎期別 | string | |
| historyList \ groupName | 玩法群組名稱 | string | |
| historyList \ gameName | 玩法名稱 | string | |
| historyList \ betData | 下注號碼 | string | |
| historyList \ betNumber | 注數 | int | |
| historyList \ odd | 賠率 | string | |
| historyList \ multiple | 倍數 | string | |
| historyList \ betAmount | 下注總額 | string | 位數格式:(19,2) ex:1000.01|
| historyList \ validbetAmount | 有效下注總額 | string | 位數格式:(19,2) ex:1000.01,當為撤單或是和局時,此總額為0,表示不計算|
| historyList \ winBonus | 中獎金額 | string | 位數格式:(19,2) ex:1000.01 |
| historyList \ refundFee | 退水 | string | 位數格式:(19,2) ex:1000.01 |
| historyList \ ip | 下注IP | string | |
| historyList \ betStatus | 注單狀態 | string | [注單狀態](#注單狀態) |
| historyList \ currency | 貨幣 | string | |
| historyList \ balance | 餘額 | string | 位數格式:(19,2) ex:1000.01 |
| historyList \ betTime | 下注時間 | string | |
| historyList \ updateTime | 更新時間(開獎時間) | string | |
| historyList \ drawingNumber | 開獎號碼 | string | |
| historyList \ defaultBet | 底注 | string | 位數格式:(19,2) ex:1000.01 |
| historyList \ percent | 佔成 | string | 位數格式:(19,2) ex:1000.01 |
| historyList \ billingDate | 歸帳日 |string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| :--| ---- | ----------- |
| 1 | {parameter} is required  | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 4 | player not found | 玩家未找到 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |
| 33 | time is over limit | 日期相隔不得大於7天 |
| 34 | the start date is greater than the end time | 開始日期不能大於結束日期 |

   ### 範例
   + 調用方法
     ```
     GET /keno-api/player/report?
          account=ifalo001&
          startAt=2017-09-01 00:00:00&
          endAt=2017-09-04 14:00:00&
          now=1&
          key=3de5b29aac97c072f5821dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```
   + 成功
     ```javascript
     {
         "status":"success",
         "data":{
           "count":"1",
           "historyList":[
               {
                 "orderID":"1408203561",
                 "memberUID":"124113565",
                 "memberAccount":"ifalo006",
                 "agentUID":"728299597",
                 "agentName":"ifalo",
                 "lotteryName":"天津時時彩",
                 "drawingNo":"20180206056",
                 "groupName":"五星玩法",
                 "gameName":"五星單式",
                 "betData":"1,5,9,8,7",
                 "betNumber":"1",
                 "odd":"1.99",
                 "multiple":"100",
                 "betAmount":"200.0000",
                 "validbetAmount":"200.0000",
                 "winBonus":"0.0000",
                 "ip":"210.71.170.48",
                 "betStatus":"1",
                 "currency":"台幣",
                 "balance":"9092680.0800",
                 "betTime":"2017-08-25 16:31:24",
                 "updateTime":"2017-08-25 16:35:24",
                 "drawingNumber":"1,2,3,4,5",
                 "defaultBet":"2",
                 "percent":"1",
                 "billingDate" :"2017-08-26"  
               }
           ],
           "uuquid":"01212c3b9e1eac371776a8e932289906"
         }
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

## <span>查詢玩家下注簡報總額</span>
   **API Name : report-multiple**</br>
   **Method : GET**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | account | 玩家帳號 | string | N | 空白表示無區分玩家區間總額
   | startAt | 開始時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
   | endAt | 結束時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(startAt+endAt+privateKey)`**

   ### 輸出參數(有帳號)
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |
| agentUID | 代理商編號 | string |
| agentName | 代理商名稱 | string |
   | currency | 貨幣 | string | [玩家貨幣類型](#支援貨幣)
   | betNumber | 總注數 | string |
   | betAmount | 總下注額 | string | 位數限制:(19,2) ex:1000.01
   | winBonus | 輸贏總金額 | string | 位數限制:(19,2) ex:1000.01
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 輸出參數(無帳號)
   | 參數名稱 | 參數說明 | 參數型態 | 說明 |
   | -- | ---- | ----------- | -- |
   | currency | 貨幣 | string | [玩家貨幣類型](#支援貨幣)
   | betNumber | 總注數 | string |
   | betAmount | 總下注額 | string | 位數限制:(19,2) ex:1000.01
   | winBonus | 輸贏總金額 | string | 位數限制:(19,2) ex:1000.01
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
| 1 | {parameter} is required | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 4 | player not found | 玩家未找到 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |
| 33 | time is over limit | 日期相隔不得大於7天 |
| 34 | the start date is greater than the end time | 開始日期不能大於結束日期 |

   ### 範例
   + 調用方法
     ```
     GET /keno-api/player/report-multiple?
          account=ifalo001&
          startAt=2017-09-01 00:00:00&
          endAt=2017-09-04 14:00:00&
          key=3de5b21aac97c072f5822dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```
   + 成功
     ```javascript
     {
        "status":"success",
        "data":{
            "account":"ifalo001",
            "agentUID":"279848181",
            "agentName":"ifalo",
            "currency":"NTD",
            "betNumber":"100",
            "betAmount":"15000.0018",
            "winBonus":"-10000.0018"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>額度轉出入</span>
   **API Name : transfer**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| --| ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | |
| credit | 轉出入額度 | integer | Y | 位數限制:±(10) ex:+1000<br>正數為轉入，負數為轉出 |
| orderNo | 平台交易編號 | string | Y | 介接平台產生，核對帳務 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+credit+orderNo+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | -- | ---- | ----------- | -- |
   | transactionNo | 交易編號 | string | |
   | time | 交易時間 | string | |
   | orginalCredit | 交易前額度 | string | |
   | transferCredit | 交易額度 | string | |
   | finalCredit | 交易後額度 | string | |
   | currency | 貨幣 | string | [玩家貨幣類型](#支援貨幣) |
   | status | [交易狀態](#交易狀態) | string | |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 | |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
| 1 | {parameter} is required | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 4 | player not found | 玩家未找到 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |

   ### 範例
   + 調用方法
     ```
     PUT /keno-api/player/transfer?
          account=ifalo001&
          credit=10000&
          key=3de5b29aac97c072f5222dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```
   + 成功
     ```javascript
     {
        "status":"success",
        "data":{
          "transactionNo":"5943a950a3227",
          "time":"2017/06/12 15:50:00",
	      "orginalCredit":"+1000.0005",
          "transferCredit":"+500.0005",
          "finalCredit":"+1500.0005",
          "message":"交易成功"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>額度轉出入狀態</span>
   **API Name : transfer-status**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| transactionNo | 交易編號 | string | Y |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(transactionNo+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | -- | ---- | ----------- | -- |
   | account | 玩家帳號 | string |
   | orginalCredit | 交易前額度 | string |
   | finalCredit | 交易後額度 | string |
   | addedCredit | 新增額度 | string |
   | transactionNo | 遊戲交易編號 | 遊戲平台產生 |
   | orderNo | 平台交易編號 | 介接平台產生 |
   | status | [交易狀態](#交易狀態) | string |
   | time | 交易時間 | string
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   錯誤碼 | 錯誤訊息 | 錯誤說明 |
   |--| ---- | ----------- |
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
     GET /keno-api/player/transfer-status?
          transactionNo=59a8c0c3198cd&
          key=3de5b29aac97c072f5822dc99c5137d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
          "status":"success",
          "data":{
              "account":"ifalo001",
              "orginalCredit":"26802693.9150"
              "addedCredit":"-100.0000"
              "finalCredit":"26802593.9150",
              "transactionNo":"59a8cddcd71d9",
              "orderNo":"xxxxxxxxxxxxx",
              "status":"0"
              "time":"2017-09-01 15:09:00"
          },
          "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>玩家踢線</span>
   **API Name : kick-multiple**</br>
   **Method : DELETE**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | -- | ---- | ----------- | -- |
   | account | 玩家帳號 | string |
   | status | 處理狀況 | string | 0:成功，1:玩家不在線，2:非該代理玩家 |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
     DELETE /keno-api/player/kick-multiple?
          account=ifalo001,ifalo002&
          key=3de5b29a2c97c072f5822dc99c5637d6&
          hash=6f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
          "status":"success",
          "data":[
            {
              "account":"ifalo001",
              "status":"0"
            },
            {
              "account":"ifalo002",
              "status":"-1"
            }
          ],
          "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>設定玩家限輸</span>
   **API Name : lose**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | |
| limit | 限輸額度 | string | Y | 整數格式，0表示無限制 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **`hash = md5(account+limit+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string | |
| limit | 限輸額度 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/lose?
          account=ifalo001&
          limit=0&
          key=3de5b29aac97c072f5822dc99c5337d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
          "status":"success",
          "data":{
              "account":"ifalo001",
              "limit":"10000000"
          }
          "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>設定玩家限贏</span>
   **API Name : win**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | |
| limit | 限贏額度 | string | Y | 整數格式，0表示無限制 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **`hash = md5(account+limit+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string | |
| limit | 限贏額度 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/win?
          account=ifalo001&
          limit=0&
          key=3de5b29aac97c072f5822dc99c5337d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
          "status":"success",
          "data":
          {
              "account":"ifalo001",
              "limit":"0"
          },
          "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>玩家限注回復</span>
   **API Name : recover**</br>
   **Method : PUT**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個代理用","隔開 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   **`hash = md5(account+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string | 多個代理用","隔開 |
| status | 玩家帳號 | string | 回復狀態，1：成功、0：失敗 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
     PUT /keno-api/player/recover?
           account=ifalo001,ifalo002&
           key=3de5b29aac97c072f5822dc9925637d6&
           hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
      {
         "status":"success",
         "data":
            {
                "account":"ifalo001",
                "status":"1"
            },
            {
                "account":"ifalo002",
                "status":"1"
            },
         ]
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
         "uuquid":"e1f3414056fcbd57c24d5289acee1b8f"
     }
     ```
<div align="right"><a href="#top">Top</a></div>

------

## <span>玩家限注查詢</span>
   **API Name : limit**</br>
   **Method : GET**
   ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

   #### **`hash = md5(account+privateKey)`**

   ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |
| winCredit | 目前輸贏 | string |
| winLimit | 限贏 | string |
| loseLimit | 限輸 | string |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
     GET /keno-api/player/limit?
          account=ifalo001,ifalo002&
          key=3de5b29aac97c272f5822dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
         "status":"success",
         "data":[
            {
              "account":"ifalo001",
              "winCredit":"1000.0000",
              "winLimit":"1000000",
              "loseLimit":"1000000"
            },
            {
              "account":"ifalo002",
              "winCredit":"-1000.0000",
              "winLimit":"0",
              "loseLimit":"0"
            },
        ]
         "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>玩家信用額度回復</span>
  **API Name : reset**</br>
  **Method : PUT**</br>
  + 限信用平台使用
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| callback | 執行完後通知平台 url	| string |  |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(account+callback+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |
| status | 回復狀態 | string | 回復狀態，1：成功、0：失敗 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
    PUT /keno-api/player/reset?
         account=ifalo001,ifalo002&
         callback=http://www.platform.com/api
         key=3de5b29aac97c272f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":[
            {
              "account":"ifalo001",
              "status":"1"
            },
            {
              "account":"ifalo001",
              "status":"0"
            }
        ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>玩家信用額度設定</span>
  **API Name : credit**</br>
  **Method : PUT**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| creditLimit | 最高額度 | string | Y | |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **` hash = md5(account+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string | |
| status | 設定狀態 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |



#### 玩家設定結果

| 狀態代碼 | status           |
| -------- | ---------------- |
| 0        | Success          |
| -1       | Player Not Found |
| -2       | Not Enough Credit|

  ### 錯誤碼

| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
| 1 | {parameter} is required  | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |
| 55 | API limit the number of pens, more than the maximum number of pens                   |API限制筆數,超過最大筆數 |
| 58 | Non own membership | 非自己所屬會員 |
| 46 |Not a credit member |不是信用制會員,無法回復或設定 |

  ### 範例
  + 調用方法
    ```
    PUT /keno-api/player/credit?
          account=ifalo001&
          creditLimit=100000&
          key=3de5b29aac97c072f5822dc99c5633d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "credit":"100000",
        "data":[
          {
             "account":"ifalo001",
             "status":"0"
          }
        ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>玩家信用額度查詢</span>
  **API Name : credit**</br>
  **Method : GET**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
  | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生)
  | hash | 驗證參數 | string | Y | md5

  #### **` hash = md5(account+privateKey)`**

  ### 輸出參數
  | 參數名稱 | 參數說明 | 參數型態 | 說明
  | -- | ---- | ----------- | -- |
  | account | 玩家帳號 | string | |
  | creditLimit | 最高額度 | string | |
  | currentCredit | 剩餘信用額度 | string ||
  | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
  | 錯誤碼 | 錯誤訊息 | 錯誤說明
  | -- | ---- | ----------- |
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
    GET /keno-api/player/credit?
        account=ifalo001,ifalo002&
        key=3de5b29aac97c072f5822dc99c5633d6&
        hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
      "status":"success",
      "data":[
        {
           "account":"ifalo001",
           "creditLimit":"100000"
        },
        {
           "account":"ifalo002",
           "creditLimit":"200000"
        }
      ],
      "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>群組信用額度回復</span>
  **API Name : credit-reset-group**</br>
  **Method : POST**</br>
  + 限信用平台使用
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| groupId | 群組類型 | string | Y | 1:日回復、2:周回復 |
| callback | 執行完後通知平台 url	| string |  |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(groupId+callback+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| groupId | 群組類型 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
    POST /keno-api/player/credit-reset-group?
         groupId=1&
         callback=http://www.platform.com/api
         key=3de5b29aac97c272f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{"groupId":"1"},
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>群組信用額度設定</span>
  **API Name : credit-reset-group**</br>
  **Method : PUT**</br>
  + 限信用平台使用
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號	| string |  |
| groupId | 群組類型 | string | Y | 1:日回復、2:周回復 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(account+groupId+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |  |
| groupId | 群組類型 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
    PUT /keno-api/player/credit-reset-group?
         account=ifalo001&
         groupId=1&
         key=3de5b29aac97c272f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
          "account":"ifalo001"
          "groupId":"1"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>群組信用額度查詢</span>
  **API Name : credit-reset-group**</br>
  **Method : GET**</br>
  + 限信用平台使用
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號	| string |  |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(account+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |  |
| groupId | 群組類型 | string |  |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
    GET /keno-api/player/credit-reset-group?
         account=ifalo001&
         key=3de5b29aac97c272f5822dc99c5637d6&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
          "account":"ifalo001"
          "groupId":"1"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>設定玩家帳號模式</span>
  **API Name : mode**</br>
  **Method : PUT**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 代理帳號 | string | Y | 多個玩家用","隔開 |
| mode | 模式 | string | Y | 0:正常，1:鎖單無法下注，2:封鎖無法登入 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  **`hash = md5(account+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string | |
| mode | 模式 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
  | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
  | -- | ---- | ----------- |
  | 1 | {parameter} is required  | 缺少參數欄位
  | 2 | invalid key  | 金鑰無效 |
  | 4 | player not found | 玩家未找到
  | 5 | method is not allowed  | 使用之Http方法不允許 |
  | 6 | function not found  | API不存在 |
  | 7 | internal server error  | 服務器內部錯誤 |
  | 15 | data format error  | 資料格式有誤 |
  | 35 | mode_error_can_only | 帳號模式設定錯誤 |

  ### 範例
  + 調用方法
    ```
    PUT /keno-api/player/mode?
         account=ifalo001&
         mode=0&
         key=3de5b29aac97c072f5822dc99c5637d4&
         hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
            "account":"ifalo001",
            "mode":"0"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>查詢玩家帳號模式</span>
  **API Name : mode**</br>
  **Method : GET**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| --| ---- | ----------- | ----------- | -- |
| account | 代理帳號 | string | Y | 多個玩家用","隔開 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **` hash = md5(account+privateKey)`**

  ### 輸出參數
  | 參數名稱 | 參數說明 | 參數型態 | 說明 |
  | -- | ---- | ----------- | -- |
  | account | 玩家帳號 | string | 多個玩家用","隔開
  | mode | 模式 | string | 0:正常，1:鎖單無法下注，2:封鎖無法登入，並踢除其餘玩家 |
  | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
  | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
  | -- | ---- | ----------- |
  | 1 | {parameter} is required  | 缺少參數欄位
  | 2 | invalid key  | 金鑰無效 |
  | 4 | player not found | 玩家未找到 |
  | 5 | method is not allowed  | 使用之Http方法不允許 |
  | 6 | function not found  | API不存在 |
  | 7 | internal server error  | 服務器內部錯誤 |
  | 15 | data format error  | 資料格式有誤 |

  ### 範例
  + 調用方法
    ```
      GET /keno-api/player/mode?
            account=ifalo001,ifalo002&
            key=3de5b29aac97c072f5824dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":[
          {
            "account":"ifalo001",
            "mode":"0"
          },
          {
            "account":"ifalo002",
            "mode":"1"
          }
        ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>設定玩家佔成</span>
  **API Name : profit-percent**</br>
  **Method : PUT**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y |
| lotteryType | [彩種編號](#彩種編號) | string | Y | |
| percent | 玩家佔成 | string | Y | 0.00 ~ 1.00 (1.00代表100%) |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **` hash = md5(account+lotteryType+percent+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |
| percent | 玩家佔成 | string | 以小數表示 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
  | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
  | -- | ---- | ----------- |
  | 1 | {parameter} is required  | 缺少參數欄位
  | 2 | invalid key  | 金鑰無效
  | 4 | player not found | 玩家未找到
  | 5 | method is not allowed  | 使用之Http方法不允許
  | 6 | function not found  | API不存在
  | 7 | internal server error  | 服務器內部錯誤
  | 15 | data format error  | 資料格式有誤

  ### 範例
  + 調用方法
    ```
      PUT /keno-api/player/profit-percent?
          account=ifalo001&
          lotteryType=10002&
          percent=0.05&
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
            "account":"ifalo001",
            "percent":"0.05"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
## <span>查詢玩家佔成</span>
  **API Name : profit-percent**</br>
  **Method : GET**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **` hash = md5(account+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |
| percent | 玩家佔成依[彩種編號](#彩種編號) | string | ["彩種編號":"佔成"] |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
  | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
  | -- | ---- | ----------- |
  | 1 | {parameter} is required  | 缺少參數欄位
  | 2 | invalid key  | 金鑰無效
  | 4 | player not found | 玩家未找到 |
  | 5 | method is not allowed  | 使用之Http方法不允許
  | 6 | function not found  | API不存在
  | 7 | internal server error  | 服務器內部錯誤
  | 15 | data format error  | 資料格式有誤

  ### 範例
  + 調用方法
    ```
      GET /keno-api/player/profit-percent?
          account=ifalo001&
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
            "account":"ifalo001",
            "percent":{
              "10002":"0.02",
              "10001":"0.01",
              "20002":"0.05",
            }
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>查詢注區範本</span>
  **API Name : stake-limit-list**</br>
  **Method : GET**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| exampleType | 注區範本 | string |
| defaultBet | 底注 | string |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      GET /keno-api/player/stake-limit-list?
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":[
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
          },
          {
             "exampleType":"D",
             "defaultBet":"100"
          },
          {
             "exampleType":"E",
             "defaultBet":"1000"
          }
        ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
## <span>設定平台注區範本</span>
  **API Name : stake-limit-platform**</br>
  **Method : PUT**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| exampleType | 範本類別 | string | Y | 注區範本 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(exampleType+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| exampleType | 範本類別 | string |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/stake-limit-platform?
          exampleType=A
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
            "exampleType":"A"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
------

## <span>查詢平台注區範本</span>
  **API Name : stake-limit-platform**</br>  
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
| exampleType |  | string |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
| 1 | {parameter} is required  | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 4 | player not found | 玩家未找到 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
  | 6 | function not found  | API不存在
  | 7 | internal server error  | 服務器內部錯誤
  | 15 | data format error  | 資料格式有誤

  ### 範例
  + 調用方法
    ```
      GET /keno-api/player/stake-limit-platform?
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
            "exampleType":"A",
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
------
-->

## <span>設定玩家注區範本</span>
  **API Name : stake-limit**</br>
  **Method : PUT**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | |
| exampleType |  | string | Y | |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(account+exampleType+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string | |
| exampleType | 範本類別 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/stake-limit?
          account=ifalo001&
          exampleType=A&
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{
            "account":"ifalo001",
            "exampleType":"A"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>注區範本</span>
  **API Name : stake-limit**</br>
  **Method : GET**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(account+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| exampleType | 注區範本 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
| 1 | {parameter} is required  | 缺少參數欄位 |
| 2 | invalid key  | 金鑰無效 |
| 4 | player not found | 玩家未找到 |
| 5 | method is not allowed  | 使用之Http方法不允許 |
| 6 | function not found  | API不存在 |
| 7 | internal server error  | 服務器內部錯誤 |
| 15 | data format error  | 資料格式有誤 |

  ## 範例
  + 調用方法
    ```
      GET /keno-api/player/stake-limit?
          account=ifalo001,ifalo002&
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":[
          {
              "account":"ifalo001"
              "exampleType":"A"*
          },
          {
              "account":"ifalo002"
              "exampleType":"B"
          }
        ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

## <span>修改退水值</span>
  **API Name : refund**</br>
  **Method : PUT**
  ## 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| data/account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| data/lotteryType | [彩種編號](#彩種編號) | string | Y | |
| data/refund | 退水值 | string | Y | 0 ~ 150 |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(data+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| data/account | 玩家帳號 | string |
| data/lotteryType | [彩種編號](#彩種編號) | string | |
| data/refund | 退水值 | string | 0 ~ 150 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/refund?data= [{ "account":"ifalo001","lotteryType":"20001","refund":"15"},{"account":"ifalo002","lotteryType":"20002","refund":"25"}]&
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":[
          {
            "account":"ifalo001",
            "lotteryType":"20001",
            "refund":"15"
          },
          {
            "account":"ifalo002",
            "lotteryType":"20001",
            "refund":"15"
          }
        ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
## <span>查詢退水值</span>
  **API Name : refund**</br>
  **Method : GET**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 玩家帳號 | string | Y | 多個玩家用","隔開 |
| lotteryType | [彩種編號](#彩種編號) | string | Y | |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(account+lotteryType+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 玩家帳號 | string |
| lotteryType | [彩種編號](#彩種編號) | string | |
| refund | 退水值 | string | 0 ~ 150 |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/refund?
          account=ifalo001,ifalo002&
          lotteryType=20001
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":[
          {
            "account":"ifalo001",
            "lotteryType":"20001",
            "refund":"15"
          },
          {
            "account":"ifalo002",
            "lotteryType":"20001",
            "refund":"15"
          }
        ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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

<!--
------
## <span>設定代理商賠率</span>
  **API Name : odds**</br>
  **Method : PUT**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| odds | 賠率 | string | Y | |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(odds+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| odds | 賠率 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/odds?
          odds=40
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data": {
            "odds":"40"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
## <span>查詢代理商賠率</span>
  **API Name : odds**</br>
  **Method : GET**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| odds | 賠率 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      GET /keno-api/player/odds?
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data": {
            "odds":"40"
        },
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
-->
------
## <span>設定玩家盤口</span>
  **API Name : class**</br>
  **Method : PUT**
  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| account | 帳號 | string | Y | 可多個,號格開|
| class  | 盤口 | string | Y | [玩家盤口設定](#盤口設定)|
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(account+class+privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| account | 帳號 | string | |
| ClassPath | 已設定盤口 | string | |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/player/class?
          account=linand&
          class=A&
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data": [
                 {"account":"linand",
                  "ClassPath":"D"
                 }
               ],
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
## <span>查詢平台盤口與各盤口退水及限注規範</span>
  **API Name : classList**</br>
  **Method : GET**

  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
| hash | 驗證參數 | string | Y | md5 |

  #### **`hash = md5(privateKey)`**

  ### 輸出參數
| 參數名稱 | 參數說明 | 參數型態 | 說明 |
| -- | ---- | ----------- | -- |
| LotteryNo | 彩種編號 | string | |
| LotteryName | 彩種名稱 | string | |
| GameNo | 玩法編號 | string | |
| GameName | 玩法名稱 | string | |
| A | A盤退水 | string | |
| B | B盤退水 | string | |
| C | C盤退水 | string | |
| D | D盤退水 | string | |
|Singlimit |單注限額 | string| |
|Curlimit |單期限額 | string| |
| uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

  ### 錯誤碼
| 錯誤碼 | 錯誤訊息 | 錯誤說明 |
| -- | ---- | ----------- |
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
      PUT /keno-api/agent/classList?
          key=3de5b29aac97c072f5824dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
    ```

  + 成功
    ```javascript
    {
        "status":"success",
        "data":{"ClassPath":"D","ClassList":[{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":52,"GameName":"猜冠軍","A":0,"B":2,"C":3,"D":5,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":54,"GameName":"猜前三名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":55,"GameName":"定位膽選","A":0,"B":2,"C":3,"D":5,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":56,"GameName":"冠軍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":57,"GameName":"亞軍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":58,"GameName":"季軍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":59,"GameName":"四名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":60,"GameName":"五名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":61,"GameName":"六名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":62,"GameName":"七名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":63,"GameName":"八名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":64,"GameName":"九名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":65,"GameName":"十名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":66,"GameName":"冠亞","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":68,"GameName":"冠軍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":69,"GameName":"亞軍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":70,"GameName":"季軍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":71,"GameName":"四名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":72,"GameName":"五名","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":73,"GameName":"冠亞","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":75,"GameName":"冠亞季選一","A":0,"B":0.5,"C":1.5,"D":3,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":76,"GameName":"冠亞組合","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":215,"GameName":"冠亞總和3","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":216,"GameName":"冠亞總和4","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":217,"GameName":"冠亞總和5","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":218,"GameName":"冠亞總和6","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":219,"GameName":"冠亞總和7","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":220,"GameName":"冠亞總和8","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":221,"GameName":"冠亞總和9","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":222,"GameName":"冠亞總和10","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":223,"GameName":"冠亞總和11","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":224,"GameName":"冠亞總和12","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":225,"GameName":"冠亞總和13","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":226,"GameName":"冠亞總和14","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":227,"GameName":"冠亞總和15","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":228,"GameName":"冠亞總和16","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":229,"GameName":"冠亞總和17","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":230,"GameName":"冠亞總和18","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10016,"LotteryName":"北京PK10","GameNo":231,"GameName":"冠亞總和19","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":1,"GameName":"五星複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":2,"GameName":"五星單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":3,"GameName":"前四單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":4,"GameName":"前四複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":5,"GameName":"後四單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":6,"GameName":"後四複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":7,"GameName":"任選單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":8,"GameName":"任選複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":9,"GameName":"前三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":10,"GameName":"前三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":11,"GameName":"中三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":12,"GameName":"中三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":13,"GameName":"後三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":14,"GameName":"後三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":15,"GameName":"任三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":16,"GameName":"任三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":17,"GameName":"前三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":18,"GameName":"前三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":19,"GameName":"中三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":20,"GameName":"中三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":21,"GameName":"後三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":22,"GameName":"後三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":23,"GameName":"任選三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":24,"GameName":"任選三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":25,"GameName":"前二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":26,"GameName":"前二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":27,"GameName":"後二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":28,"GameName":"後二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":29,"GameName":"任選二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":30,"GameName":"任選二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":31,"GameName":"前二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":32,"GameName":"前二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":33,"GameName":"後二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":34,"GameName":"後二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":35,"GameName":"任選二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":36,"GameName":"任選二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":37,"GameName":"五星定位膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":38,"GameName":"前三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":39,"GameName":"中三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":40,"GameName":"後三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":41,"GameName":"任選三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":42,"GameName":"前三二碼","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":43,"GameName":"後三二碼","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":44,"GameName":"前二大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":45,"GameName":"後二大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":46,"GameName":"任選大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":47,"GameName":"萬位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":48,"GameName":"千位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":49,"GameName":"百位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":50,"GameName":"十位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":51,"GameName":"個位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":172,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":173,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":174,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":175,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":176,"GameName":"龍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":177,"GameName":"虎","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":178,"GameName":"和","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":179,"GameName":"前三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":180,"GameName":"前三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":181,"GameName":"前三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":182,"GameName":"前三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":183,"GameName":"前三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":184,"GameName":"中三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":185,"GameName":"中三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":186,"GameName":"中三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":187,"GameName":"中三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":188,"GameName":"中三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":189,"GameName":"後三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":190,"GameName":"後三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":191,"GameName":"後三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":192,"GameName":"後三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10003,"LotteryName":"天津時時彩","GameNo":193,"GameName":"後三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":1,"GameName":"五星複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":2,"GameName":"五星單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":3,"GameName":"前四單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":4,"GameName":"前四複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":5,"GameName":"後四單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":6,"GameName":"後四複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":7,"GameName":"任選單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":8,"GameName":"任選複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":9,"GameName":"前三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":10,"GameName":"前三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":11,"GameName":"中三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":12,"GameName":"中三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":13,"GameName":"後三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":14,"GameName":"後三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":15,"GameName":"任三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":16,"GameName":"任三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":17,"GameName":"前三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":18,"GameName":"前三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":19,"GameName":"中三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":20,"GameName":"中三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":21,"GameName":"後三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":22,"GameName":"後三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":23,"GameName":"任選三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":24,"GameName":"任選三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":25,"GameName":"前二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":26,"GameName":"前二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":27,"GameName":"後二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":28,"GameName":"後二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":29,"GameName":"任選二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":30,"GameName":"任選二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":31,"GameName":"前二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":32,"GameName":"前二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":33,"GameName":"後二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":34,"GameName":"後二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":35,"GameName":"任選二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":36,"GameName":"任選二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":37,"GameName":"五星定位膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":38,"GameName":"前三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":39,"GameName":"中三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":40,"GameName":"後三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":41,"GameName":"任選三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":42,"GameName":"前三二碼","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":43,"GameName":"後三二碼","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":44,"GameName":"前二大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":45,"GameName":"後二大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":46,"GameName":"任選大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":47,"GameName":"萬位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":48,"GameName":"千位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":49,"GameName":"百位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":50,"GameName":"十位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":51,"GameName":"個位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":172,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":173,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":174,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":175,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":176,"GameName":"龍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":177,"GameName":"虎","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":178,"GameName":"和","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":179,"GameName":"前三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":180,"GameName":"前三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":181,"GameName":"前三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":182,"GameName":"前三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":183,"GameName":"前三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":184,"GameName":"中三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":185,"GameName":"中三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":186,"GameName":"中三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":187,"GameName":"中三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":188,"GameName":"中三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":189,"GameName":"後三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":190,"GameName":"後三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":191,"GameName":"後三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":192,"GameName":"後三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10004,"LotteryName":"新疆時時彩","GameNo":193,"GameName":"後三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":1,"GameName":"五星複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":2,"GameName":"五星單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":3,"GameName":"前四單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":4,"GameName":"前四複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":5,"GameName":"後四單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":6,"GameName":"後四複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":7,"GameName":"任選單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":8,"GameName":"任選複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":9,"GameName":"前三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":10,"GameName":"前三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":11,"GameName":"中三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":12,"GameName":"中三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":13,"GameName":"後三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":14,"GameName":"後三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":15,"GameName":"任三單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":16,"GameName":"任三複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":17,"GameName":"前三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":18,"GameName":"前三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":19,"GameName":"中三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":20,"GameName":"中三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":21,"GameName":"後三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":22,"GameName":"後三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":23,"GameName":"任選三組三","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":24,"GameName":"任選三組六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":25,"GameName":"前二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":26,"GameName":"前二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":27,"GameName":"後二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":28,"GameName":"後二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":29,"GameName":"任選二單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":30,"GameName":"任選二複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":31,"GameName":"前二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":32,"GameName":"前二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":33,"GameName":"後二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":34,"GameName":"後二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":35,"GameName":"任選二組單式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":36,"GameName":"任選二組複式","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":37,"GameName":"五星定位膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":38,"GameName":"前三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":39,"GameName":"中三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":40,"GameName":"後三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":41,"GameName":"任選三不定膽","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":42,"GameName":"前三二碼","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":43,"GameName":"後三二碼","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":44,"GameName":"前二大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":45,"GameName":"後二大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":46,"GameName":"任選大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":47,"GameName":"萬位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":48,"GameName":"千位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":49,"GameName":"百位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":50,"GameName":"十位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":51,"GameName":"個位大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":172,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":173,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":174,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":175,"GameName":"總和大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":176,"GameName":"龍","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":177,"GameName":"虎","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":178,"GameName":"和","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":179,"GameName":"前三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":180,"GameName":"前三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":181,"GameName":"前三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":182,"GameName":"前三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":183,"GameName":"前三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":184,"GameName":"中三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":185,"GameName":"中三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":186,"GameName":"中三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":187,"GameName":"中三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":188,"GameName":"中三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":189,"GameName":"後三特殊符號=>豹子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":190,"GameName":"後三特殊符號=>順子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":191,"GameName":"後三特殊符號=>對子","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":192,"GameName":"後三特殊符號=>半順","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":10011,"LotteryName":"重慶時時彩","GameNo":193,"GameName":"後三特殊符號=>雜六","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":119,"GameName":"1星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":120,"GameName":"2星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":121,"GameName":"3星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":122,"GameName":"4星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":123,"GameName":"5星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":127,"GameName":"大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":146,"GameName":"大小單大小雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":194,"GameName":"總和大","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":195,"GameName":"總和小","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":196,"GameName":"總和單","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":197,"GameName":"總和雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":198,"GameName":"總和810","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":200,"GameName":"總大單","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":201,"GameName":"總大雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":202,"GameName":"總小單","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":203,"GameName":"總小雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":204,"GameName":"五行=>金","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":205,"GameName":"五行=>木","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":206,"GameName":"五行=>水","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":207,"GameName":"五行=>火","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":208,"GameName":"五行=>土","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":209,"GameName":"雙面奇和偶=>奇","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":210,"GameName":"雙面奇和偶=>和","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":211,"GameName":"雙面奇和偶=>偶","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":212,"GameName":"雙面上中下=>上","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":213,"GameName":"雙面上中下=>中","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20001,"LotteryName":"賓果賓果","GameNo":214,"GameName":"雙面上中下=>下","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":119,"GameName":"1星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":120,"GameName":"2星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":121,"GameName":"3星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":122,"GameName":"4星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":123,"GameName":"5星","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":127,"GameName":"大小單雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":146,"GameName":"大小單大小雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":194,"GameName":"總和大","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":195,"GameName":"總和小","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":196,"GameName":"總和單","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":197,"GameName":"總和雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":198,"GameName":"總和810","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":200,"GameName":"總大單","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":201,"GameName":"總大雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":202,"GameName":"總小單","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":203,"GameName":"總小雙","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":204,"GameName":"五行=>金","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":205,"GameName":"五行=>木","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":206,"GameName":"五行=>水","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":207,"GameName":"五行=>火","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":208,"GameName":"五行=>土","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":209,"GameName":"雙面奇和偶=>奇","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":210,"GameName":"雙面奇和偶=>和","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":211,"GameName":"雙面奇和偶=>偶","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":212,"GameName":"雙面上中下=>上","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":213,"GameName":"雙面上中下=>中","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000},{"LotteryNo":20002,"LotteryName":"北京快樂8","GameNo":214,"GameName":"雙面上中下=>下","A":0,"B":0,"C":0,"D":0,"Singlimit":5000,"Curlimit":10000}]},
        "uuquid":"01212c3b9e1eac371776a8e932289906"
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
## <span>手機串接</span>
  **API Name :**</br>
  **Method :**

  ### 輸入參數
| 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
| -- | ---- | ----------- | ----------- | -- |
| loginURL | 玩家登入網址 | string | Y | 從取得玩家登入API取得 |
| platformURL | 返回平台網址 | string | Y | 遊戲APP返回平台使用 |

  ### 範例
  + 調用方法
    ```
    台灣版
    tw-pharao-keno-game://launch-game?
         loginURL=https://keno.com/keno-game/player/login?account=ifalo001&key=K0hvSzZvbzlPbHoybDl2QnRDaGl5K1JLZ1ZtUXdU&platformURL=http://pha888.com.tw

    大陸版
    cn-pharao-keno-game://launch-game?
         loginURL=https://keno.com/keno-game/player/login?account=ifalo001&key=K0hvSzZvbzlPbHoybDl2QnRDaGl5K1JLZ1ZtUXdU&platformURL=http://pha888.com.tw
    ```
<div align="right"><a href="#top">Top</a></div>


## <span>附表</span>
### <span>盤口設定</span>
| 代碼 | 說明 |
|--|----|
| A | A盤 |
| B | B盤 |
| C | C盤 |
| D | D盤 |

### <span>支援貨幣</span>
| 代碼 | 說明 |
|--|----|
| TWD | 台幣 |
| CNY | 人民幣 |
| USD | 美金 |

### <span>彩種編號</span>
| 彩種編號 | 彩種名稱 |
| -- | ---- |
| 10003 | 天津時時彩 |
| 10004 | 新疆時時彩 |
| 10011 | 重慶時時彩 |
| 10016 | 北京PK10 |
| 20001 | 賓果賓果 |
| 20002 | 北京快樂8 |
| 88889 | 極速賽車  |

### <span>交易狀態</span>
| 錯誤碼 | 錯誤訊息 |
| -- | ---- |
| 0 | 成功 |
| 1 | 交易失敗，餘額不足 |
| 2 | 交易失敗，帳戶凍結 |
| 3 | 服務器內部異常 |

### <span>注單狀態</span>
| 注單狀態 | 說明 |
| -- | ---- |
| 0 | 一般單未結算 |
| 1 | 一般單已結算 |
| 2 | 撤單 |
| 3 | 追號單未結算 |
| 4 | 追號餘額不足已結算 |
| 5 | 追號超過限輸限贏已結算 |
