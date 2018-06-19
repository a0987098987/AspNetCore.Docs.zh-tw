---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: ASP.NET 識別的自訂儲存體提供者的概觀 |Microsoft 文件
author: tfitzmac
description: ASP.NET Identity 是可擴充的系統可讓您建立自己的儲存體提供者，並將之插入您的應用程式而不需要重新使用應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 06e3ad3b74bf94806f56da9f579255bf2917bc48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876796"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>ASP.NET 識別的自訂儲存體提供者的概觀
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity 是可擴充的系統可讓您建立自己的儲存體提供者，並將之插入您的應用程式而不需要重新使用應用程式。 本主題描述如何建立 ASP.NET 識別的自訂儲存體提供者。 它涵蓋建立自己的儲存體提供者的重要概念，但它不是實作自訂的儲存提供者的逐步解說。
> 
> 如需實作自訂的儲存提供者的範例，請參閱[實作自訂 MySQL ASP.NET 識別儲存體提供者](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)。
> 
> 本主題已更新 ASP.NET Identity 2.0。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Visual Studio 2013 Update 2
> - ASP.NET Identity 2


## <a name="introduction"></a>簡介

根據預設，ASP.NET Identity 系統會在 SQL Server 資料庫中，會儲存使用者資訊，並使用 Entity Framework Code First 建立資料庫。 這個方法對於許多應用程式，非常適合。 不過，您可能會想要使用不同類型的持續性機制，例如 Azure 資料表儲存體，或者您可能已經有預設的實作非常不同的結構與資料庫資料表。 在任一情況下，您可以撰寫自訂提供者的儲存機制，並插入您的應用程式中的該提供者。

根據預設，在許多 Visual Studio 2013 範本包含 ASP.NET Identity。 您可以透過 ASP.NET Identity 中取得更新[Microsoft AspNet 識別 EntityFramework NuGet 套件](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)。

本主題包含下列章節：

- [了解的架構](#architecture)
- [了解儲存的資料](#data)
- [建立資料存取層](#dal)
- [自訂使用者類別](#user)
- [自訂使用者存放區](#userstore)
- [自訂角色類別](#role)
- [自訂角色存放區](#rolestore)
- [重新設定應用程式以使用新的存放裝置提供者](#reconfigure)
- [自訂的存放裝置提供者的其他實作](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>了解的架構

ASP.NET Identity 類別，稱為管理員和存放區所組成。 經理都對應用程式開發人員用來執行作業，例如 ASP.NET 識別系統中建立使用者時，高層級類別。 商店則是較低層級類別，可指定如何保存實體，例如使用者和角色。 存放區緊密結合的持續性機制，但是管理員會從分離商店這表示您可以取代持續性機制，而不會中斷整個應用程式。

下圖顯示您的 web 應用程式與管理員之間的互動方式，與資料存取層的存放區互動。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

若要建立 ASP.NET 識別的自訂儲存體提供者，您必須使用此資料存取層中建立資料來源、 資料存取分層和互動的存放區類別。 您可以繼續使用相同的管理員 Api 的使用者，但現在這點的資料儲存至不同的儲存體系統執行資料作業。

您不需要自訂管理員類別，因為您建立 UserManager 或 RoleManager 的新執行個體時提供使用者類別的型別，並做為引數傳遞的存放區類別的執行個體。 這個方法可讓您現有的結構中插入您的自訂的類別。 您會看到如何具現化您自訂存放區中的類別區段的 UserManager 和 RoleManager[重新設定應用程式以使用新的存放裝置提供者](#reconfigure)。

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>了解儲存的資料

若要實作自訂的儲存提供者，您必須了解使用具有 ASP.NET 識別的資料類型，並且決定適用於您的應用程式的功能。

| 資料 | 描述 |
| --- | --- |
| 使用者 | 註冊您的網站的使用者。 包含的使用者識別碼和使用者名稱。 如果使用者的認證登入專屬於您的網站可能包含雜湊的密碼 （而非使用認證從外部網站，例如 Facebook），以及安全性戳記，指出是否任何項目已變更的使用者認證。 可能也包含電子郵件地址、 電話號碼時，是否已啟用雙因素驗證，目前數目失敗的登入，以及是否已鎖定帳戶。 |
| 使用者宣告 | 代表使用者的身分識別之使用者的相關陳述式 （或宣告） 的一組。 可以啟用更高的使用者身分識別與透過角色能達到之效果的運算式。 |
| 使用者登入 | 外部驗證提供者 （例如 Facebook) 的相關資訊時登入使用者使用。 |
| 角色 | 您的網站的授權群組。 包含的角色識別碼和角色名稱 （例如"Admin"或"Employee"）。 |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>建立資料存取層

本主題假設您熟悉您要使用的持續性機制，以及如何建立實體，該機制。 本主題不提供如何建立儲存機制或資料存取類別; 詳細資料相反地，它會提供您需要使用 ASP.NET Identity 時進行一些建議相關的設計決策。

您有許多自由設計自訂的儲存機制時的存放區提供者。 您只需要建立儲存機制的應用程式中想要使用的功能。 例如，如果您不會在您的應用程式中使用角色，您不需要建立的儲存體的角色或使用者角色。 您的技術和現有的基礎結構，可能需要的結構非常不同於 ASP.NET Identity 的預設實作。 資料存取層，您提供的邏輯來處理您的儲存機制的結構。

ASP.NET Identity 2.0 的資料儲存機制 MySQL 實作，請參閱[MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql)。

在資料存取層中，您可以提供要從 ASP.NET Identity 的資料儲存至您的資料來源的邏輯。 資料存取層，您自訂的存放裝置提供者可能會包含下列類別，以儲存使用者和角色的資訊。

| 類別 | 描述 | 範例 |
| --- | --- | --- |
| 內容 | 封裝連接到您的持續性機制，並執行查詢的資訊。 這個類別是程式的資料存取層級的核心。 其他的資料類別需要這個類別，以執行其作業的執行個體。 您也會初始化這個類別的執行個體存放區類別。 | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| 使用者的儲存體 | 儲存和擷取使用者資訊 （例如使用者名稱和密碼雜湊）。 | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| 角色存放裝置 | 儲存和擷取角色資訊 （例如角色名稱）。 | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| UserClaims 的儲存體 | 儲存和擷取使用者宣告資訊 （例如宣告類型和值）。 | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Serlogins 的儲存體 | 儲存和擷取使用者登入資訊 （例如外部驗證提供者）。 | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| 使用者角色存放裝置 | 儲存和擷取使用者指派給哪些角色。 | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

同樣地，您只需要實作您想要使用應用程式中的類別。

在資料存取類別中，您可以提供程式碼執行特定的持續性機制的資料作業。 例如，MySQL 實作內 UserTable 類別包含使用者資料庫資料表中插入新的記錄方法。 名為的變數`_database`MySQLDatabase 類別的執行個體。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

建立您的資料存取類別之後, 您必須建立存放區特有的方法呼叫中的資料存取層的類別。

<a id="user"></a>
## <a name="customize-the-user-class"></a>自訂使用者類別

當實作您自己的儲存體提供者，您必須建立使用者類別，即等於[IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx)類別[Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)命名空間：

下圖顯示 IdentityUser 類別，您必須建立和此類別中實作的介面。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

[IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)介面定義 UserManager 會嘗試執行要求的作業時要呼叫的內容。 介面包含兩個屬性-識別碼和使用者名稱。 [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx)介面可讓您指定的索引鍵，使用者已透過泛型型別**TKey**參數。 Id 屬性的型別符合 TKey 參數的值。

識別 framework 也提供[IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx)介面 （如果沒有泛型參數） 當您想要使用索引鍵的字串值。

IdentityUser 類別會實作 IUser，並包含額外的屬性或建構函式，您的網站上的使用者。 下列範例會顯示索引鍵使用整數 IdentityUser 類別。 [識別碼] 欄位設定為**int**以符合泛型參數的值。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 如需完整的實作，請參閱[IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs)。 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>自訂使用者存放區

您也可以建立 UserStore 類別提供之使用者的所有資料作業方法。 這個類別就相當於[UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx)類別[Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)命名空間。 在您 UserStore 的類別，您會實作[IUserStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)以及任何選擇性的介面。 您選取要實作的選擇性介面根據您想要提供您的應用程式中的功能。

下圖顯示您必須建立 UserStore 類別和相關的介面。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

在 Visual Studio 中的預設專案範本包含假設已實作的選擇性介面許多使用者存放區中的程式碼。 如果您使用自訂的使用者存放區使用預設範本，您必須實作您的使用者存放區中的選擇性介面，或改變不會再呼叫方法，在您未實作的介面中的範本程式碼。

 下列範例會示範簡單的使用者存放區類別。 **TUser**泛型參數會在使用者類別，這通常是您定義的 IdentityUser 類別的類型。 **TKey**泛型參數會使用您的使用者索引鍵的類型。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 在此範例中，使用參數的建構函式命名為*資料庫*型別的 ExampleDatabase 是只如何傳遞您的資料存取類別中的圖例。 例如，在 MySQL 實作中，這個建構函式接受 MySQLDatabase 類型的參數。 

在 UserStore 類別中，您可以使用您建立用來執行作業的資料存取類別。 例如，在 MySQL 實作中，UserStore 類別有 CreateAsync 方法，這個方法會使用 UserTable 的執行個體插入新的記錄。 **插入**方法**userTable**物件是上一節所示的方法相同。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>若要實作自訂使用者存放區時的介面

下圖顯示更多詳細的每個介面中定義的功能。 全部是選擇性的介面繼承自 IUserStore。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  [IUserStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx)介面是唯一的介面，您必須在使用者存放區實作。 它會定義方法來建立、 更新、 刪除和擷取使用者。
- **IUserClaimStore**  
  [IUserClaimStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx)介面會定義方法以啟用使用者宣告您使用者存放區中，您必須實作。 它包含的方法或加入、 移除和擷取使用者宣告。
- **IUserLoginStore**  
  [IUserLoginStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx)定義方法，您必須在您的使用者存放區，以啟用外部驗證提供者中實作。 它包含加入、 移除和擷取使用者登入和擷取使用者為基礎的登入資訊方法的方法。
- **IUserRoleStore**  
  [IUserRoleStore&lt;，TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx)介面定義的方法在您將使用者對應至角色的使用者存放區中，您必須實作。 它包含方法，以新增、 移除和擷取使用者的角色，並檢查使用者是否指派給角色的方法。
- **IUserPasswordStore**  
  [IUserPasswordStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx)介面會定義的方法以保存使用者存放區中，您必須實作雜湊密碼。 它包含方法來取得和設定雜湊的密碼，並指出使用者是否已設定密碼的方法。
- **IUserSecurityStampStore**  
  [IUserSecurityStampStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx)介面會定義您要用於安全性戳記，指出是否已變更的使用者帳戶資訊的使用者存放區中，您必須實作的方法. 當使用者變更密碼，或加入或移除登入，則會更新這個戳記。 它包含方法來取得和設定的安全性戳記。
- **IUserTwoFactorStore**  
  [IUserTwoFactorStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx)介面定義必須實作要實作雙因素驗證的方法。 它包含對取得和設定是否針對使用者啟用雙因素驗證的方法。
- **IUserPhoneNumberStore**  
  [IUserPhoneNumberStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx)介面會定義您必須實作以儲存使用者電話號碼的方法。 它包含對取得和設定的電話號碼和電話號碼是否已確認的方法。
- **IUserEmailStore**  
  [IUserEmailStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx)介面會定義方法，您必須實作以儲存使用者的電子郵件地址。 它包含對取得和設定電子郵件地址和電子郵件是否已確認的方法。
- **IUserLockoutStore**  
  [IUserLockoutStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx)介面會定義您必須實作以儲存有關鎖定的帳戶資訊的方法。 它包含方法，取得目前失敗的存取嘗試次數、 取得和設定是否鎖定帳戶，取得或設定鎖定結束日期，號碼遞增失敗嘗試，以及重設嘗試失敗次數。
- **IQueryableUserStore**  
  [IQueryableUserStore&lt;TUser、 TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx)介面會定義您必須實作以提供可查詢使用者存放區的成員。 它包含保存可查詢使用者的屬性。

  您在應用程式; 實作所需的介面例如，IUserClaimStore、 IUserLoginStore、 IUserRoleStore、 IUserPasswordStore 和 IUserSecurityStampStore 介面如下所示。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

（包括所有介面） 的完整實作，請參閱[UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs)。

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim、 IdentityUserLogin 和 IdentityUserRole

Microsoft.AspNet.Identity.EntityFramework 命名空間包含的實作[IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx)， [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)，和[IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx)類別。 如果您使用這些功能，您可能想要建立您自己的版本，這些類別，以及定義您的應用程式的屬性。 不過，有時候很不載入這些實體記憶體時執行 （例如加入或移除使用者的宣告） 的基本作業更有效率。 請改為後端存放區類別可以執行這些作業，直接針對資料來源。 例如，UserStore.GetClaimsAsync() 方法可以呼叫 userClaimTable.FindByUserId(user.識別碼） 上執行的查詢資料表直接且傳回方法宣告的清單。

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>自訂角色類別

當實作您自己的儲存體提供者，您必須建立角色類別，即等於[IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx)類別[Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx)命名空間：

下圖顯示 IdentityRole 類別，您必須建立和此類別中實作的介面。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

[IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)介面定義 RoleManager 嘗試執行要求的作業時要呼叫的內容。 介面包含兩個屬性-識別碼和名稱。 [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx)介面可讓您指定的索引鍵，透過一般角色類型**TKey**參數。 Id 屬性的型別符合 TKey 參數的值。

識別 framework 也提供[IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx)介面 （如果沒有泛型參數） 當您想要使用索引鍵的字串值。

下列範例會顯示索引鍵使用整數 IdentityRole 類別。 [識別碼] 欄位設定為符合泛型參數的值為 int。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 如需完整的實作，請參閱[IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs)。 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>自訂角色存放區

您也可以建立 RoleStore 類別提供之角色的所有資料作業方法。 這個類別就相當於[RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) Microsoft.ASP.NET.Identity.EntityFramework 命名空間中的類別。 RoleStore 類別內實作[IRoleStore&lt;TRole、 TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx)並選擇性地[IQueryableRoleStore&lt;TRole、 TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx)介面。

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

下列範例會示範角色存放區類別。 TRole 泛型參數會採用這通常是您定義的 IdentityRole 類別角色類別的類型。 TKey 泛型參數會採用您角色的索引鍵的類型。 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx)介面會定義角色存放區類別中實作的方法。 它包含建立、 更新、 刪除及擷取角色的方法。
- **RoleStore&lt;TRole&gt;**  
  若要自訂 RoleStore，請建立實作 IRoleStore 介面的類別。 您只需要實作這個類別，如果想要使用您的系統上的角色。 使用具名參數的建構函式*資料庫*型別的 ExampleDatabase 是只如何傳遞您的資料存取類別中的圖例。 例如，在 MySQL 實作中，這個建構函式接受 MySQLDatabase 類型的參數。  
  
  如需完整的實作，請參閱[RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) 。

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>重新設定應用程式以使用新的存放裝置提供者

您已實作新的存放裝置提供者。 現在，您必須設定您的應用程式使用這個儲存體提供者。 如果預設的存放裝置提供者已包含在您的專案，您必須移除預設的提供者，並取代您的提供者。

### <a name="replace-default-storage-provider-in-mvc-project"></a>取代預設的 MVC 專案中的存放裝置提供者

1. 在**管理 NuGet 封裝**視窗中，解除安裝**Microsoft ASP.NET Identity EntityFramework**封裝。 您可以藉由搜尋已安裝封裝中 Identity.EntityFramework 找到這個封裝。  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) 您將會詢問您也要解除安裝 Entity Framework。 如果您不需要它的應用程式其他部分中，您就可以將它解除安裝。
2. 在 [模型] 資料夾中 IdentityModels.cs 檔案中，刪除或標記為註解**ApplicationUser**和**ApplicationDbContext**類別。 在 MVC 應用程式，您可以刪除整個 IdentityModels.cs 檔案。 在 Web Form 應用程式中，刪除兩個類別，但請確定您保留也位於 IdentityModels.cs 檔案中的協助程式類別。
3. 如果您的儲存體提供者位於不同的專案中，新增到 web 應用程式的參考。
4. 取代所有參考`using Microsoft.AspNet.Identity.EntityFramework;`使用存放裝置提供者的命名空間陳述式。
5. 在**Startup.Auth.cs**類別中，變更**ConfigureAuth**方法，以使用適當的內容中的單一執行個體。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. 應用程式中\_開始資料夾中，開啟**IdentityConfig.cs**。 在 ApplicationUserManager 類別中，變更**建立**方法來傳回使用者管理員，以便使用您的自訂的使用者存放區。 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. 取代所有參考**ApplicationUser**與**IdentityUser**。
8. 預設專案中未定義在 IUser 介面中的使用者類別包含的某些成員例如，電子郵件、 PasswordHash 和 GenerateUserIdentityAsync。 如果在使用者類別並沒有這些成員，您必須加以實作，或變更使用這些成員的程式碼。
9. 如果您已經建立 RoleManager 的任何執行個體，變更該程式碼以使用新的 RoleStore 類別。  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. 預設專案可供具有索引鍵的字串值的使用者類別。 如果您的使用者類別有不同的型別 （例如整數） 索引鍵，您必須變更專案，以使用您的類型。 請參閱[變更 ASP.NET Identity 中使用者的主索引鍵](change-primary-key-for-users-in-aspnet-identity.md)。
11. 如有需要將連接字串加入 Web.config 檔案。

<a id="other"></a>
## <a name="other-resources"></a>其他資源

- 部落格：[實作 ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- 教學課程和 GIT 的程式碼： [Simple.Data Asp.Net 身分識別提供者](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- 教學課程：[設定基本的身分識別帳戶，並指向外部的 DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)。 由[ @xivSolutions ](https://twitter.com/xivSolutions)。
- 教學課程[： 實作自訂 MySQL ASP.NET Identity 的存放裝置提供者](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent 實體](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/)由[SoftFluent](http://www.softfluent.com/)
- [Azure 資料表儲存體](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/)由 James Randall。
- Azure 資料表儲存體： [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage)由[ @stuartleeks ](https://twitter.com/stuartleeks)。
- [CouchDB / Cloudant 由奧 Wertheim。](https://github.com/danielwertheim/mycouch.aspnet.identity)
- 彈性的搜尋[h:%m 彈性識別](https://github.com/bmbsqd/elastic-identity)Bombsquad AB.由
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/)由 Jonathan Sheely Jonathan Sheely。
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity)由 Antônio Milesi Bastos。
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0)由[ @tourismgeek ](https://twitter.com/tourismgeek)。
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity)由[ILMServices](http://www.ilmservice.com/)。
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- 「 資料庫第一次 」 的使用者存放區產生 EF T4 範本程式碼： [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
