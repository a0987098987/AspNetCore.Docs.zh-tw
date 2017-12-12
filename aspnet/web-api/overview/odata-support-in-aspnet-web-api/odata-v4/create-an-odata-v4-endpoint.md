---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: "建立 ASP.NET Web API 2.2 使用 OData v4 端點 |Microsoft 文件"
author: MikeWasson
description: "開放式資料通訊協定 (OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來查詢及管理透過 CRUD 作業的資料集..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>建立 ASP.NET Web API 2.2 使用 OData v4 端點
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 開放式資料通訊協定 (OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來查詢與操作資料集，透過 CRUD 作業 （建立、 讀取、 更新和刪除）。
> 
> ASP.NET Web 應用程式開發介面支援 v3 和 v4 通訊協定。 您甚至可以執行的並行 v4 端點與 v3 端點。
> 
> 本教學課程會示範如何建立支援 CRUD 作業的 OData v4 端點。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>教學課程版本
> 
> OData 第 3 版，請參閱[建立 OData v3 端點](../odata-v3/creating-an-odata-endpoint.md)。


## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

在 Visual Studio 中，從**檔案**功能表上，選取**新增** &gt; **專案**。

展開**已安裝** &gt; **範本** &gt; **Visual C#** &gt; **Web**，然後選取**ASP.NET Web 應用程式**範本。 將專案命名&quot;ProductService&quot;。

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

在**新專案**對話方塊中，選取**空**範本。 在下&quot;加入資料夾和核心參考...&quot;，按一下  **Web API**。 按一下 [確定]。

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>OData 封裝安裝

從**工具**功能表上，選取**NuGet 套件管理員** &gt; **Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入：

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

此命令會安裝最新的 OData NuGet 封裝。

## <a name="add-a-model-class"></a>將模型類別

A*模型*是物件，代表應用程式中的資料實體。

在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。 從內容功能表中，選取**新增** &gt; **類別**。

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> 依照慣例，模型類別都會放在 [模型] 資料夾中，但您不需要遵循這個慣例，在您自己的專案中。


將類別命名為 `Product` 。 在 Product.cs 檔案中，以下列內容取代未定案程式碼：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id`屬性是實體索引鍵。 用戶端可以查詢的實體索引鍵。 例如，若要取得識別碼 5 的產品，URI 是`/Products(5)`。 `Id`屬性也會在後端資料庫中的主索引鍵。

## <a name="enable-entity-framework"></a>啟用 Entity Framework

此教學課程中，我們會使用 Entity Framework (EF) 程式碼第一次建立後端資料庫。

> [!NOTE]
> Web API OData 不需要 EF。 使用轉譯為資料庫實體模型的任何資料存取圖層。


首先，安裝 EF NuGet 封裝。 從**工具**功能表上，選取**NuGet 套件管理員** &gt; **Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入：

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

開啟 Web.config 檔案中，並新增下列區段內**組態**項目，之後**c**項目。

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

這個設定會新增 LocalDB 資料庫的連接字串。 當您在本機執行應用程式時，會使用此資料庫。

接下來，加入名為類別`ProductsContext`到 Models 資料夾：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

在建構函式，`"name=ProductsContext"`提供連接字串的名稱。

## <a name="configure-the-odata-endpoint"></a>設定 OData 端點

開啟檔案的應用程式\_Start/WebApiConfig.cs。 加入下列**使用**陳述式：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

然後加入下列程式碼加入**註冊**方法：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

此程式碼會執行兩件事：

- 建立實體資料模型 (EDM)。
- 新增一個路由。

EDM 是抽象的資料模型。 EDM 用來建立服務的中繼資料文件。 **ODataConventionModelBuilder**類別會使用預設命名慣例來建立 EDM。 這個方法需要最少的程式碼。 如果您想要更充分掌控 EDM，您可以使用**ODataModelBuilder**類別來建立 EDM 藉由明確地新增內容、 金鑰和導覽屬性。

A*路由*告知如何將 HTTP 要求傳送至端點的 Web API。 若要建立的 OData v4 路由，請呼叫**MapODataServiceRoute**擴充方法。

如果您的應用程式有多個 OData 端點，請針對每個建立個別的路由。 唯一的路由名稱和前置詞，讓每一個路由。

## <a name="add-the-odata-controller"></a>加入 OData 控制器

A*控制器*是用來處理 HTTP 要求的類別。 您建立個別的控制站，在您的 OData 服務中設定的每個實體。 在此教學課程中，您會建立一個控制站，`Product`實體。

在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增** &gt; **類別**。 將類別命名為 `ProductsController` 。

> [!NOTE]
> 針對 OData v3 會使用這個教學課程版本**加入控制器**scaffolding。 目前沒有任何 OData v4 樣板。


以下列內容取代 ProductsController.cs 的未定案程式碼。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

控制器會使用`ProductsContext`類別使用 EF 存取資料庫。 請注意，控制器會覆寫**處置**方法處置**ProductsContext**。

這是控制器的起點。 接下來，我們要加入的所有 CRUD 作業的方法。

## <a name="querying-the-entity-set"></a>查詢的實體集

將下列方法加入`ProductsController`。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

無參數的版本`Get`方法會傳回整個產品集合。 `Get`方法*金鑰*參數由其索引鍵查閱產品 (在此情況下，`Id`屬性)。

**[EnableQuery]**屬性可讓用戶端使用查詢選項，例如 $filter、 $sort 和 $page 修改查詢。 如需詳細資訊，請參閱[支援 OData 查詢選項](../supporting-odata-query-options.md)。

## <a name="adding-an-entity-to-the-entity-set"></a>將實體加入至實體集

若要啟用新的產品加入資料庫的用戶端，新增下列方法加入`ProductsController`。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>更新實體

OData 支援兩個不同的語意來更新實體，PATCH 和 PUT。

- 修補程式會執行部分更新。 用戶端指定要更新屬性。
- PUT 會取代整個實體。

PUT 的缺點是用戶端必須傳送的所有屬性的值中的實體，包括未變更的值。 [OData 規格](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719)說明慣用修補程式。

在任何情況下，以下是 PATCH 和 PUT 方法的程式碼：

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

如果修補程式時，控制器會使用**差異&lt;T&gt;** 來追蹤變更的類型。

## <a name="deleting-an-entity"></a>刪除實體

若要啟用用戶端從資料庫刪除產品，加入下列方法加入`ProductsController`。

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
