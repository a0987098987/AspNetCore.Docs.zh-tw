---
title: 加入、 下載及刪除身分識別的 ASP.NET Core 專案中的使用者資料
author: rick-anderson
description: 了解如何在 ASP.NET Core 專案中加入身分識別的自訂使用者資料。 刪除每 GDPR 的資料。
ms.author: riande
ms.date: 6/16/2018
ms.custom: seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 8c0413a16d92b717619387748ee78f0d14d6c852
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59516205"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>加入、 下載及刪除身分識別的 ASP.NET Core 專案的自訂使用者資料

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

本文說明如何：

* 新增至 ASP.NET Core web 應用程式的自訂使用者資料。
* 裝飾具有自訂使用者資料模型[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性，讓它自動可供下載和刪除。 無法下載及刪除資料可協助符合[GDPR](xref:security/gdpr)需求。

Razor 頁面 web 應用程式，從建立專案範例，但 ASP.NET Core MVC web 應用程式的指示如下。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/2.2-SDK.md)]

## <a name="create-a-razor-web-app"></a>建立 Razor Web 應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。 將專案命名為**WebApp1**如果您要符合的命名空間[下載範例](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample)程式碼。
* 選取  **ASP.NET Core Web 應用程式** > **確定**
* 選取  **ASP.NET Core 2.2**下拉式清單中
* 選取  **Web 應用程式**  > **確定**
* 建置並執行專案。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>執行身分識別框架

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。
* 從左窗格**新增 Scaffold**對話方塊中，選取**識別** > **新增**。
* 在  **ADD 身分識別**對話方塊中，下列選項：
  * 選取現有的版面配置檔 *~/Pages/Shared/_Layout.cshtml*
  * 選取要覆寫下列檔案：
    * **帳戶/註冊**
    * **帳戶/管理/索引**
  * 選取  **+** 來建立新的按鈕**資料內容類別**。 接受的型別 (**WebApp1.Models.WebApp1Context**如果專案名為**WebApp1**)。
  * 選取  **+** 來建立新的按鈕**使用者類別**。 接受的型別 (**WebApp1User**如果專案名為**WebApp1**) >**新增**。
* 選取 **新增**。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

新增的套件參考[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)專案 (.csproj) 檔案。 在專案目錄中，執行下列命令：

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令來列出識別 scaffolder 選項：

```cli
dotnet aspnet-codegenerator identity -h
```

在 [專案] 資料夾中，執行身分識別框架：

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

請依照下列中的指示[移轉、 UseAuthentication 和版面配置](xref:security/authentication/scaffold-identity#efm)來執行下列步驟：

* 建立移轉並更新資料庫。
* 將 `UseAuthentication` 加入 `Startup.Configure`。
* 新增`<partial name="_LoginPartial" />`和配置檔案。
* 測試應用程式：
  * 註冊使用者
  * 選取新的使用者名稱 (旁**登出**連結)。 您可能需要展開視窗，或選取 瀏覽列圖示以顯示使用者名稱和其他連結。
  * 選取 [**個人資料**] 索引標籤。
  * 選取 **下載**按鈕，然後檢查*PersonalData.json*檔案。
  * 測試**刪除**按鈕，這會刪除登入的使用者。

## <a name="add-custom-user-data-to-the-identity-db"></a>識別資料庫中加入自訂的使用者資料

更新`IdentityUser`衍生的類別具有自訂屬性。 如果您名為 WebApp1 的專案，檔案會命名為*Areas/Identity/Data/WebApp1User.cs*。 使用下列程式碼中更新檔案：

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Data/WebApp1User.cs)]

以屬性裝飾[PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1)屬性：

* 刪除*Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor 頁面呼叫`UserManager.Delete`。
* 包含在所下載的資料*Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor 頁面。

### <a name="update-the-accountmanageindexcshtml-page"></a>更新 Account/Manage/Index.cshtml 頁面

更新`InputModel`中*Areas/Identity/Pages/Account/Manage/Index.cshtml.cs*下列醒目標示的程式碼：

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

更新*Areas/Identity/Pages/Account/Manage/Index.cshtml*下列反白顯示的標記：

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

### <a name="update-the-accountregistercshtml-page"></a>更新 Account/Register.cshtml 頁面

更新`InputModel`中*Areas/Identity/Pages/Account/Register.cshtml.cs*下列醒目標示的程式碼：

[!code-csharp[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

更新*Areas/Identity/Pages/Account/Register.cshtml*下列反白顯示的標記：

[!code-html[Main](add-user-data/sample-2.2/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

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

---

## <a name="test-create-view-download-delete-custom-user-data"></a>測試建立、 檢視、 下載、 刪除自訂使用者資料

測試應用程式：

* 註冊新的使用者。
* 檢視上的自訂使用者資料`/Identity/Account/Manage`頁面。
* 下載並檢視中的使用者個人資料`/Identity/Account/Manage/PersonalData`頁面。
