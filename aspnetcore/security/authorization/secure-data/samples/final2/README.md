# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="700f0-101">如何建置/執行安全的使用者資料範例</span><span class="sxs-lookup"><span data-stu-id="700f0-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="700f0-102">使用 Secret Manager 工具設定密碼：</span><span class="sxs-lookup"><span data-stu-id="700f0-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="700f0-103">更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="700f0-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="700f0-104">在專案中啟用 SSL</span><span class="sxs-lookup"><span data-stu-id="700f0-104">Enable SSL in the project</span></span>
