---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: 變更 ASP.NET Identity 中的使用者的主索引鍵 |Microsoft Docs
author: tfitzmac
description: 在 Visual Studio 2013 中，預設的 web 應用程式會使用使用者帳戶的金鑰字串值。 ASP.NET 身分識別可讓您變更的類型...
ms.author: aspnetcontent
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 688a0dc09297a63bcf510aae9c25f303a5627df7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808038"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>變更 ASP.NET Identity 中的使用者的主索引鍵
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 在 Visual Studio 2013 中，預設的 web 應用程式會使用使用者帳戶的金鑰字串值。 ASP.NET 身分識別可讓您變更以符合您資料需求的索引鍵的類型。 例如，您可以從字串變更索引鍵的類型為整數。
> 
> 本主題說明如何開始使用預設 web 應用程式，並將使用者帳戶金鑰變更為整數。 您可以使用相同的修改來實作您的專案中的任何類型的金鑰。 它示範如何在預設 web 應用程式中進行這些變更，但您可以套用類似修改自訂的應用程式。 它會顯示使用 MVC 或 Web Form 時所需的變更。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Visual Studio 2013 Update 2 （或更新版本）
> - 2.1 或更新版本的 ASP.NET 身分識別


若要執行本教學課程步驟，您必須有 Visual Studio 2013 Update 2 （或更新版本） 和 ASP.NET Web 應用程式範本所建立的 web 應用程式。 在 Update 3 中變更範本。 本主題說明如何變更 Update 2 和 Update 3 中的範本。

此主題包括下列章節：

- [變更識別使用者類別中的索引鍵的類型](#userclass)
- [新增使用索引鍵類型的自訂身分識別類別](#customclass)
- [變更要使用的金鑰類型的內容類別和使用者管理員](#context)
- [若要使用的金鑰類型的變更啟動設定](#startup)
- [Update 2 的 mvc，變更將索引鍵的型別傳遞 AccountController](#mvcupdate2)
- [Update 3 的 mvc，變更 AccountController 和 ManageController 傳遞的索引鍵類型](#mvcupdate3)
- [對於 Update 2 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面](#webformsupdate2)
- [對於含 Update 3 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面](#webformsupdate3)
- [執行應用程式](#run)
- [其他資源](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>變更識別使用者類別中的索引鍵的類型

在 ASP.NET Web 應用程式範本所建立的專案中，指定 ApplicationUser 類別，會針對使用者帳戶的索引鍵使用整數。 在 IdentityModels.cs，變更 ApplicationUser 類別繼承自具有一種 IdentityUser **int** TKey 泛型參數。 您也會傳遞您有尚未實作三個自訂類別的名稱。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

您已變更的索引鍵的類型，但根據預設，應用程式的其餘部分仍假設金鑰是字串。 您必須明確指示會假設字串的程式碼中的索引鍵的類型。

在  **ApplicationUser**類別中變更**GenerateUserIdentityAsync**方法以包含整數，如下列醒目提示的程式碼所示。 這項變更不需要 Web Form 專案，使用 Update 3 範本。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>新增使用索引鍵類型的自訂身分識別類別

其他身分識別類別，例如 IdentityUserRole、 IdentityUserClaim、 IdentityUserLogin、 IdentityRole、 UserStore、 RoleStore，都仍會設定為使用字串索引鍵中。 建立新的版本，這些類別的指定索引鍵的整數。 您不需要提供太多的實作程式碼，這些類別中，您主要只要設定 int 當做索引鍵。

將下列類別加入 IdentityModels.cs 檔案。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>變更要使用的金鑰類型的內容類別和使用者管理員

在 IdentityModels.cs，變更其定義**ApplicationDbContext**類別，以使用您的新自訂類別和**int**針對索引鍵，反白顯示的程式碼所示。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

ThrowIfV1Schema 參數不再有效的建構函式。 變更建構函式，因此它未通過 ThrowIfV1Schema 值。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

開啟 IdentityConfig.cs，並將變更**ApplicationUserManger**類別，以使用新的使用者儲存保存資料的類別和**int**索引鍵。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

在 Update 3 範本中，您必須變更 ApplicationSignInManager 類別。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>若要使用的金鑰類型的變更啟動設定

在 Startup.Auth.cs，取代 OnValidateIdentity 程式碼，以反白顯示如下。 請注意，getUserIdCallback 定義中，會將字串值剖析成整數。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

如果您的專案無法辨識的泛型實作**GetUserId**方法中，您可能需要的 ASP.NET 身分識別的 NuGet 套件更新為版本 2.1

您已進行許多變更以使用 ASP.NET Identity 的基礎結構類別。 如果您嘗試編譯專案，您會發現許多錯誤。 幸運的是，剩餘的錯誤都有相似。 身分識別類別需要整數索引鍵，但控制器 （或 Web Form） 傳遞的字串值。 在每個案例中，您需要將呼叫轉換字串與整數**GetUserId&lt;int&gt;**。 您可以逐步完成從編譯錯誤清單，或遵循下列變更。

剩餘的變更取決於您所建立，而且您已安裝哪些更新 Visual Studio 中的專案類型。 您可以直接跳到相關的區段，透過下列連結

- [Update 2 的 mvc，變更將索引鍵的型別傳遞 AccountController](#mvcupdate2)
- [Update 3 的 mvc，變更 AccountController 和 ManageController 傳遞的索引鍵類型](#mvcupdate3)
- [對於 Update 2 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面](#webformsupdate2)
- [對於含 Update 3 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Update 2 的 mvc，變更將索引鍵的型別傳遞 AccountController

開啟 AccountController.cs 檔案。 您需要變更下列方法。

**ConfirmEmail**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**取消關聯**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** 方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword**方法

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

您現在可以[執行應用程式](#run)並註冊新的使用者。

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Update 3 的 mvc，變更 AccountController 和 ManageController 傳遞的索引鍵類型

開啟 AccountController.cs 檔案。 您需要將下列方法變更。

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
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>對於 Update 2 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面

對於 Update 2 的 Web Form，您需要變更下列頁面。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

您現在可以[執行應用程式](#run)並註冊新的使用者。

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>對於含 Update 3 的 Web Form，變更要傳遞的索引鍵類型的帳戶頁面

對於 Web Form Update 3，您需要變更下列頁面。

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

您已完成所有必要變更預設的 Web 應用程式範本。 執行應用程式並註冊新的使用者。 註冊使用者之後, 您會發現 AspNetUsers 資料表已經是一個整數的識別碼資料行。

![新的主要金鑰](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

如果您先前建立 ASP.NET 身分識別的資料表以不同的主索引鍵，您需要進行一些額外的變更。 可能的話，只要刪除現有的資料庫。 當您執行 web 應用程式，並新增使用者時，會以正確的設計重新建立資料庫。 如果刪除是不可能的話，請執行 code first 移轉來變更資料表。 不過，新的整數主索引鍵將不會設定為在資料庫中的 SQL 識別屬性。 您必須手動設定的識別碼資料行，做為識別。

<a id="other"></a>
## <a name="other-resources"></a>其他資源

- [ASP.NET Identity 的自訂儲存體提供者概觀](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [將現有的網站從 SQL 成員資格移轉至 ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [成員資格和使用者設定檔，以 ASP.NET 身分識別的通用提供者資料移轉](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [範例應用程式](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)已變更的主索引鍵
