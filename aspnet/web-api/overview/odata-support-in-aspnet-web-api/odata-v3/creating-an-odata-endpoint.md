---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: 建立 Web API 2 OData v3 端點 |Microsoft Docs
author: MikeWasson
description: 開放式資料通訊協定 (OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來組織資料、 查詢資料，和操作資料...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 2e0d3b45fd51192d227d852dc2f05b45ca42944c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910911"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>建立 Web API 2 OData v3 端點
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [開放式資料通訊協定](http://www.odata.org/)(OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來組織資料、 查詢資料，和操作資料集，透過 CRUD 作業 （建立、 讀取、 更新和刪除）。 OData 支援 AtomPub (XML) 和 JSON 格式。 OData 也會定義用來公開資料的中繼資料。 用戶端可以使用中繼資料來探索的型別資訊和資料集的關聯性。
>
> ASP.NET Web API 可讓您更輕鬆地建立資料集的 OData 端點。 您可以控制完全的 OData 作業端點支援。 您可以裝載多個 OData 端點，以及非 OData 端點。 您可以完整控制您的資料模型、 後端商務邏輯和資料層。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - Web API 2
> - OData 版本 3
> - Entity Framework 6
> - [Fiddler Web 偵錯 Proxy （選擇性）](http://www.fiddler2.com)
>
> 已加入 web API OData 支援[ASP.NET 和 Web 工具 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650)。 不過，本教學課程會使用 Visual Studio 2013 中已加入的 scaffolding。


在本教學課程中，您將建立簡單的 OData 端點的用戶端可以查詢。 您也會建立 C# 用戶端端點。 完成本教學課程之後下, 一組教學課程示範如何新增更多的功能，包括實體關聯性，動作，並選取 展開 / $ $。

- [建立 Visual Studio 專案](#create-project)
- [加入實體模型](#add-model)
- [加入 OData 控制器](#add-controller)
- [新增 EDM 和路由](#edm)
- [（選擇性） 資料庫中植入](#seed-db)
- [瀏覽 OData 端點](#explore)
- [OData 序列化格式](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

在本教學課程中，您將建立支援基本的 CRUD 作業的 OData 端點。 端點會公開單一資源，產品清單。 稍後的教學課程會新增更多功能。

啟動 Visual Studio，然後選取**新的專案**從 [開始] 頁面。 或從**檔案**功能表上，選取**新增**，然後**專案**。

在 **範本**窗格中，選取**已安裝的範本**並展開 Visual C# 節點。 底下**Visual C#**，選取**Web**。 選取  **ASP.NET Web 應用程式**範本。

![](creating-an-odata-endpoint/_static/image1.png)

在 **新的 ASP.NET 專案**對話方塊中，選取**空白**範本。 在下&quot;新增的資料夾和核心參考...&quot;，檢查**Web API**。 按一下 [確定 **Deploying Office Solutions**]。

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>加入實體模型

「模型」是代表應用程式中資料的物件。 本教學課程中，我們需要表示一項產品的模型。 模型會對應至我們的 OData 實體類型。

在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。 從操作功能表中，選取**新增**然後選取**類別**。

![](creating-an-odata-endpoint/_static/image3.png)

在 [**加入新**項目] 對話方塊，將類別命名為&quot;產品&quot;。

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> 依照慣例，模型類別位於 Models 資料夾中。 您不需要遵循此慣例，在您自己的專案中，但我們將在本教學課程中使用它。


在 Product.cs 檔案中，新增下列類別定義：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

ID 屬性將會是實體索引鍵。 用戶端可以查詢產品識別碼。 這個欄位也會在後端資料庫中的主索引鍵。

現在就建置專案。 在下一個步驟中，我們將使用一些使用反映來尋找產品類型的 Visual Studio scaffolding。

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>加入 OData 控制器

A*控制器*是處理 HTTP 要求的類別。 您可以定義每個實體集在您的 OData 服務的另一個控制站。 在本教學課程中，我們將建立單一控制器。

在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取 **新增**，然後選取**控制站**。

![](creating-an-odata-endpoint/_static/image5.png)

在 **新增 Scaffold**對話方塊中，選取&quot;Web API 2 OData 控制器與動作，使用 Entity Framework&quot;。

![](creating-an-odata-endpoint/_static/image6.png)

在 [**新增控制器**] 對話方塊中，將控制器"ProductsController"命名。 選取 &quot;使用非同步控制器動作&quot;核取方塊。 在 **模型**下拉式清單中選取產品類別。

![](creating-an-odata-endpoint/_static/image7.png)

按一下 [**新資料內容...** ] 按鈕。 保留的資料內容型別的預設名稱，然後按一下**新增**。

![](creating-an-odata-endpoint/_static/image8.png)

在 加入控制器的 新增控制器 對話方塊中，按一下 新增。

![](creating-an-odata-endpoint/_static/image9.png)

注意： 是否您收到錯誤訊息，指出&quot;取得類型時發生錯誤...&quot;，請確定您新增的產品類別之後，內建的 Visual Studio 專案。 Scaffolding 會使用反映來尋找類別。

![](creating-an-odata-endpoint/_static/image10.png)

Scaffolding 會將兩個程式碼檔案加入專案：

- Products.cs 定義 Web API 控制器實作 OData 端點。
- ProductServiceContext.cs 提供方法來查詢基礎資料庫，並使用 Entity Framework。

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>新增 EDM 和路由

在 方案總管 中，展開 應用程式\_啟動資料夾，然後開啟名為 WebApiConfig.cs 檔案。 這個類別會包含 Web api 組態程式碼。 您可以將此程式碼取代為下列：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

此程式碼會執行兩件事：

- 建立 OData 端點的 Entity Data Model (EDM)。
- 新增端點的路由。

EDM 是資料的抽象模型。 EDM 用來建立中繼資料文件，並定義服務的 Uri。 **ODataConventionModelBuilder**會使用一組預設命名慣例 EDM 建立 EDM。 這個方法需要最少的程式碼。 如果您想要更充分掌控 EDM 中，您可以使用**ODataModelBuilder**藉由新增屬性、 金鑰和導覽屬性明確地建立 EDM 的類別。

**EntitySet**方法會將設定為 EDM 實體：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

字串 「 產品 」 會定義實體集的名稱。 控制站的名稱必須符合實體集的名稱。 在本教學課程中，實體集名為 「 產品 」 和控制站命名為`ProductsController`。 如果您名為的實體集 」 ProductSet 」，您會將控制器命名`ProductSetController`。 請注意，端點可以有多個實體集。 呼叫**EntitySet&lt;T&gt;** 每個實體集，並接著定義相對應的控制項。

**MapODataRoute**方法會將 OData 端點的路由。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

第一個參數是路由的易記名稱。 您的服務的用戶端看不到此名稱。 第二個參數是端點的 URI 前置詞。 指定此程式碼，Products 實體集的 URI 是 http://<em>hostname</em>  /odata/產品。 您的應用程式可以有一個以上的 OData 端點。 針對每個端點，呼叫<strong>MapODataRoute</strong>並提供唯一的路由名稱和唯一的 URI 前置詞。

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>（選擇性） 資料庫中植入

在此步驟中，您將使用 Entity Framework 來植入部分測試資料的資料庫。 這個步驟是選擇性的但它可讓您立即測試您的 OData 端點。

從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

這會將新增名為移轉和名為 Configuration.cs 程式碼檔案的資料夾。

![](creating-an-odata-endpoint/_static/image12.png)

開啟此檔案並新增下列程式碼`Configuration.Seed`方法。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

這些命令會產生程式碼會建立資料庫，然後執行該程式碼。

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>瀏覽 OData 端點

在本節中，我們將使用[Fiddler Web 偵錯 Proxy](http://www.fiddler2.com)將要求傳送至端點並檢查回應訊息。 這有助於您了解 OData 端點的功能。

在 Visual Studio 中，按下 f5 鍵啟動偵錯。 根據預設，Visual Studio 會開啟您的瀏覽器`http://localhost:*port*`，其中*連接埠*專案設定中所設定的通訊埠編號。

您可以變更專案設定中的連接埠號碼。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。 在 [屬性] 視窗中，選取**Web**。 輸入下方的連接埠號碼**專案 Url**。

### <a name="service-document"></a>服務文件

*服務文件*包含 OData 端點的實體集的清單。 若要取得服務文件，GET 要求傳送至服務根 URI。

使用 Fiddler，輸入下列 URI 中的**Composer**索引標籤： `http://localhost:port/odata/`，其中*連接埠*的通訊埠編號。

![](creating-an-odata-endpoint/_static/image13.png)

按一下 [ **Execute** ] 按鈕。 Fiddler 會將 HTTP GET 要求傳送至您的應用程式。 您應該會看到 Web 工作階段清單中的回應。 如果一切運作，狀態碼將會是 200。

![](creating-an-odata-endpoint/_static/image14.png)

按兩下以查看詳細資料，回應訊息偵測器索引標籤中的 Web 工作階段清單中的回應。

![](creating-an-odata-endpoint/_static/image15.png)

未經處理的 HTTP 回應訊息看起來應該如下所示：

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

根據預設，Web API 會傳回 AtomPub 格式的服務文件。 若要要求 JSON，請將下列標頭加入 HTTP 要求：

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

現在的 HTTP 回應會包含 JSON 承載：

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>服務中繼資料文件

*服務的中繼資料文件*描述使用稱為概念結構定義語言 (CSDL) 的 XML 語言的服務資料模型。 中繼資料文件會在服務中，會顯示資料的結構，而且可用來產生用戶端程式碼。

若要取得中繼資料文件，將 GET 要求傳送至`http://localhost:port/odata/$metadata`。 以下是本教學課程中所示的端點的中繼資料。

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>實體集

若要取得的產品實體集，將 GET 要求傳送至`http://localhost:port/odata/Products`。

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>實體

若要取得個別產品，將 GET 要求傳送至`http://localhost:port/odata/Products(1)`，其中"1"是產品識別碼。

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData 序列化格式

OData 支援數種序列化格式：

- Atom Pub (XML)
- JSON 的"light"（在 OData v3 中引進）
- JSON 的"verbose"(OData v2)

根據預設，Web API 會使用 AtomPubJSON"light"格式。

若要取得 AtomPub 格式，請設定 Accept 標頭為"application/atom + xml"。 以下是範例回應主體：

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

您可以看到 Atom 格式的一項明顯的缺點： 它是更多詳細資訊，JSON light 比。 不過，如果您有了解 AtomPub 的用戶端時，用戶端可能會透過 JSON 偏好該格式。

以下是實體的相同的 JSON light 版本：

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

OData 通訊協定第 3 版中引進了 JSON light 的格式。 回溯相容性，用戶端可以要求較舊的"verbose"JSON 格式。 若要要求詳細資訊 JSON，請設定 Accept 標頭為`application/json;odata=verbose`。 以下是詳細的版本：

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

此格式會傳達在回應主體中，整個工作階段可以加入可觀的額外負荷的更多中繼資料。 此外，它會加入一層間接層包裝在一個名為"d"屬性中的物件。

## <a name="next-steps"></a>後續步驟

- [新增實體關聯性](working-with-entity-relations.md)
- [加入 OData 動作](odata-actions.md)
- [從.NET 用戶端呼叫 OData 服務](calling-an-odata-service-from-a-net-client.md)
