# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="633a1-101">如何建置/執行安全的使用者資料範例</span><span class="sxs-lookup"><span data-stu-id="633a1-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="633a1-102">設定密碼與密碼管理員工具：</span><span class="sxs-lookup"><span data-stu-id="633a1-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="633a1-103">更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="633a1-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="633a1-104">在專案中啟用 SSL</span><span class="sxs-lookup"><span data-stu-id="633a1-104">Enable SSL in the project</span></span>