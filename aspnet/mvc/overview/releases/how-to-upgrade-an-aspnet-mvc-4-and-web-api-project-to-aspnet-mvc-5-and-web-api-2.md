---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: 如何升級 ASP.NET MVC 4 和 Web API 專案，以 ASP.NET MVC 5 和 Web API 2 |Microsoft Docs
author: Rick-Anderson
description: ASP.NET MVC 5 和 Web API 2 讓新的功能，包括屬性路由、 驗證篩選條件，以及其他更多的主機。
ms.author: riande
ms.date: 10/17/2013
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d6fb40741c5f7b992e907a462ac92972fe603624
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578363"
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>如何將 ASP.NET MVC 4 和 Web API 專案升級至 ASP.NET MVC 5 和 Web API 2
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

> ASP.NET MVC 5 和 Web API 2 讓新的功能，包括屬性路由、 驗證篩選條件，以及其他更多的主機。 請參閱[ https://www.asp.net/vnext ](https://www.asp.net/core)如需詳細資訊。
> 
> 本逐步解說將引導您升級為最新版本的應用程式所需的步驟。  
> 
> > [!NOTE]
> > 請參閱[ASP.NET 及 Web Tools for Visual Studio 2013 版本資訊](../../../visual-studio/overview/2013/release-notes.md)的重大變更的 MVC 4 和 Web API 的下一個版本的詳細資訊。
> 
>   
> 
> 這篇文章所編寫的 Youngjune 香港和 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>升級步驟

1. 備份您的專案。 本逐步解說會要求您對您的專案檔、 封裝組態和 web.config 檔案中的變更。
2. 從 Web API 升級至 Web API 2 中，在 global.asax，變更：

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

   設為

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. 請確定您的專案使用的所有封裝都都使用 MVC 5 和 Web API 2 相容。 下表所示的 MVC 4 和 Web API 相關的封裝，不需要變更。 如果您有相依於其中一個下面所列封裝的封裝，請連絡來取得與 MVC 5 和 Web API 2 相容的較新版本的發行者。 如果您有這些套件的原始程式碼，您應該使用新的組件的 MVC 5 和 Web API 2 重新編譯它們。   

    | **套件識別碼** | **舊的版本** | **新的版本** |
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
    | Microsoft.Net.Http | 2.0.x。 | 2.2.x 版本。 |
    | Microsoft.Data.OData | 5.2.x | 5.6.x |
    | System.Spatial | 5.2.x | 5.6.x |
    | Microsoft.Data.Edm | 5.2.x | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o: p>< >< / o: p>< > | 已移除 |
    | Microsoft.AspNet.WebPages.Administration | < o: p>< >< / o: p>< > | 已移除 |
    | Microsoft-Web-Helpers | < o: p>< >< / o: p>< > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Microsoft Web Helpers 已被取代 Microsoft.AspNet.WebHelpers。 您應該先，移除舊的封裝，然後再安裝較新的套件。   
    >   
    > 沒有跨版本相容性之間主要的 ASP.NET 套件。 例如，MVC 5 適用於只有第 3 Razor 和不 Razor 2。
4. Visual Studio 2013 中開啟您的專案。
5. 移除任何已安裝下列 ASP.NET NuGet 封裝。 您將會移除這些使用套件管理員主控台 (PMC)。 若要開啟 PMC 中，選取**工具**功能表，然後選取**程式庫套件管理員，** 然後選取**Package Manager Console**。 您的專案可能不會包含所有這些。

    1. `Microsoft.AspNet.WebPages.Administration`  
   通常在從 MVC 3 升級至 MVC 4 時，會新增此套件。 若要移除它，請在 PMC 中執行下列命令：  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
   此套件已更名為`Microsoft.AspNet.WebHelpers`。 若要移除它，請在 PMC 中執行下列命令：  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
   此套件包含 bug 已修正在 MVC 5 的 MVC 4 中因應措施。 若要移除它，請在 PMC 中執行下列命令：  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. 升級所有使用 PMC 的 ASP.NET NuGet 封裝。 在 PMC 中，執行下列命令：  
    `Update-Package`  
   `Update-Package`命令沒有任何參數將會更新每個套件。 您可以使用識別碼引數的個別更新套件。 如需有關更新命令的詳細資訊，請執行`get-help update-package`。

## <a name="update-the-application-webconfig-file"></a>更新應用程式*web.config*檔案

請務必在應用程式中進行這些變更*web.config*檔案，不*web.config*中的檔案*檢視*資料夾。

找出`<runtime>/<assemblyBinding>`區段，然後進行下列變更：

1. 在名稱屬性"System.Web.Mvc 」 的項目，變更 「 version=5.0.0.0"從"4.0.0.0 」 版本號碼。 （在該項目中的兩個變更。）
2. 在具有 name 屬性的項目&quot;System.Web.Helpers 」 並&quot;System.Web.WebPages&quot;變更從 「 2.0.0.0"到"3.0.0.0"版本數字。 四個變更，就會在每個元素的兩個。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. 找出`<appSettings>`區段，並更新到 3.0.0.0 webpages:version 從 2.0.0.0.0，如下所示：

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. 移除任何不完整的信任層級。 例如: 

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>更新*web.config* [Views] 資料夾下的檔案

如果您的應用程式使用的區域，您也必須更新每個*web.config*中的檔案*檢視*的每個 Areas 資料夾的子資料夾。

1. 更新版本 「 version=5.0.0.0"版本"4.0.0.0 」 包含"System.Web.Mvc 」 的所有項目。  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. 更新包含 「 2.0.0.0"版 「 System.Web.WebPages.Razor 」 到 「 3.0.0.0"版的所有項目。 本節包含 「 System.Web.WebPages"，如果更新版本 「 3.0.0.0"版本"2.0.0.0 」 從這些項目  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. 如果您移除了`Microsoft-Web-Helpers`NuGet 套件，在上一個步驟中，安裝`Microsoft.AspNet.WebHelpers`使用下列命令在 PMC 中：  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. 如果您的應用程式會使用[User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx)方法，將下列內容加入*Web.config*檔案。

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>最後一個步驟

建置及測試應用程式。

從專案檔中移除 MVC 4 專案類型 GUID。

1. 在 [方案總管] 中，以滑鼠右鍵按一下專案名稱，然後按**卸載專案**。
2. 以滑鼠右鍵按一下專案，然後選取 [編輯 projectname.csproj]。
3. 找出`ProjectTypeGuids`項目，然後移除 MVC 4 專案的 GUID， `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`。
4. 儲存並關閉開啟的專案檔。
5. 以滑鼠右鍵按一下專案，然後選取**重新載入專案**。
