---
title: 加入、 下載及刪除識別 ASP.NET Core 專案中的自訂使用者資料
author: rick-anderson
description: 了解如何自訂使用者資料加入 ASP.NET Core 專案中的身分識別。 刪除每個 GDPR 資料。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: cc7b29499e9db702cab70be7c15eac53373d450d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819067"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>加入、 下載及刪除識別 ASP.NET Core 專案中的自訂使用者資料

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文將說明如何：

* 加入 ASP.NET Core web 應用程式的自訂使用者資料。
* 裝飾使用者自訂資料模型與[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性，讓自動可供下載和刪除。 無法下載及刪除資料可協助符合[GDPR](xref:security/gdpr)需求。

專案範例會建立從 Razor 頁面 web 應用程式，但的指示則類似 ASP.NET Core MVC web 應用程式。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。 將專案命名**WebApp1**如果您想要它符合命名空間[下載範例](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)程式碼。
* 選取**ASP.NET Core Web 應用程式** > **[確定]**
* 選取**ASP.NET Core 2.1**下拉式清單中
* 選取**Web 應用程式**  > **[確定]**
* 建置並執行專案。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

------

## <a name="run-the-identity-scaffolder"></a>執行身分識別 scaffolder

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**方案總管 中**，以滑鼠右鍵按一下專案 >**新增** > **新的 Scaffold 項目**。
* 從左窗格的**新增 Scaffold**對話方塊中，選取**識別** > **新增**。
* 在**新增身分識別**對話方塊中，下列選項：
  * 選取現有的版面配置檔案 *~/Pages/Shared/_Layout.cshtml*
  * 選取要覆寫的下列檔案：
    * **帳戶/註冊**
    * **帳戶/管理/索引**
  * 選取**+** 按鈕以建立新**資料內容類別**。 接受的類型 (**WebApp1.Models.WebApp1Context**如果命名專案**WebApp1**)。
  * 選取**+** 按鈕以建立新**使用者類別**。 接受的類型 (**WebApp1User**如果命名專案**WebApp1**) >**新增**。
* 選取**新增**。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果先前未安裝 ASP.NET scaffolder，請立即安裝：

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

封裝將參考加入至[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)專案 (.csproj) 檔案。 在專案目錄中，執行下列命令：

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令來列出識別 scaffolder 選項：

```cli
dotnet aspnet-codegenerator identity -h
```

在專案資料夾中，執行身分識別 scaffolder:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

請依照下列中的指示[移轉、 UseAuthentication 和版面配置](xref:security/authentication/scaffold-identity#efm)執行下列步驟：

* 建立的移轉，並更新資料庫。
* 將 `UseAuthentication` 加入 `Startup.Configure`。
* 新增`<partial name="_LoginPartial" />`配置檔案。
* 測試應用程式：
  * 註冊使用者
  * 選取新的使用者名稱 (旁**登出**連結)。 您可能需要將視窗展開或選取瀏覽列圖示來顯示使用者名稱和其他連結。
  * 選取**個人資料** 索引標籤。
  * 選取**下載**按鈕，然後檢查*PersonalData.json*檔案。
  * 測試**刪除**按鈕，這會刪除登入的使用者。

## <a name="add-custom-user-data-to-the-identity-db"></a>識別資料庫中加入自訂使用者資料

更新`IdentityUser`衍生的類別具有自訂屬性。 如果您專案 WebApp1，檔案名為*Areas/Identity/Data/WebApp1User.cs*。 下列程式碼更新檔案：

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

使用屬性來裝飾[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性：

* 刪除時*Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor 頁面呼叫`UserManager.Delete`。
* 包含在所下載的資料*Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor 頁面。

### <a name="update-the-accountmanageindexcshtml-page"></a>更新 Account/Manage/Index.cshtml 頁面

更新`InputModel`中*Areas/Identity/Pages/Account/Manage/Index.cshtml.cs*以下列反白顯示程式碼：

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95)]

更新*Areas/Identity/Pages/Account/Manage/Index.cshtml*以反白顯示下列標記：

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>更新 Account/Register.cshtml 頁面

更新`InputModel`中*Areas/Identity/Pages/Account/Register.cshtml.cs*以下列反白顯示程式碼：

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

更新*Areas/Identity/Pages/Account/Register.cshtml*以反白顯示下列標記：

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

建置專案。

### <a name="add-a-migration-for-the-custom-user-data"></a>新增自訂使用者資料移轉

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在 Visual Studio **Package Manager Console**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>測試建立、 檢視、 下載、 刪除自訂使用者資料

測試應用程式：

* 註冊新的使用者。
* 檢視上的自訂使用者資料`/Identity/Account/Manage`頁面。
* 下載並檢視中的使用者個人資料`/Identity/Account/Manage/PersonalData`頁面。
