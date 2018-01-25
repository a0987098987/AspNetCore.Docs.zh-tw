---
uid: mvc/overview/advanced/custom-mvc-templates
title: "自訂的 MVC 範本 |Microsoft 文件"
author: joeloff
description: "建立範本當做 VSIX 擴充功能。"
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
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="custom-mvc-template"></a>自訂的 MVC 範本
====================
由[Jacques Eloff](https://github.com/joeloff)

MVC 3 Tools Update for Visual Studio 2010 的版本導入了 MVC 專案的個別專案精靈。 變更所驅動兩個因素。 首先，在 MVC 3 和支援其他檢視引擎，例如 Razor 中新範本的簡介，會導致 overcrowding Visual Studio 中的 [新增專案] 對話方塊。 第二，客戶必須已要求針對擴充性點，而且新的 MVC 專案精靈會負擔我們有機會回應這些要求。

新增自訂的範本是依賴使用登錄來顯示新的範本才能 MVC 專案精靈棘手程序。 新範本的作者必須將其包裝在 MSI 以確保在安裝時，會建立必要的登錄項目內。 替代方案是包含可用範本的 ZIP 檔案，然後使用者以手動方式建立所需的登錄項目。

上述方法皆不理想，我們決定將利用現有的基礎結構所提供的某些[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)延伸模組，以便更容易撰寫，發佈和安裝自訂起始 MVC 4 的 MVC 範本適用於 Visual Studio 2012。 這種方法的好處包括：

- VSIX 擴充功能包含多個範本可支援不同的語言 （C# 和 Visual Basic） 和多個檢視引擎 （ASPX 和 Razor）。
- VSIX 擴充功能可以針對多個 Sku 的 Visual Studio 包括 Express Sku。
- [Visual Studio 組件庫](https://visualstudiogallery.msdn.microsoft.com/)有助於發佈給廣大觀眾的延伸模組。
- 您可以輕鬆地撰寫更正及更新您的自訂範本升級 VSIX 擴充功能。

## <a name="prerequisites"></a>必要條件

- 使用者需要熟悉撰寫專案範本，包括必要的標記 vstemplate 檔案等等。
- 使用者必須有 Visual Studio Professional 和更新版本安裝。 Express Sku 不支援建立 VSIX 專案。
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)安裝。

## <a name="example"></a>範例

第一個步驟是建立新的 VSIX 專案，使用 C# 或 Visual Basic。 選取**檔案 > 新的專案**，然後按一下 **擴充性**在左的窗格中選取**VSIX 專案**。

![新增專案](custom-mvc-templates/_static/image1.jpg)

在建立專案之後，就會開啟 VSIX 設計工具。

![專案設計工具中繼資料](custom-mvc-templates/_static/image2.jpg)

在設計工具可以用來編輯某些安裝擴充功能或瀏覽 Visual Studio 中安裝的擴充功能時顯示給使用者的擴充功能的一般屬性 (**工具 > 擴充功能和更新**)。 一旦您已完成的一般資訊按一下**安裝目標 索引標籤**。

![專案設計工具的安裝目標](custom-mvc-templates/_static/image3.jpg)

此索引標籤用來指定的 Sku 和版本的 Visual Studio 擴充功能所支援。 選取的核取方塊**已針對所有使用者安裝此 VSIX**啟用每部電腦安裝的 VSIX。 按一下**新增**按鈕上加入其他 Sku，例如 Web Developer Express (VWD) 的權限。

![加入新安裝目標](custom-mvc-templates/_static/image4.jpg)

如果您想要支援的所有專業版和更高 Sku （Professional、 Premium 和 Ultimate） 只需要選取最小的 SKU 系列**Microsoft.VisualStudio.Pro**。 請記得儲存所有變更，當您完成安裝的目標。

![專案設計工具的安裝目標](custom-mvc-templates/_static/image5.jpg)

**資產** 索引標籤用來將所有內容檔加入至 VSIX。 因為 MVC 要求自訂中繼資料，您會編輯而不是使用 VSIX 資訊清單檔案的原始 XML**資產**新增內容 索引標籤。 啟動者加入 VSIX 專案的範本內容。 請務必資料夾及其內容的結構鏡像專案的版面配置。 下列範例包含四個衍生自基本 MVC 專案範本的專案範本。 請確定組成您的專案範本 （ProjectTemplates 資料夾下的所有內容） 的所有檔案會都新增到**內容**itemgroup 在 VSIX 專案檔案，而且每個項目包含**複製到輸出目錄**和**IncludeInVsix**設定中繼資料，如下列範例所示。

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

如果沒有，IDE 會嘗試編譯範本的內容，當您建立此 VSIX，而且您可能會看到錯誤。 範本中的程式碼檔通常包含特殊[範本參數](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)時的專案範本會具現化，因此無法加以編譯在 IDE 中使用 Visual Studio。

![底下提供說明，包括方案總管](custom-mvc-templates/_static/image6.jpg)

關閉 VSIX 設計工具中，然後以滑鼠右鍵按一下**source.extension.manifest**檔案**方案總管 中**選取**開啟**選擇**XML (文字） 編輯器**選項。

![開啟對話方塊](custom-mvc-templates/_static/image7.jpg)

建立**&lt;資產&gt;**元素並加入**&lt;資產&gt;**元素必須包含在 VSIX 中的每個檔案。 **類型**每個屬性**&lt;資產&gt;**元素必須設定為**Microsoft.VisualStudio.Mvc.Template**。 這是僅限 MVC 專案精靈了解自訂命名空間。 請參閱 VSIX 2.0 結構描述文件上的結構與配置資訊清單檔案的其他資訊。

只將檔案加入至 VSIX 不足以向 MVC 精靈中的範本。 您需要為 MVC 精靈提供的範本名稱、 描述、 支援的檢視引擎和程式設計語言等資訊。 這項資訊會在相關聯的自訂屬性執行**&lt;資產&gt;**每個項目**vstemplate**檔案。

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language=&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

標題 =&quot;自訂基本 Web 應用程式&quot;

描述 =&quot;自訂範本衍生自基本 MVC web 應用程式 (Razor)&quot;

Version=&quot;4.0&quot;/&gt;

以下是必須要有自訂屬性的說明：

- **ProjectType**必須設為 MVC。
- **語言**指定範本所支援的開發語言。 有效值為 C# 或 VB
- **ViewEngine**指定支援的範本，例如 Aspx 或 Razor 檢視引擎。 您可以指定此欄位的自訂值。
- **TemplateId**用於群組的範本。 如果值符合現有的範本識別碼，它將會覆寫先前登錄的 MVC 精靈的範本。
- **標題**指定每個專案範本下方 MVC 精靈中顯示的簡短描述。
- **描述**指定範本的更詳細描述。

您已加入資訊清單的所有檔案，並儲存它，您會注意到之後**資產**設計工具中的索引標籤會顯示所有檔案，但不是自訂屬性都加入至您**&lt;資產&gt;**元素**vstemplate**檔案。

![專案設計工具的資產](custom-mvc-templates/_static/image8.jpg)

所有剩下的現在都只有編譯 VSIX 專案，並安裝它。

請確定 Visual Studio 的所有執行個體都已關閉電腦上您想要測試此 VSIX 擴充功能。 Visual Studio 會掃描新的延伸模組在啟動期間，因此，如果安裝的 VSIX 時，IDE 已開啟您將需要重新啟動 Visual Studio。 在 總管 中，按兩下 VSIX 檔案以啟動**VSIX 安裝程式**，按一下 **安裝**然後啟動 Visual Studio。

![VSIX 安裝程式](custom-mvc-templates/_static/image9.jpg)

從功能表中，選取**工具 > 擴充功能和更新**若要確認已安裝您的擴充功能。 如果 VSIX 安裝程式在安裝擴充功能期間報告任何錯誤，您可以檢視 VSIX 安裝程式記錄檔，如需詳細資訊。 記錄檔通常會建立在**%temp%**安裝擴充功能，例如使用者資料夾**C:\Users\Bob\AppData\Local\Temp**。

![擴充功能和更新](custom-mvc-templates/_static/image10.jpg)

關閉視窗之後，您可以建立的 MVC 4 專案，以查看是否新範本會顯示在 MVC 精靈。

![新的 ASP.NET MVC 4 專案](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>限制

1. MVC 精靈不支援當地語系化的自訂範本。
2. 找出自訂範本時，精靈將不會報告任何錯誤。 如果任何必要的自訂屬性不存在，精靈中就只包含使用範本。
