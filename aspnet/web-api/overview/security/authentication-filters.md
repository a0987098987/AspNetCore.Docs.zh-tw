---
uid: web-api/overview/security/authentication-filters
title: ASP.NET Web API 2 中的驗證篩選條件 |Microsoft Docs
author: MikeWasson
description: 驗證篩選條件是一種元件，會驗證 HTTP 要求。 Web API 2 和 MVC 5 都支援的驗證篩選條件，但他們稍有不同...
ms.author: aspnetcontent
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 6cad52e0454d685c6e96746524fbbad21e1c274d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839813"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的驗證篩選條件
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> 驗證篩選條件是一種元件，會驗證 HTTP 要求。 Web API 2 和 MVC 5 都支援的驗證篩選條件，但他們稍有不同，大部分是在篩選條件介面的命名慣例。 本主題描述 Web API 驗證篩選條件。


驗證篩選條件可讓您針對個別的控制器或動作中設定驗證配置。 這樣一來，您的應用程式可以對不同的 HTTP 資源支援不同的驗證機制。

在本文中，我將示範從程式碼[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)上的範例[ http://aspnet.codeplex.com ](http://aspnet.codeplex.com)。 此範例會示範實作 HTTP 基本存取驗證配置 (RFC 2617) 的驗證篩選條件。 名為類別中實作篩選條件`IdentityBasicAuthenticationAttribute`。 我不會顯示所有的程式碼範例中，從只說明如何撰寫的驗證篩選條件的組件。

## <a name="setting-an-authentication-filter"></a>設定的驗證篩選條件

其他篩選器，例如驗證篩選條件可以套用每個控制站、 每個動作或全域所有的 Web API 控制器。

若要套用的驗證篩選條件，控制站，來裝飾控制器類別，以篩選條件屬性。 下列程式碼設定`[IdentityBasicAuthentication]`篩選控制器類別，可讓所有控制器動作的基本驗證。

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

若要將篩選套用至一個動作，請使用篩選器裝飾的動作。 下列程式碼設定`[IdentityBasicAuthentication]`控制器的篩選`Post`方法。

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

若要將篩選套用到所有的 Web API 控制器，將它加入**GlobalConfiguration.Filters**。

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>實作 Web API 驗證篩選條件

在 Web API 驗證篩選條件會實作[System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx)介面。 它們也應該繼承自**System.Attribute**，才能套用為屬性。

**IAuthenticationFilter**介面有兩種方法：

- **AuthenticateAsync**藉由驗證要求中的認證來驗證要求，如果有的話。
- **ChallengeAsync**將驗證挑戰新增至 HTTP 回應，如有需要。

這些方法會對應至驗證流程中定義[RFC 2612](http://tools.ietf.org/html/rfc2616)並[RFC 2617](http://tools.ietf.org/html/rfc2617):

1. 用戶端會將認證傳送授權標頭中。 此外，這通常會發生之後用戶端從伺服器收到 401 （未經授權） 回應。 不過，用戶端可以取得 401 之後，不只是傳送任何要求中，使用的認證。
2. 如果伺服器未接受認證，則會傳回 401 （未經授權） 回應。 此回應包含 Www-authenticate 標頭，其中包含一或多個挑戰。 每一項挑戰會指定伺服器辨識驗證配置。

伺服器也可以從匿名要求傳回 401。 事實上，這通常是起始驗證程序的方式：

1. 用戶端傳送匿名要求。
2. 伺服器會傳回 401。
3. 用戶端重新傳送認證的要求。

此流程同時包含*驗證*並*授權*步驟。

- 驗證可證明用戶端的身分識別。
- 授權會決定用戶端是否可以存取特定資源。

在 Web API 驗證篩選條件會處理驗證，但沒有授權。 授權篩選條件或控制器動作內，應該授權。

以下是 Web API 2 管線中的流程：

1. 之前叫用動作，Web API 會建立一份驗證篩選條件，該動作。 這包括篩選器動作範圍、 控制器範圍與全域範圍。
2. Web API 呼叫**AuthenticateAsync**清單中的每個篩選器。 每個篩選條件可以驗證要求中的認證。 如果任何篩選器已成功驗證認證，就會建立篩選條件**IPrincipal**並將它附加至要求。 篩選也可以在此時觸發錯誤。 如果是的話，就不會執行管線的其餘部分。
3. 假設沒有任何錯誤，要求會流經管線的其餘部分。
4. 最後，Web API 會呼叫每個驗證篩選條件**ChallengeAsync**方法。 如有需要篩選會使用這個方法所做出的回應中加入一項挑戰。 一般而言 （但並非絕對），會發生 401 錯誤回應。

下圖顯示兩個可能的情況。 第一次，驗證篩選已成功驗證要求，授權篩選授權要求，並控制器動作傳回 200 （確定）。

![](authentication-filters/_static/image1.png)

在第二個範例中，驗證篩選條件來驗證要求，但授權篩選條件會傳回 401 （未經授權）。 在此情況下，不會叫用控制器動作。 驗證篩選條件會將 Www-authenticate 標頭加入回應。

![](authentication-filters/_static/image2.png)

其他組合都有可能&mdash;比方說，如果控制器動作允許匿名要求，您可能會有的驗證篩選條件但沒有授權。

## <a name="implementing-the-authenticateasync-method"></a>實作 AuthenticateAsync 方法

**AuthenticateAsync**方法會嘗試驗證要求。 以下是方法簽章：

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

**AuthenticateAsync**方法必須執行下列其中一項：

1. 沒有項目 （無作業）。
2. 建立**IPrincipal**並將它設定在要求上。
3. 設定錯誤結果。

選項 (1) 表示要求沒有任何篩選了解的認證。 選項 (2) 表示篩選條件成功驗證要求。 選項 (3) 表示要求有無效的認證 （例如錯誤的密碼），這會觸發錯誤回應。

以下是實作的一般概述**AuthenticateAsync**。

1. 尋找在要求中的認證。
2. 如果沒有認證，不執行任何動作，並傳回 （無作業）。
3. 如果沒有認證，但篩選條件無法辨識的驗證配置，不執行任何動作，並傳回 （無作業）。 在管線中的另一個篩選條件可能會了解配置。
4. 如果篩選了解的認證，請嘗試以進行驗證。
5. 如果認證不正確，請藉由設定傳回 401 `context.ErrorResult`。
6. 如果是有效的認證，建立**IPrincipal**並設定`context.Principal`。

下列程式碼所示**AuthenticateAsync**方法，從[基本驗證](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt)範例。 註解每個步驟。 程式碼會示範數種類型的錯誤： 沒有認證，格式不正確的認證，而且使用者名稱/密碼不正確的授權標頭。

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>設定錯誤結果

如果認證無效，必須設定篩選`context.ErrorResult`要**IHttpActionResult**這樣將會產生錯誤回應。 如需詳細資訊**IHttpActionResult**，請參閱[Web API 2 中的動作結果](../getting-started-with-aspnet-web-api/action-results.md)。

基本驗證的範例包括`AuthenticationFailureResult`適用於此用途的類別。

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>實作 ChallengeAsync

目的**ChallengeAsync**如有需要方法是將驗證挑戰新增至回應。 以下是方法簽章：

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

每個要求管線中的驗證篩選器上呼叫方法。

請務必了解**ChallengeAsync**稱為*之前*HTTP 回應便會建立，並甚至還可以在控制器動作執行之前。 當**ChallengeAsync**呼叫時，`context.Result`包含**IHttpActionResult**，稍後用來建立 HTTP 回應。 因此當**ChallengeAsync**是呼叫，您還不知道有關 HTTP 回應的任何項目。 **ChallengeAsync**方法應該取代的原始值`context.Result`的新**IHttpActionResult**。 這**IHttpActionResult**必須包裝原始`context.Result`。

![](authentication-filters/_static/image3.png)

我稱原始**IHttpActionResult** *內部結果*，和新**IHttpActionResult** *外部結果*。 外部結果必須執行下列作業：

1. 叫用內部的結果，以建立 HTTP 回應。
2. 檢查回應。
3. 如有需要則您可以加入回應驗證挑戰。

下列範例是取自基本驗證的範例。 它會定義**IHttpActionResult**外部的結果。

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

`InnerResult`屬性會保留內部**IHttpActionResult**。 `Challenge`屬性表示 Www 驗證標頭。 請注意， **ExecuteAsync**會先呼叫`InnerResult.ExecuteAsync`建立 HTTP 回應，如有需要然後加入所面臨的挑戰。

檢查回應程式碼之前加入所面臨的挑戰。 大部分的驗證配置只能新增一項挑戰是否回應 401，如下所示。 不過，某些驗證配置進行加入一項挑戰的成功回應。 例如，請參閱[交涉](http://tools.ietf.org/html/rfc4559#section-5)(RFC 4559)。

給定`AddChallengeOnUnauthorizedResult`類別中的實際程式碼**ChallengeAsync**很簡單。 您剛建立的結果，並將其附加至`context.Result`。

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

注意： 基本驗證範例抽象化此邏輯位元，將其放在擴充方法。

## <a name="combining-authentication-filters-with-host-level-authentication"></a>結合使用主機層級驗證的驗證篩選條件

[主控件層級驗證] 是由主應用程式 （例如 IIS) 中，執行驗證之前要求到達 Web API 架構。

通常，您可能要啟用您的應用程式的其餘部分的主機層級驗證，但停用您的 Web API 控制器。 例如，典型的案例是啟用表單驗證，在主機層級，但使用 Web api 的權杖型驗證。

若要停用 Web API 管線內的主機層級驗證，請呼叫`config.SuppressHostPrincipal()`組態中。 這會導致 Web API，可移除**IPrincipal**從輸入 Web API 管線的任何要求。 實際上，它&quot;取消-驗證&quot;要求。

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>其他資源

[ASP.NET Web API 安全性篩選器](https://msdn.microsoft.com/magazine/dn781361.aspx)(MSDN Magazine)
