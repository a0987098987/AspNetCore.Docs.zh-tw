<span data-ttu-id="e02df-101">產生的識別資料庫程式碼需要[Entity Framework Core 移轉](/ef/core/managing-schemas/migrations/)。</span><span class="sxs-lookup"><span data-stu-id="e02df-101">The generated Identity database code requires [Entity Framework Core Migrations](/ef/core/managing-schemas/migrations/).</span></span> <span data-ttu-id="e02df-102">建立移轉並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="e02df-102">Create a migration and update the database.</span></span> <span data-ttu-id="e02df-103">例如，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e02df-103">For example, run the following commands:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e02df-104">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e02df-104">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e02df-105">在 Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="e02df-105">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CreateIdentitySchema
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e02df-106">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e02df-106">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
```

---

<span data-ttu-id="e02df-107">「 CreateIdentitySchema"name 參數，如`Add-Migration`是任意的命令。</span><span class="sxs-lookup"><span data-stu-id="e02df-107">The "CreateIdentitySchema" name parameter for the `Add-Migration` command is arbitrary.</span></span> <span data-ttu-id="e02df-108">`"CreateIdentitySchema"` 描述如何移轉。</span><span class="sxs-lookup"><span data-stu-id="e02df-108">`"CreateIdentitySchema"` describes the migration.</span></span>
