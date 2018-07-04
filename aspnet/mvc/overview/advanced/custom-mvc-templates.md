---
uid: mvc/overview/advanced/custom-mvc-templates
title: 自訂的 MVC 範本 |Microsoft Docs
author: joeloff
description: 建立範本為 VSIX 擴充功能。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 715990e2d7151ad1ce8288169ef4bdd5c4243369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367240"
---
<a name="custom-mvc-template"></a>自訂的 MVC 範本
====================
藉由[3。jacques Eloff](https://github.com/joeloff)

MVC 3 Tools Update for Visual Studio 2010 導入了 MVC 專案的個別專案精靈。 變更是由兩個因素所驅動。 首先，引進的新 MVC 3 與支援中的其他檢視引擎，例如 Razor 的範本，會導致 overcrowding Visual Studio 中的 [新增專案] 對話方塊。 第二，客戶必須一直要求的擴充點，而且新的 MVC 專案精靈 會負擔我們有機會回應這些要求。

新增自訂的範本是依賴使用登錄來顯示 MVC 專案精靈 的新範本的艱辛過程。 新範本的作者必須將它包裝在 MSI 以確保在安裝期間，會建立必要的登錄項目。 替代方法是讓包含可用的範本的 ZIP 檔案，並讓使用者以手動方式建立所需的登錄項目。

因此我們決定以利用一些現有的基礎結構所提供，但上述方法都理想[VSIX](https://msdn.microsoft.com/library/ff363239.aspx)更輕鬆地撰寫，擴充功能散發和安裝自訂的 MVC 範本，開始使用 MVC 4適用於 Visual Studio 2012。 這種方法所提供的好處包括：

- VSIX 擴充功能可以包含多個範本的支援不同語言 （C# 和 Visual Basic） 和多個檢視引擎 （ASPX 和 Razor）。
- VSIX 擴充功能可以針對多個 Sku 的 Visual Studio，包括 Express Sku。
- [Visual Studio 元件庫](https://visualstudiogallery.msdn.microsoft.com/)有助於散發給廣大的使用者延伸模組。
- VSIX 擴充功能可以讓您更輕鬆地撰寫修正和更新您的自訂範本升級。

## <a name="prerequisites"></a>必要條件

- 使用者必須要熟悉撰寫專案範本，包含 vstemplate 檔案等等的必要的標記。
- 使用者必須擁有 Visual Studio Professional 和更新版本安裝。 Express Sku 不支援建立 VSIX 專案。
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668)安裝。

## <a name="example"></a>範例

第一個步驟是建立新的 VSIX 專案，使用 C# 或 Visual Basic。 選取 **檔案 > 新增專案**，然後按一下**擴充性**左的窗格中選取**VSIX 專案**。

![新增專案](custom-mvc-templates/_static/image1.jpg)

建立專案之後，就會開啟 VSIX 設計工具。

![專案設計工具中繼資料](custom-mvc-templates/_static/image2.jpg)

設計工具可以用來編輯某些安裝擴充功能或瀏覽 Visual Studio 中安裝的擴充功能時顯示給使用者的延伸模組的一般屬性 (**工具 > 擴充功能和更新**)。 一旦您已完成的一般資訊按一下**安裝目標 索引標籤**。

![專案設計工具的安裝目標](custom-mvc-templates/_static/image3.jpg)

此索引標籤用來指定 Sku 和版本的 Visual Studio 擴充功能所支援。 選取的核取方塊**針對所有使用者安裝此 VSIX**啟用每部電腦安裝的 VSIX。 按一下 **新增**新增其他 Sku，例如 Web Developer Express (VWD) 右側的按鈕。

![加入新的安裝目標](custom-mvc-templates/_static/image4.jpg)

如果您想要支援所有專業且較高的 Sku （Professional、 Premium 和 Ultimate） 您只需要在家族中，選取的最小的 SKU **Microsoft.VisualStudio.Pro**。 請務必儲存您的變更，當您完成安裝的目標。

![專案設計工具的安裝目標](custom-mvc-templates/_static/image5.jpg)

**資產** 索引標籤用來將您的所有內容檔加入至 VSIX。 因為 MVC 要求自訂的中繼資料，您會編輯而不是使用 VSIX 資訊清單檔案的原始 XML**資產**新增內容 索引標籤。 開始加入 VSIX 專案範本的內容。 請務必資料夾及其內容的結構會反映專案的版面配置。 下列範例包含四個專案範本的衍生自基本的 MVC 專案範本。 請確定包含您的專案範本 （ProjectTemplates 資料夾下的所有內容） 的所有檔案會都新增至**內容**itemgroup 在 VSIX 專案檔案，而且每個項目包含**複製到輸出目錄**並**IncludeInVsix**設定中繼資料，如下列範例所示。

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

如果沒有，則 IDE 會嘗試編譯範本的內容，當您建置 VSIX 和您可能會看到錯誤。 在範本中的程式碼檔案通常會包含特殊[範本參數](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx)專案範本具現化，並因此無法在 IDE 中編譯時，Visual Studio 所使用。

![底下提供說明，包括方案總管](custom-mvc-templates/_static/image6.jpg)

關閉 VSIX 設計工具中，然後以滑鼠右鍵按一下**source.extension.manifest**中的檔案**方案總管**，然後選取**開啟**，然後選擇  **XML (文字） 編輯器**選項。

![開啟對話方塊](custom-mvc-templates/_static/image7.jpg)

建立**&lt;資產&gt;** 項目，並新增**&lt;資產&gt;** 每個檔案必須包含在 VSIX 中的項目。 **型別**每個屬性**&lt;資產&gt;** 項目必須設定為**Microsoft.VisualStudio.Mvc.Template**。 這是僅限 MVC 專案精靈 了解自訂命名空間。 VSIX 2.0 結構描述文件以取得其他有關的結構與配置資訊清單檔案的參考。

只將檔案加入至 VSIX 不足，無法使用 MVC 精靈註冊的範本。 您需要提供 MVC 精靈 的資訊，例如範本名稱、 描述、 支援的檢視引擎和程式設計語言。 這項資訊會在相關聯的自訂屬性轉**&lt;Asset&gt;** 每個項目**vstemplate**檔案。

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

語言 =&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

標題 =&quot;自訂基本的 Web 應用程式&quot;

描述 =&quot;自訂範本衍生自基本的 MVC web 應用程式 (Razor)&quot;

版本 =&quot;4.0&quot;/&gt;

以下是必須要有的自訂屬性的說明：

- **ProjectType**必須設定為 MVC。
- **語言**指定範本所支援的開發語言。 有效值為 C# 或 vb。
- **ViewEngine**指定支援的範本，例如 Aspx 或 Razor 檢視引擎。 您可以指定此欄位的自訂值。
- **TemplateId**用於群組的範本。 如果值符合現有的範本識別碼，它會覆寫先前已向 [MVC] 精靈的範本。
- **標題**指定下方每個專案範本的 [MVC] 精靈中顯示的簡短描述。
- **描述**指定範本的更詳細描述。

您已加入資訊清單中的所有檔案，儲存它，您會注意到之後**資產**設計工具中的索引標籤會顯示所有檔案，但不是需將自訂屬性都新增至您**&lt;資產&gt;** 項目**vstemplate**檔案。

![專案設計工具的資產](custom-mvc-templates/_static/image8.jpg)

所有現在是編譯 VSIX 專案，並將它安裝。

請確定機器上，關閉所有 Visual Studio 執行個體在您想要測試此 VSIX 擴充功能。 Visual Studio 會掃描新的擴充功能在啟動期間，因此如果時安裝的 VSIX，IDE 已開啟您必須重新啟動 Visual Studio。 在 [總管] 中，按兩下 VSIX 檔案，以啟動**VSIX 安裝程式**，按一下**安裝**然後啟動 Visual Studio。

![VSIX 安裝程式](custom-mvc-templates/_static/image9.jpg)

從功能表中，選取**工具 > 擴充功能和更新**若要確認您的延伸模組已安裝。 如果 VSIX 安裝程式延伸模組安裝期間報告任何錯誤，您可以檢視 VSIX 安裝程式記錄檔，如需詳細資訊。 記錄檔通常會在 **%temp%** 安裝擴充功能，例如使用者資料夾**C:\Users\Bob\AppData\Local\Temp**。

![擴充功能和更新](custom-mvc-templates/_static/image10.jpg)

關閉視窗後，您可以建立的 MVC 4 專案，以查看是否在 MVC 精靈中顯示新的範本。

![新的 ASP.NET MVC 4 專案](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>限制

1. MVC 精靈不支援當地語系化的自訂範本。
2. 找出的自訂範本時，精靈將不會報告任何錯誤。 如果任何必要的自訂屬性不存在，範本就只是不會包含從精靈。
