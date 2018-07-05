產生的識別資料庫程式碼需要[Entity Framework Core 移轉](/ef/core/managing-schemas/migrations/)。 建立移轉並更新資料庫。 例如，執行下列命令：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 Visual Studio **Package Manager Console**:

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

------

「 CreateIdentitySchema"name 參數，如`Add-Migration`是任意的命令。 `"CreateIdentitySchema"` 描述如何移轉。