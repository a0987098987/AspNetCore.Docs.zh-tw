# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新專案。
1. 選擇**協助服務**。 選取 [下一步]  。
1. 在 [專案名稱]**** 欄位中提供專案名稱，或接受預設專案名稱。 選取 [建立]  。
1. 在「**創建新的工作輔助服務」** 對話方塊中,選擇 **「建立**」 。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 建立新專案。
1. 在側邊列中的 **.NET 核心**下選擇**套用 。**
1. 在**ASP.NET核心**下選擇 **「輔助角色**」。 選取 [下一步]  。
1. 為目標**框架**選擇 **.NET 核心 3.0**或更高版本。 選取 [下一步]  。
1. 在 **「專案名稱」** 欄位中提供名稱。 選取 [建立]  。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

從命令殼層以 [dotnet new](/dotnet/core/tools/dotnet-new) 命令使用背景工作服務 (`worker`) 範本。 在下列範例中，已建立名為 `ContosoWorker` 的背景工作服務應用程式。 當命令執行時，會自動建立 `ContosoWorker` 應用程式的資料夾。

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
