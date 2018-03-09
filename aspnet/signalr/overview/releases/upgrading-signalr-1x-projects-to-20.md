---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "將 SignalR 1.x 專案升級到版本 2 |Microsoft 文件"
author: pfletcher
description: "本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x，以及如何疑難排解，升級程序期間可能發生問題..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>將 SignalR 1.x 專案升級到版本 2
====================
由[Patrick Fletcher](https://github.com/pfletcher)

> 本主題描述如何將現有的 SignalR 1.x 專案升級至 SignalR 2.x，以及如何升級程序期間可能發生的問題進行疑難排解。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 版本 1 和 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>使用 Visual Studio 2012 進行這個教學課程
> 
> 
> 透過本教學課程中使用 Visual Studio 2012，請執行下列作業：
> 
> - 更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。
> - 安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web Platform Installer 中搜尋及安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。 這會安裝 Visual Studio 範本 SignalR 的類別，例如**中樞**。
> - 某些範本 (例如**OWIN 啟動類別**) 將無法使用; 這些項目，請改用類別檔案。
> 
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


SignalR 2 提供一致的開發經驗跨伺服器平台使用[OWIN](http://owin.org)。 這篇文章描述更新為版本 2 的 SignalR 1.x 應用程式所需的幾個步驟。

它鼓勵升級至 SignalR 2 應用程式，而 SignalR 1.x 仍會支援。

本教學課程說明如何升級至 SignalR 2 web 應用程式。 在 SignalR 2 現在支援自我裝載的應用程式 （其裝載的主控台應用程式、 Windows 服務或其他處理程序中的伺服器）。 如需如何開始使用 SignalR 2 建立的自我裝載應用程式資訊，請參閱[教學課程： 自我裝載的 SignalR](../deployment/tutorial-signalr-self-host.md)。

## <a name="contents"></a>內容

下列章節說明升級 SignalR 專案，以及如何疑難排解可能遇到的問題的相關工作。

- [範例： 升級至 SignalR 2 的快速入門教學課程](#example)
- [在升級期間發生錯誤的疑難排解](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>範例： 快速入門教學課程應用程式升級為 SignalR 2

在本節中，您要更新應用程式中建立[SignalR 1.x 版快速入門教學課程](../older-versions/index.md)使用 SignalR 2。

1. 一旦您完成快速入門教學課程，請以滑鼠右鍵按一下專案，然後選取**屬性**。 確認**目標 framework**設**.NET Framework 4.5。**
2. 開啟 封裝管理員主控台。 移除 SignalR 1.x 從專案中使用下列命令：

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. 安裝 SignalR 2 使用下列命令：

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. 在 HTML 頁面中，更新的指令碼參考適用於 SignalR 符合現在包含在專案中的指令碼的版本。

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. 在全域應用程式類別中，移除 MapHubs 的呼叫。

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. 以滑鼠右鍵按一下方案，然後選取**新增**，**新項目...**.在對話方塊中，選取**Owin 啟動類別**。 將新類別**Startup.cs**。

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Startup.cs 中的內容取代為下列程式碼：

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    組件屬性將類別加入至 Owin 的啟動程序，它會執行`Configuration`Owin 啟動時的方法。 這會依次呼叫`MapSignalR`方法，可在應用程式中建立所有 SignalR 中樞的路由。
8. 執行專案，並將主頁面的 URL 複製到另一個瀏覽器或瀏覽器 窗格之前。 每個頁面會要求使用者名稱，並從每一頁所傳送的訊息應該在這兩個瀏覽器窗格中顯示。

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>在升級期間發生錯誤的疑難排解

本節說明在升級期間可能遇到的問題。 如需更完整的 SignalR 應用程式，可能會發生的錯誤和問題清單，請參閱[SignalR 疑難排解](../testing-and-debugging/troubleshooting.md)。

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>' 的呼叫是下列的方法或屬性之間模稜兩可的 '

如果的參考，會發生這個錯誤`Microsoft.AspNet.SignalR.Owin`不會移除。 此套件已被取代。必須移除參照，必須先解除安裝 SelfHost 封裝 1.x 版本。

### <a name="hub-methods-fail-silently"></a>中樞的方法以無訊息模式失敗

確認您的用戶端指令碼參考最多的日期，且`OwinStartup`啟動類別具有正確的類別和專案的組件名稱屬性。 此外，再試一次開啟中樞中的位址 （/signalr/集線器） 您的瀏覽器;會出現任何錯誤會提供有關發生什麼事錯誤的詳細資訊。
