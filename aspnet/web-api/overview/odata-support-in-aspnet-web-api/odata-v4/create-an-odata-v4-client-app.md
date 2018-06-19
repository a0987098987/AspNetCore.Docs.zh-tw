---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: 建立 OData v4 用戶端應用程式 (C#) |Microsoft 文件
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036696"
---
<a name="create-an-odata-v4-client-app-c"></a>建立 OData v4 用戶端應用程式 (C#)
====================
由[Mike Wasson](https://github.com/MikeWasson)

在上一個教學課程中，您可以建立支援 CRUD 作業的基本 OData 服務。 現在讓我們來建立服務的用戶端。

啟動 Visual Studio 的新執行個體，並建立新的主控台應用程式專案。 在**新專案**對話方塊中，選取**已安裝** &gt; **範本** &gt; **Visual C#** &gt; **Windows 桌面**，然後選取**主控台應用程式**範本。 將專案命名&quot;ProductsApp&quot;。

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> 您也可以加入至相同的 Visual Studio 方案，其中包含 OData 服務的主控台應用程式。


## <a name="install-the-odata-client-code-generator"></a>安裝 OData 用戶端程式碼產生器

從**工具**功能表上，選取**擴充功能和更新**。 選取**線上** &gt; **Visual Studio 組件庫**。 在 [搜尋] 方塊中，搜尋&quot;OData 用戶端程式碼產生器&quot;。 按一下**下載**来安裝 VSIX。 您可能會提示您重新啟動 Visual Studio。

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>在本機執行 OData 服務

從 Visual Studio 執行 ProductService 專案。 根據預設，Visual Studio 會啟動瀏覽器應用程式根目錄。 請注意 URI;您將在下一個步驟中需要此設定。 繼續執行應用程式。

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> 如果您將兩個專案都放在相同的方案時，請確定執行 ProductService 專案，但不偵錯。 在下一個步驟中，您必須保留修改主控台應用程式專案時所執行的服務。


## <a name="generate-the-service-proxy"></a>產生服務 Proxy

服務 proxy 是.NET 類別定義存取 OData 服務的方法。 Proxy 將轉譯成 HTTP 要求的方法呼叫。 您將建立 proxy 類別，藉由執行[T4 範本](https://msdn.microsoft.com/library/bb126445.aspx)。

以滑鼠右鍵按一下專案。 選取**新增** &gt; **新項目**。

![](create-an-odata-v4-client-app/_static/image5.png)

在**加入新項目**對話方塊中，選取**Visual C# 項目** &gt; **程式碼** &gt; **OData 用戶端**。 命名範本&quot;ProductClient.tt&quot;。 按一下**新增**，然後按一下安全性警告。

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

此時，您會取得發生錯誤，您可以忽略。 Visual Studio 會自動執行此範本中，但需要一些組態設定的範本。 第一次。

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

開啟檔案 ProductClient.odata.config。在`Parameter`元素中，貼上的 URI 從 ProductService 專案 （上一個步驟）。 例如: 

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

再次執行範本。 在 [方案總管] 中，以滑鼠右鍵按一下 ProductClient.tt 檔案，然後選取**執行自訂工具**。

此範本隨即建立名為 ProductClient.cs 定義 proxy 的程式碼檔案。 當您開發應用程式，如果您變更 OData 端點時，執行一次，以更新 proxy 範本。

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>使用服務 Proxy 來呼叫 OData 服務

開啟 Program.cs 檔案並以下列內容取代未定案程式碼。

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

取代的值*serviceUri*從先前的服務 URI。

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

當您執行應用程式時，它應該輸出下列各項：

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
