> 如果應用程式會使用 SQLite 作為其身分識別資料存放區，不支援某些命令。 由於限制資料庫引擎，`Alter`命令會擲回下列例外狀況：
>
> 「 System.NotSupportedException: SQLite 不支援此移轉作業。 」 
>
> 因應之道，請變更資料表在資料庫上執行 Code First 移轉。
