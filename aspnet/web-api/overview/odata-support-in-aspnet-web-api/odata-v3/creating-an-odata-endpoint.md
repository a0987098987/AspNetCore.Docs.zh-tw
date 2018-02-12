---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: "建立與 Web API 2 OData v3 端點 |Microsoft 文件"
author: MikeWasson
description: "開放式資料通訊協定 (OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來組織資料、 查詢資料，以及操作資料..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 33fe4d764bf9bf64c852f1269255925b5cc42536
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>建立與 Web API 2 OData v3 端點
====================
由[Mike Wasson](https://github.com/MikeWasson)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [開放式資料通訊協定](http://www.odata.org/)(OData) 是網站的資料存取通訊協定。 OData 提供統一的方式來組織資料、 查詢資料，以及操作資料集，透過 CRUD 作業 （建立、 讀取、 更新和刪除）。 OData 支援 AtomPub (XML) 和 JSON 格式。 OData 也定義公開 （expose） 之資料的相關中繼資料的方式。 用戶端可以使用探索的類型資訊和資料集的關聯性的中繼資料。
> 
> ASP.NET Web API 輕鬆地建立資料集的 OData 端點。 您可以控制完全的 OData 作業端點支援。 您可以主控多個 OData 端點，以及非 OData 端點。 您必須完整控制您的資料模型、 後端商務邏輯和資料層。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData 第 3 版
> - Entity Framework 6
> - [Fiddler Web 偵錯 Proxy （選擇性）](http://www.fiddler2.com)
> 
> 已加入 web API OData 支援[ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。 不過，本教學課程會使用 Visual Studio 2013 中已加入的 scaffolding。


在本教學課程中，您將建立簡單的 OData 端點的用戶端可以查詢。 您也將建立的 C# 用戶端端點。 完成本教學課程之後下, 一組的教學課程顯示如何新增更多的功能，包括實體關聯的動作，，然後選取 展開 / $ $。

- [建立 Visual Studio 專案](#create-project)
- [加入實體模型](#add-model)
- [加入 OData 控制器](#add-controller)
- [新增 EDM 和路由](#edm)
- [植入資料庫 （選擇性）](#seed-db)
- [瀏覽 OData 端點](#explore)
- [OData 序列化格式](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>建立 Visual Studio 專案

在本教學課程中，您將建立支援基本 CRUD 作業的 OData 端點。 端點會公開單一資源，一份產品清單。 之後的教學課程會加入更多的功能。

啟動 Visual Studio，然後選取**新專案**從 [開始] 頁面。 或從**檔案**功能表上，選取**新增**然後**專案**。

在**範本**窗格中，選取**已安裝的範本**並展開 [Visual C#] 節點。 在下**Visual C#**，選取**Web**。 選取**ASP.NET Web 應用程式**範本。

![](creating-an-odata-endpoint/_static/image1.png)

在**新增 ASP.NET 專案**對話方塊中，選取**空**範本。 在下&quot;加入資料夾和核心參考...&quot;，檢查**Web API**。 按一下 [確定 **Deploying Office Solutions**]。

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>加入實體模型

「模型」是代表應用程式中資料的物件。 此教學課程中，我們需要代表產品的模型。 模型會對應至我們的 OData 實體類型。

在 [方案總管] 中，以滑鼠右鍵按一下 [模型] 資料夾。 從內容功能表中，選取**新增**然後選取**類別**。

![](creating-an-odata-endpoint/_static/image3.png)

在**加入新**項目 對話方塊，將類別&quot;產品&quot;。

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> 依照慣例，模型類別放在 [模型] 資料夾。 您不需要遵循這個慣例，在您自己的專案中，但我們將在本教學課程中使用它。


在 Product.cs 檔案中，加入下列類別定義：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

ID 屬性為實體索引鍵。 用戶端可以查詢產品的識別碼。 這個欄位也會在後端資料庫中的主索引鍵。

現在就建置專案。 在下一個步驟中，我們會使用某些 Visual Studio scaffolding，會使用反映來尋找產品類型。

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>加入 OData 控制器

A*控制器*是用來處理 HTTP 要求的類別。 您定義每個實體集在您的 OData 服務的個別控制站。 在本教學課程中，我們將建立單一控制器。

在 [方案總管] 中，以滑鼠右鍵按一下 [控制器] 資料夾。 選取**新增**，然後選取 **控制器**。

![](creating-an-odata-endpoint/_static/image5.png)

在**新增 Scaffold**對話方塊中，選取&quot;Web API 2 OData 控制器動作，使用 Entity Framework&quot;。

![](creating-an-odata-endpoint/_static/image6.png)

在**加入控制器**對話方塊中，控制站 」 ProductsController"。 選取&quot;使用非同步控制器動作&quot;核取方塊。 在**模型**下拉式清單中選取產品類別。

![](creating-an-odata-endpoint/_static/image7.png)

按一下**新的資料內容...**  按鈕。 保留預設名稱的資料內容型別，然後按一下**新增**。

![](creating-an-odata-endpoint/_static/image8.png)

在 加入控制器的 加入控制器 對話方塊中，按一下 新增。

![](creating-an-odata-endpoint/_static/image9.png)

注意： 是否您收到錯誤訊息，指出&quot;取得型別時發生錯誤...&quot;，請確定您加入的產品類別之後，建立 Visual Studio 專案。 樣板會使用反映來尋找類別。

![](creating-an-odata-endpoint/_static/image10.png)

Scaffolding 將兩個程式碼檔案加入專案：

- Products.cs 定義實作 OData 端點的 Web API 控制器。
- ProductServiceContext.cs 提供方法來查詢基礎資料庫，並使用 Entity Framework。

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>新增 EDM 和路由

在 方案總管 中，展開 應用程式\_啟動資料夾，然後開啟名為 WebApiConfig.cs 的檔案。 這個類別會保存組態程式碼的 Web API。 以下列內容取代此程式碼：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

此程式碼會執行兩件事：

- 建立 OData 端點的實體資料模型 (EDM)。
- 將端點的路由。

EDM 是抽象的資料模型。 EDM 用來建立中繼資料文件，並定義服務的 Uri。 **ODataConventionModelBuilder**會使用一組預設命名慣例 EDM 建立 EDM。 這個方法需要最少的程式碼。 如果您想要更充分掌控 EDM，您可以使用**ODataModelBuilder**類別來建立 EDM 藉由明確地新增內容、 金鑰和導覽屬性。

**EntitySet**方法會將設定為 EDM 實體：

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

字串"Products"定義的實體集的名稱。 控制器名稱必須符合實體集的名稱。 在本教學課程中，實體集為 「 產品 」，並控制站命名為`ProductsController`。 如果您的實體集 」 ProductSet"、 命名控制器`ProductSetController`。 請注意，端點可以有多個實體集。 呼叫**EntitySet&lt;T&gt;** 每個實體集，並接著定義相對應的控制項。

**MapODataRoute**方法將 OData 端點的路由。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

第一個參數是路由的好記名稱。 您的服務的用戶端不會看到此名稱。 第二個參數是端點的 URI 前置詞。 指定此程式碼，Products 實體集的 URI 是 http://*hostname*  /odata/產品。 您的應用程式可以有多個 OData 端點。 針對每個端點，呼叫**MapODataRoute**並提供唯一的路由名稱和唯一的 URI 前置詞。

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>植入資料庫 （選擇性）

在此步驟中，您將使用 Entity Framework 來植入某些測試資料的資料庫。 這個步驟是選擇性的但它可讓您以立即開始測試您的 OData 端點。

從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

如此會將名為移轉，以及名為 configuration.cs 中的程式碼檔案的資料夾。

![](creating-an-odata-endpoint/_static/image12.png)

開啟此檔案並加入下列程式碼加入`Configuration.Seed`方法。

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

這些命令會產生程式碼會建立資料庫，然後執行該程式碼。

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>瀏覽 OData 端點

在本節中，我們將使用[Fiddler Web 偵錯 Proxy](http://www.fiddler2.com)能將要求傳送至端點，以及檢查回應訊息。 這有助於您了解 OData 端點的功能。

在 Visual Studio 中，按 f5 鍵開始偵錯。 根據預設，Visual Studio 開啟您的瀏覽器`http://localhost:*port*`，其中*連接埠*專案設定中設定的通訊埠編號。

您可以變更專案設定中的連接埠號碼。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。 在 [屬性] 視窗中，選取**Web**。 輸入下方的連接埠號碼**專案 Url**。

### <a name="service-document"></a>服務文件

*服務文件*包含 OData 端點的實體集的清單。 若要取得服務文件，GET 要求傳送至根服務的 URI。

使用 Fiddler，輸入下列 URI 中的**編輯器** 索引標籤： `http://localhost:port/odata/`，其中*連接埠*通訊埠編號。

![](creating-an-odata-endpoint/_static/image13.png)

按一下**Execute**  按鈕。 Fiddler 會將 HTTP GET 要求傳送至您的應用程式。 您應該會看到 Web 工作階段清單中的回應。 如果一切正常，狀態碼將會是 200。

![](creating-an-odata-endpoint/_static/image14.png)

按兩下以查看 [偵測器] 索引標籤中的回應訊息的詳細資料的 Web 工作階段清單中的回應。

![](creating-an-odata-endpoint/_static/image15.png)

未經處理的 HTTP 回應訊息看起來應該如下所示：

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

根據預設，Web API 會傳回 AtomPub 格式的服務文件。 若要要求 JSON，新增下列標頭的 HTTP 要求：

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

現在 HTTP 回應會包含 JSON 裝載：

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>服務中繼資料文件

*服務中繼資料文件*描述使用稱為概念結構定義語言 (CSDL) 的 XML 語言的服務資料模型。 中繼資料文件，在服務中顯示的資料結構，而且可用來產生用戶端程式碼。

若要取得中繼資料文件，傳送 GET 要求來`http://localhost:port/odata/$metadata`。 以下是本教學課程中所顯示的端點的中繼資料。

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>實體集

若要取得 Products 實體集，傳送 GET 要求來`http://localhost:port/odata/Products`。

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>實體

若要取得個別產品，傳送 GET 要求來`http://localhost:port/odata/Products(1)`，其中"1"是產品識別碼。

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>OData 序列化格式

OData 支援數種序列化格式：

- Atom Pub (XML)
- JSON 的"light"（在 OData v3 中導入）
- JSON 的"verbose"(OData v2)

根據預設，Web API 會使用 AtomPubJSON"light"格式。 

若要取得 AtomPub 格式，請設定 Accept 標頭為"application/atom + xml"。 以下是範例回應主體：

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

您可以看到 Atom 格式的一個明顯的缺點： 它會比 JSON light 更多詳細資訊。 不過，如果您已了解 AtomPub client，用戶端可能會透過 JSON 偏好該格式。

以下是實體的相同的 JSON light 版本：

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

OData 通訊協定第 3 版中引進了 JSON light 格式。 回溯相容性，用戶端可以要求較舊的"verbose"JSON 格式。 若要要求詳細資訊 JSON，設定 Accept 標頭為`application/json;odata=verbose`。 以下是詳細的版本：

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

此格式會傳遞回應主體中，可增加可觀的額外負荷的整個工作階段中的多個中繼資料。 此外，它會加入間接取值層的級物件包裝在名為"d"的屬性。

## <a name="next-steps"></a>後續步驟

- [將實體關聯性](working-with-entity-relations.md)
- [加入 OData 動作](odata-actions.md)
- [從.NET 用戶端呼叫 OData 服務](calling-an-odata-service-from-a-net-client.md)
