產生的身分識別資料庫程式碼需要[Entity Framework Core 遷移](/ef/core/managing-schemas/migrations/)。 建立遷移並更新資料庫。 例如，執行下列命令：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [Visual Studio**套件管理員主控台**] 中：

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

`Add-Migration` 命令的 "CreateIdentitySchema" 名稱參數是任意的。 `"CreateIdentitySchema"` 說明遷移。
