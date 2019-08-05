---
title: 建立 ASP.NET Core 應用程式與受保護的授權的使用者資料
author: rick-anderson
description: 了解如何使用受保護的授權的使用者資料建立 Razor 頁面應用程式。 包含 HTTPS、 驗證、 安全性、 ASP.NET Core 身分識別。
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 4b94cc53777308deb26521a079d8a1c2742744db
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776750"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>建立 ASP.NET Core 應用程式與受保護的授權的使用者資料

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

請參閱[此 PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) ASP.NET Core MVC 版本。 本教學課程中的 ASP.NET Core 1.1 版本處於[這](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data)資料夾。 範例 ASP.NET Core 1.1[範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

請參閱[此 pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

本教學課程會示範如何建立 ASP.NET Core web 應用程式與受保護的授權的使用者資料。 它會顯示已驗證 （登錄） 的使用者的連絡人清單中已建立。 有三個安全性群組：

* **已註冊的使用者**可以檢視所有已認可的資料並可以編輯/刪除他們自己的資料。
* **管理員**可以核准或拒絕連絡人資料。 只有已核准的連絡人會對使用者顯示。
* **系統管理員**可以核准/拒絕並編輯/刪除任何資料。

本檔中的影像完全不符合最新的範本。

在下圖中，使用者 Rick (`rick@example.com`) 登入。 Rick 只能檢視已核准的連絡人和**編輯**/**刪除**/**新建**他連絡人的連結。 只有最後一筆記錄，由 Rick，顯示**編輯**並**刪除**連結。 其他使用者不會看到最後一筆記錄，直到管理員或系統管理員的狀態變更為 「 Approved 」。

![螢幕擷取畫面顯示 Rick 登入](secure-data/_static/rick.png)

在下圖中, `manager@contoso.com`已登入並以管理員的角色執行:

![螢幕擷取畫面顯示manager@contoso.com登入](secure-data/_static/manager1.png)

下圖顯示之管理員的連絡詳細資料檢視：

![連絡人的管理員的檢視](secure-data/_static/manager.png)

**核准**並**拒絕**按鈕只會顯示為管理員和系統管理員。

在下圖中, `admin@contoso.com`已登入, 並以系統管理員角色:

![螢幕擷取畫面顯示admin@contoso.com登入](secure-data/_static/admin.png)

系統管理員將擁有所有權限。 她可以讀取/編輯/刪除的任何連絡人，並變更連絡人的狀態。

藉由建立應用程式[scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)下列`Contact`模型：

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

此範例包含下列 「 授權 」 處理常式：

* `ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。
* `ContactManagerAuthorizationHandler`：可讓管理員核准或拒絕連絡人。
* `ContactAdministratorsAuthorizationHandler`：允許系統管理員核准或拒絕連絡人, 以及編輯/刪除連絡人。

## <a name="prerequisites"></a>必要條件

本教學課程會前進。 您應該先熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [驗證](xref:security/authentication/identity)
* [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [授權](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter 和已完成的應用程式

[下載](xref:index#how-to-download-a-sample)[完成](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)應用程式。 [測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。

### <a name="the-starter-app"></a>入門應用程式

[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。

執行應用程式中，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。

## <a name="secure-user-data"></a>保護使用者資料

下列各節會有所有主要的步驟，來建立安全的使用者資料的應用程式。 您可能會發現它已完成的專案參考很有幫助。

### <a name="tie-the-contact-data-to-the-user"></a>將繫結至使用者的連絡人資料

使用 ASP.NET[識別](xref:security/authentication/identity)使用者識別碼，以確保使用者可以編輯其資料，但不是其他使用者資料。 新增`OwnerID`並`ContactStatus`到`Contact`模型：

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` 是來自使用者的識別碼`AspNetUser`資料表中[身分識別](xref:security/authentication/identity)資料庫。 `Status`欄位可讓您判斷是否可供一般使用者檢視連絡人。

建立新的移轉，並更新資料庫：

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>將角色服務新增到 身分識別

附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)來新增角色服務：

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>需要已驗證的使用者

設定預設的驗證原則，以要求使用者進行驗證：

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 您可以選擇退出的 Razor 頁面、 控制器或動作方法層級的驗證`[AllowAnonymous]`屬性。 設定預設的驗證原則，以要求使用者進行驗證，可保護新加入的 Razor 頁面和控制站。 具有預設值所需的驗證會比依賴新的控制器和 Razor 頁面，以包含更安全`[Authorize]`屬性。

將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 [索引] 和 [隱私權] 頁面, 讓匿名使用者可以在註冊之前取得網站的相關資訊。

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>設定測試帳戶

`SeedData`類別會建立兩個帳戶： 系統管理員和管理員。 使用[Secret Manager 工具](xref:security/app-secrets)設定這些帳戶的密碼。 設定密碼，從專案目錄 (目錄包含*Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

如果未指定強式密碼，則會擲回例外狀況時`SeedData.Initialize`呼叫。

更新`Main`使用測試密碼：

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>建立測試帳戶和更新連絡人

更新`Initialize`方法中的`SeedData`類別來建立測試帳戶：

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

新增系統管理員使用者識別碼和`ContactStatus`連絡人。 讓其中一個 「 連絡人 」 已提交 」 和一個 「 已拒絕 」。 加入所有連絡人的使用者識別碼和狀態。 只有一個連絡人所示：

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>建立擁有者 」、 「 管理員 」 和 「 系統管理員授權的處理常式

建立`ContactIsOwnerAuthorizationHandler`類別內*授權*資料夾。 `ContactIsOwnerAuthorizationHandler`確認處理資源的使用者擁有的資源。

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼叫[內容。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)目前已驗證的使用者是否連絡擁有者。 授權的處理常式通常：

* 傳回`context.Succeed`時符合的需求。
* 傳回`Task.CompletedTask`時不符合需求。 `Task.CompletedTask`不是成功或失敗&mdash;, 它允許其他授權處理常式執行。

如果您需要明確地使失敗，傳回[內容。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。

此應用程式會允許連絡人的擁有者可以編輯/刪除/建立自己的資料。 `ContactIsOwnerAuthorizationHandler` 不需要檢查傳入的要求參數的作業。

### <a name="create-a-manager-authorization-handler"></a>建立管理員授權的處理常式

建立`ContactManagerAuthorizationHandler`類別內*授權*資料夾。 `ContactManagerAuthorizationHandler`確認處理資源的使用者是管理員。 只有經理可以核准或拒絕內容變更 （新增或變更）。

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>建立系統管理員授權處理常式

建立`ContactAdministratorsAuthorizationHandler`類別內*授權*資料夾。 `ContactAdministratorsAuthorizationHandler`確認處理資源的使用者是系統管理員。 系統管理員可以執行所有作業。

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>註冊授權處理常式

您必須針對註冊服務使用 Entity Framework Core[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。 `ContactIsOwnerAuthorizationHandler`會使用 ASP.NET Core[身分識別](xref:security/authentication/identity)，這根據 Entity Framework Core。 註冊處理常式的服務集合，讓它們能夠`ContactsController`經由[相依性插入](xref:fundamentals/dependency-injection)。 將下列程式碼新增至結尾`ConfigureServices`:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`會新增為單一子句。 它們是單次個體，因為他們不使用 EF 和所需的所有資訊都都`Context`參數`HandleRequirementAsync`方法。

## <a name="support-authorization"></a>支援的授權

在本節中，您可以更新 Razor 頁面，並新增的運算需求類別。

### <a name="review-the-contact-operations-requirements-class"></a>檢閱此連絡人的作業需求類別

檢閱`ContactOperations`類別。 這個類別包含的需求，應用程式支援：

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>建立連絡人 Razor 頁面的基底類別

建立基底類別，其中包含連絡人 Razor 頁面中使用的服務。 基底類別會將初始化程式碼置於一個位置：

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

上述程式碼：

* 新增`IAuthorizationService`服務存取授權的處理常式。
* 將身分識別新增`UserManager`服務。
* 加入 `ApplicationDbContext`。

### <a name="update-the-createmodel"></a>更新 CreateModel

更新 [建立] 頁面模型建構函式使用`DI_BasePageModel`基底類別：

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

更新`CreateModel.OnPostAsync`方法：

* 新增的使用者識別碼`Contact`模型。
* 呼叫授權處理常式，以確定使用者有權限來建立連絡人。

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>更新 IndexModel

更新`OnGetAsync`方法，讓只有核准的連絡人會向一般使用者顯示：

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>更新 EditModel

新增授權處理常式，以確認該使用者所擁有的連絡人。 正在驗證資源的授權，因為`[Authorize]`屬性還不夠。 要在評估屬性時，應用程式就不需要資源的存取權。 資源為基礎的授權必須是必要的。 一旦應用程式存取的資源中，載入頁面模型中，或載入處理常式本身內，則必須執行檢查。 您經常存取的資源，藉由傳入的資源索引鍵。

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>更新 DeleteModel

更新使用授權處理常式，以確定使用者有刪除權限，在連絡人上的 [刪除] 頁面模型。

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>插入檢視中的授權服務

目前，UI 顯示編輯和刪除的使用者無法修改的連絡人的連結。

將授權服務插入*Pages/_ViewImports*檔案中, 以供所有視圖使用:

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

上述標記會新增數個`using`陳述式。

更新**編輯**並**刪除**中的連結*Pages/Contacts/Index.cshtml*讓它們只轉譯為適當的權限的使用者：

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> 隱藏不需要變更資料的權限的使用者的連結不安全的應用程式。 隱藏的連結，讓應用程式更方便使用顯示唯一有效的連結。 使用者可以 hack 產生的 Url 以叫用 編輯和刪除作業對他們未擁有的資料。 Razor 頁面或控制站必須強制執行存取檢查，以確保資料的安全。

### <a name="update-details"></a>更新詳細資料

更新詳細資料檢視，讓經理可以核准或拒絕連絡人：

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

更新詳細資料 頁面模型：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>新增或移除使用者角色

請參閱[本期](https://github.com/aspnet/AspNetCore.Docs/issues/8502)有關：

* 移除使用者的權限。 例如, 將聊天應用程式中的使用者靜音。
* 若要新增權限。

## <a name="test-the-completed-app"></a>測試已完成的應用程式

如果您尚未設定植入的使用者帳戶的密碼，使用[Secret Manager 工具](xref:security/app-secrets#secret-manager)設定密碼：

* 選擇強式密碼:請使用八個或更多個字元, 且至少要有一個大寫字元、數位和符號。 比方說，`Passw0rd!`符合強式密碼需求。
* 執行下列命令，從專案的資料夾，其中`<PW>`的密碼：

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

如果應用程式的連絡人：

* 刪除的記錄中的所有`Contact`資料表。
* 重新啟動植入資料庫的應用程式。

測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 工作階段）。 在瀏覽器中註冊新的使用者 (例如`test@example.com`)。 使用不同的使用者登入每個瀏覽器。 請確認下列作業：

* 已註冊的使用者可以檢視所有已核准的連絡資料。
* 已註冊的使用者可以編輯/刪除他們自己的資料。
* 經理可以核准/拒絕連絡人資料。 `Details`檢視會顯示**核准**並**拒絕**按鈕。
* 系統管理員可以核准/拒絕和編輯/刪除所有資料。

| 使用者                | 植入的應用程式 | 選項                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | 否                | 編輯/刪除自己的資料。                |
| manager@contoso.com | 是               | 核准/拒絕和編輯/刪除自己的資料。 |
| admin@contoso.com   | 是               | 核准/拒絕和編輯/刪除所有資料。 |

在系統管理員的瀏覽器中建立的連絡人。 複製的 URL 刪除和編輯從系統管理員連絡。 這些連結貼入測試使用者的瀏覽器，以確認測試使用者無法執行這些作業。

## <a name="create-the-starter-app"></a>建立入門應用程式

* 建立名為"ContactManager"Razor 頁面應用程式
  * 建立應用程式與**個別使用者帳戶**。
  * 提供名稱"ContactManager"使命名空間符合此範例中使用的命名空間。
  * `-uld` 指定 LocalDB，而不是 SQLite

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* 新增*模型/連絡人 .cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Scaffold`Contact`模型。
* 建立初始移轉並更新資料庫：

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

如果您遇到`dotnet aspnet-codegenerator razorpage`命令的錯誤, 請參閱[此 GitHub 問題](https://github.com/aspnet/Scaffolding/issues/984)。

* 更新*Pages/Shared/_Layout*檔案中的**ContactManager**錨點:

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* 測試應用程式建立、 編輯和刪除連絡人

### <a name="seed-the-database"></a>植入資料庫

將[SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs)類別新增至 [*資料*] 資料夾:

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

呼叫`SeedData.Initialize`從`Main`:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

測試應用程式植入資料庫。 請連絡資料庫中有任何資料列，如果種子方法不會執行。

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

本教學課程會示範如何建立 ASP.NET Core web 應用程式與受保護的授權的使用者資料。 它會顯示已驗證 （登錄） 的使用者的連絡人清單中已建立。 有三個安全性群組：

* **已註冊的使用者**可以檢視所有已認可的資料並可以編輯/刪除他們自己的資料。
* **管理員**可以核准或拒絕連絡人資料。 只有已核准的連絡人會對使用者顯示。
* **系統管理員**可以核准/拒絕並編輯/刪除任何資料。

在下圖中，使用者 Rick (`rick@example.com`) 登入。 Rick 只能檢視已核准的連絡人和**編輯**/**刪除**/**新建**他連絡人的連結。 只有最後一筆記錄，由 Rick，顯示**編輯**並**刪除**連結。 其他使用者不會看到最後一筆記錄，直到管理員或系統管理員的狀態變更為 「 Approved 」。

![螢幕擷取畫面顯示 Rick 登入](secure-data/_static/rick.png)

在下圖中, `manager@contoso.com`已登入並以管理員的角色執行:

![螢幕擷取畫面顯示manager@contoso.com登入](secure-data/_static/manager1.png)

下圖顯示之管理員的連絡詳細資料檢視：

![連絡人的管理員的檢視](secure-data/_static/manager.png)

**核准**並**拒絕**按鈕只會顯示為管理員和系統管理員。

在下圖中, `admin@contoso.com`已登入, 並以系統管理員角色:

![螢幕擷取畫面顯示admin@contoso.com登入](secure-data/_static/admin.png)

系統管理員將擁有所有權限。 她可以讀取/編輯/刪除的任何連絡人，並變更連絡人的狀態。

藉由建立應用程式[scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model)下列`Contact`模型：

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

此範例包含下列 「 授權 」 處理常式：

* `ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。
* `ContactManagerAuthorizationHandler`：可讓管理員核准或拒絕連絡人。
* `ContactAdministratorsAuthorizationHandler`：允許系統管理員核准或拒絕連絡人, 以及編輯/刪除連絡人。

## <a name="prerequisites"></a>必要條件

本教學課程會前進。 您應該先熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [驗證](xref:security/authentication/identity)
* [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [授權](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Starter 和已完成的應用程式

[下載](xref:index#how-to-download-a-sample)[完成](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)應用程式。 [測試](#test-the-completed-app)已完成的應用程式，讓您熟悉其安全性功能。

### <a name="the-starter-app"></a>入門應用程式

[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。

執行應用程式中，點選**ContactManager**連結，並確認您可以建立、 編輯和刪除連絡人。

## <a name="secure-user-data"></a>保護使用者資料

下列各節會有所有主要的步驟，來建立安全的使用者資料的應用程式。 您可能會發現它已完成的專案參考很有幫助。

### <a name="tie-the-contact-data-to-the-user"></a>將繫結至使用者的連絡人資料

使用 ASP.NET[識別](xref:security/authentication/identity)使用者識別碼，以確保使用者可以編輯其資料，但不是其他使用者資料。 新增`OwnerID`並`ContactStatus`到`Contact`模型：

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` 是來自使用者的識別碼`AspNetUser`資料表中[身分識別](xref:security/authentication/identity)資料庫。 `Status`欄位可讓您判斷是否可供一般使用者檢視連絡人。

建立新的移轉，並更新資料庫：

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>將角色服務新增到 身分識別

附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)來新增角色服務：

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>需要已驗證的使用者

設定預設的驗證原則，以要求使用者進行驗證：

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 您可以選擇退出的 Razor 頁面、 控制器或動作方法層級的驗證`[AllowAnonymous]`屬性。 設定預設的驗證原則，以要求使用者進行驗證，可保護新加入的 Razor 頁面和控制站。 具有預設值所需的驗證會比依賴新的控制器和 Razor 頁面，以包含更安全`[Authorize]`屬性。

新增[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)索引，因此匿名使用者可以取得站台的相關資訊，才能註冊的相關和連絡人頁面。

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>設定測試帳戶

`SeedData`類別會建立兩個帳戶： 系統管理員和管理員。 使用[Secret Manager 工具](xref:security/app-secrets)設定這些帳戶的密碼。 設定密碼，從專案目錄 (目錄包含*Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

如果未指定強式密碼，則會擲回例外狀況時`SeedData.Initialize`呼叫。

更新`Main`使用測試密碼：

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>建立測試帳戶和更新連絡人

更新`Initialize`方法中的`SeedData`類別來建立測試帳戶：

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

新增系統管理員使用者識別碼和`ContactStatus`連絡人。 讓其中一個 「 連絡人 」 已提交 」 和一個 「 已拒絕 」。 加入所有連絡人的使用者識別碼和狀態。 只有一個連絡人所示：

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>建立擁有者 」、 「 管理員 」 和 「 系統管理員授權的處理常式

建立`ContactIsOwnerAuthorizationHandler`類別內*授權*資料夾。 `ContactIsOwnerAuthorizationHandler`確認處理資源的使用者擁有的資源。

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼叫[內容。成功](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)目前已驗證的使用者是否連絡擁有者。 授權的處理常式通常：

* 傳回`context.Succeed`時符合的需求。
* 傳回`Task.CompletedTask`時不符合需求。 `Task.CompletedTask`不是成功或失敗&mdash;, 它允許其他授權處理常式執行。

如果您需要明確地使失敗，傳回[內容。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。

此應用程式會允許連絡人的擁有者可以編輯/刪除/建立自己的資料。 `ContactIsOwnerAuthorizationHandler` 不需要檢查傳入的要求參數的作業。

### <a name="create-a-manager-authorization-handler"></a>建立管理員授權的處理常式

建立`ContactManagerAuthorizationHandler`類別內*授權*資料夾。 `ContactManagerAuthorizationHandler`確認處理資源的使用者是管理員。 只有經理可以核准或拒絕內容變更 （新增或變更）。

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>建立系統管理員授權處理常式

建立`ContactAdministratorsAuthorizationHandler`類別內*授權*資料夾。 `ContactAdministratorsAuthorizationHandler`確認處理資源的使用者是系統管理員。 系統管理員可以執行所有作業。

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>註冊授權處理常式

您必須針對註冊服務使用 Entity Framework Core[相依性插入](xref:fundamentals/dependency-injection)使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)。 `ContactIsOwnerAuthorizationHandler`會使用 ASP.NET Core[身分識別](xref:security/authentication/identity)，這根據 Entity Framework Core。 註冊處理常式的服務集合，讓它們能夠`ContactsController`經由[相依性插入](xref:fundamentals/dependency-injection)。 將下列程式碼新增至結尾`ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` 和`ContactManagerAuthorizationHandler`會新增為單一子句。 它們是單次個體，因為他們不使用 EF 和所需的所有資訊都都`Context`參數`HandleRequirementAsync`方法。

## <a name="support-authorization"></a>支援的授權

在本節中，您可以更新 Razor 頁面，並新增的運算需求類別。

### <a name="review-the-contact-operations-requirements-class"></a>檢閱此連絡人的作業需求類別

檢閱`ContactOperations`類別。 這個類別包含的需求，應用程式支援：

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>建立連絡人 Razor 頁面的基底類別

建立基底類別，其中包含連絡人 Razor 頁面中使用的服務。 基底類別會將初始化程式碼置於一個位置：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

上述程式碼：

* 新增`IAuthorizationService`服務存取授權的處理常式。
* 將身分識別新增`UserManager`服務。
* 加入 `ApplicationDbContext`。

### <a name="update-the-createmodel"></a>更新 CreateModel

更新 [建立] 頁面模型建構函式使用`DI_BasePageModel`基底類別：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

更新`CreateModel.OnPostAsync`方法：

* 新增的使用者識別碼`Contact`模型。
* 呼叫授權處理常式，以確定使用者有權限來建立連絡人。

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>更新 IndexModel

更新`OnGetAsync`方法，讓只有核准的連絡人會向一般使用者顯示：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>更新 EditModel

新增授權處理常式，以確認該使用者所擁有的連絡人。 正在驗證資源的授權，因為`[Authorize]`屬性還不夠。 要在評估屬性時，應用程式就不需要資源的存取權。 資源為基礎的授權必須是必要的。 一旦應用程式存取的資源中，載入頁面模型中，或載入處理常式本身內，則必須執行檢查。 您經常存取的資源，藉由傳入的資源索引鍵。

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>更新 DeleteModel

更新使用授權處理常式，以確定使用者有刪除權限，在連絡人上的 [刪除] 頁面模型。

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>插入檢視中的授權服務

目前，UI 顯示編輯和刪除的使用者無法修改的連絡人的連結。

插入中的授權服務*views/_viewimports.cshtml*檔案，以便它可供所有檢視：

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

上述標記會新增數個`using`陳述式。

更新**編輯**並**刪除**中的連結*Pages/Contacts/Index.cshtml*讓它們只轉譯為適當的權限的使用者：

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> 隱藏不需要變更資料的權限的使用者的連結不安全的應用程式。 隱藏的連結，讓應用程式更方便使用顯示唯一有效的連結。 使用者可以 hack 產生的 Url 以叫用 編輯和刪除作業對他們未擁有的資料。 Razor 頁面或控制站必須強制執行存取檢查，以確保資料的安全。

### <a name="update-details"></a>更新詳細資料

更新詳細資料檢視，讓經理可以核准或拒絕連絡人：

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

更新詳細資料 頁面模型：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>新增或移除使用者角色

請參閱[本期](https://github.com/aspnet/AspNetCore.Docs/issues/8502)有關：

* 移除使用者的權限。 例如, 將聊天應用程式中的使用者靜音。
* 若要新增權限。

## <a name="test-the-completed-app"></a>測試已完成的應用程式

如果您尚未設定植入的使用者帳戶的密碼，使用[Secret Manager 工具](xref:security/app-secrets#secret-manager)設定密碼：

* 選擇強式密碼:請使用八個或更多個字元, 且至少要有一個大寫字元、數位和符號。 比方說，`Passw0rd!`符合強式密碼需求。
* 執行下列命令，從專案的資料夾，其中`<PW>`的密碼：

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* 卸載並更新資料庫

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* 重新啟動植入資料庫的應用程式。

測試已完成的應用程式的簡單方法是啟動三個不同的瀏覽器 （或 incognito/InPrivate 工作階段）。 在瀏覽器中註冊新的使用者 (例如`test@example.com`)。 使用不同的使用者登入每個瀏覽器。 請確認下列作業：

* 已註冊的使用者可以檢視所有已核准的連絡資料。
* 已註冊的使用者可以編輯/刪除他們自己的資料。
* 經理可以核准/拒絕連絡人資料。 `Details`檢視會顯示**核准**並**拒絕**按鈕。
* 系統管理員可以核准/拒絕和編輯/刪除所有資料。

| 使用者                | 植入的應用程式 | 選項                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | 否                | 編輯/刪除自己的資料。                |
| manager@contoso.com | 是               | 核准/拒絕和編輯/刪除自己的資料。 |
| admin@contoso.com   | 是               | 核准/拒絕和編輯/刪除所有資料。 |

在系統管理員的瀏覽器中建立的連絡人。 複製的 URL 刪除和編輯從系統管理員連絡。 這些連結貼入測試使用者的瀏覽器，以確認測試使用者無法執行這些作業。

## <a name="create-the-starter-app"></a>建立入門應用程式

* 建立名為"ContactManager"Razor 頁面應用程式
  * 建立應用程式與**個別使用者帳戶**。
  * 提供名稱"ContactManager"使命名空間符合此範例中使用的命名空間。
  * `-uld` 指定 LocalDB，而不是 SQLite

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* 新增*模型/連絡人 .cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Scaffold`Contact`模型。
* 建立初始移轉並更新資料庫：

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* 更新**ContactManager**中的錨點*pages/_layout.cshtml*檔案：

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* 測試應用程式建立、 編輯和刪除連絡人

### <a name="seed-the-database"></a>植入資料庫

新增[SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs)類別，即可*資料*資料夾。

呼叫`SeedData.Initialize`從`Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

測試應用程式植入資料庫。 請連絡資料庫中有任何資料列，如果種子方法不會執行。

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>其他資源

* [建置.NET Core 和 SQL Database web 應用程式在 Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。 這個實驗室會進入此教學課程中介紹的安全性功能的更多詳細資料。
* <xref:security/authorization/introduction>
* [自訂原則式授權](xref:security/authorization/policies)
