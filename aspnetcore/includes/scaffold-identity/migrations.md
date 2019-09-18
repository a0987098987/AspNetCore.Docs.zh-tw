產生的身分識別資料庫程式碼需要[Entity Framework Core 遷移](/ef/core/managing-schemas/migrations/)。 建立移轉並更新資料庫。 例如，執行下列命令：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 Visual Studio **Package Manager Console**:

```powershell
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

`Add-Migration`命令的 "CreateIdentitySchema" 名稱參數是任意的。 `"CreateIdentitySchema"`描述遷移。
