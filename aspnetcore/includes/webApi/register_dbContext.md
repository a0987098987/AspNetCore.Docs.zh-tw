## <a name="register-the-database-context"></a>登錄資料庫內容

為了將資料庫內容插入至控制器中，我們需要向[相依性插入](xref:fundamentals/dependency-injection)容器登錄資料庫內容。 使用內建的[相依性插入](xref:fundamentals/dependency-injection)支援，向服務容器登錄資料庫內容。 以下列內容取代 *Startup.cs* 檔案的內容：

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

上述程式碼：

* 移除我們不會使用的程式碼。
* 指定將記憶體內部資料庫插入服務容器中。
