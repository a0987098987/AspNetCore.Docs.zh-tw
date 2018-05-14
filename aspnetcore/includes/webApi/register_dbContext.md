## <a name="register-the-database-context"></a>登錄資料庫內容

在此步驟中，資料庫內容是使用[相依性插入](xref:fundamentals/dependency-injection)容器來註冊。 以相依性插入 (DI) 容器註冊的服務 (如 DB 內容) 可供控制器使用。

使用內建的[相依性插入](xref:fundamentals/dependency-injection)支援，向服務容器註冊 DB 內容。 以下列程式碼取代 *Startup.cs* 檔案的內容：

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

上述程式碼：

* 移除未使用的程式碼。
* 指定將記憶體內部資料庫插入服務容器中。