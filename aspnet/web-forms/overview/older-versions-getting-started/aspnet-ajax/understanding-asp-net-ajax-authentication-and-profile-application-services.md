---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: 了解 ASP.NET AJAX 驗證和設定檔應用程式服務 |Microsoft Docs
author: scottcate
description: 驗證服務，可讓使用者提供認證以接收驗證 cookie，且閘道服務，讓自訂使用者...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: d722130e625a9f867923280fce0ef35f19bfeb9d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833470"
---
<a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>了解 ASP.NET AJAX 驗證和設定檔應用程式服務
====================
藉由[Scott Cate](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> 驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。 ASP.NET AJAX 驗證服務，使用適用於標準的 ASP.NET 表單驗證，因此目前正在使用表單驗證的應用程式 （例如與登入控制） 將不會中斷藉由升級至 AJAX 驗證服務。


## <a name="introduction"></a>簡介

.NET Framework 3.5 的一部分，Microsoft 正逐步實現的可調整大小的環境升級;不只是新的開發環境，但新的 Language-Integrated Query (LINQ) 功能，以及其他語言增強功能即將推出。 此外，有些熟悉的功能的其他工具組，值得注意的是 ASP.NET AJAX Extensions 中所包含當做最上級成員的.NET Framework 基底類別庫。 這些擴充功能可讓許多豐富的用戶端的新功能，包括部分呈現的頁面，而不需要重新整理整個頁面，透過用戶端指令碼 （包括 ASP.NET 程式碼剖析 API），以及廣泛的用戶端 API 存取 Web 服務的能力設計用來鏡像處理許多 ASP.NET 伺服器端控制項集合中的控制項配置。

本白皮書探討如何實作和使用 ASP.NET 程式碼剖析和表單驗證服務公開的 Microsoft ASP.NET AJAX ExtensionsThe AJAX 擴充功能，讓表單驗證非常輕鬆地支援，因為它 （以及「 分析 」 服務） 會公開透過 Web 服務 proxy 指令碼。 AJAX 擴充功能也支援透過 AuthenticationServiceManager 類別的自訂驗證。

本白皮書根據 Visual Studio 2008 Beta 2 版和.NET Framework 3.5。 本白皮書也會假設您將使用 Visual Studio 2008 Beta 2，不 Visual Web Developer Express，並將提供逐步解說根據 Visual Studio 的使用者介面。 某些程式碼範例可利用無法在 Visual Web Developer Express 的專案範本。

## <a name="profiles-and-authentication"></a>*設定檔和驗證*

Microsoft ASP.NET 設定檔和驗證服務所提供的 ASP.NET 表單驗證系統，ASP.NET 的標準元件。 ASP.NET AJAX Extensions 提供指令碼存取這些服務透過指令碼的 proxy，透過非常簡單的模型 Sys.Services 命名空間下的用戶端 AJAX 程式庫。

驗證服務可讓使用者提供認證以接收驗證 cookie，並為 ASP.NET 所提供的閘道服務，以允許自訂使用者設定檔。 ASP.NET AJAX 驗證服務，使用適用於標準的 ASP.NET 表單驗證，因此目前正在使用表單驗證的應用程式 （例如與登入控制） 將不會中斷藉由升級至 AJAX 驗證服務。

根據驗證服務所提供的成員資格的使用者資料的儲存體與自動整合，可讓設定檔服務。 Web.config 檔案中，指定儲存的資料和各種程式碼剖析的服務提供者處理資料管理。 如同驗證服務，使頁面目前併入 ASP.NET 設定檔服務的功能應該不會中斷包含 AJAX 支援，是相容於標準的 ASP.NET 設定檔服務，AJAX 設定檔服務。

應用程式中併入 ASP.NET 驗證和分析服務本身是在本白皮書的範圍之外。 如需有關本主題的詳細資訊，請參閱 MSDN Library 參考文章使用成員資格管理使用者在[ https://msdn.microsoft.com/library/tw292whz.aspx ](https://msdn.microsoft.com/library/tw292whz.aspx)。 ASP.NET 也會包含一個公用程式來自動設定與 SQL Server，也就是 ASP.NET 成員資格的預設驗證服務提供者的成員資格。 如需詳細資訊，請參閱文章 ASP.NET SQL Server 註冊工具 (Aspnet\_regsql.exe) 在[ https://msdn.microsoft.com/library/ms229862(vs.80).aspx ](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)。

## <a name="using-the-aspnet-ajax-authentication-service"></a>*使用 ASP.NET AJAX 驗證服務*

必須啟用 ASP.NET AJAX 驗證服務的 web.config 檔案中：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

驗證服務需要啟用 ASP.NET 表單驗證，而且需要在用戶端瀏覽器 （因為無 cookie 工作階段需要 URL 參數，指令碼無法啟用 cookie 的工作階段） 上啟用 cookie。

一旦啟用 AJAX 驗證服務，並設定，用戶端指令碼可以立即利用 Sys.Services.AuthenticationService 物件。 主要用戶端指令碼也會想要善用`login`方法和`isLoggedIn`屬性。 若要登入方法，可接受的參數提供預設值的數個屬性存在。

*Sys.Services.AuthenticationService 成員*

*登入方法：*

Login （） 方法開始的要求，以驗證使用者的認證。 這個方法是非同步的而且不會封鎖執行。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| userName | 必要。 要驗證的使用者名稱。 |
| 密碼 | 選擇性 （預設為 null）。 使用者的密碼。 |
| isPersistent | 選擇性 （預設為 false）。 不論使用者的驗證 cookie 應該跨工作階段保存。 如果為 false，使用者將登出時就會關閉瀏覽器或工作階段到期。 |
| redirectUrl | 選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。 如果此參數為 null 或空字串，就會不發生任何重新導向。 |
| customInfo | 選擇性 （預設為 null）。 這個參數是目前未使用，並且會保留供未來使用。 |
| loginCompletedCallback | 選擇性 （預設為 null）。登入已順利完成時要呼叫函式。 如果指定，此參數會覆寫 defaultLoginCompleted 屬性。 |
| failedCallback | 選擇性 （預設為 null）。登入失敗時要呼叫函式。 如果指定，此參數會覆寫 defaultFailedCallback 屬性。 |
| userContext | 選擇性 （預設為 null）。 應該傳遞至回呼函式的自訂使用者內容資料。 |

*傳回值：*

此函式不包含傳回的值。 不過，一些行為是呼叫此函式完成時包含：

- 目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數不是 null 或空字串。
- 不過，如果參數為 null 或空字串，`loginCompletedCallback`參數，或`defaultLoginCompletedCallback`稱為屬性。
- 如果 web 服務呼叫失敗，`failedCallback`參數的`defaultFailedCallback`稱為屬性。

*登出方法：*

Logout() 方法移除認證 cookie，並將目前的使用者從 web 應用程式。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| redirectUrl | 選擇性 （預設為 null）。若要驗證成功後的瀏覽器重新導向 URL。 如果此參數為 null 或空字串，就會不發生任何重新導向。 |
| logoutCompletedCallback | 選擇性 （預設為 null）。當登出已順利完成時要呼叫函式。 如果指定，此參數會覆寫 defaultLogoutCompleted 屬性。 |
| failedCallback | 選擇性 （預設為 null）。登入失敗時要呼叫函式。 如果指定，此參數會覆寫 defaultFailedCallback 屬性。 |
| userContext | 選擇性 （預設為 null）。 應該傳遞至回呼函式的自訂使用者內容資料。 |

*傳回值：*

此函式不包含傳回的值。 不過，一些行為是呼叫此函式完成時包含：

- 目前的頁面將會是 重新整理，或如果變更`redirectUrl`參數不是 null 或空字串。
- 不過，如果參數為 null 或空字串，`logoutCompletedCallback`參數，或`defaultLogoutCompletedCallback`稱為屬性。
- 如果 web 服務呼叫失敗，`failedCallback`參數的`defaultFailedCallback`稱為屬性。

*defaultFailedCallback 屬性 （get、 set）：*

此屬性會指定應該在發生與 web 服務通訊失敗時呼叫的函式。 它應該會收到的委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 個錯誤 | 指定資訊時發生錯誤。 |
| userContext | 指定的登入或登出函式呼叫時所提供的使用者內容資訊。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultLoginCompletedCallback 屬性 （get、 set）：*

此屬性會指定應該在登入 web 服務呼叫完成時呼叫的函式。 它應該會收到的委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| validCredentials | 指定使用者是否提供有效的認證。 `true` 如果使用者成功登入;否則`false`。 |
| userContext | 指定呼叫 login 函式時所提供的使用者內容資訊。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultLogoutCompletedCallback 屬性 （get、 set）：*

此屬性指定應在登出的 web 服務呼叫完成時呼叫的函式。 它應該會收到的委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 結果 | 這個參數會一律是`null`; 它保留供日後使用。 |
| userContext | 指定呼叫 login 函式時所提供的使用者內容資訊。 |
| 方法名稱 | 呼叫方法的名稱。 |

*isLoggedIn 屬性 (get):*

這個屬性會取得目前驗證狀態的使用者資訊。它是由 ScriptManager 物件在設定頁面要求。

這個屬性會傳回`true`如果使用者目前記錄中; 否則它會傳回`false`。

*（get、 set） 的路徑屬性：*

此屬性以程式設計方式決定驗證 web 服務的位置。 它可以用來覆寫預設的驗證提供者，以及一個 ScriptManager 控制項的 AuthenticationService 子節點的路徑屬性中以宣告方式設定 (如需詳細資訊，請參閱使用自訂驗證服務提供者下列主題）。

請注意，預設驗證服務的位置不會變更。 不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面，做為 ASP.NET AJAX 驗證服務 proxy 的位置。

也請注意這個屬性不應設為值，將指令碼要求導向從目前的站台。 目前的應用程式不會收到的驗證認證，因為它會是毫無用處;此外，技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。

這個屬性是`String`物件，表示驗證 web 服務的路徑。

*（get、 set），就會逾時屬性：*

此屬性會判斷失敗的驗證服務前假設登入要求等待的時間長度。 如果在逾時到期等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。

這個屬性是`Number`物件，表示要等候從驗證服務的搜尋結果的毫秒數。

*登入驗證服務的程式碼範例：*

下列標記是簡單的指令碼呼叫的登入和登出方法 AuthenticationService 類別的範例 ASP.NET 網頁。

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>存取程式碼剖析資料透過 AJAX 的 ASP.NET

ASP.NET 程式碼剖析服務也會公開透過 ASP.NET AJAX Extensions 中。 由於 ASP.NET 程式碼剖析服務提供的豐富、 細微的 API，用來儲存和擷取使用者資料，這可以是一個絕佳的生產力工具。

設定檔服務必須能夠在 web.config 中;它不是預設。 若要這樣做，請確定`profileService`子項目已啟用 = true 中所指定的 web.config，和指定的屬性可以讀取或寫入，如下所示：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

也必須設定設定檔服務。 雖然本白皮書的範圍外的程式碼剖析的服務組態，或許值得注意會有可存取子屬性群組名稱為設定檔組態設定中所定義的群組。 例如，使用指定的下列設定檔區段：

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

用戶端指令碼能夠存取名稱、 Address.Line1、 Address.Line2、 Address.City、 Address.State、 Address.Zip 及 BackgroundColor ProfileService 類別的 [屬性] 欄位的屬性。

一旦設定了 AJAX 程式碼剖析服務，它就可以立即在頁面;不過，它必須載入一次使用之前。

*Sys.Services.ProfileService 成員*

*屬性欄位：*

[屬性] 欄位會公開為.運算子名稱慣例可以參考的子屬性的所有設定的設定檔資料。 屬性群組的子系的屬性稱為 GroupName.PropertyName。 在上述範例設定檔的組態，以取得狀態的使用者，您可以使用下列識別碼：

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*load 方法：*

從伺服器載入精選的清單或所有屬性。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 之 propertyNames | 選擇性 （預設為 null）。 要從伺服器載入的屬性。 |
| loadCompletedCallback | 選擇性 （預設為 null）。 已完成載入時呼叫的函式。 |
| failedCallback | 選擇性 （預設為 null）。 發生錯誤時所要呼叫函式。 |
| userContext | 選擇性 （預設為 null）。 要傳遞至回呼函式的內容資訊。 |

Load 函式沒有傳回值。 如果在呼叫已順利完成時，它會呼叫`loadCompletedCallback`參數或`defaultLoadCompletedCallback`屬性。 如果呼叫失敗，或逾時過期，請`failedCallback`參數或`defaultFailedCallback`屬性將會呼叫。

如果`propertyNames`未提供參數，從伺服器擷取所有的讀取設定屬性。

*save 方法：*

Save （） 方法會將使用者的 ASP.NET 設定檔指定的屬性清單 （或所有屬性）。

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 之 propertyNames | 選擇性 （預設為 null）。 要儲存到伺服器的屬性。 |
| saveCompletedCallback | 選擇性 （預設為 null）。 儲存時所呼叫的函式已完成。 |
| failedCallback | 選擇性 （預設為 null）。 發生錯誤時所要呼叫函式。 |
| userContext | 選擇性 （預設為 null）。 要傳遞至回呼函式的內容資訊。 |

儲存函式沒有傳回值。 如果呼叫成功完成，它會呼叫`saveCompletedCallback`參數或`defaultSaveCompletedCallback`屬性。 如果呼叫失敗，或逾時過期，請`failedCallback`或`defaultFailedCallback`屬性將會呼叫。

如果`propertyNames`參數為 null，所有的設定檔內容將會傳送到伺服器，伺服器會決定哪些屬性可以儲存、 哪些不可安裝。

*defaultFailedCallback 屬性 （get、 set）：*

此屬性會指定應該在發生與 web 服務通訊失敗時呼叫的函式。 它應該會收到的委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| 錯誤 | 指定資訊時發生錯誤。 |
| userContext | 指定當提供的使用者內容資訊的載入或儲存函式呼叫。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultSaveCompleted 屬性 （get、 set）：*

此屬性指定應儲存使用者設定檔資料，完成時呼叫的函式。 它應該會收到的委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| numPropsSaved | 指定已儲存的屬性的數目。 |
| userContext | 指定當提供的使用者內容資訊的載入或儲存函式呼叫。 |
| 方法名稱 | 呼叫方法的名稱。 |

*defaultLoadCompleted 屬性 （get、 set）：*

此屬性會指定使用者的設定檔資料載入完成時，應該呼叫的函式。 它應該會收到的委派 （或函式參考）。

這個屬性所指定的函式參考應該具有下列簽章：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*參數：*

| **參數名稱** | **意義** |
| --- | --- |
| numPropsLoaded | 指定載入的屬性數目。 |
| userContext | 指定當提供的使用者內容資訊的載入或儲存函式呼叫。 |
| 方法名稱 | 呼叫方法的名稱。 |

*（get、 set） 的路徑屬性：*

此屬性以程式設計方式決定設定檔 web 服務的位置。 它可以用來覆寫預設設定檔服務提供者，以及一個 ScriptManager 控制項的 ProfileService 子節點的路徑屬性中以宣告方式設定。

請注意，預設設定檔服務的位置不會變更。 不過，ASP.NET AJAX 可讓您指定 web 服務可提供相同的類別介面，做為 ASP.NET AJAX 驗證服務 proxy 的位置。

也請注意這個屬性不應設為值，將指令碼要求導向從目前的站台。 技術基礎 AJAX 應該不會回傳跨網站要求，並在用戶端瀏覽器可能會產生安全性例外狀況。

這個屬性是`String`物件，表示設定檔 web 服務的路徑。

*（get、 set），就會逾時屬性：*

此屬性會判斷失敗的等候設定檔服務，然後再假設載入或儲存要求的時間長度。 如果在逾時到期等待呼叫完成時，會呼叫要求失敗的回呼，並呼叫將不會完成。

這個屬性是`Number`物件，表示要等候從設定檔服務的搜尋結果的毫秒數。

*程式碼範例： 在頁面載入的設定檔資料載入*

下列程式碼會檢查以確認是否已驗證使用者，以及如果是的話，會當做網頁的載入使用者的慣用的背景色彩。

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*使用自訂驗證提供者*

ASP.NET AJAX 擴充功能可讓您建立自訂指令碼驗證服務提供者公開您透過自訂的 web 服務的功能。 若要使用，您的 web 服務必須公開兩種方法，`Login`和`Logout`; 這些方法必須指定為預設的 ASP.NET AJAX 驗證 web 服務相同的方法簽章。

當您建立自訂的 web 服務之後時，您必須指定它，不論是以宣告方式在您的頁面的路徑，以程式設計方式在程式碼，或透過用戶端指令碼。

*若要以宣告方式設定路徑：*

若要以宣告方式設定路徑，包括 AuthenticationService 物件的子系 ScriptManager 在 ASP.NET 頁面上：

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*若要在程式碼中設定的路徑：*

若要以程式設計方式設定路徑，指定透過指令碼管理員的執行個體的路徑：

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*若要設定指令碼中的路徑：*

若要以程式設計方式設定路徑的指令碼中，利用`path`AuthenticationService 類別的屬性：

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*自訂驗證的範例 Web 服務*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>總結

ASP.NET 服務-特別是程式碼剖析、 成員資格，以及驗證服務-輕鬆地在用戶端瀏覽器上公開 JavaScript 中。 這可讓開發人員不必依靠等執行困難的 Updatepanel 控制項順暢地整合其用戶端程式碼使用的驗證機制。 設定檔資料可以保護用戶端，以防止藉由使用 web 組態設定;沒有資料可供使用，根據預設，和開發人員必須 opt-in 以設定檔屬性。

此外，藉由使用對等的方法簽章建立簡化的 web 服務實作，開發人員可以建立這些內建的 ASP.NET 服務的自訂指令碼提供者。 如需這些技術的支援可簡化開發豐富型用戶端應用程式，同時開發人員提供各種不同的彈性，來滿足特定需求。

## <a name="bio"></a>*簡歷*

Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。 可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一頁](understanding-asp-net-ajax-updatepanel-triggers.md)
> [下一頁](understanding-asp-net-ajax-localization.md)
