# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新專案。
1. 選取 [背景**工作服務**]。 選取 [下一步]。
1. 在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。 選取 [建立]。
1. 在 [**建立新的背景工作服務**] 對話方塊中，選取 [**建立**]。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 建立新專案。
1. 在提要欄位中選取 [ **.Net Core** ] 底下的 [**應用程式**]。
1. 選取  **ASP.NET Core**下的 背景**工作角色**。 選取 [下一步]。
1. 針對**目標 Framework**選取 [ **.net Core 3.0** ] 或更新版本。 選取 [下一步]。
1. 在 [**專案名稱**] 欄位中提供名稱。 選取 [建立]。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令殼層以 `worker`dotnet new[ 命令使用背景工作服務 (](/dotnet/core/tools/dotnet-new)) 範本。 在下列範例中，已建立名為 `ContosoWorker` 的背景工作服務應用程式。 當命令執行時，會自動建立 `ContosoWorker` 應用程式的資料夾。

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
