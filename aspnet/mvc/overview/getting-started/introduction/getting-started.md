---
uid: mvc/overview/getting-started/introduction/getting-started
title: 開始使用 ASP.NET MVC 5 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本是可在此處使用 Visual Studio 2015。 新的教學課程會使用 ASP.NET Core MVC 6，提供許多 improvem...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 85d5d3292ff99ade6995c710e2728c41255def4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823212"
---
<a name="getting-started-with-aspnet-mvc-5"></a>開始使用 ASP.NET MVC 5
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 本教學課程將教導您建置 ASP.NET MVC 5 web 應用程式使用的基本概念[Visual Studio 2017](https://www.visualstudio.com/)。 教學課程中的最後一個來源位於[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 本教學課程以寫入[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )， [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )與[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 您需要有 Azure 帳戶才能將此應用程式部署至 Azure:

 - 您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。
 - 您可以[啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。


## <a name="getting-started"></a>快速入門

開始安裝並執行[Visual Studio 2017](https://www.visualstudio.com/)。

Visual Studio 是 IDE 或整合式的開發環境。 就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。 在 Visual Studio 中沒有顯示各種可用的選項，您在底端的清單。 另外還有一個功能表，可讓您在 IDE 中執行工作的另一個辦法。 (例如，您可以使用功能表並選取檔案 &gt; 新專案，而不是從啟動 頁面上選取新的專案.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>建立第一個應用程式

按一下 **新的專案**，然後選取 Visual C# 在左邊，然後**Web** ，然後選取**ASP.NET Web 應用程式 (.NET Framework)**。 命名您的專案"為 MvcMovie"，然後按一下**確定**。

![](getting-started/_static/image2.png)

在 **新的 ASP.NET 專案** 對話方塊中，按一下**MVC** ，然後按一下  **確定**。

![](getting-started/_static/image3.png)

Visual Studio 使用您剛才建立的 ASP.NET MVC 專案預設範本，因此您有運作中應用程式現在無須執行任何動作 ！ 這是簡單"Hello World ！" 專案和它的啟動您的應用程式的好地方。

![](getting-started/_static/image4.png)

按 f5 鍵啟動偵錯。 F5 會導致 Visual Studio 啟動[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)並執行您的 web 應用程式。 Visual Studio 接著會啟動瀏覽器，並開啟應用程式的首頁。 通知，指出瀏覽器的網址列`localhost:port#`而不是類似`example.com`。 這是因為`localhost`永遠指向您自己的本機電腦，在此情況下執行您剛建立的應用程式。 Visual Studio 執行時 web 專案，會將隨機的連接埠用於 web 伺服器。 在下面的影像中的連接埠號碼是 1234。 當您執行應用程式時，您會看到不同的連接埠號碼。

![](getting-started/_static/image5.png)

現成這個預設範本可讓您住家、 連絡人和相關頁面。 上述映像不會顯示**首頁**，**有關**並**連絡人**連結。 根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示，以查看這些連結。

![](getting-started/_static/image6.png)  

應用程式也會提供註冊和登入的支援。 下一個步驟是要變更此應用程式的運作方式有點了解 ASP.NET MVC。 關閉 ASP.NET MVC 應用程式，並讓我們變更一些程式碼。

如需目前的教學課程的清單，請參閱 < [MVC 建議文章](../mvc-learning-sequence.md)。

## <a name="see-this-app-running-on-azure"></a>請參閱在 Azure 上執行此應用程式

若要查看為即時 web 應用程式正在執行的完成的網站嗎？ 您可以部署您的 Azure 帳戶的應用程式的完整版本，只要按一下下面的按鈕。

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

您需要有 Azure 帳戶才能將此解決方案部署至 Azure。 如果您還沒有帳戶，您會有下列選項：

- [免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。
- [啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。

> [!div class="step-by-step"]
> [下一步](adding-a-controller.md)
