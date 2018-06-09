---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: ASP.NET 和 Azure App Service 部署的密碼和其他機密資料的最佳做法 |Microsoft 文件
author: Rick-Anderson
description: 本教學課程會示範如何您的程式碼可以安全地儲存及存取的安全資訊。 最重要的一點是您應該永遠不會儲存密碼或其他服務...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28033017"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a><span data-ttu-id="9d851-104">ASP.NET 和 Azure App Service 部署的密碼和其他機密資料的最佳作法</span><span class="sxs-lookup"><span data-stu-id="9d851-104">Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service</span></span>
====================
<span data-ttu-id="9d851-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9d851-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="9d851-106">本教學課程會示範如何您的程式碼可以安全地儲存及存取的安全資訊。</span><span class="sxs-lookup"><span data-stu-id="9d851-106">This tutorial shows how your code can securely store and access secure information.</span></span> <span data-ttu-id="9d851-107">最重要的一點是，始終不該將密碼或其他機密資料儲存在原始程式碼中，也不該在開發和測試模式中使用實際執行的密碼。</span><span class="sxs-lookup"><span data-stu-id="9d851-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span>
> 
> <span data-ttu-id="9d851-108">程式碼範例是簡單的 WebJob 主控台應用程式和 ASP.NET MVC 應用程式需要存取資料庫的連接字串密碼，Twilio，Google 和 SendGrid 安全金鑰。</span><span class="sxs-lookup"><span data-stu-id="9d851-108">The sample code is a simple WebJob console app and a ASP.NET MVC app that needs access to a database connection string password, Twilio, Google and SendGrid secure keys.</span></span>
> 
> <span data-ttu-id="9d851-109">在內部部署上設定及 PHP 也提及。</span><span class="sxs-lookup"><span data-stu-id="9d851-109">On premise settings and PHP is also mentioned.</span></span>


- [<span data-ttu-id="9d851-110">使用開發環境中的密碼</span><span class="sxs-lookup"><span data-stu-id="9d851-110">Working with passwords in the development environment</span></span>](#pwd)
- [<span data-ttu-id="9d851-111">使用開發環境中的連接字串</span><span class="sxs-lookup"><span data-stu-id="9d851-111">Working with connection strings in the development environment</span></span>](#con)
- [<span data-ttu-id="9d851-112">WebJobs 主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="9d851-112">WebJobs console apps</span></span>](#wj)
- [<span data-ttu-id="9d851-113">部署至 Azure 的機密資料</span><span class="sxs-lookup"><span data-stu-id="9d851-113">Deploying secrets to Azure</span></span>](#da)
- [<span data-ttu-id="9d851-114">在內部部署和 PHP 注意事項</span><span class="sxs-lookup"><span data-stu-id="9d851-114">Notes for On-Premise and PHP</span></span>](#not)
- [<span data-ttu-id="9d851-115">其他資源</span><span class="sxs-lookup"><span data-stu-id="9d851-115">Additional Resources</span></span>](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a><span data-ttu-id="9d851-116">使用開發環境中的密碼</span><span class="sxs-lookup"><span data-stu-id="9d851-116">Working with passwords in the development environment</span></span>

<span data-ttu-id="9d851-117">教學課程經常會顯示敏感性資料來源的程式碼中希望使用要事先通知，您應該永遠不會在原始程式碼中儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="9d851-117">Tutorials frequently show sensitive data in source code, hopefully with a caveat that you should never store sensitive data in source code.</span></span> <span data-ttu-id="9d851-118">例如，我[ASP.NET MVC 5 應用程式與 SMS 和電子郵件 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)教學課程會示範中的下列*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="9d851-118">For example, my [ASP.NET MVC 5 app with SMS and email 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) tutorial shows the following in the *web.config* file:</span></span>

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

<span data-ttu-id="9d851-119">*Web.config*檔案是原始碼，因此不應這些密碼儲存在該檔案。</span><span class="sxs-lookup"><span data-stu-id="9d851-119">The *web.config* file is source code, so these secrets should never be stored in that file.</span></span> <span data-ttu-id="9d851-120">幸運的是，`<appSettings>`項目具有`file`可讓您指定外部檔案包含機密的應用程式組態設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="9d851-120">Fortunately, the `<appSettings>` element has a `file` attribute that allows you to specify an external file that contains sensitive app config settings.</span></span> <span data-ttu-id="9d851-121">只要外部的檔案不簽入原始檔樹狀，您可以將您的機密移到外部檔案中。</span><span class="sxs-lookup"><span data-stu-id="9d851-121">You can move all your secrets to an external file as long as the external file is not checked into your source tree.</span></span> <span data-ttu-id="9d851-122">例如，在下列標記檔案*AppSettingsSecrets.config*包含所有應用程式密碼：</span><span class="sxs-lookup"><span data-stu-id="9d851-122">For example, in the following markup, the file *AppSettingsSecrets.config* contains all the app secrets:</span></span>

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

<span data-ttu-id="9d851-123">外部檔案中的標記 (*AppSettingsSecrets.config*在此範例中)，在相同的標記中找到*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="9d851-123">The markup in the external file (*AppSettingsSecrets.config* in this sample), is the same markup found in the *web.config* file:</span></span>

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

<span data-ttu-id="9d851-124">ASP.NET 執行階段會合併的外部檔案中的標記與內容&lt;appSettings&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="9d851-124">The ASP.NET runtime merges the contents of the external file with the markup in &lt;appSettings&gt; element.</span></span> <span data-ttu-id="9d851-125">如果找不到指定的檔案，則執行階段會略過檔案屬性。</span><span class="sxs-lookup"><span data-stu-id="9d851-125">The runtime ignores the file attribute if the specified file cannot be found.</span></span>

> [!WARNING]
> <span data-ttu-id="9d851-126">安全性-不要新增您*密碼.config*檔案至您的專案，或將它簽入原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="9d851-126">Security - Do not add your *secrets .config* file to your project or check it into source control.</span></span> <span data-ttu-id="9d851-127">根據預設，Visual Studio 會將設定`Build Action`至`Content`，這表示檔案已部署。</span><span class="sxs-lookup"><span data-stu-id="9d851-127">By default, Visual Studio sets the `Build Action` to `Content`, which means the file is deployed.</span></span> <span data-ttu-id="9d851-128">如需詳細資訊，請參閱[為何不在我的專案資料夾中檔案的所有部署？](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment)</span><span class="sxs-lookup"><span data-stu-id="9d851-128">For more information see [Why don't all of the files in my project folder get deployed?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment)</span></span> <span data-ttu-id="9d851-129">雖然您可以使用任何副檔名*密碼.config*檔案，所以最好先將它保留 *.config*，如設定檔不會由 IIS。</span><span class="sxs-lookup"><span data-stu-id="9d851-129">Although you can use any extension for the *secrets .config* file, it's best to keep it *.config*, as config files are not served by IIS.</span></span> <span data-ttu-id="9d851-130">也請注意*AppSettingsSecrets.config*檔案是兩個目錄層級從*web.config*檔案，讓它完全超出方案目錄。</span><span class="sxs-lookup"><span data-stu-id="9d851-130">Notice also that the *AppSettingsSecrets.config* file is two directory levels up from the *web.config* file, so it's completely out of the solution directory.</span></span> <span data-ttu-id="9d851-131">移動檔案使用的方案目錄，藉以&quot;git 新增\*&quot;將不會將它加入至您的儲存機制。</span><span class="sxs-lookup"><span data-stu-id="9d851-131">By moving the file out of the solution directory, &quot;git add \*&quot; won't add it to your repository.</span></span>


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a><span data-ttu-id="9d851-132">使用開發環境中的連接字串</span><span class="sxs-lookup"><span data-stu-id="9d851-132">Working with connection strings in the development environment</span></span>

<span data-ttu-id="9d851-133">Visual Studio 會建立新的 ASP.NET 專案使用[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9d851-133">Visual Studio creates new ASP.NET projects that use [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="9d851-134">LocalDB 是專為開發環境。</span><span class="sxs-lookup"><span data-stu-id="9d851-134">LocalDB was created specifically for the development environment.</span></span> <span data-ttu-id="9d851-135">它不需要密碼，因此您不需要執行任何動作來防止機密資料被簽入您的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="9d851-135">It doesn't require a password, therefore you don't need to do anything to prevent secrets from being checked into your source code.</span></span> <span data-ttu-id="9d851-136">某些開發團隊使用 SQL Server （或其他 DBMS） 的完整的版本需要密碼。</span><span class="sxs-lookup"><span data-stu-id="9d851-136">Some development teams use the full versions of SQL Server (or other DBMS) that require a password.</span></span>

<span data-ttu-id="9d851-137">您可以使用`configSource`屬性以取代整個`<connectionStrings>`標記。</span><span class="sxs-lookup"><span data-stu-id="9d851-137">You can use the `configSource` attribute to replace the entire `<connectionStrings>` markup.</span></span> <span data-ttu-id="9d851-138">不同於`<appSettings>``file`屬性合併標記`configSource`屬性會取代標記。</span><span class="sxs-lookup"><span data-stu-id="9d851-138">Unlike the `<appSettings>` `file` attribute that merges the markup, the `configSource` attribute replaces the markup.</span></span> <span data-ttu-id="9d851-139">下列標記顯示`configSource`屬性*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="9d851-139">The following markup shows the `configSource` attribute in the *web.config* file:</span></span>

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> <span data-ttu-id="9d851-140">如果您使用`configSource`屬性如上所示，將您的連接字串移至外部檔案，而且具有 Visual Studio 建立新的網站，它無法偵測到您要使用資料庫，而且將不會取得資料庫的設定選項時您 pu從 Visual Studio azure blish。</span><span class="sxs-lookup"><span data-stu-id="9d851-140">If you use the `configSource` attribute as shown above to move your connection strings to an external file, and have Visual Studio create a new web site, it won't be able to detect you are using a database, and you won't get the option of configuring the database when you publish to Azure from Visual Studio.</span></span> <span data-ttu-id="9d851-141">如果您使用`configSource`屬性，您可以使用 PowerShell 建立和部署您的網站和資料庫，或者您可以建立網站和資料庫入口網站中發行之前。</span><span class="sxs-lookup"><span data-stu-id="9d851-141">If you are using the `configSource` attribute, you can use PowerShell to create and deploy your web site and database, or you can create the web site and the database in the portal before you publish.</span></span> <span data-ttu-id="9d851-142">[新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)指令碼會建立新的網站和資料庫。</span><span class="sxs-lookup"><span data-stu-id="9d851-142">The [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) script will create a new web site and database.</span></span>


> [!WARNING]
> <span data-ttu-id="9d851-143">安全性-不同於*AppSettingsSecrets.config*檔案中，外部連接字串檔案必須是相同的目錄做為根*web.config*檔案，所以您不必採取預防措施以確保您不要將它簽入您的來源儲存機制。</span><span class="sxs-lookup"><span data-stu-id="9d851-143">Security - Unlike the *AppSettingsSecrets.config* file, the external connection strings file must be in the same directory as the root *web.config* file, so you'll have to take precautions to ensure you don't check it into your source repository.</span></span>


> [!NOTE]
> <span data-ttu-id="9d851-144">**密碼檔案上的安全性警告：** 最佳作法是不要使用實際執行測試和開發中的機密資料。</span><span class="sxs-lookup"><span data-stu-id="9d851-144">**Security Warning on secrets file:** A best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="9d851-145">使用實際執行環境中測試或開發的密碼遺漏這些密碼。</span><span class="sxs-lookup"><span data-stu-id="9d851-145">Using production passwords in test or development leaks those secrets.</span></span>


<a id="wj"></a>
## <a name="webjobs-console-apps"></a><span data-ttu-id="9d851-146">WebJobs 主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="9d851-146">WebJobs console apps</span></span>

<span data-ttu-id="9d851-147">*App.config*主控台應用程式所使用的檔案不支援相對路徑，但支援絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="9d851-147">The *app.config* file used by a console app doesn't support relative paths, but it does support absolute paths.</span></span> <span data-ttu-id="9d851-148">若要將您的機密移出您的專案目錄，您可以使用絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="9d851-148">You can use an absolute path to move your secrets out of your project directory.</span></span> <span data-ttu-id="9d851-149">下列標記會顯示在 密碼*C:\secrets\AppSettingsSecrets.config*檔和中的非機密資料*app.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="9d851-149">The following markup shows the secrets in the *C:\secrets\AppSettingsSecrets.config* file, and non sensitive data in the *app.config* file.</span></span>

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a><span data-ttu-id="9d851-150">部署至 Azure 的機密資料</span><span class="sxs-lookup"><span data-stu-id="9d851-150">Deploying secrets to Azure</span></span>

<span data-ttu-id="9d851-151">當您將 web 應用程式部署至 Azure， *AppSettingsSecrets.config*檔案不會部署 （亦即您想要）。</span><span class="sxs-lookup"><span data-stu-id="9d851-151">When you deploy your web app to Azure, the *AppSettingsSecrets.config* file won't be deployed (that's what you want).</span></span> <span data-ttu-id="9d851-152">您可以移至[Azure 管理入口網站](https://azure.microsoft.com/services/management-portal/)並手動設定執行此作業：</span><span class="sxs-lookup"><span data-stu-id="9d851-152">You could go to the [Azure Management Portal](https://azure.microsoft.com/services/management-portal/) and set them manually, to do that:</span></span>

1. <span data-ttu-id="9d851-153">移至[ https://portal.azure.com ](https://portal.azure.com)，並以您的 Azure 認證登入。</span><span class="sxs-lookup"><span data-stu-id="9d851-153">Go to [https://portal.azure.com](https://portal.azure.com), and sign in with your Azure credentials.</span></span>
2. <span data-ttu-id="9d851-154">按一下**瀏覽&gt;Web 應用程式**，然後按一下 web 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="9d851-154">Click **Browse &gt; Web Apps**, then click the name of your web app.</span></span>
3. <span data-ttu-id="9d851-155">按一下**所有設定&gt;應用程式設定**。</span><span class="sxs-lookup"><span data-stu-id="9d851-155">Click **All settings &gt; Application settings**.</span></span>

<span data-ttu-id="9d851-156">**應用程式設定**和**連接字串**值會覆寫中的相同設定*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="9d851-156">The **app settings** and **connection string** values override the same settings in the *web.config* file.</span></span> <span data-ttu-id="9d851-157">在本例中，我們無法部署這些設定到 Azure，但如果這些機碼已在*web.config*檔案中，顯示在入口網站上的設定會優先。</span><span class="sxs-lookup"><span data-stu-id="9d851-157">In our example, we did not deploy these settings to Azure, but if these keys were in the *web.config* file, the settings shown on the portal would take precedence.</span></span>

<span data-ttu-id="9d851-158">最佳做法是遵循[DevOps 工作流程](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md)並用[Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (或另一個架構，例如[Chef](http://www.opscode.com/chef/)或[Puppet](http://puppetlabs.com/puppet/what-is-puppet)) 至在 Azure 中設定這些值會自動執行。</span><span class="sxs-lookup"><span data-stu-id="9d851-158">A best practice is to follow a [DevOps workflow](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) and use [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (or another framework such as [Chef](http://www.opscode.com/chef/) or [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) to automate setting these values in Azure.</span></span> <span data-ttu-id="9d851-159">下列 PowerShell 指令碼會使用[Export-clixml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk)加密機密資料匯出至磁碟：</span><span class="sxs-lookup"><span data-stu-id="9d851-159">The following PowerShell script uses [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) to export the encrypted secrets to disk:</span></span>

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

<span data-ttu-id="9d851-160">在上述指令碼，'Name' 是名稱之祕密金鑰，例如 '&quot;FB\_AppSecret&quot;或 「 TwitterSecret"。</span><span class="sxs-lookup"><span data-stu-id="9d851-160">In the script above, ‘Name' is the name of the secret key, such as ‘&quot;FB\_AppSecret&quot; or "TwitterSecret".</span></span> <span data-ttu-id="9d851-161">您可以檢視您的瀏覽器中的指令碼所建立之 「.credential"檔案。</span><span class="sxs-lookup"><span data-stu-id="9d851-161">You can view the ".credential" file created by the script in your browser.</span></span> <span data-ttu-id="9d851-162">以下程式碼片段會測試每個認證檔案，並將設定具名的 web 應用程式密碼：</span><span class="sxs-lookup"><span data-stu-id="9d851-162">The snippet below tests each of the credential files and sets the secrets for the named web app:</span></span>

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> <span data-ttu-id="9d851-163">安全性-不執行因此擊敗部署敏感性資料使用的 PowerShell 指令碼的目的在 PowerShell 指令碼中包含密碼或其他機密資料。</span><span class="sxs-lookup"><span data-stu-id="9d851-163">Security - Don't include passwords or other secrets in the PowerShell script, doing so defeats the purpose of using a PowerShell script to deploy sensitive data.</span></span> <span data-ttu-id="9d851-164">[Get-credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet 會提供用於取得密碼的安全機制。</span><span class="sxs-lookup"><span data-stu-id="9d851-164">The [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet provides a secure mechanism to obtain a password.</span></span> <span data-ttu-id="9d851-165">使用 UI 提示，可以防止洩漏密碼。</span><span class="sxs-lookup"><span data-stu-id="9d851-165">Using a UI prompt can prevent leaking a password.</span></span>


### <a name="deploying-db-connection-strings"></a><span data-ttu-id="9d851-166">部署資料庫的連接字串</span><span class="sxs-lookup"><span data-stu-id="9d851-166">Deploying DB connection strings</span></span>

<span data-ttu-id="9d851-167">同樣地處理 DB 連線字串的應用程式設定。</span><span class="sxs-lookup"><span data-stu-id="9d851-167">DB connection strings are handled similarly to app settings.</span></span> <span data-ttu-id="9d851-168">如果您部署您的 web 應用程式，從 Visual Studio，則會為您設定的連接字串。</span><span class="sxs-lookup"><span data-stu-id="9d851-168">If you deploy your web app from Visual Studio, the connection string will be configured for you.</span></span> <span data-ttu-id="9d851-169">您可以在入口網站來確認。</span><span class="sxs-lookup"><span data-stu-id="9d851-169">You can verify this in the portal.</span></span> <span data-ttu-id="9d851-170">若要設定的連接字串的建議的方式是使用 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="9d851-170">The recommended way to set the connection string is with PowerShell.</span></span> <span data-ttu-id="9d851-171">如需 PowerShell 指令碼的範例會建立網站和資料庫，並設定連接字串在網站上下載[新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)從[Azure 指令碼程式庫](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)。</span><span class="sxs-lookup"><span data-stu-id="9d851-171">For an example of a PowerShell script the creates a website and database and sets the connection string in the website, download [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) from the [Azure Script library](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).</span></span>

<a id="not"></a>
## <a name="notes-for-php"></a><span data-ttu-id="9d851-172">適用於 PHP 的附註</span><span class="sxs-lookup"><span data-stu-id="9d851-172">Notes for PHP</span></span>

<span data-ttu-id="9d851-173">因為兩個索引鍵 / 值組**應用程式設定**和**連接字串**儲存在環境變數在 Azure 應用程式服務，輕鬆地使用任何 web 應用程式架構 （例如 PHP) 可能的開發人員擷取這些值。</span><span class="sxs-lookup"><span data-stu-id="9d851-173">Since the key-value pairs for both **app settings** and **connection strings** are stored in environment variables on Azure App Service, developers that use any web app frameworks (such as PHP) can easily retrieve these values.</span></span> <span data-ttu-id="9d851-174">請參閱 Stefan Schackow [Windows Azure Web Sites： 如何應用程式字串與連接字串工作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)部落格文章會顯示 PHP 程式碼片段來讀取應用程式設定和連接字串。</span><span class="sxs-lookup"><span data-stu-id="9d851-174">See Stefan Schackow's [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) blog post which shows a PHP snippet to read app settings and connection strings.</span></span>

## <a name="notes-for-on-premises-servers"></a><span data-ttu-id="9d851-175">在內部部署伺服器資訊</span><span class="sxs-lookup"><span data-stu-id="9d851-175">Notes for on-premises servers</span></span>

<span data-ttu-id="9d851-176">如果您要部署至內部部署 web 伺服器，您可以協助安全密碼[加密組態檔的組態區段](https://msdn.microsoft.com/library/ff647398.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9d851-176">If you are deploying to on-premises web servers, you can help secure secrets by [encrypting the configuration sections of configuration files](https://msdn.microsoft.com/library/ff647398.aspx).</span></span> <span data-ttu-id="9d851-177">或者，您可以使用 Azure 網站的建議相同的方法： 在組態檔中保留開發設定，但使用的實際執行設定環境變數值。</span><span class="sxs-lookup"><span data-stu-id="9d851-177">As an alternative, you can use the same approach recommended for Azure Websites: keep development settings in configuration files, and use environment variable values for production settings.</span></span> <span data-ttu-id="9d851-178">在此情況下，不過，您必須撰寫應用程式程式碼會自動在 Azure 網站中的功能： 擷取環境變數設定和使用這些值來取代組態檔設定，或使用組態檔設定時找不到環境變數。</span><span class="sxs-lookup"><span data-stu-id="9d851-178">In this case, however, you have to write application code for functionality that is automatic in Azure Websites: retrieve settings from environment variables and use those values in place of configuration file settings, or use configuration file settings when environment variables are not found.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="9d851-179">其他資源</span><span class="sxs-lookup"><span data-stu-id="9d851-179">Additional Resources</span></span>

<span data-ttu-id="9d851-180">如需 PowerShell 的範例會建立 web 應用程式 + 資料庫的指令碼會設定連接字串 + 應用程式設定、 下載[新增 AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3)從[Azure 指令碼程式庫](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure)。</span><span class="sxs-lookup"><span data-stu-id="9d851-180">For an example of a PowerShell script that creates a web app + database, sets the connection string + app settings, download [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) from the [Azure Script library](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).</span></span> 

<span data-ttu-id="9d851-181">請參閱 Stefan Schackow [Windows Azure Web Sites： 應用程式字串與連接字串的工作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)</span><span class="sxs-lookup"><span data-stu-id="9d851-181">See Stefan Schackow's [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)</span></span>


<span data-ttu-id="9d851-182">特別感謝 Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) 和 Carlos Farre 檢閱。</span><span class="sxs-lookup"><span data-stu-id="9d851-182">Special thanks to Barry Dorrans ( [@blowdart](https://twitter.com/blowdart) ) and Carlos Farre for reviewing.</span></span>
