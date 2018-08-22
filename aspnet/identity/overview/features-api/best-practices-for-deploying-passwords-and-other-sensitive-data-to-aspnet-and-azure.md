---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: 將密碼和其他機密資料部署到 ASP.NET 和 Azure App Service 最佳作法 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何安全地儲存及存取安全資訊，您的程式碼。 最重要的一點是您應該永遠不會儲存密碼或其他服務...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: eda2277a4baad8f2a63aa2fdf6ab84f57f1eb0e0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826404"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>將密碼和其他機密資料部署到 ASP.NET 和 Azure App Service 的最佳作法
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 本教學課程會示範如何安全地儲存及存取安全資訊，您的程式碼。 最重要的一點是，始終不該將密碼或其他機密資料儲存在原始程式碼中，也不該在開發和測試模式中使用實際執行的密碼。
> 
> 範例程式碼是簡單的 WebJob 主控台應用程式和 ASP.NET MVC 應用程式需要資料庫連接字串的密碼，Twilio，Google 和 SendGrid 安全金鑰的存取。
> 
> 在內部部署上設定和 PHP 也會提及。


- [使用開發環境中的密碼](#pwd)
- [使用開發環境中的連接字串](#con)
- [Webjob 的主控台應用程式](#wj)
- [將機密資料部署至 Azure](#da)
- [在公司內部和 PHP 的相關資訊](#not)
- [其他資源](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>使用開發環境中的密碼

教學課程經常會示範在原始程式碼中，希望有一點要注意，您應該機密資料絕不儲存在原始程式碼中的敏感性資料。 比方說，我[ASP.NET MVC 5 應用程式使用 SMS 和電子郵件 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)教學課程會示範下列*web.config*檔案：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config*檔案是原始碼，讓這些機密資料應該永遠不會儲存在該檔案。 幸好`<appSettings>`項目具有`file`可讓您指定外部檔案包含機密的應用程式組態設定的屬性。 只要外部的檔案未簽入至您的來源樹狀結構，您可以將您所有的機密資料移至外部檔案中。 例如，在下列標記中，檔案*AppSettingsSecrets.config*包含所有應用程式祕密：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

外部檔案中的標記 (*AppSettingsSecrets.config*在此範例中)，相同的標記中找到*web.config*檔案：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

外部檔案中的標記的內容合併至 ASP.NET 執行階段&lt;appSettings&gt;項目。 如果找不到指定的檔案，則執行階段會略過檔案屬性。

> [!WARNING]
> 安全性-請勿將新增您*祕密.config*檔案至您的專案，或將它簽入原始檔控制。 根據預設，Visual Studio 會將`Build Action`至`Content`，這表示檔案部署。 如需詳細資訊，請參閱[為什麼不在我的專案資料夾中檔案的所有部署？](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) 雖然您可以使用適用於任何擴充功能*祕密.config*檔案，所以最好先將它保持 *.config*，如設定檔不會由 IIS。 也請注意*AppSettingsSecrets.config*檔案是兩個目錄從層級向上*web.config*檔案，因此，它完全超出方案目錄。 將檔案從方案目錄中，移&quot;新增 git \* &quot;不會將它新增至您的儲存機制。


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>使用開發環境中的連接字串

Visual Studio 會建立使用的新 ASP.NET 專案[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)。 LocalDB 是專為開發環境中建立。 它不需要密碼，因此您不需要採取任何動作正在簽入您的程式碼時，防止機密資料。 有些開發小組使用需要密碼的完整版本的 SQL Server （或其他 DBMS）。

您可以使用`configSource`屬性以取代整個`<connectionStrings>`標記。 不同於`<appSettings>``file`合併標記的屬性`configSource`屬性會取代標記。 下列標記示範`configSource`屬性中*web.config*檔案：

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> 如果您使用`configSource`屬性如上所示，將您的連接字串移至外部檔案，並讓 Visual Studio 建立新的網站，它將無法偵測到您要使用資料庫，而且您無法取得資料庫的設定選項時您 pu從 Visual Studio azure blish。 如果您使用`configSource`屬性，您可以使用 PowerShell 來建立及部署您的網站和資料庫，或您可以建立網站和資料庫入口網站中發行之前。 [新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)指令碼會建立新的網站和資料庫。


> [!WARNING]
> 安全性-不同於*AppSettingsSecrets.config*檔案中，外部連接字串檔案必須是相同的目錄，做為根*web.config*檔案，因此您必須採取預防措施以確保您不將它簽入您的來源存放庫。


> [!NOTE]
> **在 祕密檔案上的安全性警告：** 最佳做法是不要使用生產環境中開發和測試的祕密。 使用生產環境中測試或開發的密碼流失這些祕密。


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Webjob 的主控台應用程式

*App.config*主控台應用程式所使用的檔案不支援相對路徑，但它確實支援絕對路徑。 若要將您的機密資料移出您的專案目錄，您可以使用絕對路徑。 下列標記會顯示在 祕密*C:\secrets\AppSettingsSecrets.config*檔案，以及中的非機密資料*app.config*檔案。

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>將機密資料部署至 Azure

當您將 web 應用程式部署至 Azure *AppSettingsSecrets.config*檔案不會部署 （也就是您想要）。 您可以前往[Azure 管理入口網站](https://azure.microsoft.com/services/management-portal/)和設定它們以手動的方式，若要這樣做：

1. 移至[ https://portal.azure.com ](https://portal.azure.com)，並使用您的 Azure 認證登入。
2. 按一下 **瀏覽&gt;Web 應用程式**，然後按一下 web 應用程式的名稱。
3. 按一下 **一切&gt;應用程式設定**。

**應用程式設定**並**連接字串**值會覆寫中的相同設定*web.config*檔案。 在本例中，我們並未不將這些設定部署至 Azure，但這些機碼是否在*web.config*檔案中，顯示在入口網站上的設定會優先。

最佳做法是遵循[DevOps 工作流程](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md)並用[Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (或另一個架構，例如[Chef](http://www.opscode.com/chef/)或[Puppet](http://puppetlabs.com/puppet/what-is-puppet)) 至自動在 Azure 中設定這些值。 下列 PowerShell 指令碼會使用[Export-clixml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk)匯出至磁碟的加密的密碼：

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

在上述指令碼，'Name' 是名稱的祕密金鑰，例如 '&quot;FB\_AppSecret&quot;或 「 TwitterSecret"。 您可以檢視您的瀏覽器中的指令碼建立的 「.credential"檔案。 下列程式碼片段會測試每個認證檔案，並將設定具名的 web 應用程式祕密：

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> 安全性-不在執行因此失效的目的，將敏感性資料使用的 PowerShell 指令碼的 PowerShell 指令碼中包含密碼或其他機密資料。 [Get-credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet 會提供一個安全機制，來取得的密碼。 使用 UI 提示，可以防止洩漏密碼。


### <a name="deploying-db-connection-strings"></a>部署資料庫連接字串

同樣地處理 DB 連接字串的應用程式設定。 如果您部署您的 web 應用程式，從 Visual Studio，將會為您設定的連接字串。 您可以在入口網站中確認。 若要設定的連接字串的建議的方式是使用 PowerShell。 如需 PowerShell 指令碼的範例會建立網站和資料庫，並設定連接字串，在網站中，下載[新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)從[Azure 指令碼程式庫](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)。

<a id="not"></a>
## <a name="notes-for-php"></a>適用於 PHP 的附註

因為兩個索引鍵 / 值組**應用程式設定**並**連接字串**儲存在環境變數在 Azure App Service，開發人員輕鬆地使用任何 web 應用程式架構 （例如 PHP) 可以擷取這些值。 請參閱 Stefan Schackow [Windows Azure 網站： 應用程式字串與連接字串的運作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)部落格文章會顯示為應用程式設定和連接字串的 PHP 程式碼片段。

## <a name="notes-for-on-premises-servers"></a>在內部部署伺服器的相關資訊

如果您要部署到內部部署 web 伺服器，您可以協助安全的祕密[加密組態檔的組態區段](https://msdn.microsoft.com/library/ff647398.aspx)。 或者，您可以使用相同的方法建議用於 Azure 網站： 在組態檔中保留開發設定和環境變數的值用於實際執行設定。 在此情況下，不過，您必須撰寫應用程式程式碼便會自動在 Azure 網站中的功能： 擷取環境變數中的設定和使用這些值來取代組態檔設定，或使用組態檔設定時找不到環境變數。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

如需 PowerShell 的範例指令碼來建立 web 應用程式 + 資料庫，可設定的連接字串 + 應用程式設定、 下載[新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)從[Azure 指令碼程式庫](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)。 

請參閱 Stefan Schackow [Windows Azure 網站： 如何應用程式字串與連接字串的運作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


特別感謝 Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) 和 Carlos Farre 檢閱。
