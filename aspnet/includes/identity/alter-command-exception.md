> 如果應用程式會使用 SQLite 做為其身分識別資料存放區，不支援某些命令。 由於資料庫引擎，`Alter`命令會擲回下列例外狀況：
>
> 「 System.NotSupportedException: SQLite 不支援此移轉作業。 」 
>
> 為因應措施，請在變更資料表的資料庫上執行 Code First 移轉。
