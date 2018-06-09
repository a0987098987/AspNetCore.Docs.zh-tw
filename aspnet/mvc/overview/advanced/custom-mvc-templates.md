---
uid: mvc/overview/advanced/custom-mvc-templates
title: 自訂的 MVC 範本 |Microsoft 文件
author: joeloff
description: 建立範本當做 VSIX 擴充功能。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034603"
---
<a name="custom-mvc-template"></a><span data-ttu-id="48abb-103">自訂的 MVC 範本</span><span class="sxs-lookup"><span data-stu-id="48abb-103">Custom MVC Template</span></span>
====================
<span data-ttu-id="48abb-104">由[Jacques Eloff](https://github.com/joeloff)</span><span class="sxs-lookup"><span data-stu-id="48abb-104">by [Jacques Eloff](https://github.com/joeloff)</span></span>

<span data-ttu-id="48abb-105">MVC 3 Tools Update for Visual Studio 2010 的版本導入了 MVC 專案的個別專案精靈。</span><span class="sxs-lookup"><span data-stu-id="48abb-105">The release of MVC 3 Tools Update for Visual Studio 2010 introduced a separate project wizard for MVC projects.</span></span> <span data-ttu-id="48abb-106">變更所驅動兩個因素。</span><span class="sxs-lookup"><span data-stu-id="48abb-106">The change was driven by two factors.</span></span> <span data-ttu-id="48abb-107">首先，在 MVC 3 和支援其他檢視引擎，例如 Razor 中新範本的簡介，會導致 overcrowding Visual Studio 中的 [新增專案] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="48abb-107">First, the introduction of new templates in MVC 3 and support for additional view engines such as Razor lead to overcrowding the New Project dialog in Visual Studio.</span></span> <span data-ttu-id="48abb-108">第二，客戶必須已要求針對擴充性點，而且新的 MVC 專案精靈會負擔我們有機會回應這些要求。</span><span class="sxs-lookup"><span data-stu-id="48abb-108">Second, customers had been asking for extensibility points and the new MVC project wizard would afford us the opportunity to respond to these requests.</span></span>

<span data-ttu-id="48abb-109">新增自訂的範本是依賴使用登錄來顯示新的範本才能 MVC 專案精靈棘手程序。</span><span class="sxs-lookup"><span data-stu-id="48abb-109">Adding custom templates was an arduous process that relied on using the registry to make new templates visible to the MVC project wizard.</span></span> <span data-ttu-id="48abb-110">新範本的作者必須將其包裝在 MSI 以確保在安裝時，會建立必要的登錄項目內。</span><span class="sxs-lookup"><span data-stu-id="48abb-110">The author of a new template had to wrap it inside an MSI to ensure that the necessary registry entries would be created at install time.</span></span> <span data-ttu-id="48abb-111">替代方案是包含可用範本的 ZIP 檔案，然後使用者以手動方式建立所需的登錄項目。</span><span class="sxs-lookup"><span data-stu-id="48abb-111">The alternative was to make a ZIP file containing the template available and have the end-user create the required registry entries by hand.</span></span>

<span data-ttu-id="48abb-112">上述方法皆不理想，我們決定將利用現有的基礎結構所提供的某些[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)延伸模組，以便更容易撰寫，發佈和安裝自訂起始 MVC 4 的 MVC 範本適用於 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="48abb-112">Neither of the aforementioned approaches is ideal so we decided to leverage some of the existing infrastructure provided by [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensions to make it easier to author, distribute and install custom MVC templates starting with MVC 4 for Visual Studio 2012.</span></span> <span data-ttu-id="48abb-113">這種方法的好處包括：</span><span class="sxs-lookup"><span data-stu-id="48abb-113">Some of the benefits provided by this approach are:</span></span>

- <span data-ttu-id="48abb-114">VSIX 擴充功能包含多個範本可支援不同的語言 （C# 和 Visual Basic） 和多個檢視引擎 （ASPX 和 Razor）。</span><span class="sxs-lookup"><span data-stu-id="48abb-114">A VSIX extension can contain multiple templates that support different languages (C# and Visual Basic) and multiple view engines (ASPX and Razor).</span></span>
- <span data-ttu-id="48abb-115">VSIX 擴充功能可以針對多個 Sku 的 Visual Studio 包括 Express Sku。</span><span class="sxs-lookup"><span data-stu-id="48abb-115">A VSIX extension can target multiple SKUs of Visual Studio including Express SKUs.</span></span>
- <span data-ttu-id="48abb-116">[Visual Studio 組件庫](https://visualstudiogallery.msdn.microsoft.com/)有助於發佈給廣大觀眾的延伸模組。</span><span class="sxs-lookup"><span data-stu-id="48abb-116">The [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilitates distributing the extension to a wide audience.</span></span>
- <span data-ttu-id="48abb-117">您可以輕鬆地撰寫更正及更新您的自訂範本升級 VSIX 擴充功能。</span><span class="sxs-lookup"><span data-stu-id="48abb-117">VSIX extensions can be upgraded making it easier to author corrections and updates to your custom templates.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48abb-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="48abb-118">Prerequisites</span></span>

- <span data-ttu-id="48abb-119">使用者需要熟悉撰寫專案範本，包括必要的標記 vstemplate 檔案等等。</span><span class="sxs-lookup"><span data-stu-id="48abb-119">Users need to be familiar with authoring project templates, including the required markup for vstemplate files, etc.</span></span>
- <span data-ttu-id="48abb-120">使用者必須有 Visual Studio Professional 和更新版本安裝。</span><span class="sxs-lookup"><span data-stu-id="48abb-120">Users will need to have Visual Studio Professional and higher installed.</span></span> <span data-ttu-id="48abb-121">Express Sku 不支援建立 VSIX 專案。</span><span class="sxs-lookup"><span data-stu-id="48abb-121">Express SKUs do not support creating VSIX projects.</span></span>
- <span data-ttu-id="48abb-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)安裝。</span><span class="sxs-lookup"><span data-stu-id="48abb-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installed.</span></span>

## <a name="example"></a><span data-ttu-id="48abb-123">範例</span><span class="sxs-lookup"><span data-stu-id="48abb-123">Example</span></span>

<span data-ttu-id="48abb-124">第一個步驟是建立新的 VSIX 專案，使用 C# 或 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="48abb-124">The first step is to create a new VSIX project using either C# or Visual Basic.</span></span> <span data-ttu-id="48abb-125">選取**檔案 > 新的專案**，然後按一下 **擴充性**在左的窗格中選取**VSIX 專案**。</span><span class="sxs-lookup"><span data-stu-id="48abb-125">Select **File > New Project**, then click **Extensibility** in the left pane and select the **VSIX Project**.</span></span>

![新增專案](custom-mvc-templates/_static/image1.jpg)

<span data-ttu-id="48abb-127">在建立專案之後，就會開啟 VSIX 設計工具。</span><span class="sxs-lookup"><span data-stu-id="48abb-127">After the project is created, the VSIX designer will be opened.</span></span>

![專案設計工具中繼資料](custom-mvc-templates/_static/image2.jpg)

<span data-ttu-id="48abb-129">在設計工具可以用來編輯某些安裝擴充功能或瀏覽 Visual Studio 中安裝的擴充功能時顯示給使用者的擴充功能的一般屬性 (**工具 > 擴充功能和更新**)。</span><span class="sxs-lookup"><span data-stu-id="48abb-129">The designer can be used to edit some of the general properties of the extension that will be shown to users when they install the extension or browse the installed extensions in Visual Studio (**Tools > Extensions and Updates**).</span></span> <span data-ttu-id="48abb-130">一旦您已完成的一般資訊按一下**安裝目標 索引標籤**。</span><span class="sxs-lookup"><span data-stu-id="48abb-130">Once you have completed the general information click on the **Install Targets tab**.</span></span>

![專案設計工具的安裝目標](custom-mvc-templates/_static/image3.jpg)

<span data-ttu-id="48abb-132">此索引標籤用來指定的 Sku 和版本的 Visual Studio 擴充功能所支援。</span><span class="sxs-lookup"><span data-stu-id="48abb-132">This tab is used to specify the SKUs and versions of Visual Studio that are supported by your extension.</span></span> <span data-ttu-id="48abb-133">選取的核取方塊**已針對所有使用者安裝此 VSIX**啟用每部電腦安裝的 VSIX。</span><span class="sxs-lookup"><span data-stu-id="48abb-133">Select the checkbox for **This VSIX is installed for all users** to enable per-machine installs of the VSIX.</span></span> <span data-ttu-id="48abb-134">按一下**新增**按鈕上加入其他 Sku，例如 Web Developer Express (VWD) 的權限。</span><span class="sxs-lookup"><span data-stu-id="48abb-134">Click on the **New** button on the right to add additional SKUs such as Web Developer Express (VWD).</span></span>

![加入新安裝目標](custom-mvc-templates/_static/image4.jpg)

<span data-ttu-id="48abb-136">如果您想要支援的所有專業版和更高 Sku （Professional、 Premium 和 Ultimate） 只需要選取最小的 SKU 系列**Microsoft.VisualStudio.Pro**。</span><span class="sxs-lookup"><span data-stu-id="48abb-136">If you intend to support all the Professional and higher SKUs (Professional, Premium and Ultimate) you only need to select the minimum SKU in the family, **Microsoft.VisualStudio.Pro**.</span></span> <span data-ttu-id="48abb-137">請記得儲存所有變更，當您完成安裝的目標。</span><span class="sxs-lookup"><span data-stu-id="48abb-137">Remember to save all your changes once you have completed the Install Targets.</span></span>

![專案設計工具的安裝目標](custom-mvc-templates/_static/image5.jpg)

<span data-ttu-id="48abb-139">**資產** 索引標籤用來將所有內容檔加入至 VSIX。</span><span class="sxs-lookup"><span data-stu-id="48abb-139">The **Assets** tab is used to add all your content files to the VSIX.</span></span> <span data-ttu-id="48abb-140">因為 MVC 要求自訂中繼資料，您會編輯而不是使用 VSIX 資訊清單檔案的原始 XML**資產**新增內容 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="48abb-140">Since MVC requires custom metadata you will be editing the raw XML of the VSIX manifest file instead of using the **Assets** tab to add content.</span></span> <span data-ttu-id="48abb-141">啟動者加入 VSIX 專案的範本內容。</span><span class="sxs-lookup"><span data-stu-id="48abb-141">Start by adding the template contents to the VSIX project.</span></span> <span data-ttu-id="48abb-142">請務必資料夾及其內容的結構鏡像專案的版面配置。</span><span class="sxs-lookup"><span data-stu-id="48abb-142">It is important that the structure of the folder and the contents mirror the layout of the project.</span></span> <span data-ttu-id="48abb-143">下列範例包含四個衍生自基本 MVC 專案範本的專案範本。</span><span class="sxs-lookup"><span data-stu-id="48abb-143">The example below contains four project templates that were derived from the Basic MVC project template.</span></span> <span data-ttu-id="48abb-144">請確定組成您的專案範本 （ProjectTemplates 資料夾下的所有內容） 的所有檔案會都新增到**內容**itemgroup 在 VSIX 專案檔案，而且每個項目包含**複製到輸出目錄**和**IncludeInVsix**設定中繼資料，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="48abb-144">Make sure that all the files that comprise your project template (everything underneath the ProjectTemplates folder) are added to the **Content** itemgroup in the VSIX project file and that each item contains the **CopyToOutputDirectory** and **IncludeInVsix** metadata set as shown in the example below.</span></span>

<span data-ttu-id="48abb-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="48abb-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span></span>

<span data-ttu-id="48abb-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span><span class="sxs-lookup"><span data-stu-id="48abb-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span></span>

<span data-ttu-id="48abb-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span><span class="sxs-lookup"><span data-stu-id="48abb-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span></span>

<span data-ttu-id="48abb-148">&lt;/Content&gt;</span><span class="sxs-lookup"><span data-stu-id="48abb-148">&lt;/Content&gt;</span></span>

<span data-ttu-id="48abb-149">如果沒有，IDE 會嘗試編譯範本的內容，當您建立此 VSIX，而且您可能會看到錯誤。</span><span class="sxs-lookup"><span data-stu-id="48abb-149">If not, the IDE will try to compile the contents of the template when you build the VSIX and you will likely see an error.</span></span> <span data-ttu-id="48abb-150">範本中的程式碼檔通常包含特殊[範本參數](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)時的專案範本會具現化，因此無法加以編譯在 IDE 中使用 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="48abb-150">Code files in templates often contain special [template parameters](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) used by Visual Studio when the project template is instantiated and therefore cannot be compiled in the IDE.</span></span>

![底下提供說明，包括方案總管](custom-mvc-templates/_static/image6.jpg)

<span data-ttu-id="48abb-152">關閉 VSIX 設計工具中，然後以滑鼠右鍵按一下**source.extension.manifest**檔案**方案總管 中**選取**開啟**選擇**XML (文字） 編輯器**選項。</span><span class="sxs-lookup"><span data-stu-id="48abb-152">Close the VSIX designer, then right click on the **source.extension.manifest** file in **Solution Explorer** and select **Open With** and choose the **XML (Text) Editor** option.</span></span>

![開啟對話方塊](custom-mvc-templates/_static/image7.jpg)

<span data-ttu-id="48abb-154">建立**&lt;資產&gt;** 元素並加入**&lt;資產&gt;** 元素必須包含在 VSIX 中的每個檔案。</span><span class="sxs-lookup"><span data-stu-id="48abb-154">Create an **&lt;Assets&gt;** element and add an **&lt;Asset&gt;** element for each file that must be included in the VSIX.</span></span> <span data-ttu-id="48abb-155">**類型**每個屬性**&lt;資產&gt;** 元素必須設定為**Microsoft.VisualStudio.Mvc.Template**。</span><span class="sxs-lookup"><span data-stu-id="48abb-155">The **Type** attribute of each **&lt;Asset&gt;** element must be set to **Microsoft.VisualStudio.Mvc.Template**.</span></span> <span data-ttu-id="48abb-156">這是僅限 MVC 專案精靈了解自訂命名空間。</span><span class="sxs-lookup"><span data-stu-id="48abb-156">This is a custom namespace that only the MVC project wizard understands.</span></span> <span data-ttu-id="48abb-157">請參閱 VSIX 2.0 結構描述文件上的結構與配置資訊清單檔案的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="48abb-157">Refer to the VSIX 2.0 Schema documentation for additional information on the structure and layout of the manifest file.</span></span>

<span data-ttu-id="48abb-158">只將檔案加入至 VSIX 不足以向 MVC 精靈中的範本。</span><span class="sxs-lookup"><span data-stu-id="48abb-158">Just adding the files to the VSIX is not sufficient to register the templates with the MVC wizard.</span></span> <span data-ttu-id="48abb-159">您需要為 MVC 精靈提供的範本名稱、 描述、 支援的檢視引擎和程式設計語言等資訊。</span><span class="sxs-lookup"><span data-stu-id="48abb-159">You need to provide information such as the template name, description, supported view engines and programming language to the MVC wizard.</span></span> <span data-ttu-id="48abb-160">這項資訊會在相關聯的自訂屬性執行**&lt;資產&gt;** 每個項目**vstemplate**檔案。</span><span class="sxs-lookup"><span data-stu-id="48abb-160">This information is carried in custom attributes associated with the **&lt;Asset&gt;** element for each **vstemplate** file.</span></span>

<span data-ttu-id="48abb-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span></span>

<span data-ttu-id="48abb-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span></span>

<span data-ttu-id="48abb-163">d:Source=&quot;File&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-163">d:Source=&quot;File&quot;</span></span>

<span data-ttu-id="48abb-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span></span>

<span data-ttu-id="48abb-165">ProjectType=&quot;MVC&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-165">ProjectType=&quot;MVC&quot;</span></span>

<span data-ttu-id="48abb-166">語言 =&quot;C#&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-166">Language=&quot;C#&quot;</span></span>

<span data-ttu-id="48abb-167">ViewEngine=&quot;Aspx&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-167">ViewEngine=&quot;Aspx&quot;</span></span>

<span data-ttu-id="48abb-168">TemplateId=&quot;MyMvcApplication&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-168">TemplateId=&quot;MyMvcApplication&quot;</span></span>

<span data-ttu-id="48abb-169">標題 =&quot;自訂基本 Web 應用程式&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-169">Title=&quot;Custom Basic Web Application&quot;</span></span>

<span data-ttu-id="48abb-170">描述 =&quot;自訂範本衍生自基本 MVC web 應用程式 (Razor)&quot;</span><span class="sxs-lookup"><span data-stu-id="48abb-170">Description=&quot;A custom template derived from a Basic MVC web application (Razor)&quot;</span></span>

<span data-ttu-id="48abb-171">版本 =&quot;4.0&quot;/&gt;</span><span class="sxs-lookup"><span data-stu-id="48abb-171">Version=&quot;4.0&quot;/&gt;</span></span>

<span data-ttu-id="48abb-172">以下是必須要有自訂屬性的說明：</span><span class="sxs-lookup"><span data-stu-id="48abb-172">Below is an explanation of the custom attributes that must be present:</span></span>

- <span data-ttu-id="48abb-173">**ProjectType**必須設為 MVC。</span><span class="sxs-lookup"><span data-stu-id="48abb-173">**ProjectType** must be set to MVC.</span></span>
- <span data-ttu-id="48abb-174">**語言**指定範本所支援的開發語言。</span><span class="sxs-lookup"><span data-stu-id="48abb-174">**Language** designates the development language supported by the template.</span></span> <span data-ttu-id="48abb-175">有效值為 C# 或 VB</span><span class="sxs-lookup"><span data-stu-id="48abb-175">Valid values are either C# or VB.</span></span>
- <span data-ttu-id="48abb-176">**ViewEngine**指定支援的範本，例如 Aspx 或 Razor 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="48abb-176">**ViewEngine** designates the view engine supported by the template such as Aspx or Razor.</span></span> <span data-ttu-id="48abb-177">您可以指定此欄位的自訂值。</span><span class="sxs-lookup"><span data-stu-id="48abb-177">You can specify a custom value for this field.</span></span>
- <span data-ttu-id="48abb-178">**TemplateId**用於群組的範本。</span><span class="sxs-lookup"><span data-stu-id="48abb-178">**TemplateId** is used for grouping the templates.</span></span> <span data-ttu-id="48abb-179">如果值符合現有的範本識別碼，它將會覆寫先前登錄的 MVC 精靈的範本。</span><span class="sxs-lookup"><span data-stu-id="48abb-179">If the value matches an existing template ID it will be override templates previously registered with the MVC wizard.</span></span>
- <span data-ttu-id="48abb-180">**標題**指定每個專案範本下方 MVC 精靈中顯示的簡短描述。</span><span class="sxs-lookup"><span data-stu-id="48abb-180">**Title** designates the short description displayed in the MVC wizard beneath each project template.</span></span>
- <span data-ttu-id="48abb-181">**描述**指定範本的更詳細描述。</span><span class="sxs-lookup"><span data-stu-id="48abb-181">**Description** designates a more verbose description of the template.</span></span>

<span data-ttu-id="48abb-182">您已加入資訊清單的所有檔案，並儲存它，您會注意到之後**資產**設計工具中的索引標籤會顯示所有檔案，但不是自訂屬性都加入至您**&lt;資產&gt;** 元素**vstemplate**檔案。</span><span class="sxs-lookup"><span data-stu-id="48abb-182">After you have added all the files to the manifest and saved it, you will notice that the **Assets** tab in the designer will display all the files, but not the custom attributes you added to the **&lt;Asset&gt;** elements for the **vstemplate** files.</span></span>

![專案設計工具的資產](custom-mvc-templates/_static/image8.jpg)

<span data-ttu-id="48abb-184">所有剩下的現在都只有編譯 VSIX 專案，並安裝它。</span><span class="sxs-lookup"><span data-stu-id="48abb-184">All that remains now is to compile the VSIX project and install it.</span></span>

<span data-ttu-id="48abb-185">請確定 Visual Studio 的所有執行個體都已關閉電腦上您想要測試此 VSIX 擴充功能。</span><span class="sxs-lookup"><span data-stu-id="48abb-185">Make sure that all instances of Visual Studio are closed on the machine where you intend to test the VSIX extension.</span></span> <span data-ttu-id="48abb-186">Visual Studio 會掃描新的延伸模組在啟動期間，因此，如果安裝的 VSIX 時，IDE 已開啟您將需要重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="48abb-186">Visual Studio scans for new extensions during startup, so if the IDE is open while installing a VSIX you will need to restart Visual Studio.</span></span> <span data-ttu-id="48abb-187">在 總管 中，按兩下 VSIX 檔案以啟動**VSIX 安裝程式**，按一下 **安裝**然後啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="48abb-187">In Explorer, double click on the VSIX file to launch the **VSIX Installer**, click **Install** and then launch Visual Studio.</span></span>

![VSIX 安裝程式](custom-mvc-templates/_static/image9.jpg)

<span data-ttu-id="48abb-189">從功能表中，選取**工具 > 擴充功能和更新**若要確認已安裝您的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="48abb-189">From the menu, select **Tools > Extensions and Updates** to confirm that your extension was installed.</span></span> <span data-ttu-id="48abb-190">如果 VSIX 安裝程式在安裝擴充功能期間報告任何錯誤，您可以檢視 VSIX 安裝程式記錄檔，如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="48abb-190">If the VSIX Installer reported any errors during the installation of the extension you can view the VSIX Installer log for more information.</span></span> <span data-ttu-id="48abb-191">記錄檔通常會建立在 **%temp%** 安裝擴充功能，例如使用者資料夾**C:\Users\Bob\AppData\Local\Temp**。</span><span class="sxs-lookup"><span data-stu-id="48abb-191">The log is usually created in the **%temp%** folder of the user that installed the extension, for example **C:\Users\Bob\AppData\Local\Temp**.</span></span>

![擴充功能和更新](custom-mvc-templates/_static/image10.jpg)

<span data-ttu-id="48abb-193">關閉視窗之後，您可以建立的 MVC 4 專案，以查看是否新範本會顯示在 MVC 精靈。</span><span class="sxs-lookup"><span data-stu-id="48abb-193">After closing the window you can create an MVC 4 project to see whether your new templates are shown in the MVC wizard.</span></span>

![新的 ASP.NET MVC 4 專案](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a><span data-ttu-id="48abb-195">限制</span><span class="sxs-lookup"><span data-stu-id="48abb-195">Limitations</span></span>

1. <span data-ttu-id="48abb-196">MVC 精靈不支援當地語系化的自訂範本。</span><span class="sxs-lookup"><span data-stu-id="48abb-196">The MVC wizard does not support localized custom templates.</span></span>
2. <span data-ttu-id="48abb-197">找出自訂範本時，精靈將不會報告任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="48abb-197">The wizard will not report any errors if it fails to locate custom templates.</span></span> <span data-ttu-id="48abb-198">如果任何必要的自訂屬性不存在，精靈中就只包含使用範本。</span><span class="sxs-lookup"><span data-stu-id="48abb-198">If any of the required custom attributes are absent, the template would simply be excluded from the Wizard.</span></span>
