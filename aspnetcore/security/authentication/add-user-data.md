---
title: 在 ASP.NET Core 專案中新增、下載及刪除使用者資料至身分識別
author: rick-anderson
description: 瞭解如何將自訂使用者資料新增至 ASP.NET Core 專案中的身分識別。 刪除每個 GDPR 的資料。
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 7a67f55da0e685ed3fd5badb30e8be683411a5ae
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662684"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>在 ASP.NET Core 專案中新增、下載和刪除自訂使用者資料至身分識別

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

本文將說明如何：

* 將自訂使用者資料新增至 ASP.NET Core web 應用程式。
* 將具有 <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> 屬性的自訂使用者資料模型標記為可自動供下載和刪除。 讓資料能夠下載和刪除有助於符合[GDPR](xref:security/gdpr)需求。

專案範例是從 Razor Pages web 應用程式建立的，但這些指示類似于 ASP.NET Core MVC web 應用程式。

[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisites

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] >  [專案]。 如果您想要符合[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。
* 選取 [ **ASP.NET Core Web 應用程式**>**確定]**
* 選取下拉式清單中的 [ **ASP.NET Core 3.0** ]
* 選取 [ **Web 應用程式** **] > [確定]**
* 建置並執行專案。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] >  [專案]。 如果您想要符合[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data)程式碼的命名空間，請將專案命名為**WebApp1** 。
* 選取 [ **ASP.NET Core Web 應用程式**>**確定]**
* 選取下拉式清單中的 [ **ASP.NET Core 2.2** ]
* 選取 [ **Web 應用程式** **] > [確定]**
* 建置並執行專案。

::: moniker-end


# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>執行身分識別 scaffolder

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在**方案總管**中，以滑鼠右鍵按一下專案 >**加入** > **新的 scaffold 專案**。
* 從 [**新增 Scaffold** ] 對話方塊的左窗格中，選取 [身分**識別** > **新增**]。
* 在 [**新增識別**] 對話方塊中，有下列選項：
  * 選取現有的版面配置檔案 *~/Pages/Shared/_Layout. cshtml*
  * 選取要覆寫的下列檔案：
    * **帳戶/註冊**
    * **帳戶/管理/索引**
  * 選取 [ **+** ] 按鈕，以建立新的**資料內容類別**。 如果專案名為**WebApp1**，請接受類型（**WebApp1. WebApp1CoNtext** ）。
  * 選取 [ **+** ] 按鈕，以建立新的**使用者類別**。 接受類型（如果專案名為**WebApp1**，則為**WebApp1User** ） >**新增**。
* 選取 [新增]。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果您先前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

將 Nswag.codegeneration.csharp 的套件參考新增至專案（.csproj）檔案。（ [VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) ）。 在專案目錄中執行下列命令：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令以列出身分識別 scaffolder 選項：

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

在專案資料夾中，執行 身分識別 scaffolder：

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

依照 [遷移] [、[UseAuthentication] 和 [版面](xref:security/authentication/scaffold-identity#efm)配置] 中的指示執行下列步驟：

* 建立遷移並更新資料庫。
* 將 `UseAuthentication` 新增至 `Startup.Configure`。
* 將 `<partial name="_LoginPartial" />` 新增至版面配置檔案。
* 測試應用程式：
  * 註冊使用者
  * 選取新的使用者名稱（[**登出**] 連結旁）。 您可能需要展開視窗，或選取 [導覽列] 圖示，以顯示使用者名稱和其他連結。
  * 選取 [**個人資料**] 索引標籤。
  * 選取 [**下載**] 按鈕，並檢查*PersonalData*檔案。
  * 測試**刪除**按鈕，這會刪除已登入的使用者。

## <a name="add-custom-user-data-to-the-identity-db"></a>將自訂使用者資料新增至身分識別 DB

以自訂屬性更新 `IdentityUser` 的衍生類別。 如果您將專案命名為 WebApp1，檔案就會命名為*Areas/Identity/Data/WebApp1User .cs*。 使用下列程式碼更新檔案：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

具有[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute)屬性的屬性如下：

* 當 [*區域/身分識別/頁面/帳戶/管理/DeletePersonalData* ] Razor 頁面呼叫 `UserManager.Delete`時刪除。
* 包含在下載的資料中，依*區域/身分識別/頁面/帳戶/管理/DownloadPersonalData. cshtml* Razor 頁面。

### <a name="update-the-accountmanageindexcshtml-page"></a>更新 [帳戶/管理/索引. cshtml] 頁面

使用下列反白顯示的程式碼，更新 [*區域/身分識別/頁面/帳戶/管理/索引*] 中的 `InputModel`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

以下列反白顯示的標記，更新*區域/身分識別/頁面/帳戶/管理/索引. cshtml* ：

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

以下列反白顯示的標記，更新*區域/身分識別/頁面/帳戶/管理/索引. cshtml* ：

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>更新 [帳戶/註冊. cshtml] 頁面

使用下列反白顯示的程式碼，更新*區域/身分識別/頁面/帳戶/登錄*中的 `InputModel`：

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

以下列醒目提示的標記更新 [*區域/身分識別/頁面/帳戶/* 暫存器]：

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

以下列醒目提示的標記更新 [*區域/身分識別/頁面/帳戶/* 暫存器]：

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


建置專案。

### <a name="add-a-migration-for-the-custom-user-data"></a>新增自訂使用者資料的遷移

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [Visual Studio**套件管理員主控台**] 中：

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>測試建立、查看、下載、刪除自訂使用者資料

測試應用程式：

* 註冊新的使用者。
* 在 [`/Identity/Account/Manage`] 頁面上，查看自訂使用者資料。
* 從 [`/Identity/Account/Manage/PersonalData`] 頁面下載並查看使用者個人資料。
