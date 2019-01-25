---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: 將 SignalR 1.x 專案升級至第 2 版 |Microsoft Docs
author: bradygaster
description: 本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x 中，以及如何針對升級的程序期間可能發生的問題進行疑難排解...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: a1cb4903f3cdeef70ffd0f624a3a2170f641a395
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837334"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>將 SignalR 1.x 專案升級至第 2 版
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x 中，以及如何進行疑難排解，升級程序期間可能發生問題。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR 版本 1 和 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>本教學課程中使用 Visual Studio 2012
>
>
> 若要使用 Visual Studio 2012，本教學課程中，執行下列作業：
>
> - 更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。
> - 安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。 這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。
> - 有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。
>
>
> ## <a name="questions-and-comments"></a>提出問題或意見
>
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


SignalR 2 使用的伺服器平台提供具有一致開發體驗[OWIN](http://owin.org)。 本文說明更新到版本 2 的 SignalR 1.x 應用程式所需的幾個步驟。

雖然建議您升級至 SignalR 2 的應用程式，SignalR 1.x 仍受支援。

本教學課程說明如何升級 web 裝載的應用程式，SignalR 2。 SignalR 2 現在支援自我裝載的應用程式 （其會裝載在主控台應用程式、 Windows 服務或其他處理序伺服器）。 如需如何開始使用 SignalR 2 建立的自我裝載的應用程式的資訊，請參閱[教學課程：SignalR 自我裝載](../deployment/tutorial-signalr-self-host.md)。

## <a name="contents"></a>內容

下列各節描述升級 SignalR 專案，以及如何針對可能發生的問題進行疑難排解所涉及的工作。

- [例如：升級至 SignalR 2 的快速入門教學課程](#example)
- [在升級期間發生的錯誤進行疑難排解](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>範例：開始使用教學課程應用程式升級為 SignalR 2

在本節中，您將更新應用程式中建立[入門教學課程的 SignalR 1.x 版](../older-versions/index.md)使用 SignalR 2。

1. 一旦您完成快速入門教學課程，請以滑鼠右鍵按一下專案，然後選取**屬性**。 確認**目標 framework**設定為 **.NET Framework 4.5。**
2. 開啟 [Package Manager Console]。 移除 SignalR 1.x 專案使用下列命令：

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. 安裝 SignalR 2 使用下列命令：

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. 在 HTML 頁面中，更新以符合現在包含在專案中的指令碼版本的 SignalR 的指令碼參考。

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. 在全域應用程式類別中，移除 MapHubs 的呼叫。

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. 以滑鼠右鍵按一下方案，然後選取**新增**，**新項目...**.在對話方塊中，選取**Owin 啟動類別**。 新類別命名**Startup.cs**。

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Startup.cs 的內容取代為下列程式碼：

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    組件屬性將類別加入至 Owin 的啟動程序，它會執行`Configuration`Owin 啟動時的方法。 這會進而呼叫`MapSignalR`方法會建立應用程式中的所有 SignalR 中樞的路由。
8. 執行專案，並將主頁面的 URL 複製到另一個瀏覽器或瀏覽器窗格中的，與之前。 每一頁會詢問使用者名稱，並從每個頁面傳送的訊息應該在這兩個瀏覽器窗格中顯示。

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>在升級期間發生的錯誤進行疑難排解

本節說明在升級期間可能發生的問題。 如需更完整的 SignalR 應用程式，可能會發生的錯誤和問題清單，請參閱 < [SignalR 疑難排解](../testing-and-debugging/troubleshooting.md)。

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>' 的呼叫是下列的方法或屬性之間模稜兩可的 '

如果的參考，會發生此錯誤`Microsoft.AspNet.SignalR.Owin`不會移除。 此套件已被取代;必須先移除參考，並自行封裝的 1.x 版本必須先解除安裝。

### <a name="hub-methods-fail-silently"></a>中樞的方法以無訊息方式失敗

確認您的用戶端中的指令碼參考最多的日期，而且`OwinStartup`屬性 (attribute) 您的啟動類別具有正確的類別和專案的組件名稱。 此外，請嘗試開啟中樞位址 （signalr/中樞），以在瀏覽器中;會出現任何錯誤會提供錯誤情形的詳細資訊。
