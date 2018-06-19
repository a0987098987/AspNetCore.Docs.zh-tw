---
uid: web-api/overview/security/authentication-filters
title: ASP.NET Web API 2 中的驗證篩選條件 |Microsoft 文件
author: MikeWasson
description: 驗證篩選條件是可驗證的 HTTP 要求的元件。 Web API 2 和 MVC 5 都支援的驗證篩選條件，但他們稍有不同...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 16e451f52799625983368bc938091eff47019b52
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153514"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的驗證篩選條件
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 驗證篩選條件是可驗證的 HTTP 要求的元件。 Web API 2 和 MVC 5 都支援的驗證篩選條件，但大部分的篩選器介面的命名慣例而稍有不同。 本主題描述 Web API 驗證篩選條件。


驗證篩選條件可讓您針對個別的控制器或動作中設定驗證配置。 這樣一來，您的應用程式可支援不同的驗證機制的不同的 HTTP 資源。

在本文中，我們將示範從程式碼[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)範例上[http://aspnet.codeplex.com](http://aspnet.codeplex.com)。此範例會示範實作 HTTP 基本存取驗證配置 (RFC 2617) 的驗證篩選條件。 篩選名為類別中實作`IdentityBasicAuthenticationAttribute`。 我不會顯示所有的程式碼範例中，只說明如何撰寫的驗證篩選條件的組件。

## <a name="setting-an-authentication-filter"></a>設定的驗證篩選條件

如同其他篩選器，驗證篩選條件可以是套用每個控制站，每個動作，或是全域所有的 Web API 控制器。

若要套用的驗證篩選條件的控制站，來裝飾控制器類別，以篩選條件屬性。 下列程式碼設定`[IdentityBasicAuthentication]`控制器類別，可讓基本驗證的所有控制器的動作篩選。

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

若要將篩選套用至一個動作，請使用篩選器裝飾的動作。 下列程式碼設定`[IdentityBasicAuthentication]`在控制器上的篩選器`Post`方法。

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

將篩選套用到所有的 Web API 控制器，將它加入**GlobalConfiguration.Filters**。

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>實作 Web API 驗證篩選條件

在 Web API 驗證篩選條件會實作[System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)介面。 它們也應該繼承自**System.Attribute**，以便套用做為屬性。

**IAuthenticationFilter**介面有兩種方法：

- **AuthenticateAsync**藉由驗證認證，在要求中，驗證要求，如果有的話。
- **ChallengeAsync**將驗證挑戰新增至 HTTP 回應，如有需要。

這些方法會對應至中定義的驗證流程[RFC 2612](http://tools.ietf.org/html/rfc2616)和[RFC 2617](http://tools.ietf.org/html/rfc2617):

1. 用戶端傳送授權標頭中的認證。 這通常發生在用戶端從伺服器接收 401 （未經授權） 回應之後。 不過，用戶端可以傳送認證與任何要求中，不只是取得 401 之後。
2. 如果伺服器不接受認證，它會傳回 401 （未經授權） 的回應。 回應包括 Www-authenticate 標頭，其中包含一或多個挑戰。 每個挑戰指定伺服器辨識驗證配置。

伺服器也可以從匿名要求傳回 401。 事實上，也就是通常如何起始驗證程序：

1. 用戶端傳送的匿名要求。
2. 伺服器會傳回 401。
3. 用戶端重新傳送認證的要求。

此流程同時包含*驗證*和*授權*步驟。

- 驗證證明用戶端的身分識別。
- 授權就會決定用戶端是否可以存取特定資源。

在 Web API 驗證篩選條件會處理驗證，但沒有授權。 授權篩選條件或內部控制器動作，應該執行授權。

以下是 Web API 2 管線中的流程：

1. 在之前叫用動作，Web 應用程式開發介面會建立一份驗證篩選條件，該動作。 這包括篩選器動作範圍、 控制站的範圍，與全域範圍。
2. Web 應用程式開發介面呼叫**AuthenticateAsync**清單中的每個篩選器。 每個篩選條件可以驗證要求中的認證。 如果任何篩選器已成功驗證認證，篩選器會建立**IPrincipal**並將其附加至要求。 篩選也可以在此時觸發錯誤。 如果是的話，就不會執行管線的其餘部分。
3. 假設沒有發生錯誤，要求會流過管線的其餘部分。
4. 最後，Web 應用程式開發介面呼叫每個驗證篩選條件**ChallengeAsync**方法。 篩選器會使用這個方法將挑戰新增至回應，如有需要。 通常 （但並非一定），就是發生在 401 錯誤回應。

下圖顯示兩個可能的情況。 首先，驗證篩選條件已成功驗證要求，授權篩選條件授權要求，並控制器動作傳回 200 （確定）。

![](authentication-filters/_static/image1.png)

在第二個範例中，驗證篩選條件來驗證要求，但授權篩選條件會傳回 401 （未經授權）。 在此情況下，控制器動作不會叫用。 驗證篩選條件新增至回應的 Www-authenticate 標頭。

![](authentication-filters/_static/image2.png)

其他組合都有可能&mdash;比方說，如果控制器動作可讓匿名要求，您可能會有的驗證篩選條件，但沒有授權。

## <a name="implementing-the-authenticateasync-method"></a>實作 AuthenticateAsync 方法

**AuthenticateAsync**方法會嘗試驗證要求。 以下是方法簽章：

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync**方法必須執行下列其中一項：

1. 沒有項目 （無作業）。
2. 建立**IPrincipal**並將它設定在要求上。
3. 設定錯誤結果。

選項 (1) 表示要求沒有篩選了解的任何認證。 選項 (2) 表示篩選條件已成功驗證要求。 選項 (3) 表示要求具有無效的認證 （例如錯誤的密碼），進而觸發錯誤回應。

以下是實作的一般大綱**AuthenticateAsync**。

1. 尋找在要求中的認證。
2. 如果沒有認證，不執行任何動作，並傳回 （無作業）。
3. 如果沒有認證，但是篩選無法辨識驗證配置，不執行任何動作，並傳回 （無作業）。 在管線中的另一個篩選可能會了解配置。
4. 如果篩選了解的認證，再試一次來驗證它們。
5. 如果認證不正確，會傳回 401 藉由設定`context.ErrorResult`。
6. 如果認證有效，請建立**IPrincipal**並設定`context.Principal`。

下列程式碼所示**AuthenticateAsync**方法從[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)範例。 註解所指出的每個步驟。 程式碼會示範幾種類型的錯誤： 沒有認證，格式不正確的認證，而且不正確的使用者名稱/密碼的授權標頭。

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>設定錯誤結果

如果認證無效，必須設定篩選`context.ErrorResult`至**IHttpActionResult**會建立錯誤回應。 如需有關**IHttpActionResult**，請參閱[Web API 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。

基本驗證的範例包括`AuthenticationFailureResult`適用於此用途的類別。

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>實作 ChallengeAsync

目的**ChallengeAsync**如有需要方法是將驗證挑戰新增至回應。 以下是方法簽章：

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

每個要求管線中的驗證篩選器上呼叫方法。

請務必了解**ChallengeAsync**稱為*之前*HTTP 回應已建立，且可能甚至控制器動作執行之前。 當**ChallengeAsync**呼叫時，`context.Result`包含**IHttpActionResult**，其稍後用來建立 HTTP 回應。 因此**ChallengeAsync**是呼叫，您還不知道有關 HTTP 回應的任何項目。 **ChallengeAsync**方法應該取代的原始值`context.Result`與新**IHttpActionResult**。 這**IHttpActionResult**必須包裝原始`context.Result`。

![](authentication-filters/_static/image3.png)

我稱原始**IHttpActionResult** *的內部結果*，與新**IHttpActionResult** *外部結果*。 外部的結果必須執行下列作業：

1. 叫用來建立 HTTP 回應的內部結果。
2. 檢查回應。
3. 視需要新增至回應，驗證挑戰。

下列範例取自 「 基本驗證的範例。 它會定義**IHttpActionResult**外部的結果。

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult`屬性會保留內部**IHttpActionResult**。 `Challenge`屬性表示 Www 驗證標頭。 請注意， **ExecuteAsync**會先呼叫`InnerResult.ExecuteAsync`建立 HTTP 回應，如有需要然後將所面臨的挑戰。

檢查然後再加入挑戰的回應碼。 大部分的驗證配置只能新增一項挑戰是否回應 401，如下所示。 不過，某些驗證配置不要新增至成功回應的一項挑戰。 例如，請參閱[交涉](http://tools.ietf.org/html/rfc4559#section-5)(RFC 4559)。

指定`AddChallengeOnUnauthorizedResult`類別中的實際程式碼**ChallengeAsync**很簡單。 您剛才建立的結果，並將其附加至`context.Result`。

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

注意： 基本驗證範例擷取此邏輯位元，將它放在擴充方法。

## <a name="combining-authentication-filters-with-host-level-authentication"></a>結合使用主機層級驗證的驗證篩選條件

「 主機層級驗證 」 是由主應用程式 （例如 IIS) 執行的驗證要求到達 Web API framework 之前。

通常，您可以啟用您的應用程式的其餘部分的主機層級驗證，但停用 Web API 控制器。 例如，典型的案例是要啟用表單驗證，在主機層級，但用於 Web API 的權杖型驗證。

若要停用 Web API 管線內的主機層級驗證，請呼叫`config.SuppressHostPrincipal()`組態中。 這會導致 Web API 來移除**IPrincipal**從任何輸入 Web API 管線的要求。 實際上，它&quot;取消-驗證&quot;要求。

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>其他資源

[ASP.NET Web API 的安全性篩選](https://msdn.microsoft.com/magazine/dn781361.aspx)(MSDN Magazine)
