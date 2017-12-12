---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: "ASP.NET MVC 概觀 |Microsoft 文件"
author: microsoft
description: "深入了解 ASP.NET MVC 應用程式和 ASP.NET Web Form 應用程式之間的差異。 了解如何決定何時建立 ASP.NET MVC 應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC 概觀
====================
由[Microsoft](https://github.com/microsoft)

> 深入了解 ASP.NET MVC 應用程式和 ASP.NET Web Form 應用程式之間的差異。 了解如何決定何時建立 ASP.NET MVC 應用程式。


模型檢視控制器 (MVC) 架構模式會分隔應用程式三個主要元件： 模型、 檢視和控制器。 ASP.NET MVC 架構提供建立 MVC Web 應用程式的 ASP.NET Web Form 模式的替代方案。 ASP.NET MVC 架構是一種輕量型、 可高度測試的呈現架構，（如同搭配 Web Form 應用程式） 整合了現有 ASP.NET 功能，例如主版頁面和成員資格為基礎的驗證。 MVC 架構中定義**System.Web.Mvc**命名空間與屬於基本、 支援的**System.Web**命名空間。   
  
MVC 是許多開發人員熟悉的標準設計模式。 某些類型的 Web 應用程式將受益於 MVC 架構。 其他人將會繼續使用基礎 Web Form 和回傳的傳統 ASP.NET 應用程式模式。 其他類型的 Web 應用程式將會結合兩種方法。這兩種方法並不互斥。   
  
MVC 架構包括下列元件：


[![叫用控制器動作，預期的參數值](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**圖 01**： 叫用控制器動作，預期的參數值 ([按一下以檢視完整大小的影像](asp-net-mvc-overview/_static/image2.png))


- **模型**。 模型物件屬於實作應用程式資料網域的邏輯應用程式的組件。 通常，模型物件會擷取並儲存在資料庫中的模型狀態。 例如，Product 物件可能會從資料庫擷取資訊、 運作，和回 SQL Server 中的 Products 資料表寫入的更新的資訊。

在小型應用程式，模型通常是概念性的分隔，而不是實體。 例如，如果應用程式僅讀取資料集，並將它傳送至檢視，應用程式沒有實體模型層和相關聯的類別。 在此情況下，資料集接替模型物件的角色。

- **檢視**。 檢視會顯示應用程式的使用者介面 (UI) 的元件。 一般而言，此 UI 是從模型資料的方式建立。 範例會顯示文字方塊、 下拉式清單和核取方塊的 「 產品 」 物件的目前狀態為基礎的 Products 資料表的編輯檢視。

- **控制站**。 控制器會處理使用者互動、 處理模型，並且在最後選擇顯示 UI 來呈現檢視的元件。 MVC 應用程式中，檢視只能顯示資訊。控制器會處理和回應使用者輸入和互動。 比方說，控制器會處理查詢字串值，並將這些值傳遞到模型，接著查詢資料庫中使用的值。

MVC 模式可協助您建立個別的不同層面的應用程式 （輸入的邏輯、 商務邏輯和 UI 邏輯），同時提供這些項目之間的鬆散偶合的應用程式。 在模式指定每一種邏輯應用程式中的位置。 UI 邏輯位於檢視。 輸入邏輯位於控制器。 商務邏輯則位於模型。 此隔離有助於您管理複雜度，當您建置應用程式，因為它可讓您將焦點放在上一次實作的其中一個層面。 例如，您可以專注於檢視不含根據商務邏輯。   
  
除了管理複雜性，MVC 模式可讓您更輕鬆地測試應用程式，比測試 Web Form ASP.NET Web 應用程式。 例如，在 Web Form ASP.NET Web 應用程式中，單一類別用來顯示輸出和回應使用者輸入。 撰寫自動化的測試的 Web Form ASP.NET 應用程式可能相當複雜，因為若要測試個別頁面，您必須具現化網頁類別、 所有子控制項，以及其他相依的應用程式中的類別。 執行網頁，會具現化許多類別，因為它可以是要撰寫專門著重在應用程式的各個部分的測試。 Web Form ASP.NET 應用程式的測試可以因此更難實作比在 MVC 應用程式中的測試。 此外，Web Form ASP.NET 應用程式中的測試需要 Web 伺服器。 MVC 架構可分隔元件並且大量使用的介面，可讓測試架構的其餘部分隔離的個別元件。   
  
MVC 應用程式的三個主要元件之間的鬆散連接也有助於提升平行開發。 比方說，一個開發人員可以使用檢視、 第二個開發人員可以在控制器邏輯，而且第三個開發人員可以專注於商務邏輯模型中。

## <a name="deciding-when-to-create-an-mvc-application"></a>決定何時建立 MVC 應用程式

您必須仔細考量是否要使用 ASP.NET MVC 架構或 ASP.NET Web Form 模型實作 Web 應用程式。 MVC 架構不會取代 Web Form 模型;您可以使用任一種架構 Web 應用程式。 （如果您有現有的 Web Form 應用程式，這些繼續往常般一定）。   
  
在決定要用於特定網站的 MVC 架構或 Web Form 模型之前，權衡每一種方法的優點。

### <a name="advantages-of-an-mvc-based-web-application"></a>MVC Web 應用程式的優點

ASP.NET MVC 架構提供下列優點：

- 它可讓您更輕鬆地將應用程式細分成模型、 檢視和控制器來管理複雜性。
- 它不使用檢視狀態或伺服器表單。 這使得 MVC framework 非常適合開發人員想要完全掌控應用程式的行為。
- 它會使用前端控制器模式處理 Web 應用程式要求，透過單一控制站。 這可讓您設計支援豐富的路由基礎結構的應用程式。 如需詳細資訊，請參閱[前端控制器](https://go.microsoft.com/fwlink/?LinkId=106357 "前端控制器")MSDN 網站上。
- 它提供更佳的支援以測試為導向的開發 (TDD)。
- 它適用於 Web 應用程式所支援的大型團隊的開發人員和 Web 設計人員需要較高程度的控制權來控制應用程式行為。

### <a name="advantages-of-a-web-forms-based-web-application"></a>Web Form 為基礎的 Web 應用程式的優點

Web Form 架構提供下列優點：

- 它支援透過 HTTP，有益於業務的 Web 應用程式開發，會保留狀態的事件模型。 在 Web Form 應用程式提供數十個數百種伺服器控制項中支援的事件。
- 它會使用頁面控制器模式將功能加入至個別頁面。 如需詳細資訊，請參閱[頁面控制器](https://go.microsoft.com/fwlink/?LinkId=106359 "頁面控制器")MSDN 網站上。
- 它會使用檢視狀態或伺服器為基礎的表單，可讓管理狀態資訊更容易。
- 它非常適合小型 Web 開發人員和設計人員想要利用大量元件快速開發應用程式可用的小組。
- 一般而言，是較不複雜的應用程式開發，因為元件 (**頁面**類別、 控制項和等等) 緊密整合，而且通常需要比 MVC 模型少的程式碼。

## <a name="features-of-the-aspnet-mvc-framework"></a>ASP.NET MVC Framework 的功能

ASP.NET MVC 架構提供下列功能：

- 隔離應用程式工作 （輸入的邏輯、 商務邏輯和 UI 邏輯）、 可測試性及測試為導向的開發 (TDD) 的預設值。 MVC 架構中的所有核心合約都是介面為基礎，而且可以使用模擬物件是模仿應用程式中的實際物件的行為的模擬物件進行測試。 您可以單元測試應用程式而不需要執行控制器在 ASP.NET 處理序中，如此使單元測試迅速而靈活。 您可以使用與.NET Framework 相容的任何單元測試架構。
- 可擴充且可插入的架構。 ASP.NET MVC framework 的元件的設計可讓他們可以輕鬆地取代或自訂。 您可以插入您自己的檢視引擎、 URL 路由原則、 動作方法參數序列化，以及其他元件。 ASP.NET MVC framework 也支援相依性插入 (DI) 和的控制項反轉 (IOC) 容器模型的使用。 DI 可讓您將物件插入類別，而不是依賴類別以建立物件本身。 IOC 會指定，是否物件需要另一個物件，則第一個物件應該取得第二個物件從外部來源，例如組態檔。 這可讓測試更為容易。
- 功能強大的 URL 對映元件，可讓您建置包含易於且可搜尋的 Url 的應用程式。 Url 沒有包含檔案名稱副檔名，而且設計來支援 URL 命名模式，適用於搜尋引擎最佳化 (SEO) 和代表性狀態傳輸 (REST) 定址的引擎。
- 支援使用現有的 ASP.NET 網頁 （.aspx 檔）、 使用者控制項 （.ascx 檔案） 和主版頁面 （.master 檔） 標記檔中的標記做為檢視範本。 您可以使用現有的 ASP.NET 功能搭配 ASP.NET MVC 架構，例如巢狀主版頁面、 內嵌運算式 (&lt;%= %&gt;)、 宣告式伺服器控制項、 範本、 資料繫結、 當地語系化等等。
- 現有的 ASP.NET 功能的支援。 ASP.NET MVC 可讓您使用功能，例如表單驗證和 Windows 驗證、 URL 授權、 成員資格和角色、 輸出和資料快取、 工作階段和設定檔狀態管理、 健康狀態監控、 組態系統，以及提供者架構。
