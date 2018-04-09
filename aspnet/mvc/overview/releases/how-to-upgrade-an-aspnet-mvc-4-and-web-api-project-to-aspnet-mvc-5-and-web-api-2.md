---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: 如何升級 ASP.NET MVC 4 和 Web API 專案，以 ASP.NET MVC 5 和 Web API 2 |Microsoft 文件
author: Rick-Anderson
description: ASP.NET MVC 5 和 Web API 2 將新功能，包括路由屬性、 驗證篩選條件，以及執行更多的主機。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f61502933a5ba92896ee97cef9cff915fe23831d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> ASP.NET MVC 5 和 Web API 2 將新功能，包括路由屬性、 驗證篩選條件，以及執行更多的主機。 請參閱[ https://www.asp.net/vnext ](https://www.asp.net/core)如需詳細資訊。
> 
> 本逐步解說會引導您升級為最新版本的應用程式所需的步驟。  
> 
> > [!NOTE]
> > 請參閱[ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../visual-studio/overview/2013/release-notes.md)的重大變更到下一個版本 MVC 4 和 Web API 的詳細資訊。
> 
>   
> 
> 本文撰寫 Youngjune 香港和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>升級步驟

1. 備份您的專案。 本逐步解說將會要求您對您的專案檔、 封裝組態和 web.config 檔案中的變更。
2. 在 global.asax 中, 升級從 Web API Web API 2，變更：

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   設為

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. 請確定您的專案使用的所有封裝都都與 MVC 5 和 Web API 2 相容。 下列表格顯示 MVC 4 和 Web API 相關封裝比需要變更。 如果您有相依於其中一個下面所列封裝的封裝，請連絡發行者以取得與 MVC 5 和 Web API 2 相容的較新版本。 如果您有這些封裝的原始程式碼，則應該使用新的組件的 MVC 5 和 Web API 2 重新編譯它們。   

    | **封裝識別碼** | **舊的版本** | **新的版本** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0.x. | 2.2.x. |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | <o:p> </o:p> | 已移除 |
    | Microsoft.AspNet.WebPages.Administration | <o:p> </o:p> | 已移除 |
    | Microsoft-Web-Helpers | <o:p> </o:p> | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Web Helpers Microsoft.AspNet.WebHelpers 已取代。 您應該先移除舊的封裝，然後再安裝較新的封裝。   
    >   
    > 沒有跨版本之間的相容性主要 ASP.NET 封裝。 例如，MVC 5 是只使用 Razor 3 和 Razor 2 不相容。
4. Visual Studio 2013 中開啟您的專案。
5. 移除任何已安裝下列 ASP.NET NuGet 套件。 您將會移除這些使用封裝管理員主控台 (PMC)。 若要開啟 PMC，請選取**工具**功能表，然後選取**程式庫封裝管理員，**然後選取**Package Manager Console**。 您的專案可能不包括所有的這些。

    1. `Microsoft.AspNet.WebPages.Administration`  
   從 MVC 3 升級至 MVC 4 時，通常會將加入此封裝。 若要移除它，請在 PMC 中執行下列命令：  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   此套件有品牌已重新命名為`Microsoft.AspNet.WebHelpers`。 若要移除它，請在 PMC 中執行下列命令：  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   此套件包含因應措施的 MVC 5 中已修正問題的 MVC 4 中的 bug。 若要移除它，請在 PMC 中執行下列命令：  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. 升級使用 pmc 依存的所有 ASP.NET NuGet 封裝。 在 PMC，執行下列命令：  
    `Update-Package`  
   `Update-Package`命令沒有任何參數將會更新每個套件。 您可以使用識別碼引數，個別更新封裝。 如需有關更新命令的詳細資訊，請執行`get-help update-package`。

## <a name="update-the-application-webconfig-file"></a>更新應用程式*web.config*檔案

請務必在應用程式中進行這些變更*web.config*檔案無法*web.config*檔案*檢視*資料夾。

找出`<runtime>/<assemblyBinding>`區段，然後進行下列變更：

1. 在名稱屬性為"system.web.mvc 的參考 」 的項目，變更從 「 4.0.0.0"到"5.0.0.0 」 版本數字。 （該元素中的兩個變更。）
2. 在具有 name 屬性的項目&quot;System.Web.Helpers"和&quot;System.Web.WebPages&quot;變更從 「 2.0.0.0"的版本號碼，以 「 3.0.0.0"。 四項變更不會進行兩個在每個項目。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. 找出`<appSettings>`區段，並更新從 2.0.0.0.0 webpages:version 至 3.0.0.0，如下所示：

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. 移除任何不完整的信任層級。 例如: 

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>更新*web.config* [檢視] 資料夾下的檔案

如果您的應用程式使用的區域，您也必須更新每個*web.config*檔案*檢視*子資料夾的每個區域的資料夾。

1. 更新包含版本 」 4.0.0.0"版本"5.0.0.0"到"system.web.mvc 的參考 」 的所有項目。  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. 更新包含 「 2.0.0.0"版版本 3.0.0.0 「 開始 」 到 「 System.Web.WebPages.Razor 」 的所有項目。 如果這個區段包含 「 System.Web.WebPages"，更新 「 3.0.0.0"版本從版本"2.0.0.0"這些項目  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. 如果您移除`Microsoft-Web-Helpers`NuGet 封裝在先前步驟中，安裝`Microsoft.AspNet.WebHelpers`使用 pmc 依存在下列命令：  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. 如果您的應用程式使用[User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx)方法，將下列內容加入*Web.config*檔案。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>最後一個步驟

建置及測試應用程式。

從專案檔中移除 MVC 4 專案類型的 GUID。

1. 在方案總管 中，以滑鼠右鍵按一下專案名稱，然後選取**卸載專案**。
2. 以滑鼠右鍵按一下專案，然後選取 [編輯 projectname.csproj]。
3. 找出`ProjectTypeGuids`項目，然後移除 MVC 4 專案 GUID， `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`。
4. 儲存並關閉開啟的專案檔案。
5. 以滑鼠右鍵按一下專案，然後選取**重新載入專案**。
