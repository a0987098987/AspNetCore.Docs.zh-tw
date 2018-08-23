---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: 驗證與服務層 (VB) |Microsoft Docs
author: StephenWalther
description: 了解如何移動您的驗證邏輯出您對控制器動作並進入個別的服務層。 在本教學課程中，Stephen walther 將說明如何您...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: ecce8e4f0a901ce8c185d2b085f4d706bd57fa1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833208"
---
<a name="validating-with-a-service-layer-vb"></a>驗證與服務層 (VB)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> 了解如何移動您的驗證邏輯出您對控制器動作並進入個別的服務層。 在本教學課程中，Stephen Walther 會說明如何藉由隔離您的服務層，從您的控制器層級維護 sharp 關注點分離。


本教學課程的目標是說明 ASP.NET MVC 應用程式中執行驗證的一種方法。 在本教學課程中，您將了解如何移動您的驗證邏輯出您的控制器並進入個別的服務層。

## <a name="separating-concerns"></a>區隔的考量

當您建置 ASP.NET MVC 應用程式時，不應該將您的資料庫邏輯放在您對控制器動作。 混用您的資料庫和控制器邏輯，可讓您的應用程式更難維護一段時間。 建議您將所有資料庫邏輯放在個別的存放庫圖層。

例如，列表 1 包含名為 ProductRepository 簡單存放庫。 產品存放庫包含所有應用程式的資料存取程式碼。 清單也包含產品存放庫會實作 IProductRepository 介面。

**列表 1-Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

列表 2 中的控制站會使用其 index （）] 和 [create （） 動作中的儲存機制層。 請注意，此控制器不包含任何資料庫的邏輯。 建立儲存機制層，可讓您維持清楚分離關注點。 控制器負責應用程式的流程控制邏輯，並將存放庫會負責資料存取邏輯。

**列表 2-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>建立服務層

因此，應用程式流程控制邏輯位於控制器，而所屬的存放庫中的資料存取邏輯。 在此情況下，您在其中放置您的驗證邏輯？ 其中一個選項是將您的驗證邏輯，在*服務層*。

服務層是在 ASP.NET MVC 應用程式，以促成控制站和儲存機制層之間的通訊額外層級。 服務層包含商務邏輯。 特別是，它會包含驗證邏輯。

例如，產品的服務層，在 列表 3 中有 CreateProduct() 方法。 CreateProduct() 方法會呼叫 ValidateProduct() 来驗證的方法將新產品，然後將產品傳遞至產品存放庫。

**Listing 3 - Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

若要使用的服務層，而不是儲存機制層的列表 4 中已更新 Product 控制器。 在控制器層與服務層。 服務層與儲存機制層。 每一層都有不同的責任。

**列表 4-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

請注意，產品中會建立服務產品控制器建構函式。 建立產品服務時，位於模型狀態字典會傳遞至服務。 產品服務會用來通過驗證的設定，錯誤訊息傳回至控制器的模型狀態。

## <a name="decoupling-the-service-layer"></a>減少服務層

我們無法隔離控制站和服務層，在一個方面。 在控制站和服務層透過模型狀態進行通訊。 換句話說，服務層會將相依性對 ASP.NET MVC 架構的特定功能。

我們想要隔離服務層，從我們盡量的控制器層級。 理論上，我們應該能夠使用任何類型的應用程式和不只在 ASP.NET MVC 應用程式的服務層。 比方說，在未來，我們可能會想要建置 WPF 應用程式的前端。 我們應該找出方法來移除對 ASP.NET MVC 的相依性模型狀態，從我們的服務層。

表 5 中，使其不再使用的模型狀態已更新的服務層。 相反地，它會使用任何可實作 IValidationDictionary 介面的類別。

**列表 5-Models\ProductService.vb （低耦合）**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

IValidationDictionary 介面被定義在列表 6。 這個簡單的介面具有單一方法和單一屬性。

**列表 6-Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

列表 7，名為 ModelStateWrapper 類別中的類別會實作 IValidationDictionary 介面。 您可以將模型狀態字典傳遞至建構函式具現化 ModelStateWrapper 類別。

**列表 7-Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

最後，在 列表 8 更新的控制器會使用 ModelStateWrapper 其建構函式中建立的服務層時。

**列表 8-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

使用 IValidationDictionary 介面和 ModelStateWrapper 類別可讓我們完全區隔我們的服務層，從我們的控制器層級。 服務層不再相依於模型狀態。 您可以傳遞任何實作服務層的 IValidationDictionary 介面的類別。 比方說，WPF 應用程式可能會實作簡單的集合類別的 IValidationDictionary 介面。

## <a name="summary"></a>總結

本教學課程的目標是要討論的 ASP.NET MVC 應用程式中執行驗證的其中一種方法。 在本教學課程中，您已了解如何移動所有的驗證邏輯出您的控制器並進入個別的服務層。 您也學到如何建立 ModelStateWrapper 類別，以隔離您的服務層，從您的控制器層級。

> [!div class="step-by-step"]
> [上一頁](validating-with-the-idataerrorinfo-interface-vb.md)
> [下一頁](validation-with-the-data-annotation-validators-vb.md)
