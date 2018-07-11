# <a name="how-to-buildrun-secure-user-data-sample"></a>如何建置/執行安全的使用者資料範例

* 使用 Secret Manager 工具設定密碼：

  `dotnet user-secrets set SeedUserPW <pw>`

* 更新資料庫：

    `dotnet ef database update`

* 在專案中啟用 SSL
