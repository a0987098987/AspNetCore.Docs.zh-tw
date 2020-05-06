---
title: 建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式
author: rick-anderson
description: 瞭解如何使用受授權Razor保護的使用者資料來建立頁面應用程式。 包括 HTTPS、驗證、安全性 ASP.NET Core Identity。
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/secure-data
ms.openlocfilehash: f52b08786dde54e7dcbd2e00f43badb58879cf79
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775748"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Joe Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

如需 ASP.NET Core MVC 版本，請參閱[此 PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) 。 本教學課程的 ASP.NET Core 1.1 版本位於[此](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data)資料夾中。 1.1 ASP.NET Core 範例位於[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2)中。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

請參閱[此 pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

本教學課程示範如何建立 ASP.NET Core web 應用程式，其中包含由授權保護的使用者資料。 它會顯示已驗證（已註冊）使用者建立的連絡人清單。 有三個安全性群組：

* **已註冊的使用者**可以查看所有核准的資料，而且可以編輯/刪除自己的資料。
* **管理員**可以核准或拒絕連絡人資料。 使用者只能看到核准的連絡人。
* 系統**管理員**可以核准/拒絕和編輯/刪除任何資料。

本檔中的影像不完全符合最新的範本。

在下圖中，已登入`rick@example.com`使用者 Rick （）。 Rick 只能查看已核准的連絡人，並針對他的連絡人**編輯**/**[刪除**/]**建立新**的連結。 只有 [Rick] 所建立的最後一筆記錄會顯示 [**編輯**] 和 [**刪除**] 連結。 在管理員或系統管理員將狀態變更為「已核准」之前，其他使用者將不會看到最後一筆記錄。

![顯示 Rick 已登入的螢幕擷取畫面](secure-data/_static/rick.png)

在下圖中， `manager@contoso.com`已登入並以管理員的角色執行：

![顯示manager@contoso.com已登入的螢幕擷取畫面](secure-data/_static/manager1.png)

下圖顯示連絡人的 [管理員] 詳細資料檢視：

![經理的連絡人觀點](secure-data/_static/manager.png)

[**核准**] 和 [**拒絕**] 按鈕只會針對管理員和系統管理員顯示。

在下圖中， `admin@contoso.com`已登入，並以系統管理員角色：

![顯示admin@contoso.com已登入的螢幕擷取畫面](secure-data/_static/admin.png)

系統管理員具有擁有權限。 她可以讀取/編輯/刪除任何連絡人，並變更連絡人的狀態。

應用程式[是由下列](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) `Contact`模型所建立：

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

此範例包含下列授權處理常式：

* `ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。
* `ContactManagerAuthorizationHandler`：允許管理員核准或拒絕連絡人。
* `ContactAdministratorsAuthorizationHandler`：可讓系統管理員核准或拒絕連絡人，以及編輯/刪除連絡人。

## <a name="prerequisites"></a>必要條件

本教學課程是先進的。 您應該已熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [驗證](xref:security/authentication/identity)
* [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [授權](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>入門和已完成的應用程式

[下載](xref:index#how-to-download-a-sample)[已完成](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)的應用程式。 [測試](#test-the-completed-app)已完成的應用程式，以熟悉其安全性功能。

### <a name="the-starter-app"></a>入門應用程式

[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。

執行應用程式，並按一下 [ **ContactManager** ] 連結，並確認您可以建立、編輯和刪除連絡人。

## <a name="secure-user-data"></a>保護使用者資料

下列各節包含建立安全使用者資料應用程式的所有主要步驟。 您可能會發現參考已完成的專案很有説明。

### <a name="tie-the-contact-data-to-the-user"></a>將連絡人資料與使用者結合

使用 ASP.NET 身分[識別](xref:security/authentication/identity)使用者識別碼，以確保使用者可以編輯其資料，而不是其他使用者資料。 將`OwnerID`和`ContactStatus`新增至`Contact`模型：

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID`這是來自身分[識別](xref:security/authentication/identity)資料庫中`AspNetUser`之資料表的使用者識別碼。 `Status`欄位會決定一般使用者是否可以查看連絡人。

建立新的遷移並更新資料庫：

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>將角色服務新增至身分識別

附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>需要已驗證的使用者

將 [預設驗證原則] 設定為 [要求使用者進行驗證]：

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 您可以使用`[AllowAnonymous]`屬性，在 Razor 頁面、控制器或動作方法層級選擇不進行驗證。 將預設驗證原則設定為 [要求使用者進行驗證] 會保護新增的 Razor Pages 和控制器。 預設需要驗證，比依賴新的`[Authorize]`控制器和 Razor Pages 以包含屬性更為安全。

將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 [索引] 和 [隱私權] 頁面，讓匿名使用者可以在註冊之前取得網站的相關資訊。

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>設定測試帳戶

`SeedData`類別會建立兩個帳戶：系統管理員和管理員。 使用[秘密管理員工具](xref:security/app-secrets)來設定這些帳戶的密碼。 從專案目錄（包含*Program.cs*的目錄）設定密碼：

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

如果未指定強式密碼，則會在呼叫時`SeedData.Initialize`擲回例外狀況。

更新`Main`以使用測試密碼：

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>建立測試帳戶並更新連絡人

更新`SeedData`類別`Initialize`中的方法，以建立測試帳戶：

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

將系統管理員使用者識別碼和`ContactStatus`新增至連絡人。 讓其中一個連絡人「已提交」和一個「已拒絕」。 將 [使用者識別碼] 和 [狀態] 新增至所有連絡人。 只會顯示一個連絡人：

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>建立擁有者、管理員和系統管理員授權處理常式

在 [ `ContactIsOwnerAuthorizationHandler` *授權*] 資料夾中建立類別。 會`ContactIsOwnerAuthorizationHandler`驗證對資源進行的使用者擁有資源。

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼叫[內容。](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人擁有者，則會成功。 授權處理常式一般：

* 符合`context.Succeed`需求時傳回。
* `Task.CompletedTask`當不符合需求時傳回。 `Task.CompletedTask`不是成功或失敗&mdash;，它允許其他授權處理常式執行。

如果您需要明確失敗，請傳回[coNtext。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。

應用程式可讓連絡人擁有者編輯/刪除/建立自己的資料。 `ContactIsOwnerAuthorizationHandler`不需要檢查在需求參數中傳遞的作業。

### <a name="create-a-manager-authorization-handler"></a>建立管理員授權處理常式

在 [ `ContactManagerAuthorizationHandler` *授權*] 資料夾中建立類別。 會`ContactManagerAuthorizationHandler`驗證對資源的使用者是否為管理員。 只有管理員可以核准或拒絕內容變更（新的或已變更）。

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>建立系統管理員授權處理常式

在 [ `ContactAdministratorsAuthorizationHandler` *授權*] 資料夾中建立類別。 會`ContactAdministratorsAuthorizationHandler`驗證對資源的使用者是否為系統管理員。 系統管理員可以執行所有作業。

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>註冊授權處理常式

使用 Entity Framework Core 的服務必須使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)註冊相依性[插入](xref:fundamentals/dependency-injection)。 會`ContactIsOwnerAuthorizationHandler`使用以 Entity Framework Core 為基礎的 ASP.NET Core 身分[識別](xref:security/authentication/identity)。 向服務集合註冊處理常式，以便`ContactsController`透過相依性[插入](xref:fundamentals/dependency-injection)來使用它們。 在結尾新增下列程式碼`ConfigureServices`：

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler`和`ContactManagerAuthorizationHandler`會新增為單次個體。 它們是單次個體的`Context` ，因為它們不使用 EF，而所需的所有資訊都是`HandleRequirementAsync`在方法的參數中。

## <a name="support-authorization"></a>支援授權

在本節中，您會更新 Razor Pages 並新增作業需求類別。

### <a name="review-the-contact-operations-requirements-class"></a>審查連絡人作業需求類別

檢查`ContactOperations`類別。 此類別包含應用程式支援的需求：

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>建立連絡人的基類 Razor Pages

建立基類，其中包含 [連絡人] Razor Pages 中使用的服務。 基底類別會將初始化程式碼放在一個位置：

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

上述程式碼：

* 新增`IAuthorizationService`服務以存取授權處理常式。
* 新增身分識別`UserManager`服務。
* 加入 `ApplicationDbContext`。

### <a name="update-the-createmodel"></a>更新 CreateModel

更新 [建立] 頁面模型的函式`DI_BasePageModel` ，以使用基類：

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

將`CreateModel.OnPostAsync`方法更新為：

* 將使用者識別碼新增至`Contact`模型。
* 呼叫授權處理常式，以確認使用者擁有建立連絡人的許可權。

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>更新 IndexModel

更新`OnGetAsync`方法，讓一般使用者只會看到已核准的連絡人：

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>更新 EditModel

新增授權處理常式，以確認使用者擁有該連絡人。 因為正在驗證資源授權，所以`[Authorize]`屬性不夠。 評估屬性時，應用程式沒有資源的存取權。 以資源為基礎的授權必須是必要的。 一旦應用程式可存取資源，就必須執行檢查，方法是將它載入頁面模型中，或在處理常式本身內載入。 您經常會藉由傳入資源金鑰來存取資源。

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>更新 DeleteModel

更新 [刪除] 頁面模型，以使用授權處理常式來驗證使用者是否擁有連絡人的 [刪除] 許可權。

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>將授權服務插入至 views

目前，UI 會顯示使用者無法修改之連絡人的 [編輯] 和 [刪除] 連結。

在*Pages/_ViewImports. cshtml*檔案中插入授權服務，以供所有視圖使用：

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

上述標記會加入數`using`個語句。

更新*Pages/Contacts/Index*中的 [**編輯**] 和 [**刪除**] 連結，使其僅針對具有適當許可權的使用者呈現：

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> 隱藏不具有變更資料許可權之使用者的連結，並不會保護應用程式的安全。 隱藏連結可只顯示有效的連結，讓應用程式更容易使用。 使用者可以攻擊產生的 Url，以叫用其未擁有之資料的編輯和刪除作業。 Razor 頁面或控制器必須強制執行存取檢查，以保護資料。

### <a name="update-details"></a>更新詳細資料

更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

更新 [詳細資料] 頁面模型：

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>新增或移除角色的使用者

如需相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8502)：

* 移除使用者的許可權。 例如，將聊天應用程式中的使用者靜音。
* 將許可權新增至使用者。

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a>挑戰與禁止之間的差異

此應用程式會將預設原則設定為[要求已驗證的使用者](#require-authenticated-users)。 下列程式碼允許匿名使用者。 允許匿名使用者顯示挑戰與禁止之間的差異。

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

在上述程式碼中：

* 當使用者**未**經過驗證時， `ChallengeResult`會傳回。 `ChallengeResult`當傳回時，會將使用者重新導向至登入頁面。
* 當使用者經過驗證但未獲授權時， `ForbidResult`會傳回。 `ForbidResult`當傳回時，使用者會被重新導向至 [拒絕存取] 頁面。

## <a name="test-the-completed-app"></a>測試已完成的應用程式

如果您尚未設定已植入之使用者帳戶的密碼，請使用[秘密管理員工具](xref:security/app-secrets#secret-manager)來設定密碼：

* 選擇強式密碼：使用八個或更多個字元，且至少要有一個大寫字元、數位和符號。 例如， `Passw0rd!`符合強式密碼需求。
* 從專案的資料夾執行下列命令，其中`<PW>`是密碼：

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

如果應用程式有連絡人：

* 刪除`Contact`資料表中的所有記錄。
* 重新開機應用程式以植入資料庫。

測試已完成之應用程式的簡單方法，是啟動三個不同的瀏覽器（或 incognito/InPrivate 會話）。 在一個瀏覽器中，註冊新的使用者（例如`test@example.com`）。 使用不同的使用者登入每個瀏覽器。 確認下列作業：

* 已註冊的使用者可以查看所有已核准的連絡人資料。
* 已註冊的使用者可以編輯/刪除自己的資料。
* 管理員可以核准/拒絕連絡人資料。 此`Details`視圖會顯示 [**核准**] 和 [**拒絕**] 按鈕。
* 系統管理員可以核准/拒絕和編輯/刪除所有資料。

| User                | 由應用程式植入 | 選項。                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | 否                | 編輯/刪除自己的資料。                |
| manager@contoso.com | 是               | 核准/拒絕和編輯/刪除自己的資料。 |
| admin@contoso.com   | 是               | 核准/拒絕和編輯/刪除所有資料。 |

在系統管理員的瀏覽器中建立連絡人。 從系統管理員連絡人中複製 [刪除] 和 [編輯] 的 URL。 將這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。

## <a name="create-the-starter-app"></a>建立入門應用程式

* 建立名為 "ContactManager" 的 Razor Pages 應用程式
  * 建立具有**個別使用者帳戶**的應用程式。
  * 將它命名為 "ContactManager"，讓命名空間符合範例中使用的命名空間。
  * `-uld`指定 LocalDB，而不是 SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* 新增*模型/連絡人 .cs*：

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Scaffold `Contact`模型。
* 建立初始遷移並更新資料庫：

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

如果您遇到`dotnet aspnet-codegenerator razorpage`命令的錯誤，請參閱[此 GitHub 問題](https://github.com/aspnet/Scaffolding/issues/984)。

* 更新*Pages/Shared/_Layout. cshtml*檔案中的**ContactManager**錨點：

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* 藉由建立、編輯和刪除連絡人來測試應用程式

### <a name="seed-the-database"></a>植入資料庫

將[SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs)類別新增至 [*資料*] 資料夾：

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

呼叫`SeedData.Initialize`來源`Main`：

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

測試應用程式是否植入資料庫。 如果 contact DB 中有任何資料列，則不會執行種子方法。

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

本教學課程示範如何建立 ASP.NET Core web 應用程式，其中包含由授權保護的使用者資料。 它會顯示已驗證（已註冊）使用者建立的連絡人清單。 有三個安全性群組：

* **已註冊的使用者**可以查看所有核准的資料，而且可以編輯/刪除自己的資料。
* **管理員**可以核准或拒絕連絡人資料。 使用者只能看到核准的連絡人。
* 系統**管理員**可以核准/拒絕和編輯/刪除任何資料。

在下圖中，已登入`rick@example.com`使用者 Rick （）。 Rick 只能查看已核准的連絡人，並針對他的連絡人**編輯**/**[刪除**/]**建立新**的連結。 只有 [Rick] 所建立的最後一筆記錄會顯示 [**編輯**] 和 [**刪除**] 連結。 在管理員或系統管理員將狀態變更為「已核准」之前，其他使用者將不會看到最後一筆記錄。

![顯示 Rick 已登入的螢幕擷取畫面](secure-data/_static/rick.png)

在下圖中， `manager@contoso.com`已登入並以管理員的角色執行：

![顯示manager@contoso.com已登入的螢幕擷取畫面](secure-data/_static/manager1.png)

下圖顯示連絡人的 [管理員] 詳細資料檢視：

![經理的連絡人觀點](secure-data/_static/manager.png)

[**核准**] 和 [**拒絕**] 按鈕只會針對管理員和系統管理員顯示。

在下圖中， `admin@contoso.com`已登入，並以系統管理員角色：

![顯示admin@contoso.com已登入的螢幕擷取畫面](secure-data/_static/admin.png)

系統管理員具有擁有權限。 她可以讀取/編輯/刪除任何連絡人，並變更連絡人的狀態。

應用程式[是由下列](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) `Contact`模型所建立：

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

此範例包含下列授權處理常式：

* `ContactIsOwnerAuthorizationHandler`：確保使用者只能編輯其資料。
* `ContactManagerAuthorizationHandler`：允許管理員核准或拒絕連絡人。
* `ContactAdministratorsAuthorizationHandler`：可讓系統管理員核准或拒絕連絡人，以及編輯/刪除連絡人。

## <a name="prerequisites"></a>必要條件

本教學課程是先進的。 您應該已熟悉：

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [驗證](xref:security/authentication/identity)
* [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
* [授權](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>入門和已完成的應用程式

[下載](xref:index#how-to-download-a-sample)[已完成](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples)的應用程式。 [測試](#test-the-completed-app)已完成的應用程式，以熟悉其安全性功能。

### <a name="the-starter-app"></a>入門應用程式

[下載](xref:index#how-to-download-a-sample) [starter](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/)應用程式。

執行應用程式，並按一下 [ **ContactManager** ] 連結，並確認您可以建立、編輯和刪除連絡人。

## <a name="secure-user-data"></a>保護使用者資料

下列各節包含建立安全使用者資料應用程式的所有主要步驟。 您可能會發現參考已完成的專案很有説明。

### <a name="tie-the-contact-data-to-the-user"></a>將連絡人資料與使用者結合

使用 ASP.NET 身分[識別](xref:security/authentication/identity)使用者識別碼，以確保使用者可以編輯其資料，而不是其他使用者資料。 將`OwnerID`和`ContactStatus`新增至`Contact`模型：

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID`這是來自身分[識別](xref:security/authentication/identity)資料庫中`AspNetUser`之資料表的使用者識別碼。 `Status`欄位會決定一般使用者是否可以查看連絡人。

建立新的遷移並更新資料庫：

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>將角色服務新增至身分識別

附加[AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1)以新增角色服務：

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>需要已驗證的使用者

將 [預設驗證原則] 設定為 [要求使用者進行驗證]：

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 您可以使用`[AllowAnonymous]`屬性，在 Razor 頁面、控制器或動作方法層級選擇不進行驗證。 將預設驗證原則設定為 [要求使用者進行驗證] 會保護新增的 Razor Pages 和控制器。 預設需要驗證，比依賴新的`[Authorize]`控制器和 Razor Pages 以包含屬性更為安全。

將[AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute)新增至 [索引]、[關於] 和 [連絡人] 頁面，讓匿名使用者可以在註冊之前取得網站的相關資訊。

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>設定測試帳戶

`SeedData`類別會建立兩個帳戶：系統管理員和管理員。 使用[秘密管理員工具](xref:security/app-secrets)來設定這些帳戶的密碼。 從專案目錄（包含*Program.cs*的目錄）設定密碼：

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

如果未指定強式密碼，則會在呼叫時`SeedData.Initialize`擲回例外狀況。

更新`Main`以使用測試密碼：

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>建立測試帳戶並更新連絡人

更新`SeedData`類別`Initialize`中的方法，以建立測試帳戶：

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

將系統管理員使用者識別碼和`ContactStatus`新增至連絡人。 讓其中一個連絡人「已提交」和一個「已拒絕」。 將 [使用者識別碼] 和 [狀態] 新增至所有連絡人。 只會顯示一個連絡人：

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>建立擁有者、管理員和系統管理員授權處理常式

建立*授權*資料夾，並在其中`ContactIsOwnerAuthorizationHandler`建立類別。 會`ContactIsOwnerAuthorizationHandler`驗證對資源進行的使用者擁有資源。

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

`ContactIsOwnerAuthorizationHandler`呼叫[內容。](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_)如果目前已驗證的使用者是連絡人擁有者，則會成功。 授權處理常式一般：

* 符合`context.Succeed`需求時傳回。
* `Task.CompletedTask`當不符合需求時傳回。 `Task.CompletedTask`不是成功或失敗&mdash;，它允許其他授權處理常式執行。

如果您需要明確失敗，請傳回[coNtext。失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail)。

應用程式可讓連絡人擁有者編輯/刪除/建立自己的資料。 `ContactIsOwnerAuthorizationHandler`不需要檢查在需求參數中傳遞的作業。

### <a name="create-a-manager-authorization-handler"></a>建立管理員授權處理常式

在 [ `ContactManagerAuthorizationHandler` *授權*] 資料夾中建立類別。 會`ContactManagerAuthorizationHandler`驗證對資源的使用者是否為管理員。 只有管理員可以核准或拒絕內容變更（新的或已變更）。

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>建立系統管理員授權處理常式

在 [ `ContactAdministratorsAuthorizationHandler` *授權*] 資料夾中建立類別。 會`ContactAdministratorsAuthorizationHandler`驗證對資源的使用者是否為系統管理員。 系統管理員可以執行所有作業。

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>註冊授權處理常式

使用 Entity Framework Core 的服務必須使用[AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions)註冊相依性[插入](xref:fundamentals/dependency-injection)。 會`ContactIsOwnerAuthorizationHandler`使用以 Entity Framework Core 為基礎的 ASP.NET Core 身分[識別](xref:security/authentication/identity)。 向服務集合註冊處理常式，以便`ContactsController`透過相依性[插入](xref:fundamentals/dependency-injection)來使用它們。 在結尾新增下列程式碼`ConfigureServices`：

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler`和`ContactManagerAuthorizationHandler`會新增為單次個體。 它們是單次個體的`Context` ，因為它們不使用 EF，而所需的所有資訊都是`HandleRequirementAsync`在方法的參數中。

## <a name="support-authorization"></a>支援授權

在本節中，您會更新 Razor Pages 並新增作業需求類別。

### <a name="review-the-contact-operations-requirements-class"></a>審查連絡人作業需求類別

檢查`ContactOperations`類別。 此類別包含應用程式支援的需求：

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>建立連絡人的基類 Razor Pages

建立基類，其中包含 [連絡人] Razor Pages 中使用的服務。 基底類別會將初始化程式碼放在一個位置：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

上述程式碼：

* 新增`IAuthorizationService`服務以存取授權處理常式。
* 新增身分識別`UserManager`服務。
* 加入 `ApplicationDbContext`。

### <a name="update-the-createmodel"></a>更新 CreateModel

更新 [建立] 頁面模型的函式`DI_BasePageModel` ，以使用基類：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

將`CreateModel.OnPostAsync`方法更新為：

* 將使用者識別碼新增至`Contact`模型。
* 呼叫授權處理常式，以確認使用者擁有建立連絡人的許可權。

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>更新 IndexModel

更新`OnGetAsync`方法，讓一般使用者只會看到已核准的連絡人：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>更新 EditModel

新增授權處理常式，以確認使用者擁有該連絡人。 因為正在驗證資源授權，所以`[Authorize]`屬性不夠。 評估屬性時，應用程式沒有資源的存取權。 以資源為基礎的授權必須是必要的。 一旦應用程式可存取資源，就必須執行檢查，方法是將它載入頁面模型中，或在處理常式本身內載入。 您經常會藉由傳入資源金鑰來存取資源。

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>更新 DeleteModel

更新 [刪除] 頁面模型，以使用授權處理常式來驗證使用者是否擁有連絡人的 [刪除] 許可權。

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>將授權服務插入至 views

目前，UI 會顯示使用者無法修改之連絡人的 [編輯] 和 [刪除] 連結。

將授權服務插入*views/_ViewImports. cshtml*檔案中，以供所有視圖使用：

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

上述標記會加入數`using`個語句。

更新*Pages/Contacts/Index*中的 [**編輯**] 和 [**刪除**] 連結，使其僅針對具有適當許可權的使用者呈現：

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> 隱藏不具有變更資料許可權之使用者的連結，並不會保護應用程式的安全。 隱藏連結可只顯示有效的連結，讓應用程式更容易使用。 使用者可以攻擊產生的 Url，以叫用其未擁有之資料的編輯和刪除作業。 Razor 頁面或控制器必須強制執行存取檢查，以保護資料。

### <a name="update-details"></a>更新詳細資料

更新詳細資料檢視，讓管理員可以核准或拒絕連絡人：

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

更新 [詳細資料] 頁面模型：

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>新增或移除角色的使用者

如需相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/8502)：

* 移除使用者的許可權。 例如，將聊天應用程式中的使用者靜音。
* 將許可權新增至使用者。

## <a name="test-the-completed-app"></a>測試已完成的應用程式

如果您尚未設定已植入之使用者帳戶的密碼，請使用[秘密管理員工具](xref:security/app-secrets#secret-manager)來設定密碼：

* 選擇強式密碼：使用八個或更多個字元，且至少要有一個大寫字元、數位和符號。 例如， `Passw0rd!`符合強式密碼需求。
* 從專案的資料夾執行下列命令，其中`<PW>`是密碼：

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* 卸載並更新資料庫

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* 重新開機應用程式以植入資料庫。

測試已完成之應用程式的簡單方法，是啟動三個不同的瀏覽器（或 incognito/InPrivate 會話）。 在一個瀏覽器中，註冊新的使用者（例如`test@example.com`）。 使用不同的使用者登入每個瀏覽器。 確認下列作業：

* 已註冊的使用者可以查看所有已核准的連絡人資料。
* 已註冊的使用者可以編輯/刪除自己的資料。
* 管理員可以核准/拒絕連絡人資料。 此`Details`視圖會顯示 [**核准**] 和 [**拒絕**] 按鈕。
* 系統管理員可以核准/拒絕和編輯/刪除所有資料。

| User                | 由應用程式植入 | 選項。                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | 否                | 編輯/刪除自己的資料。                |
| manager@contoso.com | 是               | 核准/拒絕和編輯/刪除自己的資料。 |
| admin@contoso.com   | 是               | 核准/拒絕和編輯/刪除所有資料。 |

在系統管理員的瀏覽器中建立連絡人。 從系統管理員連絡人中複製 [刪除] 和 [編輯] 的 URL。 將這些連結貼入測試使用者的瀏覽器，確認測試使用者無法執行這些作業。

## <a name="create-the-starter-app"></a>建立入門應用程式

* 建立名Razor為 "ContactManager" 的頁面應用程式
  * 建立具有**個別使用者帳戶**的應用程式。
  * 將它命名為 "ContactManager"，讓命名空間符合範例中使用的命名空間。
  * `-uld`指定 LocalDB，而不是 SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* 新增*模型/連絡人 .cs*：

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Scaffold `Contact`模型。
* 建立初始遷移並更新資料庫：

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* 更新*Pages/_Layout. cshtml*檔案中的**ContactManager**錨點：

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* 藉由建立、編輯和刪除連絡人來測試應用程式

### <a name="seed-the-database"></a>植入資料庫

將[SeedData](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs)類別新增至 [ *Data* ] 資料夾。

呼叫`SeedData.Initialize`來源`Main`：

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

測試應用程式是否植入資料庫。 如果 contact DB 中有任何資料列，則不會執行種子方法。

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>其他資源

* [在 Azure App Service 中建置 .NET Core 和 SQL Database Web 應用程式](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core 授權實驗室](https://github.com/blowdart/AspNetAuthorizationWorkshop)。 這個實驗室會詳細說明本教學課程中引進的安全性功能。
* <xref:security/authorization/introduction>
* [以自訂原則為基礎的授權](xref:security/authorization/policies)
