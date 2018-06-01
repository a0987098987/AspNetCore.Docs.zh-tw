---
title: 建立 ASP.NET Core 應用程式與受保護的授權的使用者資料
author: rick-anderson
description: 了解如何建立 Razor 頁面的應用程式與受保護的授權的使用者資料。 包含 HTTPS、 驗證、 安全性、 ASP.NET Core 身分識別。
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 36475776966cfb0cb3bb40477798f6e24df9725d
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688448"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>建立 ASP.NET Core 應用程式與受保護的授權的使用者資料

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)

本教學課程會示範如何建立 ASP.NET Core web 應用程式與受保護的授權的使用者資料。 它會顯示的驗證 （登錄） 的使用者的連絡人清單已建立。 有三個安全性群組：

* **註冊使用者**就可以檢視所有已認可的資料，而且可以編輯/刪除自己的資料。
* **管理員**可以核准或拒絕連絡資料。 只有已核准的連絡人會對使用者顯示。
* **系統管理員**可以核准/拒絕並編輯/刪除任何資料。

在下列影像中，使用者 Rick (`rick@example.com`) 登入。 Rick 只能檢視核准的連絡人和**編輯**/**刪除**/**新建**他連絡人的連結。 只有最後一筆記錄，建立由 Rick，顯示**編輯**和**刪除**連結。 除非管理員或系統管理員將狀態變更為 「 已核准 」 其他使用者看最後一筆記錄。

![前面所述的映像](secure-data/_static/rick.png)

在下圖`manager@contoso.com`已登入，並在管理員角色：

![前面所述的映像](secure-data/_static/manager1.png)

下圖顯示在管理員的連絡人詳細資料檢視：

![前面所述的映像](secure-data/_static/manager.png)

**核准**和**拒絕**按鈕只會顯示管理員和系統管理員。

在下圖`admin@contoso.com`已登入，並在系統管理員角色：

![前面所述的映像](secure-data/_static/admin.png)

系統管理員將擁有所有權限。 她可以讀取/編輯/刪除任何連絡人，變更連絡人的狀態。

應用程式所建立[scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)下列`Contact`模型：

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

這個範例包含下列 「 授權 」 處理常式：

* `ContactIsOwnerAuthorizationHandler`： 可確保使用者只能編輯其資料。
* `ContactManagerAuthorizationHandler`： 允許經理核准或拒絕的連絡人。
* `ContactAdministratorsAuthorizationHandler`： 可讓系統管理員核准或拒絕的連絡人，以及編輯/刪除連絡人。

## <a name="prerequisites"></a>必要條件

本教學課程會前進。 您應該熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [驗證](xref:security/authentication/index)
* [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [授權](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

請參閱[此 PDF 檔案](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf)ASP.NET Core MVC 版本。 本教學課程中的 ASP.NET Core 1.1 版本處於[這](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data)資料夾。 ASP.NET Core 範例處於 1.1[範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)。

## <a name="the-starter-and-completed-app"></a>起始和已完成的應用程式

[下載](xref:tutorials/index#how-to-download-a-sample)[完成](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)應用程式。 [測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。

### <a name="the-starter-app"></a>起始應用程式

[下載](xref:tutorials/index#how-to-download-a-sample)[入門](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2)應用程式。

執行應用程式，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。

## <a name="secure-user-data"></a>保護使用者資料

下列各節將有建立安全的使用者資料的應用程式的主要步驟。 您可能會發現很有幫助完成的專案參考。

### <a name="tie-the-contact-data-to-the-user"></a>將繫結到使用者的連絡資料

使用 ASP.NET[識別](xref:security/authentication/identity)的使用者識別碼以確保使用者可以編輯其資料，但沒有其他使用者資料。 新增`OwnerID`和`ContactStatus`至`Contact`模型：

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` 這是使用者的識別碼，從`AspNetUser`資料表中[識別](xref:security/authentication/identity)資料庫。 `Status`欄位可讓您判斷是否為一般使用者可以檢視連絡人。

建立新的移轉，並更新資料庫：

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a>需要 HTTPS 和已驗證的使用者

新增[IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment)至`Startup`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

在`ConfigureServices`方法*Startup.cs* file、 add [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute)授權篩選條件：

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

如果您使用 Visual Studio，請啟用 HTTPS。

若要將 HTTP 要求重新導向至 HTTPS，請參閱[URL 重寫中介軟體](xref:fundamentals/url-rewriting)。 若您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，為 HTTPS:

  設定`"LocalTest:skipSSL": true`中*appsettings。Developement.json*檔案。

### <a name="require-authenticated-users"></a>需要已驗證的使用者

設定為需要驗證使用者的預設驗證原則。 您可以選擇不使用 Razor 頁面、 控制器或動作的方法層級驗證`[AllowAnonymous]`屬性。 設定為需要驗證使用者的預設驗證原則能保護新加入的 Razor 頁面和控制站。 具有所需的預設驗證比上新的控制站及 Razor 頁面，以包含安全`[Authorize]`屬性。 

與驗證，所有使用者的需求[AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_)和[AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0)呼叫並非必要項。

更新`ConfigureServices`以下列變更：

* 標記為註解`AuthorizeFolder`和`AuthorizePage`。
* 設定為需要驗證使用者的預設驗證原則。

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

新增[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)索引，因此他們註冊之前，匿名使用者可以取得站台的相關資訊的相關，以及連絡頁面。 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

新增`[AllowAnonymous]`至[LoginModel 和 RegisterModel](https://github.com/aspnet/templating/issues/238)。

### <a name="configure-the-test-account"></a>設定測試帳戶

`SeedData`類別會建立兩個帳戶： 系統管理員和管理員。 使用[密碼管理員工具](xref:security/app-secrets)設定這些帳戶的密碼。 從專案目錄設定密碼 (目錄包含*Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

更新`Main`使用測試密碼：

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>建立測試帳戶，並更新連絡人

更新`Initialize`方法中的`SeedData`類別來建立測試帳戶：

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

新增系統管理員使用者識別碼和`ContactStatus`至連絡人。 請的連絡人 」 已送出 」 和一個 「 已拒絕 」。 加入所有連絡人的使用者識別碼和狀態。 只能有一個連絡人所示：

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>建立擁有者、 管理員和系統管理員授權的處理常式

建立`ContactIsOwnerAuthorizationHandler`類別*授權*資料夾。 `ContactIsOwnerAuthorizationHandler`確認作用於資源的使用者擁有的資源。

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼叫[內容。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人的擁有者。 授權的處理常式通常：

* 傳回`context.Succeed`有符合的需求。
* 傳回`Task.CompletedTask`時並不符合需求。 `Task.CompletedTask` 都不成功或失敗&mdash;它可讓執行其他授權的處理常式。

如果您需要明確地使失敗，傳回[內容。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。

應用程式可讓連絡人的擁有者可以編輯/刪除/建立自己的資料。 `ContactIsOwnerAuthorizationHandler` 不需要檢查需求參數中的作業。

### <a name="create-a-manager-authorization-handler"></a>建立授權管理員的處理常式

建立`ContactManagerAuthorizationHandler`類別*授權*資料夾。 `ContactManagerAuthorizationHandler`確認作用於資源的使用者是管理員。 只有經理可以核准或拒絕內容變更 （新增或變更）。

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>建立的系統管理員授權的處理常式

建立`ContactAdministratorsAuthorizationHandler`類別*授權*資料夾。 `ContactAdministratorsAuthorizationHandler`確認作用於資源的使用者是系統管理員。 系統管理員可以執行所有作業。

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>註冊授權的處理常式

使用 Entity Framework 的核心服務必須登錄[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。 `ContactIsOwnerAuthorizationHandler`使用 ASP.NET Core[識別](xref:security/authentication/identity)，這建置在 Entity Framework Core。 登錄處理常式與服務的集合，所以可`ContactsController`透過[相依性插入](xref:fundamentals/dependency-injection)。 將下列程式碼加入至結尾`ConfigureServices`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`會新增為 singleton。 它們是 singleton，因為它們不使用 EF 和所需的資訊位於`Context`參數`HandleRequirementAsync`方法。

## <a name="support-authorization"></a>支援授權

在本節中，您可以更新 Razor 頁面，並將作業需求類別加入。

### <a name="review-the-contact-operations-requirements-class"></a>檢閱連絡人的作業需求類別

檢閱`ContactOperations`類別。 這個類別包含的需求，應用程式支援：

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>建立 Razor 頁面的基底類別

建立包含連絡人 Razor 頁面中會使用服務的基底類別。 基底類別會將該初始化程式碼在同一個位置：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

上述程式碼：

* 新增`IAuthorizationService`服務存取授權的處理常式。
* 新增身分識別`UserManager`服務。
* 加入 `ApplicationDbContext`。

### <a name="update-the-createmodel"></a>更新 CreateModel

更新建立頁面模型建構函式使用`DI_BasePageModel`基底類別：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

更新`CreateModel.OnPostAsync`方法：

* 新增使用者識別碼`Contact`模型。
* 呼叫此授權處理常式，以確定使用者有權限來建立的連絡人。

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>更新 IndexModel

更新`OnGetAsync`方法，使只有核准的連絡人會向一般使用者顯示：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>更新 EditModel

新增授權處理常式來驗證使用者擁有的連絡人。 正在驗證資源授權，因為`[Authorize]`屬性不足夠。 應用程式沒有存取資源，要在評估屬性時。 必須命令式資源為基礎的授權。 一旦應用程式在載入頁面模型或是載入在處理常式本身擁有資源的存取權，就必須執行檢查。 您經常存取的資源，藉由傳遞資源索引鍵。

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>更新 DeleteModel

更新刪除頁面模型，以確認使用者擁有連絡人 delete 權限使用授權的處理常式。

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>授權服務插入檢視

目前，UI 顯示編輯和刪除使用者無法修改的資料的連結。 藉由套用授權的處理常式來檢視，UI 被固定的。

插入中的授權服務*Views/_ViewImports.cshtml*檔案，因此可使用的所有檢視：

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

上述標記加入了許多`using`陳述式。

更新**編輯**和**刪除**中連結*Pages/Contacts/Index.cshtml*讓它們只呈現具有適當的權限的使用者：

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> 隱藏不需要變更資料的權限的使用者連結，並不安全應用程式。 隱藏連結，讓應用程式更容易使用顯示唯一有效的連結。 使用者可以 hack 叫用 編輯和刪除作業沒有自己的資料產生的 Url。 Razor 頁面或控制站必須強制執行存取檢查，以確保資料的安全。

### <a name="update-details"></a>更新詳細資料

更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

更新詳細資料頁面模型：

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>測試已完成的應用程式

若您是使用 Visual Studio 程式碼，或在本機的平台上測試，不包含測試憑證，為 HTTPS:

* 設定`"LocalTest:skipSSL": true`中*appsettings。Developement.json*檔案以略過的 HTTPS 要求。 略過只在開發電腦上的 HTTPS。

如果應用程式的連絡人：

* 刪除所有記錄的`Contact`資料表。
* 重新啟動植入資料庫的應用程式。

註冊使用者瀏覽的連絡人。

若要測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 版本）。 在一個瀏覽器中註冊新的使用者 (例如， `test@example.com`)。 以不同的使用者登入每個瀏覽器。 請確認下列作業：

* 已註冊的使用者可以檢視所有已核准的連絡資料。
* 已註冊的使用者可以編輯/刪除自己的資料。
* 經理可以核准或拒絕連絡資料。 `Details`檢視會顯示**核准**和**拒絕**按鈕。
* 系統管理員可以核准/拒絕及編輯/刪除任何資料。

| 使用者| 選項 |
| ------------ | ---------|
| test@example.com | 可以編輯/刪除自己的資料 |
| manager@contoso.com | 可以核准/拒絕和編輯/刪除擁有資料 |
| admin@contoso.com | 可以編輯/刪除並核准/拒絕的所有資料|

系統管理員的瀏覽器中建立的連絡人。 複製的 URL 刪除和編輯從系統管理員連絡。 這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。

## <a name="create-the-starter-app"></a>建立起始應用程式

* 建立名為"ContactManager"Razor 網頁應用程式

  * 建立應用程式與**個別使用者帳戶**。
  * 將類別命名"ContactManager 」 讓您的命名空間符合範例中使用的命名空間。

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * `-uld` 指定 LocalDB，而不是 SQLite

* 加入下列`Contact`模型：

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Scaffold`Contact`模型：

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* 更新**ContactManager**錨定在*Pages/_Layout.cshtml*檔案：

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* 為初始移轉建立結構，並更新資料庫：

```console
dotnet ef migrations add initial
dotnet ef database update
```

* 測試應用程式所建立、 編輯和刪除連絡人

### <a name="seed-the-database"></a>植入資料庫

新增`SeedData`類別*資料*資料夾。 如果您已經下載範例，您可以複製*SeedData.cs*檔案*資料*入門專案的資料夾。

呼叫`SeedData.Initialize`從`Main`:

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

測試應用程式植入資料庫。 請連絡資料庫中有任何資料列，如果種子不執行方法。

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>其他資源

* [ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。 這個實驗室會進入這個教學課程中介紹的安全性功能的更多詳細資料。
* [在 ASP.NET Core 授權： 簡單、 宣告式和自訂的角色](xref:security/authorization/index)
* [自訂原則式授權](xref:security/authorization/policies)
