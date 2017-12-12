---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: "驗證與服務層 (C#) |Microsoft 文件"
author: StephenWalther
description: "了解如何將驗證邏輯，從控制器動作移至個別的服務層移。 在此教學課程中，說明作者： Stephen Walther 如何您..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: f36301aef4377c6c00cb4fc33dbc5c57b1c426a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-a-service-layer-c"></a>驗證與服務層 (C#)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 了解如何將驗證邏輯，從控制器動作移至個別的服務層移。 在本教學課程中，作者： Stephen Walther 會說明如何維護尖的重要性分離，藉由隔離您的服務層，從控制器層。


本教學課程的目標是描述的方法之一在 ASP.NET MVC 應用程式中執行驗證。 在本教學課程中，您可以了解如何將驗證邏輯，從您的控制站移至個別的服務層移。

## <a name="separating-concerns"></a>區隔的考量

當您建置 ASP.NET MVC 應用程式時，您不應該將您的資料庫邏輯內部控制器動作。 混用您的資料庫和控制器邏輯，可讓您的應用程式更難維護一段時間。 建議您將所有的資料庫邏輯放在不同的儲存機制的圖層。

例如，列出 1 包含名為 ProductRepository 簡單的儲存機制。 產品儲存機制包含所有應用程式的資料存取程式碼。 清單也包含產品儲存機制實作 IProductRepository 介面。

**列出 1 部--Models\ProductRepository.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

列表 2 中的控制站會使用儲存機制層在 [動作] 其 index 和 create （）。 請注意，此控制站不包含任何資料庫的邏輯。 建立儲存機制層可讓您維護清楚的重要性分離。 控制站會負責使用應用程式的流程控制邏輯和儲存機制會負責資料存取邏輯。

**列出 2-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>建立服務層

因此，應用程式流程控制邏輯應該位在控制器中，資料存取邏輯應該位在儲存機制。 在此情況下，請勿放置您的驗證邏輯？ 其中一個選項是將您的驗證邏輯中*服務層*。

服務層是 ASP.NET MVC 應用程式會調解控制站和儲存機制層之間的通訊中的其他層級。 服務層包含商務邏輯。 特別是，其中包含驗證邏輯。

例如，產品的服務層中列出的 3 具有 CreateProduct() 方法。 CreateProduct() 方法呼叫 ValidateProduct() 方法來驗證新產品，然後將產品傳遞至產品儲存機制。

**列出 3-Models\ProductService.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

已更新 Product 控制器中列出的 4，以使用服務層，而不是儲存機制層。 控制器層級交談的服務層。 儲存機制層交談服務層。 每個圖層都有個別的責任。

**列出 4-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

請注意，產品服務建立於產品控制器建構函式。 建立產品服務時，位於模型狀態字典會傳遞至服務。 產品服務會將驗證錯誤訊息傳回至控制器使用模型狀態。

## <a name="decoupling-the-service-layer"></a>減少服務層

我們無法找出在控制器與一點的服務層級。 在控制器電腦與服務層透過模型狀態進行通訊。 換句話說，服務層會有的相依性的 ASP.NET MVC 架構的特定功能。

我們想要找出盡可能我們控制器層從服務層。 理論上來說，我們應該能夠使用任何類型的應用程式並不只在 ASP.NET MVC 應用程式的服務層。 比方說，在未來，我們可能會想要建置 WPF 應用程式前端。 我們應設法將移除的相依性 ASP.NET MVC 模型狀態從我們的服務層。

在列出 5，使其不再使用的模型狀態已更新服務層。 相反地，它會使用任何實作 IValidationDictionary 介面的類別。

**列出 5-Models\ProductService.cs （分隔）**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

IValidationDictionary 介面被定義在程式碼範例 6。 這個簡單的介面具有單一方法和單一屬性。

**列出 6-Models\IValidationDictionary.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

列出第 7 中名為 ModelStateWrapper 類別，類別會實作 IValidationDictionary 介面。 您可以將模型狀態字典傳遞至建構函式具現化 ModelStateWrapper 類別。

**列出 7-Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

最後，在列出 8 更新的控制器會使用 ModelStateWrapper 在其建構函式中建立服務層時。

**列出 8-Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

使用 IValidationDictionary 介面，以及 ModelStateWrapper 類別可讓我們將我們的服務層，與我們的控制器層完全隔離。 服務層不再相依於模型狀態。 您可以傳遞至服務層的 IValidationDictionary 介面會實作任何類別。 例如，WPF 應用程式可能會實作 IValidationDictionary 介面的簡單集合類別。

## <a name="summary"></a>總結

本教學課程的目標是為了討論在 ASP.NET MVC 應用程式中執行驗證的其中一個方法。 在本教學課程中，您學會如何將所有驗證邏輯從您的控制站移至個別的服務層的移動。 您也學到如何建立 ModelStateWrapper 類別隔離您的服務層，從控制器層。

>[!div class="step-by-step"]
[上一頁](validating-with-the-idataerrorinfo-interface-cs.md)
[下一頁](validation-with-the-data-annotation-validators-cs.md)
