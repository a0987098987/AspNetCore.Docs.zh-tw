> <span data-ttu-id="22fb9-101">如果應用程式會使用 SQLite 做為其身分識別資料存放區，不支援某些命令。</span><span class="sxs-lookup"><span data-stu-id="22fb9-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="22fb9-102">由於資料庫引擎，`Alter`命令會擲回下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="22fb9-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="22fb9-103">「 System.NotSupportedException: SQLite 不支援此移轉作業。 」</span><span class="sxs-lookup"><span data-stu-id="22fb9-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="22fb9-104">為因應措施，請在變更資料表的資料庫上執行 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="22fb9-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
