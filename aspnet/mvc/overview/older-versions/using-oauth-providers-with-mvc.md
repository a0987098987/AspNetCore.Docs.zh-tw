---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: 使用 OAuth 提供者與 MVC 4 |Microsoft Docs
author: tfitzmac
description: 本教學課程會示範如何建置 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebo 的認證來登入...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 9b0db2775db5c74762bdc55328ad44ef7ebe75ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833008"
---
<a name="using-oauth-providers-with-mvc-4"></a>使用 OAuth 提供者與 MVC 4
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本教學課程會示範如何建置 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebook、 Twitter、 Microsoft 或 Google 的認證登入，並整合的一些功能，這些提供者將從您web 應用程式。 為了簡單起見，本教學課程著重於使用來自 Facebook 的認證。
> 
> 若要使用 ASP.NET MVC 5 web 應用程式中的外部認證，請參閱[建立 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth2 和 OpenID 登入](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。
> 
> 啟用您的網站中的這些認證提供極大的好處，因為數百萬位使用者已將這些外部提供者的帳戶。 這些使用者可能會更有必要，如果它們不需要建立並記住一組新的認證，登入您的網站。 此外，使用者已透過其中一個提供者登入之後，您可以將社交提供者的作業。


## <a name="what-youll-build"></a>您將建置

在本教學課程中有兩個主要目標：

1. 讓使用者從 OAuth 提供者的認證來登入。
2. 從提供者擷取帳戶資訊，並整合您的網站的帳戶註冊這項資訊。

雖然本教學課程中的範例著重於使用 Facebook 作為驗證提供者，您可以修改程式碼來使用任何提供者。 實作任何提供者的步驟會在本教學課程中，您會看到的步驟非常類似。 您只會發現顯著差異時直接呼叫提供者的 API 設定。

## <a name="prerequisites"></a>必要條件

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)或[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

或

- Microsoft Visual Studio 2010 SP1 或[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

此外，本主題假設您有基本了解 ASP.NET MVC 和 Visual Studio。 如果您需要 ASP.NET MVC 4 的簡介，請參閱[ASP.NET MVC 4 簡介](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。

## <a name="create-the-project"></a>建立專案

在 Visual Studio 中建立新 ASP.NET MVC 4 Web 應用程式，並將它命名&quot;OAuthMVC&quot;。 您可以針對.NET Framework 4.5 或 4。

![建立專案](using-oauth-providers-with-mvc/_static/image1.png)

在 [新增 ASP.NET MVC 4 專案] 視窗中，選取**網際網路應用程式**並保留**Razor**做為檢視引擎。

![選取 網際網路應用程式](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>啟用提供者

當您建立的 MVC 4 web 應用程式使用網際網路應用程式範本時，專案以名為 AuthConfig.cs 應用程式中的檔案建立\_開始資料夾。

![AuthConfig 檔案](using-oauth-providers-with-mvc/_static/image3.png)

AuthConfig 檔案包含程式碼以註冊的外部驗證提供者的用戶端。 根據預設，此程式碼標記為註解，因此沒有外部提供者會啟用。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

您必須使用外部驗證用戶端的這段程式碼取消註解。 您取消註解您想要包含在您的網站中的提供者。 本教學課程中，您將只啟用 Facebook 認證。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

請注意在上述範例中，此方法包含空字串的註冊參數。 如果您嘗試執行應用程式現在，應用程式擲回引數例外狀況，因為參數不允許空字串。 若要提供有效的值，您必須向外部提供者，註冊您的網站下, 一節中所示。

## <a name="registering-with-an-external-provider"></a>使用外部提供者註冊

若要驗證從外部提供者的認證的使用者，您必須註冊您的網站與提供者。 當您註冊您的網站時，您會收到包含註冊用戶端時的參數 （例如金鑰或識別碼和密碼）。 您必須有您想要使用的提供者的帳戶。

本教學課程不會顯示所有使用這些提供者註冊時，您必須執行的步驟。 步驟通常都不困難。 若要順利註冊您的網站，請遵循這些站台上提供的指示。 若要開始註冊您的網站，請參閱 「 開發人員網站：

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

當您向 Facebook 註冊您的網站，您可以提供&quot;localhost&quot;站台網域和`&quot;http://localhost/&quot;`url，如下圖所示。 使用 localhost 適用於大部分的提供者，但目前不適用於 Microsoft 提供者。 Microsoft 提供者，您必須包含有效的網站 URL。

![註冊網站](using-oauth-providers-with-mvc/_static/image4.png)

在上圖中，已移除應用程式識別碼、 應用程式祕密和連絡人的電子郵件的值。 當您實際註冊您的網站時，這些值將會出現。 您會想要注意應用程式識別碼和應用程式祕密的值，因為您會將它們加入您的應用程式。

## <a name="creating-test-users"></a>建立測試使用者

如果您不介意使用現有的 Facebook 帳戶來測試您的網站，您可以略過本節。

您可以輕鬆建立 Facebook 應用程式管理頁面中的應用程式的測試使用者。 您可以使用這些測試帳戶登入您的網站。 您可以建立測試使用者依序按一下**角色**連結，在左側的導覽窗格中，然後按一下**建立**連結。

![建立測試使用者](using-oauth-providers-with-mvc/_static/image5.png)

Facebook 站台會自動建立測試帳戶，您要求的數目。

## <a name="adding-application-id-and-secret-from-the-provider"></a>從提供者中新增應用程式識別碼和祕密

現在，您收到的識別碼和密碼從 Facebook，請回到 AuthConfig 檔案，並將它們加入做為參數值。 如下所示的值不是真正的值。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>外部認證登入

這是您只需要啟用您的網站中的外部認證。 執行您的應用程式，然後按一下右上角中的 [登入] 連結。 範本會自動識別您已註冊 Facebook 做為提供者，並包含提供者按鈕。 如果您註冊多個提供者，針對每個按鈕就會自動包含。

![外部登入](using-oauth-providers-with-mvc/_static/image6.png)

本教學課程並未涵蓋如何自訂登入按鈕外部提供者。 如需該資訊，請參閱[自訂登入 UI，使用 OAuth/OpenID 時](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)。

按一下 [Facebook] 按鈕，以 Facebook 認證登入。 當您選取其中一個外部提供者時，您會被重新導向至該網站，並提示來登入該服務。

下圖會顯示適用於 Facebook 登入畫面。 它會註明您使用您的 Facebook 帳戶登入名為 oauthmvcexample 的網站。

![facebook 驗證](using-oauth-providers-with-mvc/_static/image7.png)

登入 Facebook 認證之後，畫面會告知使用者站台，將會擁有存取權的基本資訊。

![要求權限](using-oauth-providers-with-mvc/_static/image8.png)

選取之後**移至應用程式**，使用者必須註冊站台。 下圖顯示 [註冊] 頁面之後使用者已登入 Facebook 認證。 通常從提供者名稱預先填入使用者名稱。

![註冊](using-oauth-providers-with-mvc/_static/image9.png)

按一下 **註冊**以完成註冊。 關閉瀏覽器。

您可以看到您的資料庫中已加入新的帳戶。 在 [伺服器總管] 中，開啟**DefaultConnection**資料庫，並開啟**資料表**資料夾。

![資料庫資料表](using-oauth-providers-with-mvc/_static/image10.png)

以滑鼠右鍵按一下**UserProfile**資料表，然後選取**顯示資料表資料**。

![顯示資料](using-oauth-providers-with-mvc/_static/image11.png)

您會看到您所新增的新帳戶。 在中的資料看起來**網頁\_OAuthMembership**資料表。 您會看到您剛才新增的帳戶的外部提供者的相關詳細資料。

如果您只想要啟用外部驗證，您已完成。 不過，您可以進一步整合來自提供者的資訊到新的使用者註冊程序，如下列各節中所示。

## <a name="create-models-for-additional-user-information"></a>建立模型的其他使用者資訊

您注意到在先前章節中，您不需要擷取才能內建的帳戶註冊的任何其他資訊。 不過，大部分的外部提供者傳遞回其他使用者的相關資訊。 下列各節說明如何保留該資訊，並將它儲存到資料庫。 具體來說，您將會保留使用者的完整名稱，使用者的個人網頁的 URI 值和值，指出是否 Facebook 已驗證的帳戶。

您將使用[Code First 移轉](https://msdn.microsoft.com/data/jj591621)加入資料表來儲存額外的使用者資訊。 您將加入資料表至現有的資料庫，因此您首先要建立目前資料庫的快照集。 藉由建立目前資料庫的快照集，您可以稍後建立包含新的資料表移轉。 若要建立目前資料庫的快照集：

1. 開啟**套件管理員主控台**
2. 執行命令**啟用移轉**
3. 執行命令**IgnoreChanges 新增移轉初始 –**
4. 執行命令**更新資料庫**

現在，您將加入新的屬性。 在 [模型] 資料夾中，開啟 AccountModels.cs 檔案，並尋找 RegisterExternalLoginModel 類別。 RegisterExternalLoginModel 類別保留來自驗證提供者的值。 新增稱為 FullName 和連結，以下的醒目提示的屬性。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

也在 AccountModels.cs，加入新的類別，稱為 ExtraUserInformation。 此類別代表會在資料庫中建立的新資料表。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

在 UsersContext 類別中加入醒目提示的程式碼來建立新的類別的 DbSet 屬性。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

您現在已準備好建立新的資料表。 一次，這次，請開啟套件管理員主控台：

1. 執行命令**新增移轉 AddExtraUserInformation**
2. 執行命令**更新資料庫**

資料庫中現在有新的資料表。

## <a name="retrieve-the-additional-data"></a>擷取其他的資料

有兩種方式可擷取其他使用者資料。 第一種方式是保留使用者資料期間所傳遞回，根據預設，驗證要求。 第二種方式是特別呼叫提供者 API，並要求的詳細資訊。 FullName 和連結的值會自動傳遞回 by Facebook。 值，指出是否 Facebook 已驗證的帳戶是透過對 Facebook API 的呼叫來擷取。 首先，您將為 FullName 和連結，來填入值，並將更新版本中，會取得已驗證的值。

若要擷取的其他使用者資料，請開啟**AccountController.cs**中的檔案**控制站**資料夾。

此檔案包含的邏輯記錄、 註冊和管理帳戶。 特別是，請注意呼叫的方法**ExternalLoginCallback**並**ExternalLoginConfirmation**。 在這些方法中，您可以新增自訂應用程式的外部登入作業的程式碼。 第一行**ExternalLoginCallback**方法包含：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

其他使用者資料會傳回到**ExtraData**屬性**AuthenticationResult**從傳回的物件**VerifyAuthentication**方法。 Facebook 用戶端包含下列值**ExtraData**屬性：

- id
- 名稱
- 連結
- 性別
- accesstoken

其他提供者都會在 ExtraData 屬性類似，但稍有不同的資料。

如果您的站台的新手使用者，您會擷取部分的其他資料，並將該資料傳遞到 [確認] 檢視。 只有當使用者是您的站台的新手，就會執行方法中的程式碼的最後一個區塊。 將下面這一行：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

使用下列這一行：

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

這項變更只會包含 FullName 和 連結屬性的值。

在  **ExternalLoginConfirmation**方法中，修改下列程式碼以反白顯示儲存額外的使用者資訊。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>調整檢視

在 [註冊] 頁面中，將會顯示您從提供者擷取其他使用者資料。

在 **檢視**/**帳戶**資料夾中，開啟**ExternalLoginConfirmation.cshtml**。 下方 使用者名稱與現有欄位，請為 FullName、 連結和 PictureLink 新增欄位。

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

您就幾乎準備好執行應用程式，並註冊新的使用者儲存的其他資訊。 您必須已經不是向網站註冊的帳戶。 您可以使用不同的測試帳戶，或刪除的資料列**UserProfile**並**網頁\_OAuthMembership**您想要重複使用之帳戶的資料表。 刪除資料列，您要確定帳戶已註冊一次。

執行應用程式並註冊新的使用者。 請注意，這次 [確認] 頁面包含多個值。

![註冊](using-oauth-providers-with-mvc/_static/image12.png)

完成註冊之後, 關閉瀏覽器。 請注意新的值，在資料庫中尋找**ExtraUserInformation**資料表。

## <a name="install-nuget-package-for-facebook-api"></a>安裝適用於 Facebook API 的 NuGet 套件

提供 Facebook [API](https://developers.facebook.com/docs/reference/apis/) ，您可以呼叫執行作業。 您可以呼叫 Facebook API，將傳送的 HTTP 要求導向，或使用 安裝 NuGet 套件，可簡化傳送這些要求。 使用 NuGet 套件會顯示在本教學課程中，但安裝 NuGet 套件並不重要。 本教學課程會示範如何使用 Facebook C# SDK 套件。 另外還有其他協助呼叫 Facebook API 的 NuGet 封裝。

從**管理 NuGet 套件**windows 中，選取 Facebook C# SDK 封裝。

![安裝套件](using-oauth-providers-with-mvc/_static/image13.png)

您將使用 Facebook C# SDK 呼叫使用者需要存取權杖的作業。 下節會說明如何取得存取權杖。

## <a name="retrieve-access-token"></a>擷取存取權杖

使用者的認證通過驗證後，最外部提供者傳回存取權杖。 此存取權杖是非常重要，因為它可讓您呼叫作業，則只能用於已驗證的使用者。 因此，擷取及儲存存取權杖是必要時您想要提供更多的功能。

根據外部提供者，存取權杖只在有限可能是時間的有效的。 若要確保您擁有有效的存取權杖，您會擷取它每次使用者登入，並將它儲存為工作階段值，而非將它儲存到資料庫。

在  **ExternalLoginCallback**方法，存取權杖也會跟著傳遞回到**ExtraData**屬性**AuthenticationResult**物件。 將反白顯示的程式碼加入**ExternalLoginCallback**以儲存存取權杖**工作階段**物件。 每次使用者登入的 Facebook 帳戶，都會執行此程式碼。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

雖然此範例會擷取來自 Facebook 存取權杖，您可以透過相同的索引鍵名稱為來自任何外部提供者擷取存取權杖&quot;accesstoken&quot;。

## <a name="logging-off"></a>登出

預設值**登出**方法記錄使用者登出您的應用程式，但不加以記錄將使用者登出外部提供者。 登使用者登出 Facebook，並防止保存之後，使用者已登出此語彙基元中，您可以新增下列醒目提示的程式碼，以**登出**AccountController 中的方法。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

在您提供的值`next`參數是要使用之後使用者已登出 Facebook 的 URL。 在本機電腦上測試時，您會提供正確的連接埠號碼，您本機站台。 在生產環境中，您會提供預設頁面上，例如 contoso.com。

## <a name="retrieve-user-information-that-requires-the-access-token"></a>擷取需要存取權杖的使用者資訊

既然您已儲存的存取權杖，並安裝 Facebook C# SDK 封裝，您可以一起使用這些向 Facebook 要求額外的使用者資訊。 在  **ExternalLoginConfirmation**方法，建立的執行個體**FacebookClient**類別傳遞存取權杖的值。 要求的值**驗證**目前已經過驗證的使用者屬性。 **驗證**屬性會指出是否 Facebook 已驗證的帳戶透過其他方式，例如將訊息傳送至行動電話。 在資料庫中儲存此值。

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

您一次必須刪除使用者資料庫中的記錄，或使用不同的 Facebook 帳戶。

執行應用程式，並註冊新的使用者。 看看**ExtraUserInformation**資料表，以查看已驗證屬性的值。

## <a name="conclusion"></a>結論

在本教學課程中，您可以建立網站與 Facebook 整合進行使用者驗證和註冊資料。 您已了解設定 MVC 4 web 應用程式，以及如何自訂該預設行為的預設行為。

## <a name="related-topics"></a>相關主題

- [使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
