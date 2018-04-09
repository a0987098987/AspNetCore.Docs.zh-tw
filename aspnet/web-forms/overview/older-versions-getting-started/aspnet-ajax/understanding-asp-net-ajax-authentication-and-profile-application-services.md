---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: 了解 ASP.NET AJAX 驗證和設定檔的應用程式服務 |Microsoft 文件
author: scottcate
description: 驗證服務可讓使用者提供認證以接收驗證 cookie，且閘道服務，讓自訂使用者...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: 0bf6538d0c4ae9488e6ac29ccba6d4b243cf070e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>了解 ASP.NET AJAX 驗證和設定檔的應用程式服務
====================
由[Scott 類別](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> 驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。 使用 ASP.NET AJAX 驗證服務是使用標準 ASP.NET 表單驗證時，相容的因此目前使用表單驗證的應用程式 （例如與登入控制） 將不會中斷方法升級至 AJAX 驗證服務。


## <a name="introduction"></a>簡介

.NET Framework 3.5 的一部分，Microsoft 會將可調整大小的環境升級;不只是新的開發環境，但新的 Language-Integrated Query (LINQ) 功能和其他語言增強功能即將推出。 此外，某些熟悉的其他工具組，值得注意的是 ASP.NET AJAX Extensions 中所包含功能做為第一級的.NET Framework 基底類別庫的成員。 這些延伸可讓許多豐富型用戶端的新功能，包括局部網頁呈現，而不需要完整頁面重新整理，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API） 和廣泛的 API，用戶端存取 Web 服務的能力設計來鏡射許多 ASP.NET 伺服器端控制項集合中所見的控制項配置。

本白皮書會查看的實作和使用 ASP.NET 程式碼剖析和表單驗證服務由 Microsoft ASP.NET AJAX ExtensionsThe AJAX 擴充功能公開讓表單驗證非常容易支援，因為它 （以及程式碼剖析服務中） 會公開透過 Web 服務 proxy 指令碼。 AJAX 擴充功能也支援透過 AuthenticationServiceManager 類別的自訂驗證。

本白皮書根據在 Beta 2 版本的 Visual Studio 2008 和.NET Framework 3.5。 本白皮書也會假設您將使用 Visual Studio 2008 Beta 2、 不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。 某些程式碼範例可利用專案範本在 Visual Web Developer Express 中無法使用。

## <a name="profiles-and-authentication"></a>*設定檔和驗證*

Microsoft ASP.NET 設定檔和驗證服務所提供的 ASP.NET 表單驗證系統，標準的 ASP.NET 元件。 ASP.NET AJAX 擴充功能提供指令碼存取這些服務，透過指令碼 proxy，透過用戶端 AJAX 程式庫在 Sys.Services 命名空間之下相當簡單的模型。

驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。 使用 ASP.NET AJAX 驗證服務是使用標準 ASP.NET 表單驗證時，相容的因此目前使用表單驗證的應用程式 （例如與登入控制） 將不會中斷方法升級至 AJAX 驗證服務。

根據驗證服務所提供的成員資格的使用者資料的儲存體與自動整合，可讓設定檔服務。 Web.config 檔案中，指定儲存的資料和各種不同的程式碼剖析服務提供者處理資料管理。 如同驗證服務，使目前納入 ASP.NET 設定檔服務的功能頁面應該不會因包括 AJAX 支援，是與標準 ASP.NET 設定檔服務相容 AJAX 設定檔服務。

應用程式中併入 ASP.NET 驗證和分析服務本身已超出本白皮書的範圍。 如需有關主題的詳細資訊，請參閱 MSDN Library 參考發行項使用成員資格管理使用者在[ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx)。 ASP.NET 也會包含一個公用程式來自動設定 SQL Server，也就是 ASP.NET 成員資格的預設驗證服務提供者的成員資格。 如需詳細資訊，請參閱文章 ASP.NET SQL Server 註冊工具 (Aspnet\_regsql.exe) 在[ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)。

## <a name="using-the-aspnet-ajax-authentication-service"></a>*使用 ASP.NET AJAX 驗證服務*

必須啟用 ASP.NET AJAX 驗證服務的 web.config 檔案中：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

驗證服務需要 ASP.NET 表單驗證以啟用，而且需要在用戶端瀏覽器 （指令碼無法啟用無 cookie 工作階段因為無 cookie 工作階段需要 URL 參數） 上啟用 cookie。

一旦啟用並設定 AJAX 驗證服務，用戶端指令碼可以立即利用 Sys.Services.AuthenticationService 物件。 主要是用戶端指令碼也會想要充分利用`login`方法和`isLoggedIn`屬性。 登入方法可接受大量的參數提供預設值，存在數個屬性。

*Sys.Services.AuthenticationService 成員*

*登入方法：*

Login() 方法開始的要求來驗證使用者的認證。 這個方法是非同步，而且不會封鎖執行。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| userName | 必要。 要驗證的使用者名稱。 |
| 密碼 | 選擇性 （預設為 null）。 使用者的密碼。 |
| isPersistent | 選擇性 （預設為 false）。 是否應該保存使用者的驗證 cookie，跨工作階段。 如果為 false，使用者將登出時關閉瀏覽器或工作階段到期。 |
| redirectUrl | 選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。 如果這個參數是 null 或空字串，就會不發生任何重新導向。 |
| customInfo | 選擇性 （預設為 null）。 這個參數是目前未使用，並且會保留供未來使用。 |
| loginCompletedCallback | 選擇性 （預設為 null）。登入已順利完成時要呼叫函式。 如果指定，此參數會覆寫 defaultLoginCompleted 屬性。 |
| failedCallback | 選擇性 （預設為 null）。登入失敗時要呼叫函式。 如果指定，此參數會覆寫 defaultFailedCallback 屬性。 |
| userContext | 選擇性 （預設為 null）。 自訂的使用者內容應該傳遞至回呼函式的資料。 |

*傳回值：*

此函式不包含傳回的值。 不過，一些行為會包含在呼叫這個函式完成：

- 目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數為 null 或空字串都不。
- 不過，如果參數是 null 或空字串，`loginCompletedCallback`參數，或`defaultLoginCompletedCallback`呼叫屬性。
- 如果 web 服務呼叫失敗，`failedCallback`參數`defaultFailedCallback`呼叫屬性。

*登出方法：*

Logout() 方法移除認證 cookie，並從 web 應用程式目前的使用者登出。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| redirectUrl | 選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。 如果這個參數是 null 或空字串，就會不發生任何重新導向。 |
| logoutCompletedCallback | 選擇性 （預設為 null）。登出已順利完成時要呼叫函式。 如果指定，此參數會覆寫 defaultLogoutCompleted 屬性。 |
| failedCallback | 選擇性 （預設為 null）。登入失敗時要呼叫函式。 如果指定，此參數會覆寫 defaultFailedCallback 屬性。 |
| userContext | 選擇性 （預設為 null）。 自訂的使用者內容應該傳遞至回呼函式的資料。 |

*傳回值：*

此函式不包含傳回的值。 不過，一些行為會包含在呼叫這個函式完成：

- 目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數為 null 或空字串都不。
- 不過，如果參數是 null 或空字串，`logoutCompletedCallback`參數，或`defaultLogoutCompletedCallback`呼叫屬性。
- 如果 web 服務呼叫失敗，`failedCallback`參數`defaultFailedCallback`呼叫屬性。

*defaultFailedCallback 屬性 （get、 set）：*

此屬性會指定應該在與 web 服務通訊失敗發生時呼叫的函式。 它應該會收到委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 個錯誤 | 指定資訊時發生錯誤。 |
| userContext | 指定的登入或登出函式呼叫時所提供的使用者內容資訊。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultLoginCompletedCallback 屬性 （get、 set）：*

此屬性會指定應該在完成登入 web 服務呼叫時呼叫的函式。 它應該會收到委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| validCredentials | 指定使用者提供有效的認證。 `true` 如果使用者成功登入。否則`false`。 |
| userContext | 指定登入函式呼叫時所提供的使用者內容資訊。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultLogoutCompletedCallback 屬性 （get、 set）：*

此屬性會指定應在登出 web 服務呼叫完成時呼叫的函式。 它應該會收到委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 結果 | 這個參數一定會`null`; 它保留供未來使用。 |
| userContext | 指定登入函式呼叫時所提供的使用者內容資訊。 |
| 方法名稱 | 呼叫方法的名稱。 |

*isLoggedIn 屬性 (get):*

這個屬性會取得目前驗證狀態的使用者資訊。ScriptManager 物件設定在網頁要求期間。

這個屬性會傳回`true`如果使用者目前登入，否則它會傳回`false`。

*path 屬性 （get、 set）：*

這個屬性會以程式設計方式判斷驗證 web 服務的位置。 它可以用來覆寫預設的驗證提供者，以及一個 ScriptManager 控制項 AuthenticationService 子節點的路徑屬性中以宣告方式設定 (如需詳細資訊，請參閱 < 使用自訂驗證服務提供者下列主題）。

請注意，預設驗證服務的位置不會變更。 不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面做為 ASP.NET AJAX 驗證服務 proxy 的位置。

請注意這個屬性不應設為值，將指令碼要求導向移出目前的站台。 目前應用程式不會收到的驗證認證，因為它會是無用的訊息;此外，技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。

這個屬性是`String`物件，表示驗證 web 服務的路徑。

*（get、 set），就會逾時屬性：*

此屬性會判斷失敗的驗證服務前假設登入要求等待的時間長度。 如果超過逾時等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。

這個屬性是`Number`物件，代表要等候的驗證服務來搜尋結果的毫秒數。

*登入驗證服務的程式碼範例：*

下列標記是與登入和登出 AuthenticationService 類別方法的簡單的指令碼呼叫 ASP.NET 頁面範例。

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>存取程式碼剖析資料透過 AJAX 的 ASP.NET

ASP.NET 程式碼剖析服務也會公開透過 ASP.NET AJAX 擴充功能。 由於 ASP.NET 程式碼剖析服務提供豐富且更細微的 API，用來儲存和擷取使用者資料，這可以是絕佳的產能工具。

必須啟用此設定檔服務，在 web.config 中;不是預設值。 若要這樣做，請確認`profileService`子項目已啟用 = true 中所指定的 web.config 和指定的屬性可以讀取或寫入，如下所示：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

也必須設定設定檔服務。 雖然本白皮書的範圍內的程式碼剖析服務的組態，值得注意設定檔組態設定中所定義的群組將可存取子屬性的群組名稱。 例如，用於指定下列設定檔區段：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

用戶端指令碼就可以存取名稱、 Address.Line1、 Address.Line2、 Address.City、 Address.State、 Address.Zip 和 BackgroundColor 為 ProfileService 類別的 [屬性] 欄位的屬性。

一旦設定 AJAX 程式碼剖析服務，它就可以立即在頁面;不過，它必須載入一次才能使用。

*Sys.Services.ProfileService 成員*

*屬性欄位：*

[屬性] 欄位會公開為.運算子名稱慣例，可參考的子屬性的所有設定的設定檔資料。 屬性群組的子系的屬性會參照 GroupName.PropertyName。 在上述範例設定檔設定中，以取得狀態的使用者，您可以使用下列識別碼：

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*負載平衡方法：*

從伺服器載入選取的清單或所有屬性。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| propertyNames | 選擇性 （預設為 null）。 要從伺服器載入的屬性。 |
| loadCompletedCallback | 選擇性 （預設為 null）。 已完成載入時要呼叫的函式。 |
| failedCallback | 選擇性 （預設為 null）。 發生錯誤時所要呼叫函式。 |
| userContext | 選擇性 （預設為 null）。 要傳遞給回呼函式的內容資訊。 |

Load 函式沒有傳回值。 如果呼叫已順利完成，它會呼叫`loadCompletedCallback`參數或`defaultLoadCompletedCallback`屬性。 如果呼叫失敗，或逾時，可能是`failedCallback`參數或`defaultFailedCallback`屬性將會呼叫。

如果`propertyNames`沒有提供參數，從伺服器擷取所有的讀取設定屬性。

*save 方法：*

Save （） 方法會將使用者的 ASP.NET 設定檔指定的屬性清單 （或所有屬性）。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| propertyNames | 選擇性 （預設為 null）。 要儲存到伺服器的屬性。 |
| saveCompletedCallback | 選擇性 （預設為 null）。 儲存時要呼叫的函式已完成。 |
| failedCallback | 選擇性 （預設為 null）。 發生錯誤時所要呼叫函式。 |
| userContext | 選擇性 （預設為 null）。 要傳遞給回呼函式的內容資訊。 |

儲存函式沒有傳回值。 如果呼叫成功完成，它會呼叫`saveCompletedCallback`參數或`defaultSaveCompletedCallback`屬性。 如果呼叫失敗，或逾時，可能是`failedCallback`或`defaultFailedCallback`屬性將會呼叫。

如果`propertyNames`參數為 null，所有設定檔屬性將會傳送到伺服器，伺服器會決定可以儲存的內容，以及哪些不可。

*defaultFailedCallback 屬性 （get、 set）：*

此屬性會指定應該在與 web 服務通訊失敗發生時呼叫的函式。 它應該會收到委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 錯誤 | 指定資訊時發生錯誤。 |
| userContext | 指定當提供的使用者內容資訊的載入或儲存函式呼叫。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultSaveCompleted 屬性 （get、 set）：*

此屬性會指定應儲存使用者設定檔資料完成時呼叫的函式。 它應該會收到委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| numPropsSaved | 指定已儲存的屬性的數目。 |
| userContext | 指定當提供的使用者內容資訊的載入或儲存函式呼叫。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultLoadCompleted 屬性 （get、 set）：*

此屬性會指定應完成載入使用者設定檔資料時呼叫的函式。 它應該會收到委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| numPropsLoaded | 指定載入的屬性數目。 |
| userContext | 指定當提供的使用者內容資訊的載入或儲存函式呼叫。 |
| 方法名稱 | 呼叫方法的名稱。 |

*path 屬性 （get、 set）：*

此屬性以程式設計方式決定的設定檔的 web 服務的位置。 它可以用來覆寫預設設定檔服務提供者，以及一個 ScriptManager 控制項的 ProfileService 子節點的路徑屬性中以宣告方式設定。

請注意，不會變更預設設定檔服務的位置。 不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面做為 ASP.NET AJAX 驗證服務 proxy 的位置。

請注意這個屬性不應設為值，將指令碼要求導向移出目前的站台。 技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。

這個屬性是`String`物件代表 web 服務設定檔的路徑。

*（get、 set），就會逾時屬性：*

此屬性會判斷失敗的設定檔服務等候假設載入或儲存要求的時間長度。 如果超過逾時等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。

這個屬性是`Number`物件，代表要等候結果，從設定檔服務的毫秒數。

*程式碼範例： 載入的頁面載入的設定檔資料*

下列程式碼會檢查是否已驗證使用者，並且若是如此，載入使用者的慣用的背景色彩的頁面。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*使用自訂驗證提供者*

ASP.NET AJAX 擴充功能可讓您建立自訂指令碼驗證服務提供者公開您透過自訂的 web 服務的功能。 才能加以使用，您的 web 服務必須公開兩個方法：`Login`和`Logout`; 這些方法必須指定為預設的 ASP.NET AJAX 驗證 web 服務相同的方法簽章。

當您建立自訂的 web 服務之後時，您必須指定它，不論是以宣告方式在頁面上，路徑的程式碼，以程式設計方式或透過用戶端指令碼。

*若要以宣告方式設定路徑：*

若要以宣告方式設定的路徑，包含 AuthenticationService 物件的子節點 ScriptManager ASP.NET 網頁上：

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*若要設定程式碼中的路徑：*

若要以程式設計方式設定的路徑，指定透過指令碼管理員的執行個體的路徑：

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*若要設定的指令碼中的路徑：*

若要以程式設計方式設定的路徑，指令碼中，利用`path`AuthenticationService 類別的屬性：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*自訂驗證的範例 Web 服務*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>總結

在用戶端瀏覽器，ASP.NET 服務-特別的程式碼剖析、 成員資格，以及驗證服務-輕鬆地會公開給 JavaScript。 這可讓開發人員不必依靠控制項，例如執行困難的 updatepanel 就緊密，整合其用戶端程式碼使用的驗證機制。 藉由使用 web 組態設定; 用戶端，也可以保護設定檔資料沒有資料可根據預設，同時開發人員必須在選擇設定檔屬性。

此外，藉由使用等的方法簽章建立簡化的 web 服務實作，開發人員可以建立這些內建 ASP.NET 服務的自訂指令碼提供者。 如需這些技術的支援可簡化豐富型用戶端應用程式，同時開發人員提供各種不同的彈性，以符合特定需求的開發。

## <a name="bio"></a>*Bio*

Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。 透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一頁](understanding-asp-net-ajax-updatepanel-triggers.md)
> [下一頁](understanding-asp-net-ajax-localization.md)
