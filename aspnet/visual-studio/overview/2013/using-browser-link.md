---
uid: visual-studio/overview/2013/using-browser-link
title: 在 Visual Studio 2013 中使用瀏覽器連結 |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044106"
---
<a name="using-browser-link-in-visual-studio-2013"></a>在 Visual Studio 2013 中使用瀏覽器連結
====================
由[Mike Wasson](https://github.com/MikeWasson)

瀏覽器連結是建立在開發環境和一或多個網頁瀏覽器之間的通訊通道的 Visual Studio 2013 中的新功能。 您可以使用瀏覽器連結，以重新整理 web 應用程式在數個瀏覽器中的，這是適用於跨瀏覽器測試。

- [重新整理瀏覽器](#browser-refresh)
- [檢視瀏覽器連結儀表板](#dashboard)
- [啟用靜態 HTML 檔案的瀏覽器連結](#static-html)
- [停用瀏覽器連結](#disabling)
- [它如何運作？](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>重新整理瀏覽器

使用瀏覽器重新整理，您可以重新整理多個瀏覽器連接到 Visual Studio 透過瀏覽器連結。

若要使用瀏覽器重新整理，請先建立任何專案範本使用的 ASP.NET 應用程式。 按下 F5 或按一下工具列中的箭號圖示，偵錯應用程式：

![](using-browser-link/_static/image1.png)

您也可以使用下拉式清單中選取特定瀏覽器的偵錯。

![](using-browser-link/_static/image2.png)

若要使用多個瀏覽器進行偵錯，請選取**瀏覽**。 在**瀏覽** 對話方塊中，按住 CTRL 鍵來選取多個瀏覽器。 按一下**瀏覽**與所選的瀏覽器偵錯。 如果您啟動從 Visual Studio 外部瀏覽器並瀏覽至應用程式 URL，也適用於瀏覽器連結。

![](using-browser-link/_static/image3.png)

在下拉式清單中有循環的箭號圖示位於瀏覽器連結控制項。 箭號圖示，則為**重新整理** 按鈕。

![](using-browser-link/_static/image4.png)

若要查看哪些瀏覽器連線，將滑鼠游標**重新整理**偵錯時的按鈕。 已連線的瀏覽器會顯示在工具提示 視窗中。

![](using-browser-link/_static/image5.png)

若要重新整理連接的瀏覽器，請按一下**重新整理**按鈕或按 CTRL + ALT + ENTER。 例如，下列螢幕擷取畫面會顯示在 ASP.NET 專案中使用 MVC 5 專案範本建立。 您可以看到在頂端的兩個瀏覽器中執行的應用程式。 在底部，專案是在 Visual Studio 中開啟。

![](using-browser-link/_static/image6.png)

在 Visual Studio 中，變更&lt;h1&gt;標題首頁：

![](using-browser-link/_static/image7.png)

當我按下**重新整理**按鈕，變更出現在這兩個瀏覽器視窗中：

![](using-browser-link/_static/image8.png)

**備註**

- 若要啟用瀏覽器連結，將`debug=true`中[&lt;編譯&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx)專案的 Web.config 檔案中的項目。
- 應用程式必須在 localhost 上執行。
- 應用程式必須以.NET 4.0 或更新版本為目標。

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>檢視瀏覽器連結儀表板

瀏覽器連結儀表板會顯示瀏覽器連結連線的相關資訊。 若要檢視儀表板，請選取 [瀏覽器連結] 下拉式功能表 (旁邊的小箭頭**重新整理**按鈕)。 然後按一下 **瀏覽器連結儀表板**。

![](using-browser-link/_static/image9.png)

儀表板會列出已連線的瀏覽器和每個瀏覽器已瀏覽的 URL。

![](using-browser-link/_static/image10.png)

**必要條件**區段會顯示該專案啟用瀏覽器連結所需的任何步驟。 例如，下列螢幕擷取畫面顯示的專案之 「 偵錯 」 設定為 false 的 Web.config 檔案中。

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>啟用靜態 HTML 檔案的瀏覽器連結

若要啟用瀏覽器連結為靜態的 HTML 檔案，請將下列加入 Web.config 檔案。

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

基於效能考量，請移除此設定，當您發行專案。

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>停用瀏覽器連結

預設會啟用瀏覽器連結。 有數種方式可將它停用：

- 在瀏覽器連結 下拉式功能表中，取消核取**啟用瀏覽器連結**。 

    ![](using-browser-link/_static/image12.png)
- 在 Web.config 檔案中，加入名為"vs: EnableBrowserLink"與"false"值的 appSettings 區段中的索引鍵。 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- 在 Web.config 檔案中，設定為 false 的偵錯。 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>它如何運作？

使用瀏覽器連結[SignalR](../../../signalr/index.md)來建立 Visual Studio 和瀏覽器之間的通訊通道。 啟用瀏覽器連結時，Visual Studio 就會當做多個用戶端 （瀏覽器使用） 可以連接到的 SignalR 伺服器。 瀏覽器連結也會使用 ASP.NET 註冊 HTTP 模組。 此模組會插入特殊&lt;指令碼&gt;到每個頁面要求來自伺服器的參考。 您可以看到指令碼參考在瀏覽器中選取 [檢視來源]。

![](using-browser-link/_static/image13.png)

不會修改原始程式檔。 HTTP 模組會以動態方式插入指令碼參考。

因為瀏覽器端程式碼是所有 JavaScript，所以它適用於所有瀏覽器，[支援 SignalR](../../../signalr/overview/getting-started/supported-platforms.md)，而不需要任何瀏覽器外掛程式。
