---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: 將應用程式發佈至 Azure 的 Azure App Service |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808223"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>將應用程式發佈至 Azure 的 Azure App Service
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](https://github.com/MikeWasson/BookService)

最後一個步驟中，您將應用程式發行至 Azure。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發佈**。

![](part-10/_static/image1.png)

按一下 **發佈**叫用**發佈 Web**對話方塊。 如果您勾選**雲端中的主機**當您第一次建立專案，然後連線並已設定。 在此情況下，只要按一下**設定**索引標籤，然後檢查&quot;Execute Code First Migrations&quot;。 (如果未核取**雲端中的主機**開頭，然後依照[下一節](#new-website)。)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

若要部署應用程式，請按一下**發佈**。 您可以檢視中的發佈進度**Web 發行活動**視窗。 (從**檢視**功能表上，選取**其他 Windows**，然後選取**Web 發行活動**。)

![](part-10/_static/image4.png)

完成 Visual Studio 部署應用程式，預設瀏覽器會自動開啟至已部署的網站，URL 和您所建立的應用程式現在正在雲端中。 在瀏覽器網址列中的 URL 會顯示從網際網路載入站台。

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>部署至新的網站

如果您沒有勾選**雲端中的主機**當您第一次建立專案時，您可以現在設定新的 web 應用程式。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**發佈**。 選取 **設定檔**索引標籤，然後按一下**Microsoft Azure 網站**。 如果您不目前登入至 Azure，系統會提示您登入。

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

在 [**現有的網站**] 對話方塊中，按一下**新增**。

![](part-10/_static/image9.png)

輸入網站名稱。 選取您的 Azure 訂用帳戶和區域。 底下**資料庫伺服器**，選取**建立新的伺服器**，或選取現有的伺服器。 按一下 [建立] 。

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

按一下 **設定**索引標籤，然後檢查&quot;Execute Code First Migrations&quot;。 然後按一下**發佈**。

> [!div class="step-by-step"]
> [上一步](part-9.md)
