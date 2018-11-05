---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: 程式設計 ASP.NET Web Pages (Razor) 使用 Visual Studio |Microsoft Docs
author: Rick-Anderson
description: 此附錄將解釋如何使用 Visual Studio 2010 或 Visual Web Developer 2010 Express 含有 Razor 語法的 ASP.NET Web Pages 程式。
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 5b8df17ec1021d133579e23cb4f5b0d0f67d4c7c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020451"
---
<a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>程式設計使用 Visual Studio 的 ASP.NET Web Pages (Razor)
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何使用 Visual Studio 或 Visual Web Developer Express 程式 ASP.NET Web Pages (Razor) 網站。
>
> 您將學到什麼
>
> - 您需要以 ASP.NET Web Pages 用於您版本的 Visual Studio 安裝 （如果有任何項目）。
> - 如何加入支援 ASP.NET Web Pages Visual Web Developer 2010 Express。
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
> 本教學課程也適用於 ASP.NET Web Pages 2、 與 Visual Studio 2012、 Visual Studio 2010，WebMatrix 2。


您可以使用 Razor 語法使用 WebMatrix 或許多其他程式碼編輯器進行程式設計的 ASP.NET Web pages。 您也可以使用 Microsoft Visual Studio 也就是功能完整的整合式的開發環境 (IDE)，提供一組強大的工具建立的多種應用程式 （不只是網站）。 若要使用 ASP.NET Razor 頁面，您可以使用[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)。

Visual Studio 提供使用 ASP.NET Razor 網頁進行程式設計的兩個特別有用功能如下：

- *IntelliSense*。 Visual Studio 內建的 IntelliSense 功能會比在 WebMatrix 中的 IntelliSense 更完整。
- *偵錯工具*。 偵錯工具可讓您疑難排解您的程式碼執行、 檢查變數，並逐步執行逐行程式碼時停止程式。

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>使用 Visual Studio 使用不同版本的 ASP.NET Web Pages

若要開發 Visual Studio 2017 中的 ASP.NET web 應用程式，安裝**ASP.NET 和 web 開發**工作負載。

Visual Studio 2012 和 Visual Studio 2013 包含支援 ASP.NET Web Pages。 （只有當您安裝 Visual Studio 會安裝才能支援 ASP.NET 網頁的封裝）。

Visual Studio 2010 不支援預設包含 ASP.NET Web Pages。 若要使用 Visual Studio 2010 中的 ASP.NET Web Pages，您必須安裝 ASP.NET MVC 套件。 若要取得 ASP.NET Web Pages 2，您會安裝 ASP.NET MVC 4。

下表摘要說明支援 ASP.NET Web Pages 中不同版本的 Visual Studio。

|  | Visual Studio 2010 | Visual Studio 2012 | Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | 安裝 ASP.NET MVC 4 | （包含） | （包含） |
| **ASP.NET Web Pages 3** |  | 更新至 ASP.NET Web Pages 3 透過 NuGet | （包含） |

若要使用 Visual Studio 2010，請參閱[安裝支援的 Visual Studio 2010 中 ASP.NET Web Pages](#vs2010support)。

## <a name="launching-visual-studio-from-webmatrix"></a>啟動 Visual Studio 從 WebMatrix

如果您已在 WebMatrix 中啟動專案，並想要切換至 Visual Studio，WebMatrix 便會提供按鈕，以輕鬆地在 Visual Studio 中開啟專案。 您必須安裝 Visual Studio 這個按鈕，在電腦上啟用。 下圖顯示在 WebMatrix 中的按鈕。

![啟動 Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

當您按一下按鈕時，就會在 Visual Studio 中開啟專案。 您可以切換來回 WebMatrix 和 Visual Studio 沒有任何問題。 如果任何檔案在另一個環境中已經變更，而且需要重新載入，才能取得最新的變更，您會收到通知。

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>在 Visual Studio 中建立 ASP.NET Razor 網站

若要在 Visual Studio 中建立 ASP.NET Razor 網站：

1. 開啟 Visual Studio。
2. 在 **檔案**功能表上，按一下**新的網站**。

    ![建立新的網站](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. 在 **新的網站**對話方塊方塊中，選取要使用 （Visual C# 或 Visual Basic） 的語言。
4. 選取  **ASP.NET 網站 (Razor)** 範本。

    ![razor 的站台](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. 按一下 [確定 **Deploying Office Solutions**]。

新的專案存在，而且會填入一些預設網頁 」，以協助您開始著手。

### <a name="using-intellisense"></a>Using IntelliSense

既然您已建立站台，您可以看到 IntelliSense 在 Visual Studio 中的運作方式。

1. 在您剛才建立的網站中，開啟*Default.cshtml*頁面。
2. 在後`<h3>`標記，在頁面中，輸入`@ServerInfo.`（包括點）。 請注意 IntelliSense 如何顯示可用的方法`ServerInfo`下拉式清單中的協助程式。

    ![intellisense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. 選取`GetHtml`方法清單，然後按 Enter 鍵。 IntelliSense 會自動填入此方法。 (使用 C# 中的任何方法，您必須新增`()`方法之後的字元。)完整程式碼`GetHtml`方法如以下範例所示：

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. 按 Ctrl + F5 執行頁面。 這是頁面看起來像時顯示在瀏覽器：

    ![在瀏覽器中的預設頁面](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. 關閉瀏覽器。

### <a name="using-the-debugger"></a>使用偵錯工具

1. 在頂端*Default.cshtml*頁面上，以開頭的行後方`Page.Title`，新增下列程式碼行：

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. 左邊的程式碼編輯器的灰色邊界，按一下旁邊的 這個新的一行以新增*中斷點*。 中斷點會告知偵錯工具停止在該點執行程式，以便您可以看到發生的事情的標記。

    ![設定中斷點](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. 移除對呼叫`ServerInfo.GetHtml`方法，並將呼叫加入`@myTime`變數在它的位置。 這個呼叫會顯示新的一行程式碼所傳回的目前時間值。
4. 按 F5 以偵錯工具中執行的頁面。 頁面會在您設定的中斷點上停止。 下圖顯示頁面的外觀在編輯器中使用的中斷點 （黃色）。

    ![偵錯中斷點](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. 在 偵錯 工具列中，按一下**逐步執行**執行下的一行程式碼 按鈕 （或按 F11）。 每次您按一下此按鈕時，您會繼續執行至下一行程式碼。

    ![逐步執行按鈕](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. 檢查的值`myTime`變數上方按住滑鼠指標，或藉由檢查中顯示的值**區域變數**並**呼叫堆疊**windows。 Visual Studio 會顯示變數的值。

    ![顯示的時間值](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. 完成檢查變數及逐步執行程式碼之後，請按 f5 鍵繼續執行而不需停止每一行程式碼的網頁。 當您完成逐步執行所有程式碼時，在瀏覽器會顯示頁面。

若要深入了解如何在 Visual Studio 中的程式碼進行偵錯以及偵錯工具，請參閱[逐步解說： 在 Visual Web Developer 的 偵錯 Web Pages](https://msdn.microsoft.com/library/z9e7w6cs.aspx)。

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>使用 Visual Studio 的 ASP.NET MVC 專案中使用 Razor

Razor 語法也會在 ASP.NET MVC 專案中廣泛使用。 MVC 是功能強大、 以模式為基礎的方式建置動態網站。 如果您的 ASP.NET Web Pages 網站變得難以維護，您可能要考慮將它轉換成 ASP.NET MVC 應用程式。 如需建立 MVC 應用程式的範例，請參閱 < [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md)。

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>在 Visual Studio 2010 中安裝 ASP.NET Web 網頁的支援

本節說明如何安裝 Visual Web Developer Express 2010 和 ASP.NET Web Pages Tools for Visual Studio。

1. 如果您還沒有 Web Platform Installer，請從下列 URL 下載：

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. 執行 Web Platform Installer。
3. 按一下 [**產品**] 索引標籤。

    ![WebPI 產品 索引標籤](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. 搜尋**ASP.NET MVC 4** （適用於 ASP.NET Web Pages 2)，然後按一下**新增**。 這些產品包括 Visual Studio 工具，用於建置 ASP.NET Razor 網站。

    ![WebPi 安裝選項](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. 按一下 **安裝**才能完成安裝。
