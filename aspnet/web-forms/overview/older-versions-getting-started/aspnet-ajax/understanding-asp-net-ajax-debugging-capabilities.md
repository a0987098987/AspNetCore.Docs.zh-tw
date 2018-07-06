---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: 了解 ASP.NET AJAX 偵錯功能 |Microsoft Docs
author: scottcate
description: 偵錯程式碼的能力是每位開發人員應該在其利器，不論他們使用的技術技能。 雖然許多開發人員...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 533eb8d2faf735915fa5cf5044db09d0ab636938
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390968"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>了解 ASP.NET AJAX 偵錯功能
====================
藉由[Scott Cate](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> 偵錯程式碼的能力是每位開發人員應該在其利器，不論他們使用的技術技能。 雖然許多開發人員都習慣使用 VB.NET 或 C# 程式碼的 ASP.NET 應用程式偵錯使用 Visual Studio.NET 或 Web Developer Express，有些不曉得還有非常適合用來偵錯 JavaScript 等用戶端程式碼。 相同類型的偵錯.NET 應用程式所使用的技術也可以套用更具體來說是 ASP.NET AJAX 應用程式與啟用 AJAX 的應用程式。


## <a name="debugging-aspnet-ajax-applications"></a>偵錯 ASP.NET AJAX 應用程式

Dan Wahlin

偵錯程式碼的能力是每位開發人員應該在其利器，不論他們使用的技術技能。 不用說，了解可用的不同的偵錯選項可以省下大量的時間上的專案和甚至幾個問題。 雖然許多開發人員都習慣使用 VB.NET 或 C# 程式碼的 ASP.NET 應用程式偵錯使用 Visual Studio.NET 或 Web Developer Express，有些不曉得還有非常適合用來偵錯 JavaScript 等用戶端程式碼。 相同類型的偵錯.NET 應用程式所使用的技術也可以套用更具體來說是 ASP.NET AJAX 應用程式與啟用 AJAX 的應用程式。

在這篇文章中，您會看到如何快速找出 bug 和其他問題的 ASP.NET AJAX 應用程式偵錯使用 Visual Studio 2008 和其他工具。 這個討論將包含啟用 Internet Explorer 6 或更高的偵錯、 逐步執行程式碼中使用 Visual Studio 2008 和指令碼總管，以及使用其他可用的工具，例如 Web Development Helper 的相關資訊。 您也將了解如何在 Firefox 中使用延伸模組名為 Firebug 可以讓您逐步執行 JavaScript 程式碼，直接在沒有任何其他工具的瀏覽器中的 ASP.NET AJAX 應用程式進行偵錯。 最後，您將介紹 ASP.NET AJAX 程式庫，可協助進行各種偵錯的工作，例如追蹤和程式碼判斷提示陳述式中的類別。

您嘗試偵錯在 Internet Explorer 中檢視的頁面之前有幾個基本步驟，您必須執行才能進行偵錯。 讓我們看看要開始執行一些基本設定需求。

## <a name="configuring-internet-explorer-for-debugging"></a>設定 Internet Explorer 偵錯

大部分的人不感興趣發現發現使用 Internet Explorer 檢視網站上的 JavaScript 問題。 事實上，一般使用者不會甚至知道他們會看到一則錯誤訊息時該怎麼辦。 如此一來，偵錯的選項為關閉狀態預設會在瀏覽器。 不過，它是非常簡單，開啟偵錯，並使其使用您在開發新的 AJAX 應用程式。

若要啟用偵錯功能，請移至工具功能表中的 Internet Explorer 的網際網路選項並選取 進階 索引標籤。瀏覽 區段中，請確定下列項目會取消核取：

- 停用指令碼除錯 (Internet Explorer)
- 停用指令碼除錯 （其他）

雖然並非必要，如果您嘗試偵錯應用程式，您可能會想要立即可見及明顯頁面中的任何 JavaScript 錯誤。 您可以強制所有錯誤的 「 顯示每個指令碼錯誤的相關通知 」 核取方塊顯示訊息方塊。 雖然這是絕佳的選項，當您開發應用程式開啟時，它可能很快就會造成困擾，如果因為發生 JavaScript 錯誤的機率都相當好只研究過其他網站。

圖 1 顯示何種 Internet Explorer 進階對話方塊應該看起來之後它已正確設定偵錯。


[![設定 Internet Explorer 偵錯。](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**圖 1**： 設定 Internet Explorer 進行偵錯。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


偵錯已開啟，您會看到新的功能表項目，會出現在名為指令碼偵錯工具的 [檢視] 功能表。 它在下一個陳述式有兩個選項可用，包括開啟和符號。 選取開啟時將會提示您進行偵錯 [Visual Studio 2008 （請注意，Visual Web Developer Express 也可用來偵錯）] 頁面。 如果目前正在執行 Visual Studio.NET，您可以選擇使用該執行個體，或建立新的執行個體。 選取下一個陳述式在中斷時將會提示您執行的 JavaScript 程式碼時，偵錯 頁面。 如果 JavaScript 程式碼會執行頁面的 onLoad 事件中，您可以重新整理頁面以觸發程序的偵錯工作階段。 如果按下按鈕後執行的 JavaScript 程式碼則會執行偵錯工具，在按下按鈕之後，立即。

> *> [!NOTE] 如果您已啟用，使用者存取控制 (UAC) 的 Windows Vista 上執行，而且您必須設定為系統管理員身分執行 Visual Studio 2008，Visual Studio 將無法附加至處理序，當系統提示您連結。若要解決此問題，首先，啟動 Visual Studio，並使用該執行個體來偵錯。*


雖然下一節將示範如何偵錯 ASP.NET AJAX 頁面直接從 Visual Studio 2008 中的，使用 Internet Explorer 指令碼偵錯工具選項時，頁面已經開啟，且您想要更完整檢查。

## <a name="debugging-with-visual-studio-2008"></a>偵錯使用 Visual Studio 2008

Visual Studio 2008 提供世界各地的開發人員依賴每天偵錯.NET 應用程式的偵錯功能。 內建的偵錯工具可讓您逐步執行程式碼，呼叫堆疊，還有更多資源，監視物件的資料，對於特定變數，監看式 檢視。 除了在 VB.NET 或 C# 程式碼偵錯，偵錯工具也很有用的偵錯 ASP.NET AJAX 應用程式，並可讓您逐步執行 JavaScript 程式碼行。 跟隨焦點可以用來偵錯用戶端指令碼檔案，而不是提供 discourse 偵錯使用 Visual Studio 2008 的應用程式的整體程序上的技術詳細資料。

偵錯 Visual Studio 2008 中的頁面的處理序可以啟動數個不同的方式。 首先，您可以使用上一節中所述的 Internet Explorer 指令碼偵錯工具選項。 這適用於瀏覽器中已載入的頁面和您想要開始偵錯時，正常。 或者，您可以以滑鼠右鍵按一下方案總管] 中的.aspx 頁面，並從功能表中選取 [設定為起始頁。 如果您已經習慣偵錯 ASP.NET 網頁則您可能已經做過過。 一旦按下 F5 就可以偵錯 頁面。 不過，您通常可以任意處設定中斷點時您想要在 VB.NET 或 C# 程式碼中，不一定與 JavaScript 的情況下一步，您會看到。

*內嵌和外部指令碼*

Visual Studio 2008 偵錯工具會內嵌在外部 JavaScript 檔案的不同頁面的 JavaScript。 外部指令碼檔案，您可以開啟檔案，並在您選擇的任一行上設定中斷點。 按一下左側的 [程式碼編輯器] 視窗的 [灰色的紙匣] 區域中，可以設定中斷點。 當 JavaScript 直接內嵌至頁面上，使用`<script>`標記中，按一下灰色的紙匣 區域中設定中斷點不可行。 內嵌指令碼的行上設定中斷點的嘗試會導致的警告訊息 「 不是有效的位置，中斷點 」。

您可以藉由將程式碼移到外部.js 檔案，並參考使用的 src 屬性取得解決此問題&lt;指令碼&gt;標記：


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

如果將程式碼移到外部檔案辦不到或是需要更多的工作，比起值得嗎？ 雖然您無法設定中斷點，使用編輯器，您可以新增直接在您想要開始偵錯的程式碼的偵錯工具陳述式。 您也可以使用 ASP.NET AJAX 程式庫中的 Sys.Debug 類別可用來強制啟動偵錯。 您將進一步了解本文稍後的 Sys.Debug 類別。

使用的範例`debugger`關鍵字如 [列表 1] 所示。 此範例會強制偵錯工具正確更新函式呼叫之前。

**列表 1。您可以使用 偵錯工具關鍵字來強制 Visual Studio.NET 偵錯工具中斷。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

一旦叫用偵錯工具陳述式時您會提示您進行偵錯使用 Visual Studio.NET 的網頁，而可以開始逐步執行程式碼。 執行您可能會遇到存取在頁面中讓我們使用 ASP.NET AJAX 程式庫的指令碼檔案的問題時看看使用 Visual Studio。NET 的指令碼總管 中。

## <a name="using-visual-studio-net-windows-to-debug"></a>使用 Visual Studio.NET Windows 偵錯

一旦啟動偵錯工作階段並開始逐步解說的程式碼使用預設 F11 鍵，您可能會遇到錯誤所示的對話方塊中看到 圖 2，除非在網頁中使用的所有指令碼檔案是開放且可供偵錯。


[![沒有原始程式碼不供偵錯時顯示錯誤對話方塊。](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**圖 2**： 沒有原始程式碼不供偵錯時顯示錯誤對話方塊。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


因為 Visual Studio.NET 不確定如何取得原始碼的頁面，參考的指令碼的一部分，會顯示此對話方塊。 雖然這可以是相當令人一開始，沒有簡單的修正程式。 一旦您啟動偵錯工作階段並叫用中斷點，請移至 [偵錯 Windows 指令碼總管] 視窗，在 Visual Studio 2008 功能表上，或使用 Ctrl + Alt + N 快速鍵。

> *> [!NOTE] 如果看不到列出的指令碼總管 功能表，請移至工具**自訂**在 Visual Studio.NET] 功能表上的命令。在 [類別] 區段中找出偵錯項目，然後按一下以顯示所有可用的功能表項目。在 [命令] 清單中，捲動到指令碼總管]，並將它拖曳拖曳至偵錯* *Windows 功能表中的先前所述。如此一來，即可使用 [指令碼總管] 功能表項目每次執行 Visual Studio.NET。*


指令碼總管 可以用於檢視頁面中使用的所有指令碼，並在程式碼編輯器中開啟它們。 [指令碼總管] 中開啟後，按兩下目前所偵錯在程式碼編輯器視窗中開啟它的.aspx 頁面。 所有其他指令碼總管 中顯示的指令碼執行相同的動作。 一旦所有指令碼會在程式碼視窗中，您可以開啟來逐步執行程式碼，按下 F11 （和使用其他偵錯快速鍵）。 [圖 3] 顯示指令碼總管的範例。 它會列出正在偵錯目前的檔案 (Demo.aspx) 以及兩個自訂的指令碼和由 ASP.NET AJAX ScriptManager 中以動態方式插入頁面的兩個指令碼。


[![指令碼總管 可輕鬆存取 頁面中所使用的指令碼。](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**圖 3**。 指令碼總管 可輕鬆存取 頁面中所使用的指令碼。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


數個其他 windows 也可用來提供有用的資訊，當您逐步執行程式碼，在網頁中。 例如，您可以使用 [區域變數] 視窗，以查看在頁面中，評估特定變數或條件，並檢視輸出的即時運算視窗所使用的不同變數的值。 您也可以使用 [輸出] 視窗來檢視追蹤陳述式寫出使用 Sys.Debug.trace 函式 （這將涵蓋在本文稍後） 或 Internet Explorer 的 Debug.writeln 函式。

當您逐步執行程式碼偵錯工具使用您可以將滑鼠移到變數中的程式碼，以檢視指派的值。 不過，指令碼偵錯工具有時不會顯示任何項目，您將滑鼠移指定的 JavaScript 變數。 若要查看的值，醒目提示的陳述式或您想要在程式碼編輯器視窗中看到，然後將滑鼠移它的變數。 雖然這項技巧並不適用於每一種情況，許多次您將能夠查看而不需要在不同的偵錯視窗中，例如 [區域變數] 視窗中尋找的值。

可在檢視影片教學課程中示範此處所討論的功能的一些[ http://www.xmlforasp.net ](http://www.xmlforasp.net)。

## <a name="debugging-with-web-development-helper"></a>使用 Web Development Helper 偵錯

雖然 Visual Studio 2008 （和 Visual Web Developer Express 2008） 是功能很強大偵錯工具，還有其他選項也可以使用，也就是更輕量級。 發行的最新工具的其中一個是 Web Development Helper。 Microsoft 的 Nikhil Kothari （主要 ASP.NET AJAX 架構設計人員，microsoft 其中之一） 撰寫了此絕佳的工具，可以從簡單的偵錯檢視 HTTP 要求和回應訊息執行許多不同的工作。 您可以在下載 web Development Helper [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx)。

直接在 Internet Explorer 可以方便地使用可用 web Development helper。 它會從 Internet Explorer 的功能表中選取工具 Web Development Helper 來啟動。 這是不錯的因為您不必離開瀏覽器，以執行數個工作，例如 HTTP 要求和回應訊息的記錄之瀏覽器的下半部中，這會開啟此工具。 圖 4 顯示 Web Development Helper 作用中的外觀。


[![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**圖 4**: Web Development Helper ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web Development helper 不是您將使用的工具來逐步執行逐行程式碼做為 Visual Studio 2008。 不過，它可用來檢視追蹤輸出，輕鬆地評估指令碼中的變數，或瀏覽資料是 JSON 物件內。 它也是非常適合檢視 ASP.NET AJAX 頁面和伺服器來回傳遞資料。

在 Internet Explorer 中開啟 Web Development Helper 後，指令碼偵錯必須由啟用指令碼啟用指令碼偵錯從功能表中選取 Web Development helper 稍早在 圖 4 中所示。 這可讓此工具來執行頁面時，就會發生的攔截錯誤。 它也可讓網頁中輸出追蹤訊息的簡易存取。 若要檢視追蹤資訊或執行指令碼命令，以測試不同的函式，在頁面內，選取 從 Web Development Helper 功能表的 指令碼顯示指令碼主控台。 這提供存取命令視窗和簡單的 [即時運算] 視窗。

*檢視追蹤訊息和物件的 JSON 資料*

即時運算視窗可用來執行指令碼命令，或甚至載入或儲存用來在網頁中測試不同的函式的指令碼。 [命令] 視窗會顯示正在檢視的頁面寫出的追蹤或偵錯訊息。 列表 2 說明如何撰寫使用 Internet Explorer 的 Debug.writeln 函式的追蹤訊息。

**列表 2。寫出使用 Debug 類別將用戶端追蹤訊息。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

LastName 屬性包含 Doe 的值，如果 Web Development Helper 會顯示訊息 「 人名稱： Doe 」 在指令碼主控台的命令視窗中 （假設，偵錯已啟用）。 Web Development Helper 也會將最上層 debugService 物件加入到可用來寫出追蹤資訊，或檢視其內容的 JSON 物件的頁面。 列表 3 顯示使用 debugService 類別的追蹤函式的範例。

**列表 3。您可以使用 Web Development Helper 的 debugService 類別來寫入追蹤訊息。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

DebugService 類別的不錯的功能是，它會處理即使未啟用偵錯在 Internet Explorer 中輕鬆執行 Web Development Helper 時存取追蹤的資料。 此工具不使用偵錯 頁面，將會忽略追蹤陳述式，因為 window.debugService 的呼叫會傳回 false。

DebugService 類別也可讓 JSON 物件資料，以使用 Web Development Helper 的偵測器視窗來檢視。 列表 4 時，會建立簡單的 JSON 物件，包含個人資料。 一旦建立物件時，進行呼叫以 debugService 類別的檢查函式，允許以視覺化方式檢查的 JSON 物件。

**列表 4。若要檢視物件的 JSON 資料使用 debugService.inspect 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

在網頁中或透過即時運算視窗呼叫 GetPerson() 函式會在物件偵測器對話方塊視窗中出現 圖 5 所示。 在物件內的屬性可以動態變更反白，變更顯示在 [值] 文字方塊中的值，然後按一下 [更新] 連結。 使用物件偵測器讓您更容易檢視 JSON 物件的資料，以及試驗不同的值套用至屬性。

*偵錯錯誤*

除了允許追蹤資料和要顯示的 JSON 物件，Web Development helper 可以也有助於偵錯頁面中的錯誤。 如果發生錯誤，您會提示來繼續至下一行程式碼或指令碼偵錯 （請參閱 圖 6）。 指令碼錯誤對話方塊視窗會顯示完整的呼叫堆疊以及行號，因此您可以輕鬆地識別問題所在位置，在指令碼內。


[![您可以使用物件偵測器視窗來檢視 JSON 物件。](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**圖 5**： 使用物件偵測器視窗來檢視 JSON 物件。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


選取 偵錯選項，可讓您直接在檢視變數的值，寫出 JSON 物件，還有更多的 Web Development Helper 的即時運算視窗中執行指令碼陳述式。 如果再次執行相同動作觸發錯誤，而且在電腦上使用 Visual Studio 2008，系統會提示您啟動偵錯工作階段，讓您可以逐步執行程式碼行上一節中所述。


[![Web Development Helper 指令碼錯誤對話方塊](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**圖 6**: Web Development Helper 的指令碼錯誤對話方塊 ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*檢查要求和回應訊息*

偵錯 ASP.NET AJAX 頁面時通常很有用來查看網頁和伺服器之間傳送的要求和回應訊息。 檢視訊息內的內容，可讓您查看是否訊息的大小以及傳遞適當的資料。 Web Development Helper 提供絕佳 HTTP 訊息記錄器的功能，可讓您輕鬆地檢視資料，做為未經處理文字或更容易閱讀的格式。

若要檢視 ASP.NET AJAX 要求和回應訊息，您必須啟用 HTTP 記錄器，從 Web Development Helper 功能表中選取 HTTP 啟用 HTTP 記錄。 啟用之後，就可以檢視所有從目前頁面傳送的訊息可以透過選取 HTTP 顯示 HTTP 記錄檔的 HTTP 記錄檔檢視器中。

雖然檢視每個要求/回應訊息中傳送的原始文字是的確很實用 （和一個選項，在 Web Development Helper） 通常很容易以圖形顯示更豐富的格式檢視訊息資料。 一旦已啟用 HTTP 記錄，而且已將訊息記錄，可以檢視訊息資料，方法是按兩下 HTTP 記錄檔檢視器中的訊息。 如此一來，可讓您檢視所有訊息，以及實際的訊息相關聯的標頭內容。 [圖 7] 顯示要求訊息和回應訊息，在 [HTTP 記錄檔檢視器] 視窗中檢視的範例。


[![您可以使用 HTTP 記錄檔檢視器來檢視要求和回應訊息資料。](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**圖 7**： 使用 HTTP 記錄檔檢視器來檢視要求和回應訊息資料。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


HTTP 記錄檔檢視器會自動剖析 JSON 物件，並顯示它們使用樹狀檢視，就能快速又容易檢視物件的屬性資料。 當 UpdatePanel 用於 ASP.NET AJAX 頁面中時，檢視器細分成個別的組件訊息的每一個部分如 圖 8 所示。 這是很棒的功能，可讓您更容易查看並了解什麼是相較於檢視未經處理的訊息資料訊息中。


[![使用 HTTP 記錄檔檢視器檢視 UpdatePanel 回應訊息。](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**圖 8**： 使用 HTTP 記錄檔檢視器檢視 UpdatePanel 回應訊息。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


有數個其他工具，可用來檢視 Web Development Helper 除了要求和回應訊息。 另一個不錯的選項是可免費在 Fiddler [ http://www.fiddlertool.com ](http://www.fiddlertool.com)。 雖然此處不會討論 Fiddler，它也是不錯的選項時您需要徹底地檢查訊息標頭和資料。

## <a name="debugging-with-firefox-and-firebug"></a>使用 Firefox 和 Firebug 偵錯

而 Internet Explorer 仍是最廣泛使用的瀏覽器，Firefox 等其他瀏覽器變得很熱門，並使用更多。 如此一來，您會想要檢視和偵錯您的 ASP.NET AJAX 頁面，在 Firefox 與 Internet Explorer 以確保您的應用程式正常運作。 雖然 Firefox 不能直接在 Visual Studio 2008 有相同的偵錯，它會有呼叫可用來偵錯網頁的 Firebug 延伸模組。 Firebug 可以免費下載方法是前往[ http://www.getfirebug.com ](http://www.getfirebug.com)。

Firebug 提供功能完整的偵錯環境，可用來逐步執行逐行程式碼、 存取網頁內使用的所有指令碼、 檢視 DOM 結構、 在網頁中顯示的 CSS 樣式，甚至是追蹤事件發生。 安裝之後，可以從 [Firefox] 功能表中選取工具 Firebug 開啟 Firebug 存取 Firebug。 Web Development Helper，例如 Firebug 都使用直接在瀏覽器中，不過它也可用來當做獨立的應用程式。

一旦執行了 Firebug，中斷點可以設定 JavaScript 檔案的任一行，是否或不在網頁中內嵌指令碼。 若要設定中斷點，您必須先將載入適當的頁面，您想要在 Firefox 中偵錯。 頁面載入之後，選取要從下拉式清單中 Firebug 的指令碼偵錯的指令碼。 將顯示在頁面所使用的所有指令碼。 依序按一下 Firebug 的灰色的紙匣 區域的行中斷點去處必須如同 Visual Studio 2008 中設定中斷點。

在 Firebug 中設定中斷點後，您可以執行執行指令碼需要例如按一下按鈕，或重新整理瀏覽器的 onLoad 事件觸發程序進行偵錯所需的動作。 包含中斷點的那一行上，將會自動停止執行。 圖 9 顯示已觸發中斷點的範例，在 Firebug 中。


[![處理在 Firebug 中的中斷點。](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**圖 9**： 處理在 Firebug 中的中斷點。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


一旦叫用中斷點時，可以逐步執行、 不進入或跳離程式碼使用箭號按鈕。 當您逐步執行程式碼，指令碼變數會顯示在 偵錯工具可讓您看到的值，並向下鑽研到物件的右側部分。 Firebug 也包括呼叫堆疊下拉式清單中，若要檢視導致目前正在進行偵錯這一行的指令碼的執行步驟。

Firebug 也包含可用來測試不同的指令碼陳述式、 評估變數和檢視追蹤輸出的主控台視窗。 它是在 Firebug 視窗頂端的 [主控台] 索引標籤，即可存取。 正在進行偵錯頁面可以也 「 檢查 」 以查看其 DOM 結構和內容，依序按一下 [檢查] 索引標籤上。為您的結束滑鼠停留不同頁面的適當部分所示的偵測器視窗的 DOM 項目將會反白顯示讓您輕鬆查看頁面中的使用項目。 指定的項目相關聯的屬性值可以 「 即時 」 協助嘗試套用不同的寬度、 樣式等項目的變更。 這是不錯的功能，讓您不必經常原始程式碼編輯器與 Firefox 瀏覽器來檢視如何簡單的變更會影響頁面之間切換。

[圖 10] 顯示使用 DOM 偵測器來尋找名為 txtCountry 頁面中的 textbox 的範例。 Firebug 偵測器也可用來檢視頁面，以及發生例如追蹤滑鼠移動、 按鈕點選，還有更多的事件中所使用的 CSS 樣式。


[![使用 Firebug 的 DOM 偵測器。](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**圖 10**： 使用 Firebug DOM 偵測器。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug 提供輕量的方式，快速地偵錯直接在 Firefox，以及一個優異的工具來檢查網頁內的不同項目中的頁面。

## <a name="debugging-support-in-aspnet-ajax"></a>ASP.NET AJAX 中的偵錯支援

ASP.NET AJAX 程式庫包含許多不同的類別可用來簡化新增至網頁的 AJAX 功能的程序。 您可以使用這些類別，尋找頁面內的項目和操作這些、 加入新的控制項，呼叫 Web 服務和甚至是處理事件。 ASP.NET AJAX 程式庫也包含可用來增強偵錯頁面的程序的類別。 在本節中您將介紹 Sys.Debug 類別，並了解如何在應用程式中使用。

*使用 Sys.Debug 類別*

Sys.Debug 類別 （位於 Sys 命名空間中的 JavaScript 類別） 可用來執行數個不同的功能，包括寫入追蹤輸出、 執行程式碼判斷提示和強制執行失敗，讓它可以進行偵錯的程式碼。 它廣泛用於 ASP.NET AJAX 程式庫的偵錯檔案 （位於 C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 預設安裝） 來執行條件式的測試 （稱為判斷提示），請確定函式物件，包含預期的資料，以及撰寫追蹤陳述式正確傳遞參數。

Sys.Debug 類別會公開數個不同的函式可用來處理追蹤、 程式碼判斷提示或失敗，如表 1 中所示。

**表 1。Sys.Debug 類別函式。**

| **函式名稱** | **描述** |
| --- | --- |
| 判斷提示條件、 訊息 （displayCaller） | 判斷提示條件參數為 true。 如果正在測試的條件為 false，訊息方塊將會用來顯示訊息的參數值。 如果 displayCaller 參數為 true，則方法也會顯示呼叫者的相關資訊。 |
| clearTrace() | 清除追蹤作業的陳述式輸出。 |
| fail(message) | 會導致程式停止執行並偵錯工具會中斷。 訊息參數可用來提供失敗的原因。 |
| trace(message) | 寫入追蹤輸出中的訊息參數。 |
| traceDump （物件、 名稱） | 輸出的可讀取的格式物件的資料。 Name 參數可用來提供用於追蹤傾印的標籤。 根據預設，物件傾印內任何子物件會寫出。 |

用戶端追蹤可以用於相同的方式為 ASP.NET 中所提供的追蹤功能。 它可讓不同的訊息容易看到而不會中斷應用程式的流程。 列表 5 顯示使用 Sys.Debug.trace 函式，將寫入追蹤記錄檔的範例。 此函數只接受應該寫出做為參數的訊息。

**列表 5。使用 Sys.Debug.trace 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

如果您執行 [列表 5] 所示的程式碼不會看到任何追蹤輸出，在頁面中。 若要查看它的唯一方法是使用適用於 Visual Studio.NET、 Web Development Helper 或 Firebug 主控台視窗。 如果您想查看追蹤輸出的頁面中，將需要新增 TextArea 標記，並為它提供 TraceConsole 識別碼，如下所示：


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

將寫入 TraceConsole TextArea Sys.Debug.trace 中任何陳述式的頁面。

在您要查看包含在 JSON 物件內資料的情況下，您可以使用 Sys.Debug 類別的 traceDump 函式。 此函數會採用兩個參數，包括應該追蹤主控台傾印的物件和可用來識別追蹤輸出中的物件的名稱。 列表 6 顯示使用 traceDump 函式的範例。

**列表 6。使用 Sys.Debug.traceDump 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

[圖 11] 顯示呼叫 Sys.Debug.traceDump 函式的輸出。 請注意，除了撰寫出 Person 物件的資料，它也會寫入位址子物件的資料。

除了追蹤 Sys.Debug 類別也可以用來執行程式碼判斷提示。 判斷提示用來測試應用程式執行時，就會符合特定條件。 ASP.NET AJAX 程式庫指令碼的偵錯版本包含幾個判斷提示陳述式來測試各種不同的條件。

列表 7 顯示使用 Sys.Debug.assert 函式來測試條件的範例。 程式碼會測試更新 Person 物件之前，位址物件為 null。


[![Sys.Debug.traceDump 函式的輸出。](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**圖 11**: Sys.Debug.traceDump 函式的輸出。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**列表 7。使用 debug.assert 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

三個參數會傳遞包含要評估，判斷提示會傳回 false，而且應該顯示呼叫者的相關資訊時所要顯示訊息的條件。 在其中判斷提示失敗的情況下，訊息會顯示呼叫端資訊以及第三個參數，則為 true 時。 [圖 12 顯示失敗對話方塊列表 7] 所示的判斷提示失敗時所顯示的範例。

最後的函式，以涵蓋是 Sys.Debug.fail。 當您想要強制在指令碼中的特定資料行上失敗的程式碼可以加入 Sys.Debug.fail 呼叫，而不是 JavaScript 應用程式中一般使用的偵錯工具陳述式。 Sys.Debug.fail 函式會接受單一字串參數，表示失敗的原因，如下所示：


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Sys.Debug.assert 失敗訊息。](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**圖 12**: Sys.Debug.assert 失敗訊息。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


當遇到 Sys.Debug.fail 陳述式時執行指令碼時，訊息參數的值會顯示在主控台中的偵錯應用程式，例如 Visual Studio 2008，並將提示您進行偵錯應用程式。 其中，這可以是非常有用的一個案例是當您無法在內嵌指令碼上設定中斷點，以使用 Visual Studio 2008，但想要的程式碼，以停止在特定的行，因此您可以檢查變數的值。

*了解 ScriptManager 控制項的 ScriptMode 屬性*

ASP.NET AJAX 程式庫包含偵錯和發行指令碼版本，依預設會安裝在 C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0。 偵錯指令碼是正確格式化，方便閱讀，有數個 Sys.Debug.assert 呼叫分散於它們時發行指令碼已清除的空白字元，並謹慎使用 Sys.Debug 類別，其整體大小降到最低。

ScriptManager 控制項加入 ASP.NET AJAX 頁面讀取來判斷哪些版本的程式庫載入指令碼的 web.config 中的編譯項目的偵錯屬性。 不過，您可以控制是否偵錯或發行指令碼會載入 （程式庫指令碼或您自己的自訂指令碼） 變更的 ScriptMode 屬性。 ScriptMode 接受其成員包括自動、 偵錯、 發行和繼承的 ScriptMode 列舉型別。

ScriptMode 預設為 [自動] 代表 ScriptManager 會檢查 web.config 中的偵錯屬性的值。當為 false 時偵錯 ScriptManager 會載入 ASP.NET AJAX 程式庫指令碼的發行版本。 偵錯，則為 true 時，將會載入指令碼的偵錯版本。 變更版本或偵錯的 ScriptMode 屬性將會強制 ScriptManager 載入適當的指令碼，不論 web.config 中的偵錯屬性的值。列表 8 顯示使用 ScriptManager 控制項，從 ASP.NET AJAX 程式庫載入偵錯指令碼的範例。

**列表 8。正在載入偵錯指令碼使用 ScriptManager**。


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

您也可以使用 ScriptManager 的指令碼屬性以及 ScriptReference 元件列表 9 所示，以載入您自己自訂的指令碼的不同版本 （debug 或 release）。

**列表 9。使用 ScriptManager 載入自訂指令碼。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> 如果您正在載入使用 ScriptReference 元件的自訂指令碼必須在指令碼底部的 指令碼中加入下列程式碼完成載入時通知 ScriptManager:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

列表 9 所示的程式碼會告訴尋找人員指令碼的偵錯版本，因此它會自動尋找而不是 Person.js Person.debug.js ScriptManager。 Person.debug.js 是找不到檔案，就會引發錯誤。

在您想要偵錯或發行版本要載入的自訂指令碼會根據在 ScriptManager 控制項上設定的 ScriptMode 屬性值的情況下，您可以設定成 Inherit 的 ScriptReference 控制項的 ScriptMode 屬性。 這會導致要載入的自訂指令碼的適當版本根據 ScriptManager 的 ScriptMode 屬性，如 [列表 10] 所示。 因為 ScriptManager 控制項的 ScriptMode 屬性設定為 偵錯時，就會載入並使用於頁面 Person.debug.js 指令碼。

**列表 10。繼承的自訂指令碼 ScriptManager 的 ScriptMode。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

適當地使用 ScriptMode 屬性可以更輕鬆地偵錯應用程式，並簡化整體程序。 ASP.NET AJAX 程式庫的發行指令碼會逐步執行和讀取程式碼格式設定已移除時偵錯指令碼會格式化專為偵錯的目的並非易事。

## <a name="conclusion"></a>結論

Microsoft ASP.NET AJAX 技術，可提供穩固的基礎建置 AJAX 功能的應用程式，可增強使用者的整體體驗。 不過，為任何程式設計技術，bug 和其他應用程式的問題將會確實發生。 了解可用的其他偵錯選項可以省下許多時間與結果更穩定的產品中。

這篇文章中您已學到數個不同的技術，用於偵錯 ASP.NET AJAX 頁面，包括 Visual Studio 2008、 Web Development Helper 與 Firebug 的 Internet Explorer。 這些工具可以簡化整體的偵錯程序，因為您可以存取變數的資料，將逐步解說程式碼行，並檢視追蹤陳述式。 除了討論的其他偵錯工具，您也看到了如何使用 ASP.NET AJAX 程式庫的 Sys.Debug 類別，在應用程式以及如何使用 ScriptManager 類別，將偵錯或發行指令碼的版本。

## <a name="bio"></a>簡歷

Dan Wahlin （Microsoft 最有價值專家適用於 ASP.NET 和 XML Web Service） 是介面的技術訓練的.NET 開發的講師和架構顧問 ([www.interfacett.com)](http://www.interfacett.com)。 Dan 建構的 XML for ASP.NET 開發人員網站 ([www.XMLforASP.NET](http://www.XMLforASP.NET)) 上的 INETA 講師機構，在數個會議上發表演說。 Dan 合著 Professional Windows DNA (Wrox)，ASP.NET： 秘訣、 教學課程和程式碼 (Sam)、 ASP.NET 1.1 Insider 解決方案、 Professional ASP.NET 2.0 AJAX (Wrox)、 ASP.NET 2.0 MVP Hacks 和 ASP.NET 開發人員 (Sam) 撰寫的 XML。 當他不撰寫程式碼、 文章或書籍時，Dan 工作之餘喜歡撰寫和錄製音樂和播放高爾夫球和他的妻子和孩子的籃球。

Scott Cate 從事 Microsoft Web 技術工作自 1997 年，而且 myKB.com 總裁 ([www.myKB.com](http://www.myKB.com))，專精於撰寫 ASP.NET 架構著重於知識庫軟體解決方案的應用程式。 可以透過電子郵件連絡 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的部落格[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [上一步](understanding-asp-net-ajax-web-services.md)
