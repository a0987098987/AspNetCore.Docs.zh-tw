---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: "ASP.NET 和 Azure App Service 部署的密碼和其他機密資料的最佳做法 |Microsoft 文件"
author: Rick-Anderson
description: "本教學課程會示範如何您的程式碼可以安全地儲存及存取的安全資訊。 最重要的一點是您應該永遠不會儲存密碼或其他服務..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>ASP.NET 和 Azure App Service 部署的密碼和其他機密資料的最佳作法
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何您的程式碼可以安全地儲存及存取的安全資訊。 您應該永遠不會儲存密碼或其他機密資料來源的程式碼，而您不應該在開發和測試模式中使用實際執行的密碼最重要的一點。
> 
> 程式碼範例是簡單的 WebJob 主控台應用程式和 ASP.NET MVC 應用程式需要存取資料庫的連接字串密碼，Twilio，Google 和 SendGrid 安全金鑰。
> 
> 在內部部署上設定及 PHP 也提及。


- [使用開發環境中的密碼](#pwd)
- [使用開發環境中的連接字串](#con)
- [WebJobs 主控台應用程式](#wj)
- [部署至 Azure 的機密資料](#da)
- [在內部部署和 PHP 注意事項](#not)
- [其他資源](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>使用開發環境中的密碼

教學課程經常會顯示敏感性資料來源的程式碼中希望使用要事先通知，您應該永遠不會在原始程式碼中儲存機密資料。 例如，我[ASP.NET MVC 5 應用程式與 SMS 和電子郵件 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)教學課程會示範中的下列*web.config*檔案：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config*檔案是原始碼，因此不應這些密碼儲存在該檔案。 幸運的是，`<appSettings>`項目具有`file`可讓您指定外部檔案包含機密的應用程式組態設定的屬性。 只要外部的檔案不簽入原始檔樹狀，您可以將您的機密移到外部檔案中。 例如，在下列標記檔案*AppSettingsSecrets.config*包含所有應用程式密碼：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

外部檔案中的標記 (*AppSettingsSecrets.config*在此範例中)，在相同的標記中找到*web.config*檔案：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

ASP.NET 執行階段會合併的外部檔案中的標記與內容&lt;appSettings&gt;項目。 如果找不到指定的檔案，則執行階段會略過檔案屬性。

> [!WARNING]
> 安全性-不要新增您*密碼.config*檔案至您的專案，或將它簽入原始檔控制。 根據預設，Visual Studio 會將設定`Build Action`至`Content`，這表示檔案已部署。 如需詳細資訊，請參閱[為何不在我的專案資料夾中檔案的所有部署？](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) 雖然您可以使用任何副檔名*密碼.config*檔案，所以最好先將它保留*.config*，如設定檔不會由 IIS。 也請注意*AppSettingsSecrets.config*檔案是兩個目錄層級從*web.config*檔案，讓它完全超出方案目錄。 移動檔案使用的方案目錄，藉以&quot;git 新增\*&quot;將不會將它加入至您的儲存機制。


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>使用開發環境中的連接字串

Visual Studio 會建立新的 ASP.NET 專案使用[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)。 LocalDB 是專為開發環境。 它不需要密碼，因此您不需要執行任何動作來防止機密資料被簽入您的原始程式碼。 某些開發團隊使用 SQL Server （或其他 DBMS） 的完整的版本需要密碼。

您可以使用`configSource`屬性以取代整個`<connectionStrings>`標記。 不同於`<appSettings>``file`屬性合併標記`configSource`屬性會取代標記。 下列標記顯示`configSource`屬性*web.config*檔案：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> 如果您使用`configSource`屬性如上所示，將您的連接字串移至外部檔案，而且具有 Visual Studio 建立新的網站，它無法偵測到您要使用資料庫，而且將不會取得資料庫的設定選項時您 pu從 Visual Studio azure blish。 如果您使用`configSource`屬性，您可以使用 PowerShell 建立和部署您的網站和資料庫，或者您可以建立網站和資料庫入口網站中發行之前。 [新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)指令碼會建立新的網站和資料庫。


> [!WARNING]
> 安全性-不同於*AppSettingsSecrets.config*檔案中，外部連接字串檔案必須是相同的目錄做為根*web.config*檔案，所以您不必採取預防措施以確保您不要將它簽入您的來源儲存機制。


> [!NOTE]
> **密碼檔案上的安全性警告：**最佳作法是不要使用實際執行測試和開發中的機密資料。 使用實際執行環境中測試或開發的密碼遺漏這些密碼。


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>WebJobs 主控台應用程式

*App.config*主控台應用程式所使用的檔案不支援相對路徑，但支援絕對路徑。 若要將您的機密移出您的專案目錄，您可以使用絕對路徑。 下列標記會顯示在 密碼*C:\secrets\AppSettingsSecrets.config*檔和中的非機密資料*app.config*檔案。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>部署至 Azure 的機密資料

當您將 web 應用程式部署至 Azure， *AppSettingsSecrets.config*檔案不會部署 （亦即您想要）。 您可以移至[Azure 管理入口網站](https://azure.microsoft.com/services/management-portal/)並手動設定執行此作業：

1. 移至[https://portal.azure.com](https://portal.azure.com)，並以您的 Azure 認證登入。
2. 按一下**瀏覽&gt;Web 應用程式**，然後按一下 web 應用程式的名稱。
3. 按一下**所有設定&gt;應用程式設定**。

**應用程式設定**和**連接字串**值會覆寫中的相同設定*web.config*檔案。 在本例中，我們無法部署這些設定到 Azure，但如果這些機碼已在*web.config*檔案中，顯示在入口網站上的設定會優先。

最佳做法是遵循[DevOps 工作流程](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md)並用[Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (或另一個架構，例如[Chef](http://www.opscode.com/chef/)或[Puppet](http://puppetlabs.com/puppet/what-is-puppet)) 至在 Azure 中設定這些值會自動執行。 下列 PowerShell 指令碼會使用[Export-clixml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk)加密機密資料匯出至磁碟：

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

在上述指令碼，'Name' 是名稱之祕密金鑰，例如 '&quot;FB\_AppSecret&quot;或 「 TwitterSecret"。 您可以檢視您的瀏覽器中的指令碼所建立之 「.credential"檔案。 以下程式碼片段會測試每個認證檔案，並將設定具名的 web 應用程式密碼：

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> 安全性-不執行因此擊敗部署敏感性資料使用的 PowerShell 指令碼的目的在 PowerShell 指令碼中包含密碼或其他機密資料。 [Get-credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet 會提供用於取得密碼的安全機制。 使用 UI 提示，可以防止洩漏密碼。


### <a name="deploying-db-connection-strings"></a>部署資料庫的連接字串

同樣地處理 DB 連線字串的應用程式設定。 如果您部署您的 web 應用程式，從 Visual Studio，則會為您設定的連接字串。 您可以在入口網站來確認。 若要設定的連接字串的建議的方式是使用 PowerShell。 如需 PowerShell 指令碼的範例會建立網站和資料庫，並設定連接字串在網站上下載[新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)從[Azure 指令碼程式庫](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)。

<a id="not"></a>
## <a name="notes-for-php"></a>適用於 PHP 的附註

因為兩個索引鍵 / 值組**應用程式設定**和**連接字串**儲存在環境變數在 Azure 應用程式服務，輕鬆地使用任何 web 應用程式架構 （例如 PHP) 可能的開發人員擷取這些值。 請參閱 Stefan Schackow [Windows Azure Web Sites： 如何應用程式字串與連接字串工作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)部落格文章會顯示 PHP 程式碼片段來讀取應用程式設定和連接字串。

## <a name="notes-for-on-premises-servers"></a>在內部部署伺服器資訊

如果您要部署至內部部署 web 伺服器，您可以協助安全密碼[加密組態檔的組態區段](https://msdn.microsoft.com/library/ff647398.aspx)。 或者，您可以使用 Azure 網站的建議相同的方法： 在組態檔中保留開發設定，但使用的實際執行設定環境變數值。 在此情況下，不過，您必須撰寫應用程式程式碼會自動在 Azure 網站中的功能： 擷取環境變數設定和使用這些值來取代組態檔設定，或使用組態檔設定時找不到環境變數。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

如需 PowerShell 的範例會建立 web 應用程式 + 資料庫的指令碼會設定連接字串 + 應用程式設定、 下載[新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)從[Azure 指令碼程式庫](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)。 

請參閱 Stefan Schackow [Windows Azure Web Sites： 應用程式字串與連接字串的工作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


特別感謝 Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) 和 Carlos Farre 檢閱。
