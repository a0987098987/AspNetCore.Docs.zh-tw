---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: "了解專案檔 |Microsoft 文件"
author: jrjlee
description: "Microsoft Build Engine (MSBuild) 專案檔之間的組建和部署程序的核心。 本主題一開始的 MSBuild 概觀..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 8d0f9604529db9cf4ee5d333450a551e46e6ba4f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-project-file"></a><span data-ttu-id="8574d-104">了解專案檔</span><span class="sxs-lookup"><span data-stu-id="8574d-104">Understanding the Project File</span></span>
====================
<span data-ttu-id="8574d-105">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="8574d-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="8574d-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="8574d-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="8574d-107">Microsoft Build Engine (MSBuild) 專案檔之間的組建和部署程序的核心。</span><span class="sxs-lookup"><span data-stu-id="8574d-107">Microsoft Build Engine (MSBuild) project files lie at the heart of the build and deployment process.</span></span> <span data-ttu-id="8574d-108">本主題一開始 MSBuild 和專案檔的概觀。</span><span class="sxs-lookup"><span data-stu-id="8574d-108">This topic starts with a conceptual overview of MSBuild and the project file.</span></span> <span data-ttu-id="8574d-109">它會描述當您使用專案檔和它的運作方式透過使用專案檔將真實世界應用程式部署的範例將會遇到的重要元件。</span><span class="sxs-lookup"><span data-stu-id="8574d-109">It describes the key components you'll come across when you work with project files, and it works through an example of how you can use project files to deploy real-world applications.</span></span>
> 
> <span data-ttu-id="8574d-110">您將學習：</span><span class="sxs-lookup"><span data-stu-id="8574d-110">What you'll learn:</span></span>
> 
> - <span data-ttu-id="8574d-111">如何 MSBuild 會使用 MSBuild 專案檔建置專案。</span><span class="sxs-lookup"><span data-stu-id="8574d-111">How MSBuild uses MSBuild project files to build projects.</span></span>
> - <span data-ttu-id="8574d-112">如何與網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 這類的部署技術整合 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="8574d-112">How MSBuild integrates with deployment technologies, like the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span>
> - <span data-ttu-id="8574d-113">如何了解專案檔案的重要元件。</span><span class="sxs-lookup"><span data-stu-id="8574d-113">How to understand the key components of a project file.</span></span>
> - <span data-ttu-id="8574d-114">如何使用專案檔建置並部署複雜的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8574d-114">How you can use project files to build and deploy complex applications.</span></span>


## <a name="msbuild-and-the-project-file"></a><span data-ttu-id="8574d-115">MSBuild 和專案檔</span><span class="sxs-lookup"><span data-stu-id="8574d-115">MSBuild and the Project File</span></span>

<span data-ttu-id="8574d-116">當您建立並建置 Visual Studio 中的方案時，Visual Studio 會使用 MSBuild 建置您的方案中每個專案。</span><span class="sxs-lookup"><span data-stu-id="8574d-116">When you create and build solutions in Visual Studio, Visual Studio uses MSBuild to build each project in your solution.</span></span> <span data-ttu-id="8574d-117">每個 Visual Studio 專案包含 MSBuild 專案檔，副檔名為檔案，以反映專案 & #x 2014年的類型; 例如，C# 專案 (.csproj)、 Visual Basic.NET 專案 (.vbproj) 或資料庫專案 (.dbproj)。</span><span class="sxs-lookup"><span data-stu-id="8574d-117">Every Visual Studio project includes an MSBuild project file, with a file extension that reflects the type of project&#x2014;for example, a C# project (.csproj), a Visual Basic.NET project (.vbproj), or a database project (.dbproj).</span></span> <span data-ttu-id="8574d-118">若要建置專案時，MSBuild 必須處理與專案相關的專案檔。</span><span class="sxs-lookup"><span data-stu-id="8574d-118">In order to build a project, MSBuild must process the project file associated with the project.</span></span> <span data-ttu-id="8574d-119">專案檔是 XML 文件，其中包含所有資訊和指示 MSBuild 能夠建置您的專案，例如平台需求、 版本設定資訊、 web 伺服器或資料庫伺服器設定，包括內容和必須執行的工作。</span><span class="sxs-lookup"><span data-stu-id="8574d-119">The project file is an XML document that contains all the information and instructions that MSBuild needs in order to build your project, like the content to include, the platform requirements, versioning information, web server or database server settings, and the tasks that must be performed.</span></span>

<span data-ttu-id="8574d-120">MSBuild 專案檔根據[MSBuild XML 結構描述](https://msdn.microsoft.com/en-us/library/5dy88c2e.aspx)，如此一來，在建置程序是完全開啟和透明和。</span><span class="sxs-lookup"><span data-stu-id="8574d-120">MSBuild project files are based on the [MSBuild XML schema](https://msdn.microsoft.com/en-us/library/5dy88c2e.aspx), and as a result the build process is entirely open and transparent.</span></span> <span data-ttu-id="8574d-121">此外，您不需要安裝 Visual Studio 時才能使用 MSBuild 引擎 & #x 2014年; MSBuild.exe 可執行檔是.NET framework 的一部分，而且您可以從命令提示字元中執行它。</span><span class="sxs-lookup"><span data-stu-id="8574d-121">In addition, you don't need to install Visual Studio in order to use the MSBuild engine&#x2014;the MSBuild.exe executable is part of the .NET Framework, and you can run it from a command prompt.</span></span> <span data-ttu-id="8574d-122">身為開發人員，您就可以建立您自己的 MSBuild 專案檔，使用 MSBuild XML 結構描述來強制執行精密且更細緻控制如何建立與部署您的專案。</span><span class="sxs-lookup"><span data-stu-id="8574d-122">As a developer, you can craft your own MSBuild project files, using the MSBuild XML schema, to impose sophisticated and fine-grained control over how your projects are built and deployed.</span></span> <span data-ttu-id="8574d-123">這些自訂專案檔案在 Visual Studio 會自動產生的專案檔案完全相同的方式。</span><span class="sxs-lookup"><span data-stu-id="8574d-123">These custom project files work in exactly the same way as the project files that Visual Studio generates automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="8574d-124">您也可以與 Team Build 服務 Team Foundation Server (TFS) 中使用 MSBuild 專案檔。</span><span class="sxs-lookup"><span data-stu-id="8574d-124">You can also use MSBuild project files with the Team Build service in Team Foundation Server (TFS).</span></span> <span data-ttu-id="8574d-125">例如，您可以使用持續整合 (CI) 案例中的專案檔來自動化部署至測試環境，新的程式碼簽入時。</span><span class="sxs-lookup"><span data-stu-id="8574d-125">For example, you can use project files in continuous integration (CI) scenarios to automate deployment to a test environment when new code is checked in.</span></span> <span data-ttu-id="8574d-126">如需詳細資訊，請參閱[用於自動化 Web 部署設定 Team Foundation Server](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="8574d-126">For more information, see [Configuring Team Foundation Server for Automated Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span>


### <a name="project-file-naming-conventions"></a><span data-ttu-id="8574d-127">專案檔案命名慣例</span><span class="sxs-lookup"><span data-stu-id="8574d-127">Project File Naming Conventions</span></span>

<span data-ttu-id="8574d-128">當您建立您自己的專案檔時，您可以使用任何您喜歡的副檔名。</span><span class="sxs-lookup"><span data-stu-id="8574d-128">When you create your own project files, you can use any file extension you like.</span></span> <span data-ttu-id="8574d-129">不過，若要簡化您的方案來了解其他人，您應該使用這些常用的慣例：</span><span class="sxs-lookup"><span data-stu-id="8574d-129">However, to make your solutions easier for others to understand, you should use these common conventions:</span></span>

- <span data-ttu-id="8574d-130">當您建立的專案檔建置專案時，請使用.proj 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="8574d-130">Use the .proj extension when you create a project file that builds projects.</span></span>
- <span data-ttu-id="8574d-131">當您建立可重複使用的專案檔案匯入到其他專案檔案，請使用.targets 延伸模組。</span><span class="sxs-lookup"><span data-stu-id="8574d-131">Use the .targets extension when you create a reusable project file to import into other project files.</span></span> <span data-ttu-id="8574d-132">副檔名為.targets 檔案通常沒有建置任何項目本身，它們只會包含您可以匯入.proj 檔案的指示。</span><span class="sxs-lookup"><span data-stu-id="8574d-132">Files with a .targets extension typically don't build anything themselves, they simply contain instructions that you can import into your .proj files.</span></span>

### <a name="integration-with-deployment-technologies"></a><span data-ttu-id="8574d-133">與部署技術的整合</span><span class="sxs-lookup"><span data-stu-id="8574d-133">Integration with Deployment Technologies</span></span>

<span data-ttu-id="8574d-134">如果您已使用 web 應用程式專案，在 Visual Studio 2010 中，例如 ASP.NET web 應用程式和 ASP.NET MVC web 應用程式，就會知道這些專案包含封裝和部署 web 應用程式目標環境的內建支援。</span><span class="sxs-lookup"><span data-stu-id="8574d-134">If you've worked with web application projects in Visual Studio 2010, like ASP.NET web applications and ASP.NET MVC web applications, you'll know that these projects include built-in support for packaging and deploying the web application to a target environment.</span></span> <span data-ttu-id="8574d-135">**屬性**這些專案包含頁**封裝/發行 Web**和**封裝/發行 SQL**索引標籤可讓您設定如何元件的程式封裝和部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="8574d-135">The **Properties** pages for these projects include **Package/Publish Web** and **Package/Publish SQL** tabs that you can use to configure how the components of your application are packaged and deployed.</span></span> <span data-ttu-id="8574d-136">這會顯示**封裝/發行 Web**  索引標籤：</span><span class="sxs-lookup"><span data-stu-id="8574d-136">This shows the **Package/Publish Web** tab:</span></span>

![](understanding-the-project-file/_static/image1.png)

<span data-ttu-id="8574d-137">這些功能的基礎技術，即做為 Web 發行管線 (WPP)。</span><span class="sxs-lookup"><span data-stu-id="8574d-137">The underlying technology behind these capabilities is known as the Web Publishing Pipeline (WPP).</span></span> <span data-ttu-id="8574d-138">WPP 基本上是將 MSBuild 和[Web Deploy](https://go.microsoft.com/?linkid=9805122)在一起以提供 web 應用程式的完整建置、 封裝和部署程序。</span><span class="sxs-lookup"><span data-stu-id="8574d-138">The WPP essentially brings MSBuild and [Web Deploy](https://go.microsoft.com/?linkid=9805122) together to provide a complete build, package, and deployment process for your web applications.</span></span>

<span data-ttu-id="8574d-139">好消息是，您可以利用 WPP 提供當您建立 web 專案的自訂專案檔案的整合點。</span><span class="sxs-lookup"><span data-stu-id="8574d-139">The good news is that you can take advantage of the integration points that the WPP provides when you create custom project files for web projects.</span></span> <span data-ttu-id="8574d-140">您可以在專案檔，可讓您建置專案、 建立 web 部署套件，以及在遠端伺服器上安裝這些套件，透過單一專案檔和 MSBuild 的單一呼叫中包含部署指示。</span><span class="sxs-lookup"><span data-stu-id="8574d-140">You can include deployment instructions in your project file, which allows you to build your projects, create web deployment packages, and install these packages on remote servers through a single project file and a single call to MSBuild.</span></span> <span data-ttu-id="8574d-141">您也可以呼叫任何其他可執行檔做為建置流程的一部分。</span><span class="sxs-lookup"><span data-stu-id="8574d-141">You can also call any other executables as part of your build process.</span></span> <span data-ttu-id="8574d-142">例如，您可以執行 VSDBCMD.exe 命令列工具來部署結構描述檔案的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8574d-142">For example, you can run the VSDBCMD.exe command-line tool to deploy a database from a schema file.</span></span> <span data-ttu-id="8574d-143">在本主題的過程中，您會看到如何使用這些功能，以符合您的企業部署案例的需求的利用。</span><span class="sxs-lookup"><span data-stu-id="8574d-143">Over the course of this topic, you'll see how you can take advantage of these capabilities to meet the requirements of your enterprise deployment scenarios.</span></span>

> [!NOTE]
> <span data-ttu-id="8574d-144">如需 web 應用程式部署程序的運作方式的詳細資訊，請參閱[ASP.NET Web 應用程式專案部署概觀](https://msdn.microsoft.com/en-us/library/dd394698.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-144">For more information on how the web application deployment process works, see [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/en-us/library/dd394698.aspx).</span></span>


## <a name="the-anatomy-of-a-project-file"></a><span data-ttu-id="8574d-145">專案檔的結構</span><span class="sxs-lookup"><span data-stu-id="8574d-145">The Anatomy of a Project File</span></span>

<span data-ttu-id="8574d-146">您可以在建置程序，更詳細地查看之前，它是值得花幾分鐘的時間讓自己熟悉如何使用 MSBuild 專案檔的基本結構。</span><span class="sxs-lookup"><span data-stu-id="8574d-146">Before you look at the build process in more detail, it's worth taking a few moments to familiarize yourself with the basic structure of an MSBuild project file.</span></span> <span data-ttu-id="8574d-147">本節提供當您檢視、 編輯或建立的專案檔時，就會發生常見元素的概觀。</span><span class="sxs-lookup"><span data-stu-id="8574d-147">This section provides an overview of the more common elements that you'll encounter when you review, edit, or create a project file.</span></span> <span data-ttu-id="8574d-148">特別是，您將學習：</span><span class="sxs-lookup"><span data-stu-id="8574d-148">In particular, you'll learn:</span></span>

- <span data-ttu-id="8574d-149">如何使用*屬性*管理建置程序的變數。</span><span class="sxs-lookup"><span data-stu-id="8574d-149">How to use *properties* to manage variables for the build process.</span></span>
- <span data-ttu-id="8574d-150">如何使用*項目*以識別要在建置程序的輸入，例如 程式碼檔案。</span><span class="sxs-lookup"><span data-stu-id="8574d-150">How to use *items* to identify the inputs to the build process, like code files.</span></span>
- <span data-ttu-id="8574d-151">如何使用*目標*和*工作*來執行指示 MSBuild，使用*屬性*和*項目*中其他位置定義專案檔。</span><span class="sxs-lookup"><span data-stu-id="8574d-151">How to use *targets* and *tasks* to provide execution instructions to MSBuild, using *properties* and *items* defined elsewhere in the project file.</span></span>

<span data-ttu-id="8574d-152">這會顯示在 MSBuild 專案檔中索引鍵的項目之間的關聯性：</span><span class="sxs-lookup"><span data-stu-id="8574d-152">This shows the relationship between the key elements in an MSBuild project file:</span></span>

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a><span data-ttu-id="8574d-153">專案項目</span><span class="sxs-lookup"><span data-stu-id="8574d-153">The Project Element</span></span>

<span data-ttu-id="8574d-154">[專案](https://msdn.microsoft.com/en-us/library/bcxfsh87.aspx)元素是每個專案檔的根項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-154">The [Project](https://msdn.microsoft.com/en-us/library/bcxfsh87.aspx) element is the root element of every project file.</span></span> <span data-ttu-id="8574d-155">除了識別專案檔的 XML 結構描述**專案**元素可以包含屬性，指定在建置程序的進入點。</span><span class="sxs-lookup"><span data-stu-id="8574d-155">In addition to identifying the XML schema for the project file, the **Project** element can include attributes to specify the entry points for the build process.</span></span> <span data-ttu-id="8574d-156">例如，在[連絡人管理員範例方案](the-contact-manager-solution.md)、 *Publish.proj*檔案會指定開始組建時，應該藉由呼叫名為目標**FullPublish**。</span><span class="sxs-lookup"><span data-stu-id="8574d-156">For example, in the [Contact Manager sample solution](the-contact-manager-solution.md), the *Publish.proj* file specifies that the build should start by calling the target named **FullPublish**.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a><span data-ttu-id="8574d-157">屬性和條件</span><span class="sxs-lookup"><span data-stu-id="8574d-157">Properties and Conditions</span></span>

<span data-ttu-id="8574d-158">專案檔通常需要提供大量項資訊才能成功建立及部署您的專案。</span><span class="sxs-lookup"><span data-stu-id="8574d-158">A project file typically needs to provide lots of different pieces of information in order to successfully build and deploy your projects.</span></span> <span data-ttu-id="8574d-159">這項資訊可能包括伺服器名稱、 連接字串、 認證、 組建組態、 來源和目的地檔案路徑和任何其他您想要包含為支援自訂的資訊。</span><span class="sxs-lookup"><span data-stu-id="8574d-159">These pieces of information could include server names, connection strings, credentials, build configurations, source and destination file paths, and any other information you want to include to support customization.</span></span> <span data-ttu-id="8574d-160">在專案檔中，屬性必須在定義內[PropertyGroup](https://msdn.microsoft.com/en-us/library/t4w159bs.aspx)項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-160">In a project file, properties must be defined within a [PropertyGroup](https://msdn.microsoft.com/en-us/library/t4w159bs.aspx) element.</span></span> <span data-ttu-id="8574d-161">MSBuild 屬性是由索引鍵-值配對所組成。</span><span class="sxs-lookup"><span data-stu-id="8574d-161">MSBuild properties consist of key-value pairs.</span></span> <span data-ttu-id="8574d-162">內**PropertyGroup**項目，項目名稱會定義屬性索引鍵和項目的內容定義的屬性值。</span><span class="sxs-lookup"><span data-stu-id="8574d-162">Within the **PropertyGroup** element, the element name defines the property key and the content of the element defines the property value.</span></span> <span data-ttu-id="8574d-163">例如，您可以定義名為屬性**ServerName**和**ConnectionString**來儲存靜態伺服器名稱和連接字串。</span><span class="sxs-lookup"><span data-stu-id="8574d-163">For example, you could define properties named **ServerName** and **ConnectionString** to store a static server name and connection string.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


<span data-ttu-id="8574d-164">若要擷取屬性值，您可以使用格式**$(***PropertyName***)***。*例如，若要擷取的值**ServerName**屬性，您會輸入：</span><span class="sxs-lookup"><span data-stu-id="8574d-164">To retrieve a property value, you use the format **$(***PropertyName***)***.*For example, to retrieve the value of the **ServerName** property, you would type:</span></span>


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> <span data-ttu-id="8574d-165">您會看到的範例，以及何時使用本主題稍後的屬性值。</span><span class="sxs-lookup"><span data-stu-id="8574d-165">You'll see examples of how and when to use property values later in this topic.</span></span>


<span data-ttu-id="8574d-166">在專案檔中內嵌為靜態屬性的資訊不是理想的方法來管理建置程序。</span><span class="sxs-lookup"><span data-stu-id="8574d-166">Embedding information as static properties in a project file is not always the ideal approach to managing the build process.</span></span> <span data-ttu-id="8574d-167">在許多案例中，您會想要從其他來源取得資訊或讓使用者能夠提供命令提示字元中的資訊。</span><span class="sxs-lookup"><span data-stu-id="8574d-167">In a lot of scenarios, you'll want to obtain the information from other sources or empower the user to provide the information from the command prompt.</span></span> <span data-ttu-id="8574d-168">MSBuild 可讓您指定為命令列參數的任何屬性值。</span><span class="sxs-lookup"><span data-stu-id="8574d-168">MSBuild allows you to specify any property value as a command-line parameter.</span></span> <span data-ttu-id="8574d-169">例如，使用者無法提供值給**ServerName**他或她執行時 MSBuild.exe 從命令列。</span><span class="sxs-lookup"><span data-stu-id="8574d-169">For example, the user could provide a value for **ServerName** when he or she runs MSBuild.exe from the command line.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> <span data-ttu-id="8574d-170">如需有關引數和參數，您可以使用 MSBuild.exe 的詳細資訊，請參閱[MSBuild 命令列參考](https://msdn.microsoft.com/en-us/library/ms164311.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-170">For more information on the arguments and switches you can use with MSBuild.exe, see [MSBuild Command Line Reference](https://msdn.microsoft.com/en-us/library/ms164311.aspx).</span></span>


<span data-ttu-id="8574d-171">您可以使用相同的屬性語法，以取得環境變數和內建的專案屬性的值。</span><span class="sxs-lookup"><span data-stu-id="8574d-171">You can use the same property syntax to obtain the values of environment variables and built-in project properties.</span></span> <span data-ttu-id="8574d-172">針對您所定義的許多常用的屬性，您可以使用這些專案檔中包括相關的參數名稱。</span><span class="sxs-lookup"><span data-stu-id="8574d-172">Lots of commonly used properties are defined for you, and you can use them in your project files by including the relevant parameter name.</span></span> <span data-ttu-id="8574d-173">例如，若要擷取目前的專案平台 & #x 2014; 例如， **x86**或**AnyCpu**（& s) #x 2014; 可以包含**$(Platform)**中的屬性參考您的專案檔。</span><span class="sxs-lookup"><span data-stu-id="8574d-173">For example, to retrieve the current project platform&#x2014;for example, **x86** or **AnyCpu**&#x2014;you can include the **$(Platform)** property reference in your project file.</span></span> <span data-ttu-id="8574d-174">如需詳細資訊，請參閱[建置命令和屬性的巨集](https://msdn.microsoft.com/en-us/library/c02as0cs.aspx)，[一般 MSBuild 專案屬性](https://msdn.microsoft.com/en-us/library/bb629394.aspx)，和[保留的屬性](https://msdn.microsoft.com/en-us/library/ms164309.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-174">For more information, see [Macros for Build Commands and Properties](https://msdn.microsoft.com/en-us/library/c02as0cs.aspx), [Common MSBuild Project Properties](https://msdn.microsoft.com/en-us/library/bb629394.aspx), and [Reserved Properties](https://msdn.microsoft.com/en-us/library/ms164309.aspx).</span></span>

<span data-ttu-id="8574d-175">屬性通常用於搭配*條件*。</span><span class="sxs-lookup"><span data-stu-id="8574d-175">Properties are often used in conjunction with *conditions*.</span></span> <span data-ttu-id="8574d-176">大部分的 MSBuild 項目支援**條件**屬性，可讓您指定的準則的 MSBuild 應評估項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-176">Most MSBuild elements support the **Condition** attribute, which lets you specify the criteria upon which MSBuild should evaluate the element.</span></span> <span data-ttu-id="8574d-177">例如，請考慮此屬性定義：</span><span class="sxs-lookup"><span data-stu-id="8574d-177">For example, consider this property definition:</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


<span data-ttu-id="8574d-178">當 MSBuild 處理此屬性定義時，它會先檢查以查看是否**$(OutputRoot)**就可使用屬性值。</span><span class="sxs-lookup"><span data-stu-id="8574d-178">When MSBuild processes this property definition, it first checks to see whether an **$(OutputRoot)** property value is available.</span></span> <span data-ttu-id="8574d-179">如果屬性值為空白 & #x 2014; 也就是說，使用者未提供值的這個屬性 & #x 2014年; 條件評估為**true**和屬性值設定為**...\Publish\Out**。如果使用者已經提供值，這個屬性，條件評估為**false**並不會使用靜態屬性值。</span><span class="sxs-lookup"><span data-stu-id="8574d-179">If the property value is blank&#x2014;in other words, the user hasn't provided a value for this property&#x2014;the condition evaluates to **true** and the property value is set to **..\Publish\Out**. If the user has provided a value for this property, the condition evaluates to **false** and the static property value is not used.</span></span>

<span data-ttu-id="8574d-180">如需您可在指定條件的不同方式的詳細資訊，請參閱[MSBuild 條件](https://msdn.microsoft.com/en-us/library/7szfhaft.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-180">For more information on the different ways in which you can specify conditions, see [MSBuild Conditions](https://msdn.microsoft.com/en-us/library/7szfhaft.aspx).</span></span>

### <a name="items-and-item-groups"></a><span data-ttu-id="8574d-181">項目和項目群組</span><span class="sxs-lookup"><span data-stu-id="8574d-181">Items and Item Groups</span></span>

<span data-ttu-id="8574d-182">其中一個重要的角色專案檔是定義建置流程的輸入。</span><span class="sxs-lookup"><span data-stu-id="8574d-182">One of the important roles of the project file is to define the inputs to the build process.</span></span> <span data-ttu-id="8574d-183">一般來說，這些輸入是檔案 & #x 2014年; 程式碼檔案、 組態檔、 指令檔和任何其他您需要處理，或做為複製的檔案組件在建置程序。</span><span class="sxs-lookup"><span data-stu-id="8574d-183">Typically, these inputs are files&#x2014;code files, configuration files, command files, and any other files that you need to process or copy as part of the build process.</span></span> <span data-ttu-id="8574d-184">在 MSBuild 專案結構描述，以表示這些輸入[項目](https://msdn.microsoft.com/en-us/library/ms164283.aspx)項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-184">In the MSBuild project schema, these inputs are represented by [Item](https://msdn.microsoft.com/en-us/library/ms164283.aspx) elements.</span></span> <span data-ttu-id="8574d-185">在專案檔中，項目必須在定義內[ItemGroup](https://msdn.microsoft.com/en-us/library/646dk05y.aspx)項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-185">In a project file, items must be defined within an [ItemGroup](https://msdn.microsoft.com/en-us/library/646dk05y.aspx) element.</span></span> <span data-ttu-id="8574d-186">就像**屬性**項目，您可以命名**項目**元素不過嗎。</span><span class="sxs-lookup"><span data-stu-id="8574d-186">Just like **Property** elements, you can name an **Item** element however you like.</span></span> <span data-ttu-id="8574d-187">不過，您必須指定**Include**屬性識別的檔案或項目所表示的萬用字元。</span><span class="sxs-lookup"><span data-stu-id="8574d-187">However, you must specify an **Include** attribute to identify the file or wildcard that the item represents.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


<span data-ttu-id="8574d-188">藉由指定多個**項目**具有相同名稱的項目，您要有效地建立具名的資源的清單。</span><span class="sxs-lookup"><span data-stu-id="8574d-188">By specifying multiple **Item** elements with the same name, you're effectively creating a named list of resources.</span></span> <span data-ttu-id="8574d-189">若要查看此動作中的好方法是來看看其中一個 Visual Studio 建立的專案檔案內。</span><span class="sxs-lookup"><span data-stu-id="8574d-189">A good way to see this in action is to take a look inside one of the project files that Visual Studio creates.</span></span> <span data-ttu-id="8574d-190">例如， *ContactManager.Mvc.csproj*範例方案中的檔案包含大量項目群組，每個都有數個相同的已命名**項目**項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-190">For example, the *ContactManager.Mvc.csproj* file in the sample solution includes a lot of item groups, each with several identically named **Item** elements.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


<span data-ttu-id="8574d-191">如此一來，專案檔會指示 MSBuild 建構來處理相同的方法 & #x 2014; 需要的檔案清單**參考**清單含有組件必須先準備就緒，成功的組建， **編譯**清單包含必須編譯的程式碼檔案和**內容**清單包含必須複製的資源不變。</span><span class="sxs-lookup"><span data-stu-id="8574d-191">In this way, the project file is instructing MSBuild to construct lists of files that need to be processed in the same way&#x2014;the **Reference** list includes assemblies that must be in place for a successful build, the **Compile** list includes code files that must be compiled, and the **Content** list includes resources that must be copied unaltered.</span></span> <span data-ttu-id="8574d-192">我們會探討如何建置程序參考，但在本主題稍後會使用這些項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-192">We'll look at how the build process references and uses these items later in this topic.</span></span>

<span data-ttu-id="8574d-193">也可以包含項目[ItemMetadata](https://msdn.microsoft.com/en-us/library/ms164284.aspx)子項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-193">Item elements can also include [ItemMetadata](https://msdn.microsoft.com/en-us/library/ms164284.aspx) child elements.</span></span> <span data-ttu-id="8574d-194">這些是使用者定義的索引鍵-值組，基本上表示該項目特有的屬性。</span><span class="sxs-lookup"><span data-stu-id="8574d-194">These are user-defined key-value pairs and essentially represent properties that are specific to that item.</span></span> <span data-ttu-id="8574d-195">例如，許多**編譯**專案檔中的項目包含**DependentUpon**子項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-195">For example, a lot of the **Compile** item elements in the project file include **DependentUpon** child elements.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> <span data-ttu-id="8574d-196">除了使用者建立的項目中繼資料，所有的項目指派上建立各種常見的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="8574d-196">In addition to user-created item metadata, all items are assigned various common metadata on creation.</span></span> <span data-ttu-id="8574d-197">如需詳細資訊，請參閱[已知的項目中繼資料](https://msdn.microsoft.com/en-us/library/ms164313.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-197">For more information, see [Well-known Item Metadata](https://msdn.microsoft.com/en-us/library/ms164313.aspx).</span></span>


<span data-ttu-id="8574d-198">您可以建立**ItemGroup**根層級中的項目**專案**項目或在特定**目標**項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-198">You can create **ItemGroup** elements within the root-level **Project** element or within specific **Target** elements.</span></span> <span data-ttu-id="8574d-199">**ItemGroup**項目也支援**條件**屬性，可讓您自訂的建置程序，根據平台專案組態等條件來輸入。</span><span class="sxs-lookup"><span data-stu-id="8574d-199">**ItemGroup** elements also support **Condition** attributes, which lets you tailor the inputs to the build process according to conditions like the project configuration or platform.</span></span>

### <a name="targets-and-tasks"></a><span data-ttu-id="8574d-200">目標和工作</span><span class="sxs-lookup"><span data-stu-id="8574d-200">Targets and Tasks</span></span>

<span data-ttu-id="8574d-201">MSBuild 結構描述中[工作](https://msdn.microsoft.com/en-us/library/77f2hx1s.aspx)元素代表個別的組建指令 （或工作）。</span><span class="sxs-lookup"><span data-stu-id="8574d-201">In the MSBuild schema, a [Task](https://msdn.microsoft.com/en-us/library/77f2hx1s.aspx) element represents an individual build instruction (or task).</span></span> <span data-ttu-id="8574d-202">MSBuild 會包含許多預先定義的工作。</span><span class="sxs-lookup"><span data-stu-id="8574d-202">MSBuild includes a multitude of predefined tasks.</span></span> <span data-ttu-id="8574d-203">例如: </span><span class="sxs-lookup"><span data-stu-id="8574d-203">For example:</span></span>

- <span data-ttu-id="8574d-204">**複製**工作將檔案複製到新位置。</span><span class="sxs-lookup"><span data-stu-id="8574d-204">The **Copy** task copies files to a new location.</span></span>
- <span data-ttu-id="8574d-205">**Csc**工作叫用 Visual C# 編譯器。</span><span class="sxs-lookup"><span data-stu-id="8574d-205">The **Csc** task invokes the Visual C# compiler.</span></span>
- <span data-ttu-id="8574d-206">**Vbc**工作叫用 Visual Basic 編譯器。</span><span class="sxs-lookup"><span data-stu-id="8574d-206">The **Vbc** task invokes the Visual Basic compiler.</span></span>
- <span data-ttu-id="8574d-207">**Exec**工作會執行指定的程式。</span><span class="sxs-lookup"><span data-stu-id="8574d-207">The **Exec** task runs a specified program.</span></span>
- <span data-ttu-id="8574d-208">**訊息**工作會將訊息寫入記錄器。</span><span class="sxs-lookup"><span data-stu-id="8574d-208">The **Message** task writes a message to a logger.</span></span>

> [!NOTE]
> <span data-ttu-id="8574d-209">完整的現成可用的工作的詳細資訊，請參閱[MSBuild 工作參考](https://msdn.microsoft.com/en-us/library/7z253716.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-209">For full details of the tasks that are available out of the box, see [MSBuild Task Reference](https://msdn.microsoft.com/en-us/library/7z253716.aspx).</span></span> <span data-ttu-id="8574d-210">如需有關工作，包括如何建立您自己自訂的工作，請參閱[MSBuild 工作](https://msdn.microsoft.com/en-us/library/ms171466.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-210">For more information on tasks, including how to create your own custom tasks, see [MSBuild Tasks](https://msdn.microsoft.com/en-us/library/ms171466.aspx).</span></span>


<span data-ttu-id="8574d-211">工作必須一律包含在[目標](https://msdn.microsoft.com/en-us/library/t50z2hka.aspx)項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-211">Tasks must always be contained within [Target](https://msdn.microsoft.com/en-us/library/t50z2hka.aspx) elements.</span></span> <span data-ttu-id="8574d-212">A**目標**項目是一組一個或多個工作，會循序執行，且專案檔包含多個目標。</span><span class="sxs-lookup"><span data-stu-id="8574d-212">A **Target** element is a set of one or more tasks that are executed sequentially, and a project file can contain multiple targets.</span></span> <span data-ttu-id="8574d-213">您想要執行的工作或一組工作，當您叫用包含它們的目標。</span><span class="sxs-lookup"><span data-stu-id="8574d-213">When you want to run a task, or a set of tasks, you invoke the target that contains them.</span></span> <span data-ttu-id="8574d-214">例如，假設您有簡單的專案檔，記錄的訊息。</span><span class="sxs-lookup"><span data-stu-id="8574d-214">For example, suppose you have a simple project file that logs a message.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


<span data-ttu-id="8574d-215">您可以使用叫用命令列中，從目標**/t**切換到指定的目標。</span><span class="sxs-lookup"><span data-stu-id="8574d-215">You can invoke the target from the command line, by using the **/t** switch to specify the target.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


<span data-ttu-id="8574d-216">或者，您可以新增**DefaultTargets**屬性**專案**項目，來指定您想要叫用的目標。</span><span class="sxs-lookup"><span data-stu-id="8574d-216">Alternatively, you can add a **DefaultTargets** attribute to the **Project** element, to specify the targets that you want to invoke.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


<span data-ttu-id="8574d-217">在此情況下，您不需要指定命令列從目標。</span><span class="sxs-lookup"><span data-stu-id="8574d-217">In this case, you don't need to specify the target from the command line.</span></span> <span data-ttu-id="8574d-218">您可以僅指定專案檔和 MSBuild 將會叫用**FullPublish**為您的目標。</span><span class="sxs-lookup"><span data-stu-id="8574d-218">You can simply specify the project file, and MSBuild will invoke the **FullPublish** target for you.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


<span data-ttu-id="8574d-219">目標和工作可以包含**條件**屬性。</span><span class="sxs-lookup"><span data-stu-id="8574d-219">Both targets and tasks can include **Condition** attributes.</span></span> <span data-ttu-id="8574d-220">因此，您可以選擇略過整個目標或個別的工作必須符合特定條件。</span><span class="sxs-lookup"><span data-stu-id="8574d-220">As such, you can choose to omit entire targets or individual tasks if certain conditions are met.</span></span>

<span data-ttu-id="8574d-221">一般而言，當您建立有用的工作和目標，您將需要的屬性和項目已在其他位置中定義的專案檔，請參閱：</span><span class="sxs-lookup"><span data-stu-id="8574d-221">Generally speaking, when you create useful tasks and targets, you'll need to refer to the properties and items that you've defined elsewhere in the project file:</span></span>

- <span data-ttu-id="8574d-222">若要使用的屬性值，輸入**$(***PropertyName***)**，其中*PropertyName*名稱**屬性**項目或參數的名稱。</span><span class="sxs-lookup"><span data-stu-id="8574d-222">To use a property value, type **$(***PropertyName***)**, where *PropertyName* is the name of the **Property** element or the name of the parameter.</span></span>
- <span data-ttu-id="8574d-223">若要使用的項目，請輸入**@(***ItemName***)**，其中*ItemName*名稱**項目**項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-223">To use an item, type **@(***ItemName***)**, where *ItemName* is the name of the **Item** element.</span></span>

> [!NOTE]
> <span data-ttu-id="8574d-224">請記住您建立多個項目具有相同名稱時，是否您要建置清單。</span><span class="sxs-lookup"><span data-stu-id="8574d-224">Remember that if you create multiple items with the same name, you're building a list.</span></span> <span data-ttu-id="8574d-225">相反地，如果您建立多個屬性具有相同名稱時，您提供的最後一個屬性值將會覆寫任何先前的屬性相同的名稱 & #x 2014年; 屬性只能包含單一值。</span><span class="sxs-lookup"><span data-stu-id="8574d-225">In contrast, if you create multiple properties with the same name, the last property value you provide will overwrite any previous properties with the same name&#x2014;a property can only contain a single value.</span></span>


<span data-ttu-id="8574d-226">例如，在*Publish.proj*檔案範例方案中，看看**BuildProjects**目標。</span><span class="sxs-lookup"><span data-stu-id="8574d-226">For example, in the *Publish.proj* file in the sample solution, take a look at the **BuildProjects** target.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


<span data-ttu-id="8574d-227">在此範例中，您可以觀察這些關鍵點：</span><span class="sxs-lookup"><span data-stu-id="8574d-227">In this sample, you can observe these key points:</span></span>

- <span data-ttu-id="8574d-228">如果**BuildingInTeamBuild**參數指定，且值為**true**，這個目標內的工作將會執行。</span><span class="sxs-lookup"><span data-stu-id="8574d-228">If the **BuildingInTeamBuild** parameter is specified and has a value of **true**, none of the tasks within this target will be executed.</span></span>
- <span data-ttu-id="8574d-229">目標中包含的單一執行個體[MSBuild](https://msdn.microsoft.com/en-us/library/z7f65y0d.aspx)工作。</span><span class="sxs-lookup"><span data-stu-id="8574d-229">The target contains a single instance of the [MSBuild](https://msdn.microsoft.com/en-us/library/z7f65y0d.aspx) task.</span></span> <span data-ttu-id="8574d-230">這項工作可讓您建立其他的 MSBuild 專案。</span><span class="sxs-lookup"><span data-stu-id="8574d-230">This task lets you build other MSBuild projects.</span></span>
- <span data-ttu-id="8574d-231">**ProjectsToBuild**項目傳遞給工作。</span><span class="sxs-lookup"><span data-stu-id="8574d-231">The **ProjectsToBuild** item is passed to the task.</span></span> <span data-ttu-id="8574d-232">此項目可能代表的專案或方案檔，它是由所有定義清單**ProjectsToBuild**項目項目群組中的項目。</span><span class="sxs-lookup"><span data-stu-id="8574d-232">This item could represent a list of project or solution files, all defined by **ProjectsToBuild** item elements within an item group.</span></span> <span data-ttu-id="8574d-233">在此情況下， **ProjectsToBuild**項目會參考單一方案檔案。</span><span class="sxs-lookup"><span data-stu-id="8574d-233">In this case, the **ProjectsToBuild** item refers to a single solution file.</span></span>

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- <span data-ttu-id="8574d-234">屬性值傳遞至**MSBuild**工作包含名為參數**OutputRoot**和**組態**。</span><span class="sxs-lookup"><span data-stu-id="8574d-234">The property values passed to the **MSBuild** task include parameters named **OutputRoot** and **Configuration**.</span></span> <span data-ttu-id="8574d-235">如果它們不是，這些都會設為參數值，如果提供或靜態屬性值。</span><span class="sxs-lookup"><span data-stu-id="8574d-235">These are set to parameter values if they are provided, or static property values if they are not.</span></span>

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

<span data-ttu-id="8574d-236">您也可以看到**MSBuild**工作叫用名為目標**建置**。</span><span class="sxs-lookup"><span data-stu-id="8574d-236">You can also see that the **MSBuild** task invokes a target named **Build**.</span></span> <span data-ttu-id="8574d-237">這是其中一個廣泛使用在 Visual Studio 專案檔，您可以在自訂專案檔案中，使用類似的數個內建目標**建置**，**清除**，**重建**，和**發行**。</span><span class="sxs-lookup"><span data-stu-id="8574d-237">This is one of several built-in targets that are widely used in Visual Studio project files and are available to you in your custom project files, like **Build**, **Clean**, **Rebuild**, and **Publish**.</span></span> <span data-ttu-id="8574d-238">您將進一步了解使用目標和工作，以控制建置流程中，以及有關**MSBuild**工作特別的是，本主題稍後的。</span><span class="sxs-lookup"><span data-stu-id="8574d-238">You'll learn more about using targets and tasks to control the build process, and about the **MSBuild** task in particular, later in this topic.</span></span>

> [!NOTE]
> <span data-ttu-id="8574d-239">如需目標的詳細資訊，請參閱[MSBuild 目標](https://msdn.microsoft.com/en-us/library/ms171462.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8574d-239">For more information on targets, see [MSBuild Targets](https://msdn.microsoft.com/en-us/library/ms171462.aspx).</span></span>


## <a name="splitting-project-files-to-support-multiple-environments"></a><span data-ttu-id="8574d-240">分割的專案檔，以支援多個環境</span><span class="sxs-lookup"><span data-stu-id="8574d-240">Splitting Project Files to Support Multiple Environments</span></span>

<span data-ttu-id="8574d-241">假設您想要能夠將方案部署到多個環境，例如測試伺服器、 暫存的平台和實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="8574d-241">Suppose you want to be able to deploy a solution to multiple environments, like test servers, staging platforms, and production environments.</span></span> <span data-ttu-id="8574d-242">組態可能因本質上這些環境 & #x 2014年; 不只根據伺服器名稱、 連接字串等等，但也可能根據認證、 安全性設定，以及許多其他因素。</span><span class="sxs-lookup"><span data-stu-id="8574d-242">The configuration may vary substantially between these environments&#x2014;not just in terms of server names, connection strings, and so on, but also potentially in terms of credentials, security settings, and lots of other factors.</span></span> <span data-ttu-id="8574d-243">如果您需要定期執行這項操作，並不真的有利編輯專案檔中的多個屬性，每次切換的目標環境。</span><span class="sxs-lookup"><span data-stu-id="8574d-243">If you need to do this regularly, it's not really expedient to edit multiple properties in your project file every time you switch the target environment.</span></span> <span data-ttu-id="8574d-244">也不是理想的解決方案需要的屬性值，以提供給建置處理序無止盡的清單。</span><span class="sxs-lookup"><span data-stu-id="8574d-244">Nor is it an ideal solution to require an endless list of property values to be provided to the build process.</span></span>

<span data-ttu-id="8574d-245">所幸還有另一個方法。</span><span class="sxs-lookup"><span data-stu-id="8574d-245">Fortunately there is an alternative.</span></span> <span data-ttu-id="8574d-246">MSBuild 可讓您跨多個專案檔案分割您的組建組態。</span><span class="sxs-lookup"><span data-stu-id="8574d-246">MSBuild lets you split your build configuration across multiple project files.</span></span> <span data-ttu-id="8574d-247">若要查看其運作方式，在範例方案，請注意，有兩個自訂專案檔案：</span><span class="sxs-lookup"><span data-stu-id="8574d-247">To see how this works, in the sample solution, notice that there are two custom project files:</span></span>

- <span data-ttu-id="8574d-248">*Publish.proj*，其中包含內容，項目，與目標的通用於所有環境。</span><span class="sxs-lookup"><span data-stu-id="8574d-248">*Publish.proj*, which contains properties, items, and targets that are common to all environments.</span></span>
- <span data-ttu-id="8574d-249">*Env Dev.proj*，其中包含開發人員環境特有的屬性。</span><span class="sxs-lookup"><span data-stu-id="8574d-249">*Env-Dev.proj*, which contains properties that are specific to a developer environment.</span></span>

<span data-ttu-id="8574d-250">現在請注意， *Publish.proj*檔案包含[匯入](https://msdn.microsoft.com/en-us/library/92x05xfs.aspx)項目，下方的 開啟**專案**標記。</span><span class="sxs-lookup"><span data-stu-id="8574d-250">Now notice that the *Publish.proj* file includes an [Import](https://msdn.microsoft.com/en-us/library/92x05xfs.aspx) element, immediately beneath the opening **Project** tag.</span></span>


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


<span data-ttu-id="8574d-251">**匯入**元素用來匯入到目前的 MSBuild 專案檔的另一個的 MSBuild 專案檔的內容。</span><span class="sxs-lookup"><span data-stu-id="8574d-251">The **Import** element is used to import the contents of another MSBuild project file into the current MSBuild project file.</span></span> <span data-ttu-id="8574d-252">在此情況下， **TargetEnvPropsFile**參數可讓您想要匯入專案檔的檔名。</span><span class="sxs-lookup"><span data-stu-id="8574d-252">In this case, the **TargetEnvPropsFile** parameter provides the filename of the project file you want to import.</span></span> <span data-ttu-id="8574d-253">當您執行 MSBuild 時，您可以為此參數提供值。</span><span class="sxs-lookup"><span data-stu-id="8574d-253">You can provide a value for this parameter when you run MSBuild.</span></span>


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


<span data-ttu-id="8574d-254">兩個檔案的內容有效地合併至單一專案檔案中。</span><span class="sxs-lookup"><span data-stu-id="8574d-254">This effectively merges the contents of the two files into a single project file.</span></span> <span data-ttu-id="8574d-255">使用這個方法，您可以建立一個專案檔包含通用的組建組態和多個增補專案檔包含特定環境的屬性。</span><span class="sxs-lookup"><span data-stu-id="8574d-255">Using this approach, you can create one project file containing your universal build configuration and multiple supplementary project files containing environment-specific properties.</span></span> <span data-ttu-id="8574d-256">如此一來，只需執行命令，以不同的參數值可讓您將方案部署至不同的環境。</span><span class="sxs-lookup"><span data-stu-id="8574d-256">As a result, simply running a command with a different parameter value lets you deploy your solution to a different environment.</span></span>

![](understanding-the-project-file/_static/image3.png)

<span data-ttu-id="8574d-257">請遵循最佳作法是分割您的專案檔案，以這種方式。</span><span class="sxs-lookup"><span data-stu-id="8574d-257">Splitting your project files in this way is a good practice to follow.</span></span> <span data-ttu-id="8574d-258">它可讓開發人員執行單一命令，同時避免重複通用建置屬性跨多個專案檔以部署至多個環境。</span><span class="sxs-lookup"><span data-stu-id="8574d-258">It allows developers to deploy to multiple environments by running a single command, while avoiding the duplication of universal build properties across multiple project files.</span></span>

> [!NOTE]
> <span data-ttu-id="8574d-259">如需如何自訂伺服器環境的特定環境的專案檔案的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="8574d-259">For guidance on how to customize the environment-specific project files for your own server environments, see [Configuring Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


## <a name="conclusion"></a><span data-ttu-id="8574d-260">結論</span><span class="sxs-lookup"><span data-stu-id="8574d-260">Conclusion</span></span>

<span data-ttu-id="8574d-261">本主題介紹一般 MSBuild 專案檔，並說明如何建立自訂專案檔，以控制建置流程。</span><span class="sxs-lookup"><span data-stu-id="8574d-261">This topic provided a general introduction to MSBuild project files and explained how you can create your own custom project files to control the build process.</span></span> <span data-ttu-id="8574d-262">它也導入的概念將專案檔案分割成通用組建指示和特定環境的建置屬性，讓您輕鬆建置和部署專案至多個目的地。</span><span class="sxs-lookup"><span data-stu-id="8574d-262">It also introduced the concept of splitting project files into universal build instructions and environment-specific build properties, to make it easy to build and deploy projects to multiple destinations.</span></span>

<span data-ttu-id="8574d-263">下一個主題[瞭解建置程序](understanding-the-build-process.md)，提供更多深入了解如何使用專案檔，以控制建置和部署帶您逐步部署解決方案與實際的層級的複雜性。</span><span class="sxs-lookup"><span data-stu-id="8574d-263">The next topic, [Understanding the Build Process](understanding-the-build-process.md), provides more insight into how you can use project files to control build and deployment by walking you through the deployment of a solution with a realistic level of complexity.</span></span>

## <a name="further-reading"></a><span data-ttu-id="8574d-264">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="8574d-264">Further Reading</span></span>

<span data-ttu-id="8574d-265">專案檔和 WPP 的更深入簡介，請參閱[內 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi 和 William Bartholomew，ISBN: 978-0-7356-4524-0。</span><span class="sxs-lookup"><span data-stu-id="8574d-265">For a more in-depth introduction to project files and the WPP, see [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://amzn.com/0735645248) by Sayed Ibrahim Hashimi and William Bartholomew, ISBN: 978-0-7356-4524-0.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8574d-266">[上一頁](setting-up-the-contact-manager-solution.md)
[下一頁](understanding-the-build-process.md)</span><span class="sxs-lookup"><span data-stu-id="8574d-266">[Previous](setting-up-the-contact-manager-solution.md)
[Next](understanding-the-build-process.md)</span></span>
