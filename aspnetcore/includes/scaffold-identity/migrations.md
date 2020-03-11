<span data-ttu-id="ab89e-101">產生的身分識別資料庫程式碼需要[Entity Framework Core 遷移](/ef/core/managing-schemas/migrations/)。</span><span class="sxs-lookup"><span data-stu-id="ab89e-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="ab89e-102">建立遷移並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="ab89e-102">Create a migration and update the database.</span></span> <span data-ttu-id="ab89e-103">例如，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ab89e-103">For example, run the following commands:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab89e-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab89e-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ab89e-105">在 [Visual Studio**套件管理員主控台**] 中：</span><span class="sxs-lookup"><span data-stu-id="ab89e-105">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="ab89e-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ab89e-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="ab89e-107">`Add-Migration` 命令的 "CreateIdentitySchema" 名稱參數是任意的。</span><span class="sxs-lookup"><span data-stu-id="ab89e-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="ab89e-108">`"CreateIdentitySchema"` 說明遷移。</span><span class="sxs-lookup"><span data-stu-id="ab89e-108">`"CreateIdentitySchema"` describes the migration.</span></span>
