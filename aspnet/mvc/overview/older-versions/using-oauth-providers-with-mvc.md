---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: "OAuth 提供者使用 MVC 4 |Microsoft 文件"
author: tfitzmac
description: "本教學課程會示範如何建立 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebo 認證登入..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 965d2e740cc76838b1b4e1c618a2a6d784672fcc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="using-oauth-providers-with-mvc-4"></a>MVC 4 搭配使用 OAuth 提供者
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何建立 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebook、 Twitter、 Microsoft 或 Google 認證登入，然後整合到那些提供者的功能部分您web 應用程式。 為了簡單起見，本教學課程著重於使用來自 Facebook 的認證。
> 
> 若要使用 ASP.NET MVC 5 web 應用程式中的外部認證，請參閱[建立 ASP.NET MVC 5 應用程式與 Facebook、 Google OAuth2 和 OpenID 登入](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
> 
> 啟用瀏覽的網站中的這些認證提供更大的優點，因為已經有數百萬名使用者的帳戶與這些外部提供者。 這些使用者可能比較想要註冊您的站台如果它們不需要建立並記住一組新的認證。 此外，使用者已透過其中一個提供者登入之後，您可以合併社交提供者的作業。


## <a name="what-youll-build"></a>您將建置

在本教學課程中有兩個主要目標：

1. 讓使用者從 OAuth 提供者的認證登入。
2. 從提供者擷取帳戶資訊和整合該資訊進行帳戶註冊您的網站。

雖然使用 Facebook 做為驗證提供者專注在本教學課程的範例，您可以修改程式碼以使用任何提供者。 實作任何提供者的步驟會在本教學課程，您會看到的步驟非常類似。 您只會注意到顯著的差異提供者的應用程式開發介面直接呼叫時設定。

## <a name="prerequisites"></a>必要條件

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)或[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

或

- Microsoft Visual Studio 2010 SP1 或[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

此外，本主題假設您有關於 ASP.NET MVC 和 Visual Studio 的基本知識。 如果您需要 ASP.NET MVC 4 的簡介，請參閱[簡介 ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。

## <a name="create-the-project"></a>建立專案

在 Visual Studio 中，建立新 ASP.NET MVC 4 Web 應用程式，並將其命名&quot;OAuthMVC&quot;。 您可以針對.NET Framework 4.5 或 4。

![建立專案](using-oauth-providers-with-mvc/_static/image1.png)

在新的 ASP.NET MVC 4 專案 視窗中，選取**網際網路應用程式**留下**Razor**做為檢視引擎。

![選取 網際網路應用程式](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>啟用的提供者

當您使用網際網路應用程式範本建立的 MVC 4 web 應用程式時，使用名為 AuthConfig.cs 應用程式中的檔案建立專案\_開始資料夾。

![AuthConfig 檔案](using-oauth-providers-with-mvc/_static/image3.png)

AuthConfig 檔案包含要登錄外部驗證提供者的用戶端程式碼。 根據預設，此程式碼會標記為註解，因此未啟用任何外部提供者。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

您必須使用外部驗證用戶端的這段程式碼取消註解。 請取消註解您想要包含在您的網站中的提供者。 此教學課程中，您將只能啟用 Facebook 認證。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

請注意在上述範例中，這個方法會包含空字串的註冊參數。 如果您嘗試執行應用程式現在，應用程式擲回引數例外狀況，因為參數不允許空字串。 若要提供有效的值，您必須使用外部提供者，註冊您的網站下一節中所示。

## <a name="registering-with-an-external-provider"></a>使用外部提供者登錄

若要驗證從外部提供者的認證的使用者，您必須註冊您的網站與提供者。 當您註冊您的網站時，您會收到註冊用戶端時所要包括的參數 （例如索引鍵或識別碼和密碼）。 您必須有您想要使用的提供者的帳戶。

本教學課程不會顯示所有使用這些提供者註冊，必須執行的步驟。 步驟通常是不困難。 若要成功註冊您的網站，請遵循這些站台所提供的指示。 若要開始使用註冊您的網站，請參閱的開發人員網站：

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

當註冊您的網站與 Facebook，您可以提供&quot;localhost&quot;站台網域和`&quot;http://localhost/&quot;`對於 URL，請在下圖所示。 使用 localhost 適用於大部分的提供者，但目前無法搭配 Microsoft 提供者。 Microsoft 提供者，您必須包含有效的網站 URL。

![註冊站台](using-oauth-providers-with-mvc/_static/image4.png)

在上圖中，已移除應用程式識別碼、 應用程式密碼及連絡人的電子郵件的值。 當您實際註冊您的網站時，這些值將會出現。 您會想要因為您將會加入您的應用程式，請注意應用程式識別碼和應用程式密碼的值。

## <a name="creating-test-users"></a>建立測試使用者

如果您不介意來測試您的網站使用現有的 Facebook 帳戶，您可以略過本節。

您的應用程式內的 Facebook 應用程式管理頁面中，您可以輕鬆建立測試使用者。 您可以使用這些測試帳戶來登入您的網站。 按一下 建立測試使用者**角色**左瀏覽窗格中按一下連結**建立**連結。

![建立測試使用者](using-oauth-providers-with-mvc/_static/image5.png)

Facebook 站台會自動建立測試帳戶，您所要求的數目。

## <a name="adding-application-id-and-secret-from-the-provider"></a>新增應用程式識別碼和密碼，從提供者

既然您已收到識別碼和密碼，來自 Facebook，返回 AuthConfig 檔案，並將它們加入做為參數值。 如下所示的值不是實際的值。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>外部認證登入

這就是您必須啟用您的網站中的外部認證執行。 執行您的應用程式，然後按一下右上角的登入連結。 範本會自動識別您已註冊 Facebook 做為提供者，並包含提供者 按鈕。 如果您註冊多個提供者，針對每個按鈕會自動包含。

![外部登入](using-oauth-providers-with-mvc/_static/image6.png)

本教學課程並未涵蓋如何自訂登入按鈕的外部提供者。 如需該資訊，請參閱[自訂登入 UI，使用 OAuth/OpenID 時](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)。

按一下 [Facebook] 按鈕以 Facebook 認證登入。 當您選取其中一個外部提供者時，您會重新導向至該網站，而且該服務提示您登入。

下圖顯示 Facebook 登入畫面。 它會註明您使用您的 Facebook 帳戶登入名為 oauthmvcexample 網站。

![facebook 驗證](using-oauth-providers-with-mvc/_static/image7.png)

登入 Facebook 認證之後，頁面會通知使用者站台，將會擁有存取權的基本資訊。

![要求權限](using-oauth-providers-with-mvc/_static/image8.png)

選取之後**前往應用程式**，使用者必須註冊站台。 使用者已透過 Facebook 認證登入之後下, 圖顯示 [註冊] 頁面。 使用者名稱是以從提供者名稱通常預先填入。

![註冊](using-oauth-providers-with-mvc/_static/image9.png)

按一下**註冊**以完成註冊。 關閉瀏覽器。

您可以看到您的資料庫中已加入新的帳戶。 在 [伺服器總管] 中，開啟**DefaultConnection**資料庫，然後開啟**資料表**資料夾。

![資料庫資料表](using-oauth-providers-with-mvc/_static/image10.png)

以滑鼠右鍵按一下**UserProfile**資料表，然後選取**顯示資料表資料**。

![顯示資料](using-oauth-providers-with-mvc/_static/image11.png)

您會看到您加入新的帳戶。 查看中的資料**網頁\_OAuthMembership**資料表。 您會看到您剛才加入的帳戶的外部提供者相關的詳細資料。

如果您只想要啟用外部驗證，您已完成。 不過，您可以進一步整合提供者資訊到新的使用者註冊程序，如下列各節中所示。

## <a name="create-models-for-additional-user-information"></a>建立模型以供其他使用者資訊

您注意到上一節，您不需要擷取要處理的內建帳戶註冊的任何其他資訊。 不過，最外部提供者傳遞使用者的其他資訊。 下列各節顯示如何以保留該資訊，並將它儲存至資料庫。 具體來說，您將會保留使用者的完整名稱，使用者的個人網頁，URI 的值和值，指出是否 Facebook 已驗證帳戶。

您將使用[Code First 移轉](https://msdn.microsoft.com/en-us/data/jj591621)加入儲存額外的使用者資訊的資料表。 因此第一次您必須建立在目前資料庫的快照集，您會加入至現有的資料庫資料表。 藉由建立目前資料庫的快照集，您稍後可以建立包含新的資料表移轉。 若要建立目前資料庫的快照集：

1. 開啟**Package Manager Console**
2. 執行命令**啟用移轉**
3. 執行命令**IgnoreChanges 新增移轉初始 –**
4. 執行命令**更新資料庫**

現在，您將加入新的屬性。 在 [模型] 資料夾中，開啟 AccountModels.cs 檔案，並尋找 RegisterExternalLoginModel 類別。 RegisterExternalLoginModel 類別保留來自驗證提供者的值。 加入以反白顯示下面名為 FullName] 和 [連結的屬性。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

也在 AccountModels.cs，加入新的類別稱為 ExtraUserInformation。 此類別代表會在資料庫中建立的新資料表。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

UsersContext 類別中加入反白顯示下列程式碼以建立新類別 DbSet 屬性。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

您現在準備建立新的資料表。 開啟 Package Manager Console 再次和此時間：

1. 執行命令**新增移轉 AddExtraUserInformation**
2. 執行命令**更新資料庫**

資料庫中現在有新的資料表。

## <a name="retrieve-the-additional-data"></a>擷取其他資料

有兩種方式，以擷取其他的使用者資料。 第一種方式是在保留期間所傳入，根據預設，驗證要求的使用者資料。 第二種方式是特別呼叫提供者應用程式開發介面，並要求的詳細資訊。 FullName 和連結的值會自動傳遞回由 Facebook。 透過對 Facebook API 呼叫擷取值，指出是否 Facebook 已驗證帳戶。 首先，您將會填入值 FullName 和連結，並將更新版本中，會取得已驗證的值。

若要擷取的其他使用者資料，請開啟**AccountController.cs**檔案**控制器**資料夾。

此檔案包含的邏輯記錄、 註冊和管理帳戶。 特別是，請注意呼叫的方法**ExternalLoginCallback**和**ExternalLoginConfirmation**。 在這些方法中，您可以加入程式碼以自訂您的應用程式的外部登入作業。 第一行**ExternalLoginCallback**方法包含：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

額外的使用者資料會傳遞回**ExtraData**屬性**AuthenticationResult**從傳回的物件**VerifyAuthentication**方法。 Facebook 用戶端包含下列中的值**ExtraData**屬性：

- id
- name
- link
- 性別
- accesstoken

其他提供者將 ExtraData 屬性中有相似但稍微不同的資料。

如果使用者是您的站台的新手，您會擷取一些額外的資料，並將資料傳遞到 [確認] 檢視。 只有當使用者是您的站台的新執行方法中的程式碼的最後一個區塊。 取代為下列行：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

以這行：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

這項變更只會包含 FullName 和 連結屬性的值。

在**ExternalLoginConfirmation**方法，修改下列程式碼以反白顯示，儲存額外的使用者資訊。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>調整檢視

您從提供者擷取額外的使用者資料將顯示在 [註冊] 頁面中。

在**檢視**/**帳戶**資料夾中，開啟**ExternalLoginConfirmation.cshtml**。 下列使用者名稱與現有欄位，加入 FullName、 連結和 PictureLink 欄位。

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

現在您已經準備就緒執行應用程式並註冊新的使用者與儲存的其他資訊。 您必須具有尚未註冊與站台的帳戶。 您可以使用不同的測試帳戶，或刪除資料列中的**UserProfile**和**網頁\_OAuthMembership**您想要重複使用之帳戶的資料表。 刪除這些資料列，您要確定此帳戶重新登錄。

執行應用程式，並註冊新的使用者。 請注意，此時確認頁面包含多個值。

![註冊](using-oauth-providers-with-mvc/_static/image12.png)

完成之後註冊，請關閉瀏覽器。 查詢資料庫中的新值會注意到**ExtraUserInformation**資料表。

## <a name="install-nuget-package-for-facebook-api"></a>安裝 NuGet 套件的 Facebook 應用程式開發介面

提供 Facebook [API](https://developers.facebook.com/docs/reference/apis/) ，您可以呼叫執行的作業。 您可以呼叫 Facebook API，將傳送 HTTP 要求，或使用 安裝 NuGet 套件，可促進傳送這些要求。 使用 NuGet 封裝會顯示在本教學課程中，但安裝 NuGet 套件並不重要。 本教學課程會示範如何使用 Facebook C# SDK 封裝。 另外還有其他協助呼叫 Facebook API 的 NuGet 封裝。

從**管理 NuGet 封裝**windows 中，選取 Facebook C# SDK 封裝。

![安裝套件](using-oauth-providers-with-mvc/_static/image13.png)

您將使用 Facebook C# SDK 呼叫的使用者需要存取權杖的作業。 下一節將示範如何取得存取權杖。

## <a name="retrieve-access-token"></a>擷取存取權杖

使用者的認證通過驗證後，最外部提供者傳回存取權杖。 此存取權杖是非常重要，因為它可讓您呼叫作業，只可用於驗證的使用者。 因此，擷取和儲存的存取語彙基元很重要時，您想要提供更多的功能。

根據外部提供者，存取權杖只在有限可能是時間的有效的。 若要確保您擁有的有效存取權杖，您將會擷取每次使用者登入，並將它儲存成工作階段值，而非將它儲存到資料庫。

在**ExternalLoginCallback**方法，存取語彙基元也會跟著傳遞回**ExtraData**屬性**AuthenticationResult**物件。 將反白顯示的程式碼加入**ExternalLoginCallback**儲存存取權杖**工作階段**物件。 每次使用者的 Facebook 帳戶登入時，會執行這段程式碼。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

雖然這個範例會擷取來自 Facebook 存取權杖，您可以透過名為相同的索引鍵從任何外部提供者擷取存取權杖&quot;accesstoken&quot;。

## <a name="logging-off"></a>登出

預設值**登出**方法使用者登出您的應用程式，但不會記錄使用者登出外部提供者。 若要讓使用者從 Facebook 登並防止保存使用者登出後權杖，您可以加入下列反白顯示的程式碼加入**登出**AccountController 中的方法。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

您在中提供的值`next`參數是使用者已登出 Facebook 後使用的 URL。 測試時在本機電腦上，您會提供正確的連接埠號碼的本機網站。 在生產環境中，您會提供預設頁面上，例如 contoso.com。

## <a name="retrieve-user-information-that-requires-the-access-token"></a>擷取需要的存取權杖的使用者資訊

現在，您有儲存的存取權杖，並已安裝的 Facebook C# SDK 套件，您可以一起使用這些要求來自 Facebook 的其他使用者資訊。 在**ExternalLoginConfirmation**方法，建立的執行個體**FacebookClient**類別藉由傳遞存取權杖的值。 要求的值**驗證**目前已驗證使用者的屬性。 **驗證**屬性會指出是否 Facebook 已驗證的帳戶透過其他方式，例如將訊息傳送到行動電話。 將此值儲存在資料庫中。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

您一次必須刪除使用者的資料庫中的記錄，或使用不同的 Facebook 帳戶。

執行應用程式，並註冊新的使用者。 查看**ExtraUserInformation**資料表來查看已驗證屬性的值。

## <a name="conclusion"></a>結論

在此教學課程中，您可以建立網站與 Facebook 整合使用者驗證和註冊資料。 您會學為設定之 MVC 4 web 應用程式，以及如何自訂該預設行為的預設行為。

## <a name="related-topics"></a>相關主題

- [驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
