---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "變更主索引鍵中 ASP.NET 識別的使用者 |Microsoft 文件"
author: tfitzmac
description: "在 Visual Studio 2013 中，預設 web 應用程式會使用使用者帳戶的金鑰字串值。 ASP.NET 識別可讓您變更的類型..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>在 ASP.NET Identity 中的使用者變更主索引鍵
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 在 Visual Studio 2013 中，預設 web 應用程式會使用使用者帳戶的金鑰字串值。 ASP.NET 識別可讓您變更以符合資料需求索引鍵的類型。 例如，您可以從字串變更索引鍵的類型為整數。
> 
> 本主題說明如何開始使用預設 web 應用程式，並將使用者帳戶金鑰變更為整數。 您可以使用相同的修改，在您的專案中實作任何類型的索引鍵。 它示範如何進行這些變更在預設 web 應用程式，但您無法套用類似的自訂應用程式的修改。 它會顯示使用 MVC 或 Web Form 時所需的變更。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Visual Studio 2013 Update 2 （或更新版本）
> - ASP.NET Identity 2.1 或更新版本


若要執行本教學課程步驟，您必須有 Visual Studio 2013 Update 2 （或更新版本） 和 ASP.NET Web 應用程式範本所建立的 web 應用程式。 在 Update 3 中變更範本。 本主題說明如何變更更新 2 和 Update 3 中的範本。

此主題包括下列章節：

- [變更中識別的使用者類別的索引鍵的類型](#userclass)
- [新增自訂身分識別類別，可使用的金鑰類型](#customclass)
- [變更要使用的金鑰類型的內容類別及使用者管理員](#context)
- [變更啟動設定来使用的金鑰類型](#startup)
- [MVC Update 2 中，變更將索引鍵的型別傳遞 AccountController](#mvcupdate2)
- [MVC 中使用 Update 3，變更 AccountController 以及 ManageController 傳遞的索引鍵類型](#mvcupdate3)
- [Web Form Update 2，變更要傳遞的索引鍵類型的帳戶頁面](#webformsupdate2)
- [Web Form 的 Update 3，將變更傳遞的索引鍵類型的帳戶頁面](#webformsupdate3)
- [執行應用程式](#run)
- [其他資源](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>變更中識別的使用者類別的索引鍵的類型

在 ASP.NET Web 應用程式範本所建立的專案中，指定 ApplicationUser 類別使用整數之金鑰的使用者帳戶。 在 IdentityModels.cs，變更 ApplicationUser 類別繼承自具有一種 IdentityUser **int** TKey 泛型參數。 您也會傳遞三個自訂類別尚未不實作它的名稱。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

您已變更的金鑰類型，但根據預設，應用程式的其餘部分仍假設索引鍵是字串。 您必須明確指示會假設字串的程式碼中的索引鍵的類型。

在**ApplicationUser**類別中，變更**GenerateUserIdentityAsync**方法，將包含 int、 反白顯示的下列程式碼所示。 這項變更不需要的 Web Form Update 3 範本專案。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>新增自訂身分識別類別，可使用的金鑰類型

其他身分識別類別，例如 IdentityUserRole、 IdentityUserClaim、 IdentityUserLogin、 IdentityRole、 UserStore、 RoleStore，仍會使用字串索引鍵設定。 建立指定索引鍵的整數這些類別的新的版本。 您不需要提供太多的實作程式碼，這些類別中，您主要是只做為索引鍵設定 int。

將下列類別加入至 IdentityModels.cs 檔案。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>變更要使用的金鑰類型的內容類別及使用者管理員

在 IdentityModels.cs，變更的定義**ApplicationDbContext**類別，以使用您的新自訂類別和**int**的索引鍵，反白顯示的程式碼所示。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

ThrowIfV1Schema 參數不再有效的建構函式中。 變更建構函式，讓它未通過 ThrowIfV1Schema 值。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

開啟 IdentityConfig.cs，並將變更**ApplicationUserManger**類別，以使用新的使用者儲存的保存資料的類別和**int**索引鍵。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

在 Update 3 範本中，您必須變更 ApplicationSignInManager 類別。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>變更啟動設定来使用的金鑰類型

在 Startup.Auth.cs，取代 OnValidateIdentity 程式碼，以反白顯示下面。 請注意 getUserIdCallback 定義中，將字串值剖析為整數。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

如果您的專案無法辨識的泛型實作**GetUserId**方法，您可能需要更新到 2.1 版的 ASP.NET Identity NuGet 套件

您對進行許多變更 ASP.NET Identity 所使用的基礎結構類別。 如果您嘗試編譯專案，您會發現許多錯誤。 幸運的是，其他錯誤都有相似。 識別類別必須要有整數索引鍵，但控制器 （或 Web Form） 傳遞的字串值。 在各案例中，您需要從字串和整數轉換，藉由呼叫**GetUserId&lt;int&gt;**。 您可以逐步完成從編譯錯誤清單，或請遵循以下的變更。

剩餘的變更取決於您所建立，而且您已安裝 Visual Studio 中的哪個更新專案的類型。 您可以直接前往下列連結相關的章節

- [MVC Update 2 中，變更將索引鍵的型別傳遞 AccountController](#mvcupdate2)
- [MVC 中使用 Update 3，變更 AccountController 以及 ManageController 傳遞的索引鍵類型](#mvcupdate3)
- [Web Form Update 2，變更要傳遞的索引鍵類型的帳戶頁面](#webformsupdate2)
- [Web Form 的 Update 3，將變更傳遞的索引鍵類型的帳戶頁面](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>MVC Update 2 中，變更將索引鍵的型別傳遞 AccountController

開啟 AccountController.cs 檔案。 您需要變更下列方法。

**ConfirmEmail**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**取消關聯**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

您現在可以[執行應用程式](#run)並註冊新的使用者。

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>MVC 中使用 Update 3，變更 AccountController 以及 ManageController 傳遞的索引鍵類型

開啟 AccountController.cs 檔案。 您需要變更以下的方法。

**ConfirmEmail**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

開啟 ManageController.cs 檔案。 您需要變更下列方法。

**索引**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

您現在可以[執行應用程式](#run)並註冊新的使用者。

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Web Form Update 2，變更要傳遞的索引鍵類型的帳戶頁面

Web Form Update 2，您要變更之下列頁面。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

您現在可以[執行應用程式](#run)並註冊新的使用者。

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Web Form 的 Update 3，將變更傳遞的索引鍵類型的帳戶頁面

Web Form 的 Update 3，您要變更之下列頁面。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>執行應用程式

您已完成所有的預設 Web 應用程式範本所需的變更。 執行應用程式並註冊新的使用者。 註冊使用者之後, 您會發現 AspNetUsers 資料表有整數的識別碼資料行。

![新的主要金鑰](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

如果您先前已經建立 ASP.NET 識別的資料表具有不同主索引鍵，您需要進行一些額外的變更。 可能的話，只要刪除現有的資料庫。 當您執行 web 應用程式，並加入新的使用者，會以正確的設計重新建立資料庫。 如果刪除，則不可能執行 code first 移轉，若要變更的資料表。 不過，新的整數的主索引鍵將不設定為資料庫中的 SQL 識別屬性。 您必須手動設定的識別碼資料行，做為識別。

<a id="other"></a>
## <a name="other-resources"></a>其他資源

- [ASP.NET 識別的自訂儲存體提供者的概觀](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [從 SQL 成員資格移轉現有的網站，以 ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [成員資格和 ASP.NET 識別的使用者設定檔通用的提供者資料移轉](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [範例應用程式](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)已變更的主索引鍵
