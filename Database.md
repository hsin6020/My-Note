# Database

## Transaction
資料庫執行過程中的一個邏輯單位，涵蓋一連串對資料庫的操作。每個 transaction 只會有兩種結果:
1. 全部成功(commit)
2. 全部失敗(只要有任一 SQL 指令失敗，就會 rollback)


### 為甚麼要有 Transaction?
舉個小例子，今天 A 要給 B 100元，會有以下操作
1. 查看 A 的餘額
2. 從 A 的餘額減 100
3. 查看 B 的餘額
4. 從 B 的餘額加 100


假設這些操作沒有在同一個 transaction 裡操作，如果第3個操作發生不預期的問題，A 的 100元就白白消失了，所以 transaction 很重要！可以避免這種情況的發生


## 四大特性
1. **Atomicity(原子性)**：把 transaction 視為一個原子，不可分割，所以要嘛成功，要嘛失敗，沒有成功一半這種事
2. **Consistency(一致性)**：不論 transaction 成功與否，都保持資料的完整
3. **Isolation(獨立性)**：當多個 transaction 同時執行的時候，每個 transaction 都應該要保持獨立，不影響到其他 transaction。
4. **Durability(永久性)**：一旦資料修改完成，這些修改也不會因為後續的失敗而丟失。

## 常見問題

多個 transaction 同時操作，總會遇到一些錯誤問題，以下介紹四類：
1. **Dirty Read(髒讀)**：transaction A 讀取了 transaction B 還沒有提交(commit)的數據


| Transaction A | Transaction B   |
| ------------- | --------------- |
| READ x = 100  |                 |
|               | UPDATE x = 1000 |
| READ x = 1000 |                 |
| dirty read(x 其實是100，但 Transaction A 以為是 100)   | ROLLBACK        |


2. **Non-repeatable read(不可重複讀)**：同個 transaction 內，針對同個欄位得到的數據不一樣



| Transaction A                                      | Transaction B   |
| -------------------------------------------------- | --------------- |
| READ x = 100                                       |                 |
|                                                    | UPDATE x = 1000 |
|                                                    | COMMIT x = 1000 |
| READ x = 1000                                      |                 |
| non-repeated read(第一次讀到 100，第二次讀到 1000)     |                 |

3. **Phantom Read(幻讀)**：Transaction A 使用同個查詢條件所得到的筆數，和第二次查詢得到的筆數不相同（可能 Transaction B 新增或刪減)


| Transaction A                                       | Transaction B                            |
| --------------------------------------------------- | ---------------------------------------- |
| SELECT COUNT(\*) AS count FROM Table /\*count=1 \*/ |                                          |
|                                                     | INSERT INTO Table VALUES(2, 200); COMMIT;|
|SELECT COUNT(\*) AS count FROM Table
/\*count=2 \[(id=1, price=100),(id=1, price=100)] \*/ |                                          |

如何解決併行問題？
第一直覺會想到不要併行就好了，一筆一筆 transaction 依序執行就好了，但是這樣遇到高流量的狀況，系統就會當掉了...，因此更好的方式就是因應不同需求使用**隔離層級**。

## 隔離層級(Transaction Isolation Level)
由嚴謹到寬鬆


| 隔離層級               | 介紹 | 不可解決的問題 |
| ---------------------- | -------- | -------- |
| 1. **Read Uncommited** | 會讀取到別的 transaction 尚未 commit 的內容。transaction B 交易更新但還沒 commit 的內容被 rollback，transaction A 會讀取到取消的內容，但「更新」的操作不會成功 | 髒讀、不可重複讀、幻讀     |
| 2. **Read Committed**  |只允許讀到 commit 過的內容，不允許讀取尚未 commit 的內容|不可重複讀、幻讀|
| 3. **Repeatable Read** |讀取中內容會被鎖住，除非自己修改，不然對資料查詢的結果都相同|幻讀|
| 4. **Serializable** |transaction 依序執行，避免發生併行所發生的問題||



---

參考資料：

https://fauna.com/blog/database-transaction

https://ithelp.ithome.com.tw/articles/10246319
