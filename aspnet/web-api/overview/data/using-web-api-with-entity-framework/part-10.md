---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: "將應用程式發行至 Azure 的 Azure App Service |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a>將應用程式發行至 Azure 的 Azure App Service
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](https://github.com/MikeWasson/BookService)

最後一個步驟中，您將會發行至 Azure 應用程式。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發行**。

![](part-10/_static/image1.png)

按一下**發行**叫用**發行 Web**對話方塊。 如果您核取**雲端中的主機**當您第一次建立專案，然後連線並已設定。 在此情況下，只要按一下**設定**索引標籤上，並檢查&quot;執行 Code First 移轉&quot;。 (如果您沒有核取**雲端中的主機**開頭，然後依照中的步驟[下一節](#new-website)。)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

若要部署應用程式，按一下 **發行**。 您可以檢視中的發行進度**Web 發行活動**視窗。 (從**檢視**功能表上，選取**其他視窗**，然後選取**Web 發行活動**。)

![](part-10/_static/image4.png)

當 Visual Studio 完成部署應用程式時，預設瀏覽器會自動開啟至已部署網站的 URL，並且您建立的應用程式現在正在執行雲端中。 在瀏覽器網址列中的 URL 會顯示從網際網路載入站台。

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>部署到新的網站

如果您沒有勾選**雲端中的主機**當您第一次建立專案時，您可以現在設定新的 web 應用程式。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發行**。 選取**設定檔**索引標籤上，按一下  **Microsoft Azure 網站**。 如果您不目前登入至 Azure，系統會提示您登入。

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

在**現有網站**] 對話方塊中，按一下 [**新增**。

![](part-10/_static/image9.png)

輸入網站名稱。 選取您的 Azure 訂閱和區域。 在下**資料庫伺服器**，選取**建立新的伺服器**，或選取現有的伺服器。 按一下 [建立] 。

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

按一下**設定**索引標籤上，並檢查&quot;執行 Code First 移轉&quot;。 然後按一下 **發行**。

>[!div class="step-by-step"]
[上一步](part-9.md)
