---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: 程式設計 ASP.NET Web Pages (Razor) 使用 Visual Studio |Microsoft 文件
author: tfitzmac
description: 此附錄將解釋如何使用 Visual Studio 2010 或 Visual Web Developer 2010 Express 含有 Razor 語法的 ASP.NET Web Pages 程式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2014
ms.topic: article
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: eb17c8cc1fab5b552c8495e74bb86ae9dbc5b972
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896574"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>使用 Visual Studio 程式設計 ASP.NET Web Pages (Razor)
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何使用 Visual Studio 或 Visual Web Developer Express 程式 ASP.NET Web Pages (Razor) 網站。
> 
> 您將學習
> 
> - 您必須安裝在您的 Visual Studio 版本中運作以 ASP.NET Web Pages 的 （如果有的話）。
> - 如何加入支援的 ASP.NET Web Pages Visual Web Developer 2010 Express。
> - 如何使用 Visual Studio 中的功能，才能使用 ASP.NET Razor 頁面，包括 IntelliSense 和偵錯工具。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2、 Visual Studio 2012、 Visual Studio 2010 和 WebMatrix 2。


您可以使用 WebMatrix 或許多其他程式碼編輯器的 Razor 語法與程式的 ASP.NET Web pages。 您也可以使用 Microsoft Visual Studio 也就是功能完整的整合式的開發環境 (IDE) 提供一組強大的工具來建立許多類型的應用程式 （不只是網站）。 若要使用 ASP.NET Razor 頁面，您也可以使用其中一個 Visual Studio 的完整版本或免費[Visual Studio Express for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express)版本。

Visual Studio 使用 ASP.NET Razor 網頁進行程式設計所提供的兩個特別有用功能如下：

- *IntelliSense*。 建置到 Visual Studio 的 IntelliSense 功能會比在 WebMatrix IntelliSense 更廣泛。
- *偵錯工具*。 偵錯工具可讓您針對您的程式碼來停止程式執行、 檢查變數，並逐步執行逐行程式碼時進行疑難排解。

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>使用 Visual Studio 使用不同版本的 ASP.NET Web Pages

Visual Studio 2012 和 Visual Studio 2013 包括支援的 ASP.NET Web Pages。 （當您安裝 Visual Studio 會安裝之封裝的條件，才能支援 ASP.NET Web Pages）。

Visual Studio 2010 不支援預設的 ASP.NET Web Pages。 若要使用 ASP.NET Web Pages Visual Studio 2010，您必須安裝 ASP.NET MVC 封裝。 若要取得 ASP.NET Web Pages 2，您必須安裝 ASP.NET MVC 4。

下表摘要說明支援的 ASP.NET Web Pages 中不同版本的 Visual Studio。

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | 安裝 ASP.NET MVC 4 | （包含） | （包含） |
| **ASP.NET Web Pages 3** |  | 更新至 ASP.NET Web Pages 3 透過 NuGet | （包含） |

若要使用 Visual Studio 2010，請參閱[安裝支援的 Visual Studio 2010 中 ASP.NET Web Pages](#vs2010support)。

## <a name="launching-visual-studio-from-webmatrix"></a>啟動 Visual Studio 從 WebMatrix

如果您已經在 WebMatrix 啟動專案，而且想要切換至 Visual Studio，WebMatrix 會提供按鈕，以輕鬆地在 Visual Studio 中開啟專案。 您必須啟用 Visual Studio 安裝在您的電腦，此按鈕上。 下圖顯示的按鈕在 WebMatrix。

![啟動 Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

當您按一下按鈕時，Visual Studio 中開啟專案。 您可以切換來回 WebMatrix 和 Visual Studio 不會有任何問題。 如果另一個環境中已經變更的任何檔案，而且需要重新載入，才能取得最新的變更，系統會通知您。

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Visual Studio 中建立 ASP.NET Razor 網站

若要在 Visual Studio 中建立 ASP.NET Razor 網站：

1. 啟動 Visual Studio 或 Visual Web Developer。
2. 在**檔案**功能表上，按一下 **新網站**。

    ![建立新的網站](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. 在**新網站**對話方塊方塊中，選取要使用 （Visual C# 或 Visual Basic） 語言。
4. 選取**ASP.NET 網站 (Razor)** 範本。

    ![razor 的站台](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. 按一下 [確定 **Deploying Office Solutions**]。

新的專案存在，而且會填入一些預設的網頁以協助您快速入門。

### <a name="using-intellisense"></a>Using IntelliSense

既然您已建立站台，您可以看到在 Visual Studio 中的 IntelliSense 如何運作。

1. 在您剛才建立的網站，開啟*Default.cshtml*頁面。
2. 之後`<h3>`標記在頁面中，輸入`@ServerInfo.`（包括點）。 請注意如何 IntelliSense 會顯示可用的方法`ServerInfo`下拉式清單中的協助程式。 

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. 選取`GetHtml`中方法的清單，然後按 Enter 鍵。 在方法中的 IntelliSense 會自動填入。 (使用 C# 中的任何方法，您必須新增`()`方法之後的字元。)  
   已完成的程式碼，如`GetHtml`方法看起來類似下列的範例：  

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. 按 Ctrl + F5 執行頁面。 這是頁面看起來像時顯示在瀏覽器中： 

    ![在瀏覽器中的預設頁面](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. 關閉瀏覽器。

### <a name="using-the-debugger"></a>使用偵錯工具

1. 在頂端*Default.cshtml*開頭的該行之後 頁面上， `Page.Title`，加入下列程式碼行： 

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. 在編輯器中，以在程式碼左側的灰色邊界，按一下這個新的一行的旁邊才能加入*中斷點*。 中斷點會告知偵錯工具停止在該點執行程式，讓您可以查看發生什麼事的標記。

    ![設定中斷點](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. 移除呼叫`ServerInfo.GetHtml`方法，並將呼叫加入`@myTime`變數在它的位置。 此呼叫會顯示新程式碼行所傳回的目前時間值。
4. 按 F5 以偵錯工具中執行網頁。 頁面會在您設定的中斷點上停止。 下圖顯示頁面的外觀在編輯器中使用的中斷點 （黃色）。 

    ![偵錯中斷點](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. 在 [偵錯] 工具列中，按一下 [**逐步執行**執行下的一行程式碼] 按鈕 （或按下 F11）。 每當您按一下這個按鈕，您會繼續執行至下一行程式碼。

    ![逐步執行 按鈕](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. 檢查值`myTime`按住滑鼠指標進入此或藉由檢查中顯示之值的變數**區域變數**和**呼叫堆疊**windows。 Visual Studio 會顯示變數的值。

    ![顯示的時間值](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. 完成檢查變數及逐步執行程式碼之後，請按 f5 鍵繼續執行而不需要停止每一行程式碼的網頁。 逐步執行所有程式碼完成後，瀏覽器顯示頁面。

若要進一步了解有關偵錯工具以及如何偵錯 Visual Studio 中的程式碼，請參閱[逐步解說： 在 Visual Web Developer 的 偵錯 Web Pages](https://msdn.microsoft.com/library/z9e7w6cs.aspx)。

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>在 ASP.NET MVC 專案與 Visual Studio 中使用 Razor

Razor 語法也廣泛使用在 ASP.NET MVC 專案。 MVC 是功能強大、 以模式為基礎的方式建置動態網站。 如果您的 ASP.NET Web Pages 網站變得難以維護，您可能要考慮將它轉換成 ASP.NET MVC 應用程式。 如需建立 MVC 應用程式的範例，請參閱[開始使用 ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>安裝 Visual Studio 2010 中的 ASP.NET Web 網頁的支援

本節說明如何安裝 Visual Web Developer Express 2010 和 ASP.NET Web Pages Tools for Visual Studio。

1. 如果您還沒有 Web Platform Installer，請從下列 URL 下載：

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. 執行 Web Platform Installer。
3. 按一下**產品** 索引標籤。

    ![WebPI 產品 索引標籤](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. 搜尋**ASP.NET MVC 4** （適用於 ASP.NET Web Pages 2)，然後按一下 **新增**。 這些產品包含用於建置 ASP.NET Razor 網站的 Visual Studio 工具。

    ![WebPi 安裝選項](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. 按一下**安裝**以完成安裝。
