# 彩票系統對接API範本
1. [註冊玩家帳號](#註冊玩家帳號)
2. [取得登入網址](#取得登入網址)
3. [修改玩家帳號暱稱](#修改玩家帳號暱稱)
4. [查詢玩家](#查詢玩家)
5. [查詢玩家下注紀錄](#查詢玩家下注紀錄)
6. [查詢多玩家下注簡報總額](#查詢多玩家下注簡報總額)
7. [額度轉出入](#額度轉出入)
8. [額度轉出入狀態](#額度轉出入狀態)
9. [玩家踢線](#玩家踢線)
10. [設定會員限輸](#設定會員限輸)
11. [設定會員限贏](#設定會員限贏)
12. [限注回復-信用](#限注回復-信用)
13. [會員額度回復-信用](#會員額度回復-信用)
14. [額度設定-信用](#額度設定-信用)
15. [設定玩家帳號模式](#設定玩家帳號模式)
16. [查詢玩家帳號模式](#查詢玩家帳號模式)
17. [設定玩家佔成](#設定玩家佔成)
18. [設定平台注區範本](#設定平台注區範本)
19. [查詢平台注區範本](#查詢平台注區範本)
20. [設定玩家注區範本](#設定玩家注區範本)
21. [查詢玩家注區範本](#查詢玩家注區範本)
22. [修改退水值](#修改退水值)
23. [查詢退水值](#查詢退水值)
24. [手機串接API](#手機串接API)

------
1. ## <span>註冊玩家帳號</span>
   **API Name : register**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   |----|----|----|----|----|
   |  account | 會員帳號 | string | Y | 長度最短4，最長20 |
   |  nickname | 會員暱稱 | string | Y | 長度最短4，最長20 |
   |  currency | 貨幣 | string | N | [玩家貨幣類型](#支援貨幣)，不指定以代理商為準 |
   | maxbetlimit | 單次最大下注額 | string | Y |
   |  limitwin | 限贏 | string | Y | 0表示無限制 |
   |  limitlose | 限輸 | string | Y | 0表示無限制 |
   |  key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   |  hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+nickname+maxbetlimit+limitwin+limitlose+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明 |
   |--|----|----------|--|
   | account | 會員帳號  | string  |
   | nickname | 會員暱稱 | string |
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

2. ## <span>取得登入網址</span>
   **API Name : auth**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | account | 會員帳號 | string | Y | 長度最短4，最長20 |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |
   #### **`  hash = md5(account+privateKey) `**

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
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 服務器內部錯誤 |
   | 25 | locked_and_can_not_login | 帳號或上層被鎖 |
   | 52 | Trial_account_expired | 試玩帳號限期已過 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/auth?
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

3. ## <span>修改玩家帳號暱稱</span>
   **API Name : modifyn**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | account | 會員帳號 | string | Y | 長度最短4，最長20 |
   | nickname | 會員暱稱 | string | Y | 長度最短4，最長20 |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+nickname+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | :-- | ---- | ----------- | --
   | account | 會員帳號 | string |
   | nickname | 會員暱稱 |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |


   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | ---- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位 |
   | 2 | invalid key  | 金鑰無效 |
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 資料格式有誤 |
   | 31 | account_not_exist | 帳號不存在 |
   | 32 | prohibited_to_modify_their_own_members | 禁止修改非自己的會員 |

  ###範例
  + 調用方法
    ```
    POST /keno-api/player/modifyn?
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
4. ## <span>查詢玩家</span>
   **API Name : playerInfo**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | account | 會員帳號 | string | Y | 長度最短4，最長20 |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明 |
   | -- | ---- | ----------- | -- |
   | account | 會員帳號 | string |
   | nickname | 會員暱稱 | string |
   | credit | 會員餘額 | float | 格式: 0.0000 |
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
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 資料格式有誤 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/playerInfo?
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
5. ## <span>查詢玩家下注紀錄</span>
   **API Name : betDetail**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | account | 會員帳號 | string | Y |
   | startAt | 開始時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
   | endAt | 結束時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
   | now | 第x頁 | string | Y | 空值代表第1頁 |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+startAt+endAt+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明 |
   | --| ---- | ----------- | -- |
   | count | 下注筆數 | string |
   | totalPage | 總頁數 | string |
   | historyList | 歷史紀錄 | array |
   | historyList \ orderID | 下注單號 | string |
   | historyList \ memberUID | 會員編號 | string |
   | historyList \ memberAccount | 會員帳號 | string |
   | historyList \ agentUID | 代理商編號 | string |
   | historyList \ agentName | 代理商名稱 | string |
   | historyList \ lotteryName | 彩票名稱 | string |
   | historyList \ drawingNo | 開獎期別 | string |
   | historyList \ groupName | 玩法群組名稱 | string |
   | historyList \ gameName | 玩法名稱 | string |
   | historyList \ betData | 下注號碼 | string |
   | historyList \ betNumber | 注數 | int |
   | historyList \ odd | 賠率 | string |
   | historyList \ multiple | 倍數 | string |
   | historyList \ betAmount | 下注總額 | string |
   | historyList \ winBonus | 中獎金額 | string |
   | historyList \ ip | 下注IP | string |
   | historyList \ betStatus | 注單狀態 | string |注單狀態<br>0:未結算，<br>1:已結算，<br>2:撤單，<br>3:追號，<br>4:追號中獎停追，<br>5:金額不足 |
   | historyList \ currency | 貨幣 | string |
   |historyList \ balance | 餘額 | string |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | :--| ---- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位 |
   | 2 | invalid key  | 金鑰無效 |
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 資料格式有誤 |
   | 33 | time is over limit | 日期相隔不得大於7天 |
   | 34 | the start date is greater than the end time | 開始日期不能大於結束日期 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/betDetail?
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
           "count":1,
           "historyList":[["3993747581","996291332","ifalo001",728299597,"ifalo","重慶時時彩","20170904045","五星玩法","五星複式","0,1,2,3,4",1,180000,1,2,0,"192.168.111.77",1,0]]
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
6. ## <span>查詢多玩家下注簡報總額</span>
   **API Name : betReport**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | account | 會員帳號 | string | Y |
   | startAt | 開始時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
   | endAt | 結束時間 | string | Y | 0 代表不限制，至多查詢七天內的資料<br>格式：YYYY-MM-dd hh:mm:ss |
   | now | 第x頁 | string | Y | 空值代表第1頁 |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+startAt+endAt+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明 |
   | -- | ---- | ----------- | -- |
   | memberAccount | 會員帳號 | string |
   | agentUID | 代理商編號 | string |
   | agentName | 代理商名稱 | string |
   | type | 分類項目 | array |
   | type \ betNumber | 注數 | string |
   | type \ betAmount | 下注總額 | string |
   | type \ winBonus | 中獎金額 | string |
   | type \ currency | 貨幣 | string |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | ---- | ----------- |
   | 1 | {parameter} is required | 缺少參數欄位 |
   | 2 | invalid key  | 金鑰無效 |
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 資料格式有誤 |
   | 33 | time is over limit | 日期相隔不得大於7天 |
   | 34 | the start date is greater than the end time | 開始日期不能大於結束日期 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/betReport?
          account=ifalo001&
          startAt=2017-09-01 00:00:00&
          endAt=2017-09-04 14:00:00&
          now=1&
          key=3de5b21aac97c072f5822dc99c5637d6&
          hash=26f6b1074e1c9e80e9b613bf79a923a6
     ```
   + 成功
     ```javascript
     {
        "status":"success",
        "data":{
            "memberAccount":"ifalo001",
            "agentUID":"ifalo001",
            "agentName":"ifalo",
            "type":[
                {1000,5000,500,NTD},
                {250,7000,500,MCY},
            ]
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
7. ## <span>額度轉出入</span>
   **API Name : transaction**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | --| ---- | ----------- | ----------- | -- |
   | account | 會員帳號 | string | Y |
   |credit | 轉出入額度 | string | Y | 格式: ±0.0000<br>正數為轉入，負數為轉出 |
   | orderNo | 平台交易編號 | string | Y | 介接平台產生，核對帳務 |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+credit+orderNo+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | -- | ---- | ----------- | -- |
   | transactionNo | 交易編號 | string |
   | time | 交易時間 | string |
   | orginalCredit | 交易前額度 | string |
   | transferCredit | 交易額度 | string |
   | finalCredit | 交易後額度 | string |
   | currency | 貨幣 | string |
   | status | 交易狀態 | string |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 交易錯誤碼
   | 錯誤碼 | 錯誤訊息 |
   | -- | ---- |
   | 0 | 成功 |
   | 1 | 交易失敗，餘額不足 |
   | 2 | 交易失敗，帳戶凍結 |
   | 3 | 服務器內部異常 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | ---- | ----------- |
   | 1 | {parameter} is required | 缺少參數欄位 |
   | 2 | invalid key  | 金鑰無效 |
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 資料格式有誤 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/transaction?
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
	        "orginalCredit":"+1000",
          "transferCredit":"+500",
          "finalCredit":"+1500",
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

8. ## <span>額度轉出入狀態</span>
   **API Name : gettransaction**</br>
   **Method : POST**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | transactionNo | 交易編號 | string | Y |
   | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(transactionNo+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | --| ---- | ----------- | -- |
   | account | 會員帳號 | string |
   | orginalCredit | 交易前額度 | string |
   | finalCredit | 交易後額度 | string |
   | addedCredit | 新增額度 | string |
   | transactionNo | 遊戲交易編號 | 遊戲平台產生 |
   | orderNo | 平台交易編號 | 介接平台產生 |
   | status | 交易狀態 | string |
   | time | 交易時間 | string
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   錯誤碼 | 錯誤訊息 | 錯誤說明 |
   |--| ---- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位 |
   | 2 | invalid key  | 金鑰無效 |
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 資料格式有誤 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/gettransaction?
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

9. ## <span>玩家踢線</span>
   **API Name : kick-multiple**</br>
   **Method : DELETE**
   ### 輸入參數
   | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
   | -- | ---- | ----------- | ----------- | -- |
   | account | 會員帳號 | string | Y | 多個玩家用","隔開 |
   |reason | 踢線原因 | string | N |
   |key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
   | hash | 驗證參數 | string | Y | md5 |

   #### **` hash = md5(account+privateKey)`**

   ### 輸出參數
   | 參數名稱 | 參數說明 | 參數型態 | 說明
   | -- | ---- | ----------- | -- |
   | account | 會員帳號 | string |
   | status | 處理狀況 | string | 0:成功，1:玩家不在線，2:非該代理會員 |
   | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

   ### 錯誤碼
   | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
   | -- | ---- | ----------- |
   | 1 | {parameter} is required  | 缺少參數欄位 |
   | 2 | invalid key  | 金鑰無效 |
   | 4 | {parameter} not found | 欄位參數值無效 |
   | 5 | method is not allowed  | 使用之Http方法不允許 |
   | 6 | function not found  | API不存在 |
   | 7 | internal server error  | 服務器內部錯誤 |
   | 15 | data format error  | 資料格式有誤 |

   ### 範例
   + 調用方法
     ```
     POST /keno-api/player/kick-multiple?
          transactionNo=59a8c0c3198cd&
          key=3de5b29a2c97c072f5822dc99c5637d6&
          hash=6f6b1074e1c9e80e9b613bf79a923a6
     ```

   + 成功
     ```javascript
     {
          "status":"success",
          "data":[{
             "account":"ifalo001",
             "status":"0"
          },
          {
             "account":"ifalo002",
             "status":"-1"
          }],
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
10. ## <span>設定會員限輸</span>
    **API Name : loseLimit**</br>
    **Method : PUT**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 會員帳號 | string | Y | 多個玩家用","隔開 |
    | limit | 限輸額度 | string | Y | 0表示無限制 |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+limit+privateKey)`**

    ### 輸出參數
    |參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | limit | 限輸額度 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
      PUT /keno-api/player/loseLimit?
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
11. ## <span>設定會員限贏</span>
    **API Name : winLimit**</br>
    **Method : PUT**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 會員帳號 | string | Y | 多個玩家用","隔開 |
    | limit | 限贏額度 | string | Y | 0表示無限制 |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+limit+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | limit | 限贏額度 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
      PUT /keno-api/player/winLimit?
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
              "winLimit":"0"
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

12. ## <span>限注回復-信用</span>
    **API Name : limitReturn**</br>
    **Method : POST**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 代理帳號 | string | Y | 多個代理用","隔開 |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    **`hash = md5(account+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
      POST /keno-api/player/limitReturn?
           account=falo&
           limitWin=0&
           limitLose=0&
           key=3de5b29aac97c072f5822dc9925637d6&
           hash=26f6b1074e1c9e80e9b613bf79a923a6
      ```

    + 成功
      ```javascript
      {
         "status":"success",
         "data":{
            "account":"falo"
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
          "uuquid":"e1f3414056fcbd57c24d5289acee1b8f"
      }
      ```

13. ## <span>會員額度回復-信用</span>
    **API Name : balanceReturn**</br>
    **Method : POST**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 代理帳號 | string | Y | 多個會員用","隔開 |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
      POST /keno-api/player/balanceReturn?
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
14. ## 額度設定-信用
    **API Name : creditLimit**</br>
    **Method : POST**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 代理帳號 | string | Y |
    | creditLimit | 最高額度 | string | Y |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生)
    | hash | 驗證參數 | string | Y | md5

    #### **` hash = md5(account+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
      POST /keno-api/player/creditLimit?
            account=ifalo001&
            creditLimit=100000&
            key=3de5b29aac97c072f5822dc99c5633d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
       ```

    + 成功
      ```javascript
      {
          "status":"success",
          "data":{
               "account":"ifalo001"
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

15. ## 設定玩家帳號模式
    **API Name : modifyb**</br>
    **Method : POST**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 代理帳號 | string | Y |
    | mode | 模式 | string | Y | 0:正常，1:鎖單無法下注，2:封鎖無法登入 |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    **`hash = md5(account+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |
    | 35 | mode_error_can_only | 帳號模式設定錯誤 |

    ### 範例
    + 調用方法
      ```
      POST /keno-api/player/modifyb?
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
              "account":"ifalo001"
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

16. ## 查詢玩家帳號模式
    **API Name : block**</br>
    **Method : POST**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | --| ---- | ----------- | ----------- | -- |
    | account | 代理帳號 | string | Y |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **` hash = md5(account+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | mode | 模式 | string | 0:正常，1:鎖單無法下注，2:封鎖無法登入 |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
      POST /keno-api/player/block?
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
              "mode":""
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

17. ### 設定玩家佔成
    **API Name : profit-percent**</br>
    **Method : POST**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 會員帳號 | string | Y |
    | percent | 會員佔成 | string | Y |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **` hash = md5(account+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | percent | 會員佔成 | string | 以小數表示 |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位
    | 2 | invalid key  | 金鑰無效
    | 4 | {parameter} not found | 欄位參數值無效
    | 5 | method is not allowed  | 使用之Http方法不允許
    | 6 | function not found  | API不存在
    | 7 | internal server error  | 服務器內部錯誤
    | 15 | data format error  | 資料格式有誤

    ### 範例
    + 調用方法
      ```
        POST /keno-api/player/profit-percent?
            account=ifalo001&
            percent=0.5&
            key=3de5b29aac97c072f5824dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
      ```

    + 成功
      ```javascript
      {
          "status":"success",
          "data":{
              "account":"ifalo001",
              "percent":"0.5"
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
18. ## <span>設定平台注區範本</span>
    **API Name : stake-limit-list**</br>
    **Method : PUT**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | exampleType | 範本類別 | string | Y | [注區範本](#注區範本) |
    | maxbetlimit | 單次最大下注額 | string | Y |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+exampleType+maxbetlimit+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | exampleType | 範本類別 | string |
    | maxbetlimit | 單次最大下注額 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
        PUT /keno-api/player/stake-limit-list?
            account=ifalo001&
            exampleType=A&
            maxbetlimit=100000&
            key=3de5b29aac97c072f5824dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
      ```

    + 成功
      ```javascript
      {
          "status":"success",
          "data":{
              "exampleType":"A",
              "maxbetlimit":"1000000"
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

19. ### 查詢平台注區範本
    **API Name : stake-limit-list**</br>  
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
    | defaultBet | 底注 | string |
    | maxbetlimit | 單次最大下注額 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在
    | 7 | internal server error  | 服務器內部錯誤
    | 15 | data format error  | 資料格式有誤

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
          "data":{
              "exampleType":"A",
              "maxbetlimit":"1000000"
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

20. ### 設定玩家注區範本
    **API Name : stake-limit**</br>
    **Method : PUT**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 會員帳號 | string | Y |
    | exampleType | 範本類別 | string | Y | [注區範本](#注區範本)
    | maxbetlimit | 單次最大下注額 | string | Y |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+exampleType+maxbetlimit+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | exampleType | 範本類別 | string |
    | maxbetlimit | 單次最大下注額 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
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
            maxbetlimit=100000&
            key=3de5b29aac97c072f5824dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
       ```

    + 成功
      ```javascript
      {
          "status":"success",
          "data":{
             "exampleType":"A",
             "maxbetlimit":"1000000"
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

21. ### 查詢玩家注區範本
    **API Name : stake-limit**</br>
    **Method : GET**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 會員帳號 | string | Y |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | defaultBet | 底注 | string |
    | maxbetlimit | 單次最大下注額 | string |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ## 範例
    + 調用方法
      ```
        GET /keno-api/player/stake-limit?
            account=ifalo&
            key=3de5b29aac97c072f5824dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
      ```

    + 成功
      ```javascript
      {
          "status":"success",
          "data":{
              "exampleType":"A",
              "maxbetlimit":"1000000"
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

22. ### 修改退水值
    **API Name : refund**</br>
    **Method : PUT**
    ## 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 會員帳號 | string | Y |
    | refund | 退水值 | string | Y | 0 ~ 150 |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+refund+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | refund | 退水值 | string | 0 ~ 150 |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
        PUT /keno-api/player/refund?
            account=ifalo&
            refund=15
            key=3de5b29aac97c072f5824dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
      ```

    + 成功
      ```javascript
      {
          "status":"success",
          "data":{
              "account":"ifalo",
              "refund":"15"
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
23. ### 查詢退水值
    **API Name : refund**
    **Method : PUT**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | account | 會員帳號 | string | Y |
    | refund | 退水值 | string | Y | 0 ~ 150 |
    | key | 公鑰 | string | Y | 各代理商公鑰(註冊代理商產生) |
    | hash | 驗證參數 | string | Y | md5 |

    #### **`hash = md5(account+refund+privateKey)`**

    ### 輸出參數
    | 參數名稱 | 參數說明 | 參數型態 | 說明 |
    | -- | ---- | ----------- | -- |
    | account | 會員帳號 | string |
    | refund | 退水值 | string | 0 ~ 150 |
    | uuquid | 交易序號 | string | 用於追蹤查詢紀錄 |

    ### 錯誤碼
    | 錯誤碼 | 錯誤訊息 | 錯誤說明 |
    | -- | ---- | ----------- |
    | 1 | {parameter} is required  | 缺少參數欄位 |
    | 2 | invalid key  | 金鑰無效 |
    | 4 | {parameter} not found | 欄位參數值無效 |
    | 5 | method is not allowed  | 使用之Http方法不允許 |
    | 6 | function not found  | API不存在 |
    | 7 | internal server error  | 服務器內部錯誤 |
    | 15 | data format error  | 資料格式有誤 |

    ### 範例
    + 調用方法
      ```
        PUT /keno-api/player/refund?
            account=ifalo&
            refund=15
            key=3de5b29aac97c072f5824dc99c5637d6&
            hash=26f6b1074e1c9e80e9b613bf79a923a6
      ```

    + 成功
      ```javascript
      {
          "status":"success",
          "data":{
             "account":"ifalo",
             "refund":"15"
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

24. ### 手機串接API
    **API Name :**</br>
    **Method :**
    ### 輸入參數
    | 參數名稱 | 參數說明 | 參數型態 | 必填 | 說明 |
    | -- | ---- | ----------- | ----------- | -- |
    | loginURL | 玩家登入網址 | string | Y | 從取得玩家登入API取得 |
    | platormURL | 返回平台網址 | string | Y | 遊戲APP返回平台使用 |

## <span>附表</span>
### <span>支援貨幣</span>
| 代碼 | 說明 |
|--|----|
| TWD | 台幣 |
| CNY | 人民幣 |
| USD | 美金 |

### <span>注區範本</span>
| 類別 | 範本 |
| -- | ---- |
| A | 2 |
| B | 5 |
| C | 10 |
| D | 50 |
| E | 100 |
