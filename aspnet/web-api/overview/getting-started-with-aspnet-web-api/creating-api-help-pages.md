---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: 建立的 ASP.NET Web API 說明頁面 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8803c406660398ad3314306a1bcdf99af418082d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393124"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>建立的 ASP.NET Web API 說明頁面
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

當您建立的 web API 時，它通常很有用來建立說明網頁，以便在其他開發人員知道如何呼叫您的 API。 您可以手動建立所有文件，但最好是盡量的自動產生。

若要簡化這項工作中，ASP.NET Web API 會在執行階段，程式庫提供自動產生說明頁面。

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>建立 API 說明頁面

安裝[ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。 此更新會整合到 Web API 專案範本的 [說明] 頁面。

接下來，建立新的 ASP.NET MVC 4 專案，然後選取 Web API 專案範本。 專案範本會建立名為範例 API 控制器`ValuesController`。 此範本也會建立 API 說明頁面。 所有 [說明] 頁面的程式碼檔案位於專案的 [區域] 資料夾。

![](creating-api-help-pages/_static/image2.png)

當您執行應用程式時，[首頁] 頁面會包含 API 說明頁面的連結。 從 [首頁] 頁面中，相對路徑會是 /Help。

![](creating-api-help-pages/_static/image3.png)

此連結將帶您前往 API 摘要頁面。

![](creating-api-help-pages/_static/image4.png)

此頁面的 [MVC] 檢視會定義在 Areas/HelpPage/Views/Help/Index.cshtml。 您可以編輯此頁面修改版面配置、 簡介、 標題、 樣式和其他等等。

頁面的主要部分是依控制站的 api 的資料表。 資料表項目使用，以動態方式產生**IApiExplorer**介面。 （我會詳細討論有關此介面更新版本。）如果您新增新的 API 控制器，請在執行階段時，會自動更新的資料表。

「 API 」 資料行列出的 HTTP 方法和相對 URI。 「 說明 」 資料行包含每個 API 的文件。 一開始，文件就是只是預留位置文字。 下一節，我將示範如何從 XML 註解加入文件。

每個 API 有更詳細的資訊，包括範例要求和回應內文的頁面的連結。

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>加入現有的專案中的 [說明] 頁面

您可以透過 NuGet 套件管理員，將 [說明] 頁面新增至現有的 Web API 專案中。 這個選項適合您開始從不同的專案範本，於 [Web API] 範本。

從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在  [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)視窗中，輸入下列命令之一：

針對**C#** 應用程式： `Install-Package Microsoft.AspNet.WebApi.HelpPage`

針對**Visual Basic**應用程式： `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

有兩個套件，一個適用於 C#，一個適用於 Visual Basic。 請務必使用符合您的專案。

此命令會安裝必要的組件，並新增 （位於 [區域/HelpPage] 資料夾） 的 [說明] 頁的 MVC 檢視。 您必須手動將連結新增至 [說明] 頁面。 URI 是 /Help。 若要建立連結，在 razor 檢視中，新增下列內容：

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

此外，請確定註冊區域。 在 Global.asax 檔案中，新增下列程式碼**應用程式\_啟動**方法，如果找不到已：

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>新增 API 文件

根據預設，說明 頁面都有文件的預留位置字串。 您可以使用[XML 文件註解](https://msdn.microsoft.com/library/b2s063f7.aspx)建立文件。 若要啟用此功能，開啟 區域/HelpPage/應用程式的檔案\_Start/HelpPageConfig.cs 並取消註解下面這一行：

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

現在可讓 XML 文件。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。 選取 **建置**頁面。

![](creating-api-help-pages/_static/image6.png)

底下**輸出**，檢查**XML 文件檔案**。 在 [編輯] 方塊中，輸入 「 應用程式\_Data/XmlDocument.xml"。

![](creating-api-help-pages/_static/image7.png)

接下來，開啟的程式碼`ValuesController`API 控制器，其定義於 /Controllers/ValuesControler.cs。 控制器方法中加入一些文件註解。 例如: 

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> 提示： 如果您的方法上方列上放置插入號，並輸入三個正斜線，Visual Studio 會自動插入的 XML 項目。 然後您可以填上空白。


現在建置並再次執行應用程式，並瀏覽至 [說明] 頁面。 文件字串，應該會出現在 API 資料表。

![](creating-api-help-pages/_static/image8.png)

[說明] 頁面會在執行階段，從 XML 檔案讀取的字串。 （當您部署應用程式時，請確定部署的 XML 檔案。）

## <a name="under-the-hood"></a>背後原理

說明網頁為基礎建置的**ApiExplorer**類別，這是 Web API 架構的一部分。 **ApiExplorer**類別提供原始資料來建立說明頁面。 每個 API，如**ApiExplorer**包含**ApiDescription**描述 API。 基於此目的，「 API 」 定義為 HTTP 方法和相對 URI 的組合。 例如，以下是一些不同的 Api:

- 取得 /api/Products
- 取得 /api/產品 / {id}
- 張貼/api/產品

如果控制器動作支援多個 HTTP 方法， **ApiExplorer**視為不同的 API 中的每個方法。

若要隱藏的 API **ApiExplorer**，新增**ApiExplorerSettings**屬性設為動作和 set *IgnoreApi*設為 true。

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

您也可以將這個屬性加入到控制站，以排除整個控制器中。

ApiExplorer 類別取得文件字串，從**IDocumentationProvider**介面。 如您稍早所見，說明頁面的程式庫會提供**IDocumentationProvider** ，取得文件從 XML 文件字串。 程式碼位於 /Areas/HelpPage/XmlDocumentationProvider.cs。 您可以取得文件從其他來源藉由撰寫您自己**IDocumentationProvider**。 若要將其註冊，呼叫**SetDocumentationProvider**擴充方法，定義於**HelpPageConfigurationExtensions**

**ApiExplorer**會自動呼叫**IDocumentationProvider**介面，以取得每個 API 的文件字串。 它會儲存在**文件**屬性**ApiDescription**並**ApiParameterDescription**物件。

## <a name="next-steps"></a>後續步驟

您不限於如下所示的 [說明] 頁。 事實上， **ApiExplorer**不限於建立 [說明] 頁面。 Yao Huang 連結已寫入一些很棒的部落格文章以取得您想要立即可用的：

- [將簡單的測試用戶端新增至 ASP.NET Web API 說明頁面](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [進行 ASP.NET Web API 說明頁面在自我裝載的服務上運作](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [設計階段產生說明頁面 （或用戶端） 的 ASP.NET Web api](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [進階的說明頁面自訂](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
