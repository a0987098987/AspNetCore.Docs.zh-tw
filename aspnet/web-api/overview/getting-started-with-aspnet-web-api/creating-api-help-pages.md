---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "建立 ASP.NET Web API 說明頁面 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 18d04492529e96b6c0e14f1d7a30378b4832f4c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-help-pages-for-aspnet-web-api"></a>建立 ASP.NET Web API 說明頁面
====================
由[Mike Wasson](https://github.com/MikeWasson)

當您建立 web 應用程式開發介面時，它通常是用於建立一份說明網頁，，以便在其他開發人員知道如何呼叫您的 API。 您可以手動建立所有的文件，但最好是盡可能自動產生。

若要簡化這項工作中，ASP.NET Web API 會提供文件庫在執行階段自動產生說明頁面。

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>建立 API 說明頁面

安裝[ASP.NET 及 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。 此更新會整合至 Web API 專案範本的 [說明] 頁。

接下來，建立新的 ASP.NET MVC 4 專案，然後選取 Web API 專案範本。 專案範本會建立名為範例應用程式開發介面控制站`ValuesController`。 範本也會建立應用程式開發介面的 [說明] 頁。 所有 [說明] 頁面的程式碼檔案會放置在專案的 [區域] 資料夾。

![](creating-api-help-pages/_static/image2.png)

當您執行應用程式時，[首頁] 頁面包含 API 說明頁面的連結。 在首頁上，從相對路徑會是 /Help。

![](creating-api-help-pages/_static/image3.png)

此連結會帶您到 API 摘要頁面。

![](creating-api-help-pages/_static/image4.png)

此頁面的 MVC 檢視 Areas/HelpPage/Views/Help/Index.cshtml 中定義。 您可以編輯這個頁面，即可修改版面配置、 簡介、 標題、 樣式和其他等等。

[] 頁面上的主要部分是應用程式開發介面，控制站所分組的資料表。 資料表項目使用，以動態方式產生**IApiExplorer**介面。 （我會詳細討論此介面更新版本。）如果您加入新的 API 控制器時，在執行階段時，會自動更新資料表。

「 應用程式開發介面 」 資料行列出的 HTTP 方法和相對 URI。 「 描述 」 資料行包含每個應用程式開發介面文件。 一開始，文件是只預留位置文字。 在下一步 區段中，我將為您示範如何從 XML 註解加入文件。

每個應用程式開發介面有更詳細的資訊，包括範例要求和回應主體與頁面的連結。

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>將 [說明] 頁新增至現有的專案

您可以加入至現有的 Web API 專案的 [說明] 頁使用 NuGet 封裝管理員。 這個選項適合您從比 「 Web API 」 範本不同的專案範本開始。

從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在[Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)視窗中，輸入下列命令的其中一個：

如**C#**應用程式：`Install-Package Microsoft.AspNet.WebApi.HelpPage`

如**Visual Basic**應用程式：`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

有兩個封裝，另一個適用於 C# 和 Visual basic 中的一個。 請務必使用符合您的專案。

此命令會安裝必要的組件，並新增 （位於 區域/HelpPage 資料夾） 的 說明 頁的 MVC 檢視。 您必須手動將連結加入至說明頁面。 URI 是 /Help。 若要建立 razor 檢視的連結，加入下列內容：

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

此外，請確定註冊區域。 在 Global.asax 檔案中，加入下列程式碼**應用程式\_啟動**方法，如果它尚未有：

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>加入應用程式開發介面文件

根據預設，說明 頁面會提供文件集的預留位置字串。 您可以使用[XML 文件註解](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx)建立文件。 若要啟用此功能，開啟檔案的 應用程式的區域/HelpPage\_Start/HelpPageConfig.cs 並取消註解下列一行：

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

現在可讓 XML 文件。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。 選取**建置**頁面。

![](creating-api-help-pages/_static/image6.png)

在下**輸出**，檢查**XML 文件檔**。 在編輯方塊中，輸入 「 應用程式\_Data/XmlDocument.xml"。

![](creating-api-help-pages/_static/image7.png)

接下來，開啟的程式碼`ValuesController`/Controllers/ValuesControler.cs 中定義的 API 控制器。 將某些文件註解加入至控制器方法。 例如: 

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> 秘訣： 如果您插入號位置的方法上方列上，並輸入三個正斜線，Visual Studio 會自動插入 XML 項目。 然後您可以填入之空白個數。


現在建置並再次執行應用程式，以及瀏覽至 [說明] 頁。 文件字串應該會出現在應用程式開發介面資料表。

![](creating-api-help-pages/_static/image8.png)

說明頁面會在執行階段，從 XML 檔案讀取字串。 （當您部署應用程式時，請確定部署 XML 檔案。）

## <a name="under-the-hood"></a>背後原理

說明頁面會建立最上層的**ApiExplorer**類別，這是 Web API framework 的一部分。 **ApiExplorer**類別提供原料建立一份說明網頁。 每個 api， **ApiExplorer**包含**ApiDescription**描述應用程式開發介面。 基於此目的，「 API 」 定義為 HTTP 方法和相對 URI 的組合。 例如，以下是一些不同的應用程式開發介面：

- 取得 /api/Products
- 取得 /api/產品 / {id}
- 張貼/api/產品

如果控制器動作，支援多個 HTTP 方法， **ApiExplorer**視為不同的應用程式開發介面中的每個方法。

若要隱藏的 API **ApiExplorer**，新增**ApiExplorerSettings**屬性至動作，並將*IgnoreApi*為 true。

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

您也可以將此屬性加入控制器，以排除整個控制器中。

ApiExplorer 類別取得的文件字串從**IDocumentationProvider**介面。 如稍早所見，說明 頁面的程式庫提供**IDocumentationProvider**來取得文件從 XML 文件的字串。 程式碼位於 /Areas/HelpPage/XmlDocumentationProvider.cs。 您可以取得文件從其他來源藉由撰寫您自己**IDocumentationProvider**。 若要連接它，呼叫**SetDocumentationProvider**中定義的擴充方法**HelpPageConfigurationExtensions**

**ApiExplorer**自動呼叫**IDocumentationProvider**介面來取得每個 API 文件的字串。 它將其儲存在**文件**屬性**ApiDescription**和**ApiParameterDescription**物件。

## <a name="next-steps"></a>後續步驟

您不限於如下所示的 [說明] 頁。 事實上， **ApiExplorer**不限於建立說明頁面。 一些很棒的部落格文章，讓您以為現成寫入 Yao Huang 連結：

- [將簡單的測試用戶端加入至 ASP.NET Web API 說明頁面](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [進行 ASP.NET Web API 說明頁面在自我裝載服務上運作](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [設計階段產生的說明頁面 （或用戶端） 的 ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [說明頁面的進階自訂內容](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
