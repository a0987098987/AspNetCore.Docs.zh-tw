---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: "了解偵錯功能的 ASP.NET AJAX |Microsoft 文件"
author: scottcate
description: "若要偵錯程式碼的功能是每個開發人員應該要有其利器，無論他們所使用的技術的技術。 雖然許多開發人員..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 426d0182978faf7fc7516203fcc84ef0152790ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>了解 ASP.NET AJAX 偵錯功能
====================
由[Scott 類別](https://github.com/scottcate)

[下載 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> 若要偵錯程式碼的功能是每個開發人員應該要有其利器，無論他們所使用的技術的技術。 許多開發人員都習慣使用 Visual Studio.NET 或 Web Developer Express 偵錯 ASP.NET 應用程式使用 VB.NET 或 C# 程式碼，而不某些注意，它是也非常適合用來偵錯 JavaScript 等用戶端程式碼。 啟用 AJAX 的應用程式和更明確地說 ASP.NET AJAX 應用程式也可以套用相同類型的偵錯.NET 應用程式使用的技術。


## <a name="debugging-aspnet-ajax-applications"></a>偵錯 ASP.NET AJAX 應用程式

Dan Wahlin

若要偵錯程式碼的功能是每個開發人員應該要有其利器，無論他們所使用的技術的技術。 不用說，了解可用的不同的偵錯選項可以節省極大的時間的專案和甚至幾個問題。 許多開發人員都習慣使用 Visual Studio.NET 或 Web Developer Express 偵錯 ASP.NET 應用程式使用 VB.NET 或 C# 程式碼，而不某些注意，它是也非常適合用來偵錯 JavaScript 等用戶端程式碼。 啟用 AJAX 的應用程式和更明確地說 ASP.NET AJAX 應用程式也可以套用相同類型的偵錯.NET 應用程式使用的技術。

在本文中，您會看到如何偵錯來快速找出 bug 和其他問題的 ASP.NET AJAX 應用程式使用 Visual Studio 2008 和許多其他工具。 本討論將包含啟用 Internet Explorer 6 或更新版本，偵錯、 逐步執行程式碼使用 Visual Studio 2008 與指令碼總管，以及使用其他可用的工具，例如 Web 開發協助程式的相關資訊。 您也將學習如何在 Firefox 中使用擴充功能命名為 firebug 這類可以讓您逐步執行 JavaScript 程式碼直接在沒有任何其他工具的瀏覽器中的 ASP.NET AJAX 應用程式進行偵錯。 最後，您將介紹有助於不同的偵錯工作，例如追蹤和程式碼的判斷提示陳述式與 ASP.NET AJAX 程式庫中的類別。

您嘗試偵錯 Internet Explorer 中檢視的頁面之前有一些您需要執行以啟用偵錯的基本步驟。 讓我們看看需要若要開始執行一些基本設定需求。

## <a name="configuring-internet-explorer-for-debugging"></a>設定 Internet Explorer 偵錯

大部分的人不感興趣 JavaScript 會使用 Internet Explorer 檢視網站上遇到的問題。 事實上，一般使用者可能不甚至知道該怎麼辦看到一則錯誤訊息。 如此一來，偵錯選項會關閉預設會在瀏覽器。 不過，它是非常簡單，開啟偵錯，並將其做為開發新 AJAX 應用程式。

若要啟用偵錯功能，請移至 Internet Explorer 功能表上的工具網際網路選項並選取 進階 索引標籤。瀏覽 區段中，請確定下列項目已取消選取：

- 停用指令碼除錯 (Internet Explorer)
- 停用指令碼除錯 （其他）

雖然不必要的如果您嘗試偵錯應用程式，您可能會想要立即可見及明顯頁面中的任何 JavaScript 錯誤。 您可以強制所有藉由檢查 「 顯示每個指令碼錯誤的相關通知 」 核取方塊，以訊息方塊顯示的錯誤。 雖然這是一個絕佳的選項，雖然您正在開發應用程式開啟時，它可以快速地成為造成困擾，如果您只因為發生 JavaScript 錯誤的機率都相當好研究其他網站。

它已正確設定偵錯之後，看起來應該圖 1 顯示哪些 Internet Explorer 進階 對話方塊。


[![設定 Internet Explorer 偵錯。](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**圖 1**： 偵錯設定 Internet Explorer。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


偵錯已開啟，您會看到新的功能表項目，會出現在名為指令碼偵錯工具的 [檢視] 功能表。 它在下一個陳述式有兩個選項可用，包括開啟和中斷。 選取開啟時將會提示您進行偵錯 Visual Studio 2008 （請注意，Visual Web Developer Express 也會用於偵錯） 中的頁面。 如果目前正在執行 Visual Studio.NET，您可以選擇使用該執行個體，或建立的新執行個體。 選取下一個陳述式處中斷時將會提示您執行 JavaScript 程式碼時，偵錯網頁。 如果 JavaScript 程式碼執行的 onLoad 事件的頁面中，您可以重新整理頁面以觸發偵錯工作階段。 如果 JavaScript 程式碼執行之後的 button 已按下然後偵錯工具會在按下按鈕之後，立即執行。

> *>[!NOTE]如果您要啟用，使用者存取控制 (UAC) 的 Windows Vista 上執行，而且您擁有設定為系統管理員身分執行 Visual Studio 2008，Visual Studio 將會附加至處理序，當系統提示您附加失敗。若要解決此問題，請啟動 Visual Studio 第一次，並使用該執行個體來偵錯。*


雖然下一節將示範如何偵錯 ASP.NET AJAX 頁面直接從 Visual Studio 2008 中的，使用 Internet Explorer 指令碼偵錯工具選項時，頁面已經開啟，且您想要更完整檢查它。

## <a name="debugging-with-visual-studio-2008"></a>使用 Visual Studio 2008 偵錯

Visual Studio 2008 提供世界各地的開發人員必須依賴每天偵錯.NET 應用程式的偵錯功能。 內建的偵錯工具可讓您逐步執行程式碼，呼叫堆疊以及執行更多，監視物件資料，對於特定變數，監看式 檢視。 除了在 VB.NET 或 C# 程式碼進行偵錯，偵錯工具也很有用的偵錯 ASP.NET AJAX 應用程式，並可讓您逐步執行逐行 JavaScript 程式碼。 遵循焦點可用來偵錯用戶端指令碼檔案，而不是提供 discourse 偵錯使用 Visual Studio 2008 的應用程式的整體程序上的技術詳細資料。

可以數種不同的方式啟動的程序的偵錯 Visual Studio 2008 中的頁面。 首先，您可以使用上一節中所述的 Internet Explorer 指令碼偵錯工具選項。 這適用於網頁已經載入瀏覽器中，您想要開始偵錯時正常。 或者，您可以在 方案總管 中的.aspx 頁面上按一下滑鼠右鍵，並從功能表選取 設定為起始頁。 如果您已經習慣偵錯 ASP.NET 網頁然後可能完成之前。 一旦按下 F5 就可以偵錯頁面。 不過，您通常可以任意處設定中斷點時您會希望在 VB.NET 或 C# 程式碼中，不一定是以 JavaScript 接下來，您會看到。

*內嵌與外部指令碼*

Visual Studio 2008 偵錯工具會將內嵌於頁面外部的 JavaScript 檔案不同的 JavaScript。 外部指令碼檔案時，您可以開啟檔案，並在您選擇的任何行上設定中斷點。 按一下程式碼編輯器視窗左邊灰色的紙匣區域中，可以設定中斷點。 當 JavaScript 直接內嵌至頁面上，使用`<script>`標籤中，按一下灰色的紙匣區域中設定中斷點不可行。 內嵌指令碼的行上設定中斷點的嘗試會導致警告，指出 「 這不是有效的中斷點位置 」。

您可以取得解決此問題的程式碼移到外部的.js 檔案，並參考使用的 src 屬性&lt;指令碼&gt;標記：


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

如果程式碼移到外部的檔案不是選項或需要比它的多個工作是值得？ 雖然您無法設定中斷點，使用編輯器 中，您可以新增直接在您想要開始偵錯的程式碼的偵錯工具陳述式。 您也可以使用 ASP.NET AJAX 程式庫中的可用 Sys.Debug 類別來強制啟動偵錯。 您將進一步了解本文中稍後的 Sys.Debug 類別。

示範如何使用`debugger`關鍵字以列表 1 所示。 此範例會強制偵錯工具右上更新函式呼叫之前。

**清單 1。您可以使用偵錯工具關鍵字來強制執行 Visual Studio.NET 偵錯工具中斷。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

一旦叫用偵錯工具陳述式將會提示您進行偵錯使用 Visual Studio.NET 的網頁，並逐步執行程式碼就可以開始。 執行您可能會與存取 ASP.NET AJAX 程式庫指令碼檔案，因此讓我們先使用於頁面發生問題時看看使用 Visual Studio。網路的指令碼總管。

## <a name="using-visual-studio-net-windows-to-debug"></a>使用 Visual Studio.NET 視窗來偵錯

一旦啟動偵錯工作階段並開始逐步解說的程式碼使用預設 F11 鍵，您可能會遇到錯誤對話方塊所示請參閱圖 2，除非在網頁中使用的所有指令碼檔案是開啟且可供偵錯。


[![適用於偵錯沒有原始程式碼時會顯示錯誤對話方塊。](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**圖 2**： 沒有原始程式碼不用於偵錯時顯示錯誤對話方塊。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


因為 Visual Studio.NET 不確定如何取得一些此網頁所參考的指令碼的原始碼，會顯示此對話方塊。 這可以是很不方便而在一開始，沒有簡單的修正。 一旦您啟動偵錯工作階段並叫用中斷點，請移至 [偵錯 Windows 指令碼總管] 視窗，在 Visual Studio 2008 功能表上，或使用 Ctrl + Alt + N 快速鍵。

> *>[!NOTE]如果看不到列出的指令碼總管 功能表，請移至 工具**自訂* *Visual Studio.NET 功能表上的命令。在 [類別] 區段中尋找偵錯項目，然後按一下來顯示所有可用的功能表項目。在命令清單中，向下捲動至 [指令碼總管]，然後將其拖曳到偵錯* *Windows 功能表中的先前所述。這樣會提供指令碼總管 功能表項目每次執行 Visual Studio.NET。*


指令碼總管可用來檢視在網頁中使用的所有指令碼，並在程式碼編輯器中開啟它們。 一旦開啟 [指令碼總管] 中，按兩下目前所偵錯程式碼編輯器視窗中開啟.aspx 網頁。 所有其他指令碼總管 中顯示的指令碼執行相同的動作。 所有指令碼已在您可以在程式碼視窗中開啟後按下 F11 （並使用其他的偵錯快速鍵） 來逐步執行程式碼。 圖 3 顯示指令碼總管的範例。 它會列出目前的檔案進行偵錯 (Demo.aspx) 以及兩個自訂指令碼和兩個指令碼以動態方式插入頁面的 ASP.NET AJAX ScriptManager。


[![指令碼總管 提供讓您輕鬆存取 頁面中使用的指令碼。](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**圖 3**。 指令碼總管 提供讓您輕鬆存取 頁面中使用的指令碼。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


其他視窗也可用來提供有用的資訊，當您逐步執行程式碼，在網頁中。 例如，您可以使用 [區域變數] 視窗來檢視不同頁面上，評估特定變數或條件並檢視輸出即時運算視窗中所使用的變數值。 您也可以使用 [輸出] 視窗來檢視追蹤陳述式寫出使用 Sys.Debug.trace 函式 （這將在本文稍後討論） 或 Internet Explorer Debug.writeln 函式。

當您逐步執行程式碼使用偵錯工具可以滑鼠移過程式碼 檢視時將獲指派的值中的變數。 不過，指令碼偵錯工具有時不會顯示任何項目為滑鼠移過指定的 JavaScript 變數。 若要查看的值，反白顯示陳述式或您嘗試在程式碼編輯器視窗中看到和滑鼠游標移至的變數。 雖然每一種情況不會使用這項技術，許多次您將能夠看到而不需要在不同的偵錯視窗，例如 [區域變數] 視窗中尋找的值。

示範的某些功能這裡所討論的視訊教學課程可以在檢視[http://www.xmlforasp.net](http://www.xmlforasp.net)。

## <a name="debugging-with-web-development-helper"></a>使用 Web 開發協助程式進行偵錯

雖然 Visual Studio 2008 （和 Visual Web Developer Express 2008） 是偵錯工具功能非常強，還有其他選項也可以使用，也就是一個輕量。 釋出最新的工具之一是 Web 開發協助程式。 Microsoft 的 Nikhil Kothari （一個在 Microsoft 索引鍵的 ASP.NET AJAX 架構設計人員） 所撰寫的絕佳工具可以從簡單偵錯，以檢視的 HTTP 要求和回應訊息執行許多不同的工作。 Web 開發協助程式的下載網址[http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx)。

這可讓您更方便使用 Internet Explorer 內直接可用 web 開發協助程式。 它會從 Internet Explorer 功能表中選取工具 Web 開發協助程式開始。 這會很好，因為您不必離開執行數個工作，例如，HTTP 要求和回應訊息記錄瀏覽器的瀏覽器的下半部中開啟此工具。 圖 4 顯示 Web 開發協助程式在動作中的外觀。


[![Web 開發協助程式](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**圖 4**: Web 開發協助程式 ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web 開發協助程式不是您將使用的工具來逐步執行逐行程式碼做為 Visual Studio 2008。 不過，它可用來檢視追蹤輸出，輕鬆地評估指令碼中的變數，或瀏覽資料是在 JSON 物件。 它也是非常有用的檢視 ASP.NET AJAX 頁面和伺服器來回傳遞資料。

在 Internet Explorer 中開啟 Web 開發協助程式後，指令碼偵錯必須啟用指令碼啟用指令碼偵錯從網頁程式開發 helper 功能表選取稍早在圖 4 所示。 這可讓執行網頁時，就會發生的攔截錯誤工具。 它也可讓您輕鬆存取追蹤訊息內的頁面之輸出。 若要檢視追蹤資訊或執行指令碼命令，以測試不同的函數，在一頁，選取 從 Web 開發協助程式功能表的 指令碼顯示指令碼主控台。 這會提供存取命令視窗，以及簡單的 [即時運算] 視窗。

*檢視追蹤訊息和 JSON 物件資料*

即時運算視窗可用來執行指令碼命令，或甚至載入或儲存指令碼，可用來在網頁中測試不同的函式。 [命令] 視窗會顯示正在檢視的頁面寫出追蹤或偵錯訊息。 表 2 顯示如何將寫入追蹤訊息，使用 Internet Explorer Debug.writeln 函式。

**表 2。寫出使用 Debug 類別的用戶端追蹤訊息。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

如果 LastName 屬性包含值為 Doe，Web 開發 Helper 會顯示訊息 「 人員名稱： Doe 」 在指令碼主控台的命令 視窗 （假設，偵錯已啟用）。 Web 開發協助程式也會將最上層 debugService 物件新增到可以用來寫出追蹤資訊，或檢視 JSON 物件的內容頁面。 列出 3 顯示使用 debugService 類別的追蹤函式的範例。

**列出 3。您可以使用 Web 開發協助專家的 debugService 類別來寫入追蹤訊息。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

DebugService 類別的方便的功能是可即使未啟用偵錯在 Internet Explorer 中以便輕鬆地 Web 開發協助程式執行時存取追蹤的資料。 當此工具不用來偵錯網頁時，因為 window.debugService 呼叫將傳回 false，將會忽略追蹤陳述式。

DebugService 類別也可讓 Web 開發協助專家的偵測器視窗檢視 JSON 物件資料。 列出 4 建立簡單的 JSON 物件，包含個人資料。 一旦建立物件之後，進行呼叫以 debugService 類別的檢查功能，允許以視覺化方式檢查的 JSON 物件。

**列出 4。若要檢視物件的 JSON 資料使用 debugService.inspect 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

在網頁或透過即時運算視窗呼叫 GetPerson() 函式會導致出現如圖 5 所示物件偵測器對話方塊視窗。 在物件內的屬性可以動態變更反白，變更 [值] 文字方塊中所顯示的值，然後按一下 [更新] 連結。 使用物件偵測器會直接檢視 JSON 物件的資料和試驗不同的值套用至屬性。

*偵錯錯誤*

除了允許追蹤資料和顯示的 JSON 物件，協助 Web 程式開發人員可以也有助於偵錯頁面中的錯誤。 如果遇到錯誤時，系統會提示您繼續到下一行程式碼，或偵錯指令碼 （請參閱圖 6）。 指令碼錯誤對話方塊視窗會顯示完整的呼叫堆疊以及行號，因此您可以輕鬆地識別問題所在位置，在指令碼內。


[![您可以使用物件的偵測器視窗來檢視 JSON 物件。](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**圖 5**： 使用物件的偵測器視窗來檢視 JSON 物件。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


選取偵錯選項，可讓您直接在檢視變數值、 寫出 JSON 物件，再加上更多的 Web 開發協助專家的即時運算視窗中執行指令碼陳述式。 如果相同觸發錯誤的動作再次執行，而且在電腦上使用 Visual Studio 2008，系統會提示您啟動偵錯工作階段，讓您可以逐步執行逐行程式碼在上一節中所述。


[![Web 開發協助專家的指令碼錯誤對話方塊](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**圖 6**: Web 開發協助專家的指令碼錯誤對話方塊 ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*檢查要求和回應訊息*

偵錯 ASP.NET AJAX 網頁時通常會很有用，查看頁面和伺服器之間傳送的要求和回應訊息。 檢視訊息內的內容，可讓您查看是否訊息的大小以及傳遞適當的資料。 Web 開發協助程式提供了絕佳 HTTP 訊息記錄器功能，能輕鬆檢視資料，做為未經處理文字或更容易閱讀的格式。

若要檢視 ASP.NET AJAX 要求和回應訊息，您必須以從 Web 開發協助程式功能表中選取 HTTP 啟用 HTTP 記錄，啟用 HTTP 記錄器。 啟用之後，就可以檢視從目前的頁面傳送所有訊息可以透過選取 HTTP 顯示 HTTP 記錄檔中的 HTTP 記錄檔檢視器中。

雖然檢視每個要求/回應訊息中傳送的原始文字是的確很實用 （及 Web 程式開發的協助程式中的選項），通常很輕鬆地檢視多圖形格式的訊息資料。 一旦已啟用 HTTP 記錄，而且已將訊息記錄，訊息資料可以檢視在 HTTP 記錄檔檢視器中的訊息上按兩下。 如此一來，可讓您檢視所有訊息，以及實際的訊息相關聯的標頭內容。 圖 7 顯示要求訊息和回應訊息的 HTTP 記錄檔檢視器視窗中檢視的範例。


[![您可以使用 HTTP 記錄檔檢視器來檢視要求和回應訊息資料。](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**圖 7**： 使用 HTTP 記錄檔檢視器檢視要求和回應訊息資料。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


HTTP 記錄檔檢視器會自動剖析 JSON 物件，並顯示它們使用快速且容易檢視物件的屬性資料的樹狀檢視。 UpdatePanel 用於 ASP.NET AJAX 頁面中，當檢視器會中斷出訊息的個別部分的每個部分圖 8 所示。 這是很棒的功能，可讓您更能更輕鬆地查看並了解什麼是訊息相較於檢視未經處理的訊息資料中。


[![使用 HTTP 記錄檔檢視器檢視 UpdatePanel 回應訊息。](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**圖 8**： 使用 HTTP 記錄檔檢視器檢視 UpdatePanel 回應訊息。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


有數個其他工具，可用來檢視 Web 開發協助程式除了要求和回應訊息。 另一個很好的選項是可以免費在 Fiddler [http://www.fiddlertool.com](http://www.fiddlertool.com)。Fiddler 不討論以下，雖然它時也是很好的選擇您要徹底地檢查訊息標頭和資料。

## <a name="debugging-with-firefox-and-firebug"></a>使用 Firefox 和 firebug 這類偵錯

Internet Explorer 仍是最普遍使用的瀏覽器，而其他瀏覽器，例如 Firefox 變得非常受歡迎，並使用越來越多。 如此一來，您會想要檢視及偵錯您的 ASP.NET AJAX 網頁中 Firefox，以及 Internet Explorer 以確保您的應用程式正常運作。 雖然 Firefox 無法直接在 Visual Studio 2008 有相同的偵錯，但其呼叫可用來偵錯網頁的 firebug 這類延伸模組。 Firebug 這類可以免費下載，請前往[http://www.getfirebug.com](http://www.getfirebug.com)。

Firebug 這類提供功能完整的偵錯環境，可用來逐步執行逐行程式碼存取網頁內使用的所有指令碼、 檢視 DOM 結構，顯示在網頁中的 CSS 樣式，甚至是追蹤事件發生的。 安裝之後，可以存取 firebug 這類工具 firebug 這類開啟 Firebug 從 Firefox 功能表中選取。 Web 開發的協助程式，例如 firebug 這類都使用直接在瀏覽器中，不過它也可用來當作獨立的應用程式。

Firebug 這類開始執行之後，中斷點可以設定任何一行的 JavaScript 檔案，是否在指令碼或不內嵌於頁面。 若要設定中斷點，請先載入適當的頁面，您想要在 Firefox 中偵錯。 一旦載入的頁面，選取要從下拉式清單中 firebug 這類的指令碼進行偵錯的指令碼。 將顯示所有頁面所用的指令碼。 依序按一下 firebug 這類的灰色的紙匣 區域的行中斷點應該在哪裡必須像您一樣在 Visual Studio 2008 中設定中斷點。

一旦 firebug 這類中設定中斷點，您可以執行執行指令碼需要例如按一下按鈕，或重新整理瀏覽器 onLoad 事件觸發程序進行偵錯所需的動作。 在包含中斷點的行上，將會自動停止執行。 圖 9 中 Firebug 顯示已經觸發中斷點的範例。


[![正在處理中 firebug 這類的中斷點。](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**圖 9**： 處理 firebug 這類的中斷點。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


一旦叫用中斷點時您可以逐步執行、 不進入或跳離程式碼使用箭號按鈕。 當您逐步執行程式碼，指令碼變數會顯示在右邊的 偵錯工具可讓您查看值和向下鑽研功能的物件。 Firebug 這類也包括呼叫堆疊下拉式清單來檢視導致目前的行正在進行偵錯的指令碼的執行步驟。

Firebug 這類也包含可用來測試不同的指令碼陳述式、 評估變數並檢視追蹤輸出的主控台視窗。 Firebug 這類視窗頂端的 [主控台] 索引標籤，即可存取。 [偵錯] 頁面上可以也 「 檢查 」 依序按一下 [檢查] 索引標籤上看到的 DOM 結構和內容。為您的結束滑鼠停留不同頁面的適當部分所示的偵測器視窗的 DOM 項目就會反白以便輕鬆地查看項目使用中頁面的位置。 「 即時 」 試驗套用不同的寬度、 樣式等等的項目可以變更指定的項目相關聯的屬性值。 這是需要不斷切換來源的程式碼編輯器和 Firefox 瀏覽器檢視簡單的變更會影響頁面的實用功能。

圖 10 說明使用 DOM 偵測器來尋找名為 txtCountry 網頁中的文字方塊中的範例。 Firebug 這類偵測器也可用來檢視網頁，以及發生例如追蹤滑鼠移動、 按一下按鈕，加上更多的事件中使用的 CSS 樣式。


[![使用 firebug 這類的 DOM 偵測器。](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**圖 10**： 使用 Firebug DOM 偵測器。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug 這類提供輕量方式快速偵錯直接中 Firefox，以及最佳工具來檢查不同的項目頁面內的頁面。

## <a name="debugging-support-in-aspnet-ajax"></a>在 ASP.NET AJAX 中的偵錯支援

ASP.NET AJAX 程式庫包含許多不同的類別可以用來簡化新增至網頁的 AJAX 功能的程序。 您可以使用這些類別，尋找頁面內的項目以及操作它們、 加入新的控制項、 呼叫 Web 服務和即使處理事件。 ASP.NET AJAX 程式庫也包含類別，可以用來增強偵錯網頁的程序。 本節中您要將介紹 Sys.Debug 類別，並查看如何在應用程式中使用。

*使用 Sys.Debug 類別*

Sys.Debug 類別 （位於 Sys 命名空間中的 JavaScript 類別） 可以用來執行數個不同的功能，包括寫入追蹤輸出中，執行程式碼的判斷提示和強制執行失敗，以便進行偵錯的程式碼。 它是廣泛使用在 ASP.NET AJAX 程式庫的偵錯檔 （位於 C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 預設安裝） 來執行條件式的測試 （稱為 「 判斷提示），請務必函式，物件包含預期的資料，以及寫入追蹤陳述式正確傳遞參數。

Sys.Debug 類別會公開數個不同的函式可用來處理追蹤、 程式碼的判斷提示或失敗，如表 1 中所示。

**表 1。Sys.Debug 類別函式。**

| **函式名稱** | **說明** |
| --- | --- |
| 判斷提示 （條件、 訊息、 displayCaller） | 判斷提示的條件參數為 true。 如果要測試的條件為 false，訊息方塊會用於顯示訊息的參數值。 如果 displayCaller 參數為 true，則方法也會顯示呼叫者的相關資訊。 |
| clearTrace() | 清除追蹤作業的陳述式輸出。 |
| fail(message) | 會造成程式停止執行並偵錯工具。 訊息參數可以用來提供失敗的原因。 |
| trace(message) | 寫入追蹤輸出訊息參數。 |
| traceDump （物件、 名稱） | 會輸出物件的資料，可讀取的格式。 Name 參數可以用來提供用於追蹤傾印的標籤。 根據預設，物件傾印內之任何子物件將寫出。 |

用戶端追蹤可在 ASP.NET 中可用的追蹤功能很多相同的方式。 它可讓不同的訊息容易看到而不會中斷應用程式的流程。 列出 5 顯示使用 Sys.Debug.trace 函式，將寫入追蹤記錄檔的範例。 此函數只接受應該寫出做為參數的訊息。

**列出 5。使用 Sys.Debug.trace 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

如果您執行中的程式碼列表 5 不會看到頁面中的任何追蹤輸出。 若要查看它的唯一方式是使用可用在 Visual Studio.NET、 Web 開發 Helper 或 firebug 這類的主控台視窗。 如果您想查看網頁中的追蹤輸出，將需要加入文字區域標記，並給定識別碼為 TraceConsole 如下所示：


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

網頁中的任何 Sys.Debug.trace 陳述式將會寫入 TraceConsole 文字區域。

在您要查看 JSON 物件內所包含的資料的情況下，您可以使用 Sys.Debug 類別的 traceDump 函式。 此函數會採用兩個參數，包括應該可傾印以追蹤主控台的物件和可以用來識別追蹤輸出中的物件名稱。 列出 6 顯示使用 traceDump 函式的範例。

**列出 6。使用 Sys.Debug.traceDump 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

圖 11 顯示呼叫 Sys.Debug.traceDump 函式的輸出。 請注意，除了寫出人員物件的資料，它也會撰寫出位址子物件的資料。

追蹤，除了 Sys.Debug 類別也可以用來執行程式碼的判斷提示。 判斷提示用來測試應用程式執行時，符合特定條件。 ASP.NET AJAX 程式庫指令碼的偵錯版本包含數個 assert 陳述式來測試各種不同的條件。

列出 7 顯示使用 Sys.Debug.assert 函式來測試條件的範例。 程式碼會測試更新 Person 物件之前，位址物件為 null。


[![Sys.Debug.traceDump 函式的輸出。](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**圖 11**: Sys.Debug.traceDump 函式的輸出。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**列出 7。使用 debug.assert 函式。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

會傳遞三個參數，包括要評估，判斷提示會傳回 false，而且應顯示呼叫者的相關資訊時要顯示訊息的條件。 在判斷提示失敗的地方的情況下，訊息將顯示呼叫端資訊以及若第三個參數，則為 true。 圖 12 顯示列出 7 所示的判斷提示失敗時才會顯示 [失敗] 對話方塊的範例。

最後的函式，以涵蓋是 Sys.Debug.fail。 當您想要強制執行的程式碼無法在指令碼中的特定資料行上您可以加入 Sys.Debug.fail 呼叫，而不是 JavaScript 應用程式中一般使用的偵錯工具陳述式。 Sys.Debug.fail 函式會接受單一字串參數，表示失敗的原因，如下所示：


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Sys.Debug.assert 失敗訊息。](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**圖 12**: Sys.Debug.assert 失敗訊息。  ([按一下以檢視完整大小的影像](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


當遇到 Sys.Debug.fail 陳述式時執行指令碼時，訊息參數的值將顯示在主控台中的偵錯應用程式，例如 Visual Studio 2008，將提示您進行偵錯應用程式。 您無法在內嵌指令碼上設定中斷點，以使用 Visual Studio 2008，但想要如此，您可以檢查變數值，在特定行停止的程式碼時，這會很有用的一種情況。

*了解 ScriptManager 控制項 ScriptMode 屬性*

ASP.NET AJAX 程式庫包含偵錯和發行指令碼版本，預設會安裝在 C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 的。 偵錯指令碼格式工整且、 容易閱讀也更有數個 Sys.Debug.assert 呼叫分散至它們同時發行指令碼則會移除空白字元而且謹慎使用 Sys.Debug 類別，其整體大小降到最低。

ScriptManager 控制項加入至 ASP.NET AJAX 頁面讀取 compilation 項目來判斷哪些版本的程式庫指令碼載入 web.config 中的偵錯屬性。 不過，您可以控制偵錯或發行指令碼會載入 （程式庫指令碼或您自己的自訂指令碼） 藉由變更 ScriptMode 屬性。 ScriptMode 接受 ScriptMode 列舉的成員包括自動、 偵錯、 發行和繼承。

ScriptMode 預設為自動，這表示 ScriptManager 會檢查在 web.config 中的偵錯屬性的值。當為 false 時偵錯 ScriptManager 將會載入 ASP.NET AJAX 程式庫指令碼的發行版本。 如果偵錯的則為 true，就會載入指令碼的偵錯版本。 釋出或偵錯 ScriptMode 屬性變更，將會強制載入適當的指令碼，不論在 web.config 中的偵錯屬性有了什麼價值 ScriptManager。列出 8 顯示使用 ScriptManager 控制項從 ASP.NET AJAX 程式庫載入偵錯指令碼的範例。

**列出 8。載入偵錯指令碼使用 ScriptManager**。


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

您也可以列出 9 中所示，使用在 ScriptManager 的指令碼屬性一起使用，指令碼參考元件，以載入您自己的自訂指令碼的不同版本 （偵錯或發行）。

**列出 9。載入使用 ScriptManager 的自訂指令碼。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> 如果您正在載入使用指令碼參考元件的自訂指令碼必須在指令碼已完成載入時，在指令碼底端加入下列程式碼時通知 ScriptManager:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

列出 9 所示的程式碼會告知尋找人員指令碼的偵錯版本，因此它會自動尋找而不是 Person.js Person.debug.js ScriptManager。 Person.debug.js 檔案找不到時就會引發錯誤。

在您想要偵錯或發行版本的自訂指令碼載入依據 ScriptManager 控制項上設定的 ScriptMode 屬性的值位置的情況下，您可以將指令碼參考控制項的 ScriptMode 屬性繼承。 這會導致要載入的自訂指令碼的適當版本依據 ScriptManager ScriptMode 屬性列出 10 所示。 因為 ScriptManager 控制項的 ScriptMode 屬性設定為 偵錯，就會載入並使用於頁面 Person.debug.js 指令碼。

**列出 10。繼承的自訂指令碼 ScriptManager ScriptMode。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

適當地使用 ScriptMode 屬性可以更輕鬆地偵錯應用程式，並簡化整體程序。 ASP.NET AJAX 程式庫的發行指令碼是以逐步執行及讀取程式碼格式設定已移除偵錯指令碼的格式特別以進行偵錯時相當困難。

## <a name="conclusion"></a>結論

Microsoft ASP.NET AJAX 技術，提供穩固的基礎建置，可強化終端使用者的整體體驗 ajax 能力的應用程式。 不過，做為任何的程式設計技術 bug 和其他應用程式問題，將會確實發生。 深入了解不同的偵錯選項可用，可以節省許多時間與結果的更穩定的產品。

在本文中您已學到幾個不同技術可偵錯 ASP.NET AJAX 網頁包括 Visual Studio 2008、 Web 開發協助程式與 firebug 這類的 Internet Explorer。 這些工具可以簡化整個偵錯程序，因為您可以存取變數的資料、 逐步執行逐行程式碼，並檢視追蹤陳述式。 除了所討論的不同偵錯工具，您也看到了如何使用 ASP.NET AJAX 程式庫的 Sys.Debug 類別，在應用程式及如何 ScriptManager 類別可以用來載入偵錯或發行指令碼版本。

## <a name="bio"></a>簡歷

Dan Wahlin （Microsoft 最有價值專家適用於 ASP.NET 和 XML Web Service） 是.NET 開發講師和架構顧問在介面技術訓練 ([www.interfacett.com)](http://www.interfacett.com)。 Dan 所建構的 XML for ASP.NET 開發人員網站 ([www.XMLforASP.NET](http://www.XMLforASP.NET))，位於 INETA 喇叭局所免費提供，並在數個所做的心得。 Dan 共同撰寫 Professional Windows DNA (Wrox)，ASP.NET： 秘訣、 教學課程與程式碼 (Sam)、 ASP.NET 1.1 測試人員方案、 Professional ASP.NET 2.0 AJAX (Wrox)、 ASP.NET 2.0 MVP 缺失，以及 ASP.NET 開發人員 (Sam) 撰寫的 XML。 當他不撰寫程式碼、 文件或活頁簿時，Dan 喜歡的休閒活動撰寫和錄製音樂和播放高爾夫球和籃球與他的妻子小孩。

Scott 是否已從 1997 年使用 Microsoft Web 技術，且 myKB.com 總統 ([www.myKB.com](http://www.myKB.com)) 擅長撰寫 ASP.NET 架構的重點 Knowledge Base 軟體解決方案的應用程式。 透過在電子郵件，即可以聯繫 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或在他的部落格[ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[上一步](understanding-asp-net-ajax-web-services.md)
