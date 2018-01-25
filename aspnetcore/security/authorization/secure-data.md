---
title: "建立 ASP.NET Core 應用程式與受保護的授權的使用者資料"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 7404b8ec20ed6a00554c8a7ade9a282362b9a186
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>建立 ASP.NET Core 應用程式與受保護的授權的使用者資料

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)

本教學課程會示範如何建立 web 應用程式與受保護的授權的使用者資料。 它會顯示的驗證 （登錄） 的使用者的連絡人清單已建立。 有三個安全性群組：

* 已註冊的使用者可以檢視所有已核准的連絡資料。
* 已註冊的使用者可以編輯/刪除自己的資料。 
* 經理可以核准或拒絕連絡資料。 只有已核准的連絡人會對使用者顯示。
* 系統管理員可以核准/拒絕及編輯/刪除任何資料。

在下列影像中，使用者 Rick (`rick@example.com`) 登入。 使用者 Rick 只能檢視核准連絡人和編輯/刪除他的連絡人。 只有最後一筆記錄，建立由 Rick、 顯示編輯和刪除連結

![上面所述的映像](secure-data/_static/rick.png)

在下圖`manager@contoso.com`已登入，並在管理員角色。 

![上面所述的映像](secure-data/_static/manager1.png)

下圖顯示在管理員的連絡人詳細資料檢視。

![上面所述的映像](secure-data/_static/manager.png)

只有管理員和系統管理員已核准和拒絕按鈕。

在下圖`admin@contoso.com`已登入，並在系統管理員的角色。 

![上面所述的映像](secure-data/_static/admin.png)

系統管理員將擁有所有權限。 她可以讀取/編輯/刪除任何連絡人，變更連絡人的狀態。

應用程式所建立[scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)下列`Contact`模型：

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

A`ContactIsOwnerAuthorizationHandler`授權的處理常式可確保使用者只能編輯其資料。 A`ContactManagerAuthorizationHandler`授權的處理常式可讓管理員核准或拒絕的連絡人。  A`ContactAdministratorsAuthorizationHandler`授權的處理常式可讓系統管理員核准或拒絕的連絡人，以及編輯/刪除連絡人。 

## <a name="prerequisites"></a>必要條件

這不是開始教學課程。 您應該熟悉：

* [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>起始和已完成的應用程式

[下載](xref:tutorials/index#how-to-download-a-sample)[完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final)應用程式。 [測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。 

### <a name="the-starter-app"></a>起始應用程式

最好先比較已完成的範例程式碼。

[下載](xref:tutorials/index#how-to-download-a-sample)[入門](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter)應用程式。 

請參閱[建立入門應用程式](#create-the-starter-app)如果您想要從頭開始建立它。

更新資料庫：

```none
   dotnet ef database update
```

執行應用程式，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。

本教學課程已建立安全的使用者資料的應用程式的主要步驟。 您可能會發現很有幫助完成的專案參考。

## <a name="modify-the-app-to-secure-user-data"></a>修改應用程式，以保護使用者資料

下列各節將有建立安全的使用者資料的應用程式的主要步驟。 您可能會發現很有幫助完成的專案參考。

### <a name="tie-the-contact-data-to-the-user"></a>將繫結到使用者的連絡資料

使用 ASP.NET[識別](xref:security/authentication/identity)的使用者識別碼以確保使用者可以編輯其資料，但沒有其他使用者資料。 新增`OwnerID`至`Contact`模型：

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`這是使用者的識別碼，從`AspNetUser`資料表中[識別](xref:security/authentication/identity)資料庫。 `Status`欄位可讓您判斷是否為一般使用者可以檢視連絡人。 

為新的移轉建立結構，並更新資料庫：

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>需要 SSL 和已驗證的使用者

在`ConfigureServices`方法*Startup.cs* file、 add [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute)授權篩選條件：

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

若要將 HTTP 要求重新導向至 HTTPS，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。 如果您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，針對 SSL:

- 設定`"LocalTest:skipSSL": true`中*appsettings.json*檔案。

### <a name="require-authenticated-users"></a>需要已驗證的使用者

設定為需要驗證使用者的預設驗證原則。 您可以選擇不在控制器或動作方法以驗證`[AllowAnonymous]`屬性。 使用這個方法，加入任何新的控制站會自動需要驗證，也就是比信賴憑證者上包含新的控制站安全`[Authorize]`屬性。 將下列內容加入`ConfigureServices`方法*Startup.cs*檔案：

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

新增`[AllowAnonymous]`至主控制器，因此他們註冊之前，匿名使用者可以取得站台的相關資訊。

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>設定測試帳戶

`SeedData`類別會建立兩個帳戶，系統管理員和管理員。 使用[密碼管理員工具](xref:security/app-secrets)設定這些帳戶的密碼。 從專案目錄 (directory 包含*Program.cs*)。

```console
dotnet user-secrets set SeedUserPW <PW>
```

更新`Configure`使用測試密碼：

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

新增系統管理員使用者識別碼和`Status = ContactStatus.Approved`至連絡人。 只能有一個連絡人會顯示，可加入的使用者識別碼的所有連絡人：

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>建立擁有者、 管理員和系統管理員授權的處理常式

建立`ContactIsOwnerAuthorizationHandler`類別*授權*資料夾。 `ContactIsOwnerAuthorizationHandler`會確認作用於資源的使用者擁有的資源。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼叫`context.Succeed`如果目前已驗證的使用者是連絡人的擁有者。 授權的處理常式通常會傳回`context.Succeed`有符合的需求。 它們會傳回`Task.FromResult(0)`時不符合需求。 `Task.FromResult(0)`不是成功或失敗，它可讓執行其他授權處理常式。 如果您需要明確地使失敗，傳回`context.Fail()`。

我們可以連絡的擁有者可以編輯/刪除自己的資料，所以我們不需要檢查需求參數中的作業。

### <a name="create-a-manager-authorization-handler"></a>建立授權管理員的處理常式

建立`ContactManagerAuthorizationHandler`類別*授權*資料夾。 `ContactManagerAuthorizationHandler`會作用於資源的使用者是管理員確認。 只有經理可以核准或拒絕內容變更 （新增或變更）。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>建立的系統管理員授權的處理常式

建立`ContactAdministratorsAuthorizationHandler`類別*授權*資料夾。 `ContactAdministratorsAuthorizationHandler`會確認使用者資源上做為系統管理員。 系統管理員可以執行所有作業。

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>註冊授權的處理常式

使用 Entity Framework 的核心服務必須登錄[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。 `ContactIsOwnerAuthorizationHandler`使用 ASP.NET Core[識別](xref:security/authentication/identity)，這建置在 Entity Framework Core。 登錄處理常式與服務的集合，以便將可用的它們`ContactsController`透過[相依性插入](xref:fundamentals/dependency-injection)。 將下列程式碼加入至結尾`ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`和`ContactManagerAuthorizationHandler`會新增為 singleton。 它們是 singleton，因為它們不使用 EF 和所需的資訊位於`Context`參數`HandleRequirementAsync`方法。

完整`ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>更新以支援授權的程式碼

在本節中，您會更新控制器和檢視，並加入作業需求類別。

### <a name="update-the-contacts-controller"></a>更新連絡人控制站

更新`ContactsController`建構函式：

* 新增`IAuthorizationService`服務存取授權的處理常式。 
* 新增`Identity``UserManager`服務：

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>加入連絡人的作業需求類別

新增`ContactOperations`類別*授權*資料夾。 這個類別包含需求我們的應用程式支援：

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>建立更新

更新`HTTP POST Create`方法：

* 新增使用者識別碼`Contact`模型。
* 呼叫以確認該使用者所擁有之連絡人的授權處理常式。

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>更新編輯

更新`Edit`用來驗證使用者的授權的處理常式方法擁有的連絡人。 因為我們要執行的資源授權，無法使用`[Authorize]`屬性。 要在評估屬性時，我們不需要資源的存取權。 根據資源授權必須是命令性。 一旦載入中我們控制站，或是載入在處理常式本身都可存取資源，必須執行檢查。 通常您會藉由傳遞資源索引鍵存取資源。

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>更新 Delete 方法

更新`Delete`用來驗證使用者的授權的處理常式方法擁有的連絡人。

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>授權服務插入檢視

目前 UI 顯示編輯和刪除使用者無法修改的資料的連結。 我們將修正，藉由套用授權的處理常式來檢視。

插入中的授權服務*Views/_ViewImports.cshtml*檔案，因此將予以提供至所有的檢視：

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

更新*Views/Contacts/Index.cshtml* Razor 檢視，只顯示編輯，並刪除連結的使用者可以編輯/刪除連絡人。

新增`@using ContactManager.Authorization;`

更新`Edit`和`Delete`連結，讓它們只呈現為具有權限的使用者編輯或刪除連絡人。

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

警告： 隱藏連結之使用者無權編輯或刪除資料的不安全的應用程式。 隱藏連結，讓應用程式更多的使用者易記顯示唯一有效的連結。 使用者可以 hack 叫用 編輯和刪除作業沒有自己的資料產生的 Url。  控制器必須是安全的存取檢查重複的。

### <a name="update-the-details-view"></a>更新詳細資料檢視

更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>測試已完成的應用程式

如果您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，針對 SSL:

- 設定`"LocalTest:skipSSL": true`中*appsettings.json*檔案。

如果您有執行應用程式，並以連絡人，刪除所有記錄的`Contact`資料表，並重新啟動植入資料庫應用程式。 如果您使用 Visual Studio，您要結束並重新啟動 IIS Express 種子資料庫。

註冊使用者瀏覽的連絡人。

若要測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 版本）。 在一個瀏覽器中註冊新的使用者，例如`test@example.com`。 以不同的使用者登入每個瀏覽器。 驗證下列各項：

* 已註冊的使用者可以檢視所有已核准的連絡資料。
* 已註冊的使用者可以編輯/刪除自己的資料。 
* 經理可以核准或拒絕連絡資料。 `Details`檢視會顯示**核准**和**拒絕**按鈕。 
* 系統管理員可以核准/拒絕及編輯/刪除任何資料。

| 使用者| 選項 |
| ------------ | ---------|
| test@example.com | 可以編輯/刪除自己的資料 |
| manager@contoso.com | 可以核准/拒絕和編輯/刪除擁有資料  |
| admin@contoso.com | 可以編輯/刪除並核准/拒絕的所有資料|

系統管理員的瀏覽器中建立的連絡人。 複製的 URL 刪除和編輯從系統管理員連絡。 這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。

## <a name="create-the-starter-app"></a>建立起始應用程式

請遵循這些指示來建立起始應用程式。

* 建立**ASP.NET Core Web 應用程式**使用[Visual Studio 2017](https://www.visualstudio.com/)名為"ContactManager"

  * 建立應用程式與**個別使用者帳戶**。
  * 將類別命名"ContactManager 」 讓您的命名空間會比對命名空間中的使用範例。

* 加入下列`Contact`模型：

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Scaffold`Contact`模型使用 Entity Framework Core 和`ApplicationDbContext`資料內容。 接受所有 scaffolding 預設值。 使用`ApplicationDbContext`資料內容類別將 contact 資料表放入[識別](xref:security/authentication/identity)資料庫。 請參閱[加入模型](xref:tutorials/first-mvc-app/adding-model)如需詳細資訊。

* 更新**ContactManager**錨定在*Views/Shared/_Layout.cshtml*檔案從`asp-controller="Home"`至`asp-controller="Contacts"`因此點選**ContactManager**連結會叫用的連絡人控制站。 原始的標記：

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

更新的標記：

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* 為初始移轉建立結構，並更新資料庫

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* 測試應用程式所建立、 編輯和刪除連絡人

### <a name="seed-the-database"></a>植入資料庫

新增`SeedData`類別*資料*資料夾。 如果您已經下載範例，您可以複製*SeedData.cs*檔案*資料*入門專案的資料夾。

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

將反白顯示的程式碼的結尾加入`Configure`方法中的*Startup.cs*檔案：

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

測試應用程式植入資料庫。 請連絡資料庫中沒有任何資料列時，不執行 seed 方法。

### <a name="create-a-class-used-in-the-tutorial"></a>建立本教學課程中使用的類別

* 建立名為的資料夾*授權*。
* 複製*Authorization\ContactOperations.cs*檔案從已完成的專案下載，或複製下列程式碼：

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>其他資源

* [ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。 這個實驗室會進入這個教學課程中介紹的安全性功能的更多詳細資料。
* [在 ASP.NET Core 授權： 簡單、 宣告式和自訂的角色](index.md)
* [自訂原則式授權](policies.md)
