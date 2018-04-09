---
uid: mvc/overview/getting-started/introduction/getting-started
title: 開始使用 ASP.NET MVC 5 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本可在此處使用 Visual Studio 2015。 新的教學課程會使用 ASP.NET Core MVC 6，提供許多 improvem...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 0f1fd2026691d3bc0e81b20a9731879d7a6041bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a>開始使用 ASP.NET MVC 5
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 本教學課程將告訴您建置 ASP.NET MVC 5 web 應用程式使用的基本概念[Visual Studio 2017](https://www.visualstudio.com/)。 教學課程中的最後一個來源位於[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 本教學課程中所編寫的[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )， [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )與[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 您需要 Azure 帳戶，以將此應用程式部署至 Azure:

 - 您可以[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。
 - 您可以[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。


## <a name="getting-started"></a>快速入門

開始安裝並執行[Visual Studio 2017](https://www.visualstudio.com/)。

Visual Studio 是一個 IDE 中或整合式的開發環境。 就像您使用 Microsoft Word 來寫入的文件時，您將使用 IDE 來建立應用程式。 在 Visual Studio 中會列出沿著底部顯示各種可用的選項給您。 也會提供另一種方式，在 IDE 中執行工作的功能表。 (例如，而不是選取**新的專案**從**啟動** 頁面上，您可以使用功能表並選取**檔案** &gt; **新專案**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>建立第一個應用程式

按一下**新專案**，然後選取 Visual C# 在左邊，然後**Web** ，然後選取  **ASP.NET Web 應用程式 (.NET Framework)**。 將您的專案"MvcMovie"，再按一下**確定**。

![](getting-started/_static/image2.png)

在**新增 ASP.NET 專案** 對話方塊中，按一下  **MVC** ，然後按一下 **確定**。

![](getting-started/_static/image3.png)

Visual Studio 使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有可用的應用程式立即而不做任何事 ！ 這是簡單"Hello World ！" 專案和它的啟動您的應用程式的好地方。

![](getting-started/_static/image4.png)

按 f5 鍵啟動偵錯。 F5 會導致 Visual Studio 啟動[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)並執行您 web 應用程式。 然後，visual Studio 會啟動瀏覽器，並開啟應用程式的首頁。 請注意，該處會指示瀏覽器的網址列`localhost:port#`並不是類似`example.com`。 這是因為`localhost`一律會指向您自己的本機電腦，在此情況下執行您剛才建置的應用程式。 Visual Studio 執行時 web 專案，隨機連接埠用於 web 伺服器。 在下面的影像中的連接埠號碼是 1234年。 當您執行應用程式時，您會看到不同的通訊埠編號。

![](getting-started/_static/image5.png)

現成這個預設範本可讓您家用、 連絡人和相關頁面。 不會顯示上面的影像**首頁**，**有關**和**連絡人**連結。 根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示以查看這些連結。

![](getting-started/_static/image6.png)  

應用程式也提供支援註冊和登入。 下一個步驟是變更這個應用程式的運作方式，並了解 ASP.NET MVC 的一點。 關閉 ASP.NET MVC 應用程式，讓我們變更一些程式碼。

如需目前的教學課程，請參閱[建議使用發行項的 MVC](../mvc-learning-sequence.md)。

## <a name="see-this-app-running-on-azure"></a>請參閱 < 在 Azure 上執行此應用程式

若要查看即時 web 應用程式執行完成站台嗎？ 您可以部署到 Azure 帳戶的完整版本的應用程式，只要按一下下列按鈕。

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

您需要 Azure 帳戶，以將此方案部署到 Azure。 如果您沒有帳戶，您會有下列選項：

- [開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。
- [啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。

> [!div class="step-by-step"]
> [下一步](adding-a-controller.md)
