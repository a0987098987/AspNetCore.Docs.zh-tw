---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: 在 ASP.NET 4.5 的新 Web Form |Microsoft Docs
author: rick-anderson
description: ASP.NET Web Form 的新版本導入了一些重點在於提升使用者經驗，使用資料時的增強功能。 在舊版的...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 4e8c8f303851b7f1a01744cab58e27a9b37127a6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389227"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>在 asp.net 4.5 Web Form 中最新消息
====================
藉由[Web Camp 小組](https://twitter.com/webcamps)

> ASP.NET Web Form 的新版本導入了一些重點在於提升使用者經驗，使用資料時的增強功能。
> 
> 在舊版的 Web Form，以發出物件成員的值中使用資料繫結時，您可以使用 bind （） 或 eval （） 的資料繫結運算式。 在新的 ASP.NET 版本中，您就能夠宣告哪些類型的資料控制項即將使用新的 ItemType 屬性繫結至項目。 設定這個屬性可讓您使用強型別變數，以接收之完整優勢的 Visual Studio 開發經驗中，例如 IntelliSense、 成員瀏覽，以及編譯時間檢查。
> 
> 資料繫結控制項中，您現在也可以指定您自己自訂的方法，如選取、 更新、 刪除和插入資料，簡化頁面控制項和應用程式邏輯之間的互動。 此外，ASP.NET 中，這表示您可以從頁面的資料直接對應至方法的型別參數已新增模型繫結功能。
> 
> 驗證使用者輸入也應該容易，因為最新版的 Web Form。 您現在可以加上註解您的模型類別，以驗證屬性從**System.ComponentModel.DataAnnotations**命名空間和您的網站控制的要求驗證使用者輸入使用該資訊。 與 jQuery，提供清除器用戶端程式碼和不顯眼的 JavaScript 功能，現在已整合在 Web Form 中的用戶端驗證。
> 
> 在要求驗證區域中，進行了改進以便選擇性地關閉您的應用程式的特定部分的要求驗證，或讀取無效的要求資料變得更加容易。
> 
> 一些改善已對 Web Form 伺服器控制項，以充分利用的 HTML5 的新功能：
> 
> - TextBox 控制項的 TextMode 屬性已更新以支援新 HTML5 輸入的類型，例如電子郵件、 datetime、 依此類推。
> - FileUpload 控制項現在支援從瀏覽器支援此 HTML5 功能的多個檔案上傳。
> - 驗證程式控制項現在支援驗證 HTML5 輸入項目。
> - 新的 HTML5 項目具有表示屬性的 URL 現在支援 runat =&quot;server&quot;。 如此一來，您可以使用 ASP.NET 慣例在 URL 路徑，例如 ~ 來代表應用程式根目錄運算子 (例如&lt;視訊 runat =&quot;伺服器&quot;src =&quot;~/myVideo.wmv&quot; &gt; &lt;/視訊&gt;)。
> - 已修正 UpdatePanel 控制項來支援張貼的 HTML5 輸入的欄位。
> 
> 您可以在官方 ASP.NET 入口網站中找到的新功能的更多範例 ASP.NET WebForms 4.5 中： [What's New in ASP.NET 4.5 和 Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> 所有的範例程式碼和程式碼片段會包含在 Web 研討會訓練套件，可在[ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409)。


<a id="Objectives"></a>
### <a name="objectives"></a>目標

在這個實際操作實驗室中，您將了解如何：

- 使用強型別資料繫結運算式
- 在 Web Form 中使用模型繫結的新功能
- 將頁面資料對應到程式碼後置方法時，用於值提供者
- 使用資料註解來驗證使用者輸入
- 在 Web Form 中採取 unobstrusive 用戶端驗證，與 jQuery 的 advange
- 實作詳細的要求驗證
- 實作非同步頁面處理 Web Form 中

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必要條件

您必須具備下列項目，即可完成此實驗室：

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或更好 (讀取[附錄 A](#AppendixA)如需有關如何安裝它)。

<a id="Setup"></a>
### <a name="setup"></a>安裝程式

**安裝程式碼片段**

為了方便起見，大部分的程式碼，您將沿著這個實驗室管理可從 Visual Studio 程式碼片段。 若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。

如果您不熟悉 Visual Studio 程式碼片段，而且想要了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。

<a id="Exercises"></a>
## <a name="exercises"></a>練習

這個實際操作實驗室還包含下列練習：

1. [練習 1： 建立 ASP.NET Web Form 中的繫結的模型](#Exercise1)
2. [練習 2： 資料驗證](#Exercise2)
3. [練習 3： 非同步頁面處理在 ASP.NET Web Form](#Exercise3)

> [!NOTE]
> 每個練習會伴隨**結束**包含完成練習之後，您應該取得所產生的方案資料夾。 如果您需要的所有練習所使用的其他說明，您可以使用此解決方案作為指南。


完成這個實驗室估計時間： **60 分鐘**。

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>練習 1： 建立 ASP.NET Web Form 中的繫結的模型

ASP.NET Web Form 的新版本導入了一些增強功能，著重於使用資料時，改善經驗。 在這個練習中，您將了解強型別資料控制項，並模型繫結。

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>工作 1-使用強型別資料繫結

在這個工作中，您會發現的新強型別繫結 ASP.NET 4.5 中所提供。

1. 開啟**開始**解決方案位於**來源/Ex1-ModelBinding/開始/** 資料夾。

   1. 您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**Customers.aspx**頁面。 將未編號的清單中的主控制項，並包含重複項控制項內用於列出每個客戶。 中繼器的名稱設為**customersRepeater**如下列程式碼所示。

    在舊版的 Web Form，以發出物件成員的值中使用資料繫結時您是資料繫結，您會使用資料繫結運算式，以及呼叫 Eval 方法，傳遞該成員的名稱做為字串。

    在執行階段，評估這些呼叫會使用反映，對目前繫結的物件來讀取具有指定名稱，該成員的值和 html 格式顯示結果。 這種方法可讓您很容易針對任意、 unshaped 資料進行資料繫結。

    不幸的是，您會遺失的許多絕佳的開發階段體驗功能，在 Visual Studio 中的成員名稱，讓您瀏覽 （例如移至定義），並編譯時間檢查，包括 IntelliSense。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. 開啟**Customers.aspx.cs**檔案。
4. 新增下列 using 陳述式。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. 在 **網頁\_負載**方法，加入程式碼以填入客戶清單與重複項。

    (程式碼片段- *Web Form 實驗室-Ex01-繫結的客戶資料來源*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    解決方案會使用 EntityFramework CodeFirst 以及建立及存取資料庫。 下列程式碼，customersRepeater 繫結至具體化的查詢，傳回資料庫中的所有客戶。
6. 按下**F5**來執行方案，然後前往**客戶**頁面，以查看作用中的重複項。 解決方案是使用 CodeFirst，就會建立並執行應用程式時，本機 SQL Express 執行個體中填入資料庫。

    ![列出的客戶 repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "列出 repeater 的客戶")

    *列出 repeater 的客戶*

    > [!NOTE]
    > 在 Visual Studio 2012 中，IIS Express 是預設的 Web 程式開發伺服器。
7. 關閉瀏覽器並移回至 Visual Studio。
8. 現在將使用強型別繫結的實作。 開啟**Customers.aspx**頁面上，然後使用新**ItemType**屬性設定中繼器中的**客戶**繫結類型為的型別。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    ItemType 屬性可讓您宣告的資料類型是要繫結至控制項和可讓您使用強型別內的資料繫結控制項繫結。
9. 取代為下列程式碼內容 ItemTemplate。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    使用上述方法的一項缺點是，eval 和 bind （） 呼叫晚期繫結-這表示您傳遞來表示屬性名稱的字串。 這表示您未取得成員名稱、 支援程式碼瀏覽 （例如移至定義），也不是編譯時期檢查支援 Intellisense。

    設定 ItemType 屬性會導致兩個新的具型別的變數，來產生資料繫結運算式的範圍內：**項目**並**BindItem**。 您可以在資料繫結運算式中使用這些強類型的變數，並取得之完整優勢的 Visual Studio 開發經驗。

    &quot; **:** &quot;運算式中使用會自動進行 HTML 編碼的輸出，以避免發生安全性問題 （例如，跨網站指令碼攻擊）。 這個標記法是自.NET 4 回應撰寫，但現在也會提供資料繫結運算式。

    > [!NOTE]
    > 項目成員適用於單向繫結。 如果您想要執行使用雙向繫結**BindItem**成員。

    ![強型別繫結中的 IntelliSense 支援](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "強型別繫結中的 IntelliSense 支援")

    *強型別繫結中的 IntelliSense 支援*
10. 按下**F5**執行方案，並移至 [客戶] 頁面，以確定所做的變更在如預期般運作。

    ![列出客戶詳細資料](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "列出客戶詳細資料")

    *列出客戶詳細資料*
11. 關閉瀏覽器並移回至 Visual Studio。

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>工作 2-在 Web Form 中繫結的模型簡介

在舊版的 ASP.NET Web Form 中，當您想要執行雙向資料繫結，擷取和更新資料，您需要使用資料來源物件。 這可能是物件資料來源、 SQL 資料來源、 LINQ 資料來源，依此類推。 不過如果您的案例需要自訂程式碼來處理資料，您必須使用物件資料來源中，這會帶來一些缺點。 例如，您需要避免使用複雜類型，而您需要執行驗證邏輯時處理例外狀況。

在新版本中的資料繫結控制項支援的 ASP.NET Web Form 模型繫結。 這表示，您可以指定選取、 更新、 插入和刪除方法直接在呼叫邏輯，從您的程式碼後置檔案，或從另一個類別的資料繫結控制項中。

若要深入了解，您將使用 GridView 列出使用新的產品類別**SelectMethod**屬性。 這個屬性可讓您指定的擷取 GridView 資料方法。

1. 開啟**Products.aspx**頁面上，而且包含**GridView**。 若要使用強型別繫結並啟用排序和分頁，如下所示，請設定 GridView。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. 使用新**SelectMethod**屬性來設定要呼叫 GridView **GetCategories**方法，以選取的資料。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. 開啟**Products.aspx.cs**程式碼後置檔案，並新增下列 using 陳述式。

    (程式碼片段- *Web Form 實驗室-Ex01-命名空間*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. 加入私用成員**產品**類別，將指派的新執行個體**ProductsContext**。 這個屬性會儲存 Entity Framework 資料內容，可讓您連接到資料庫。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. 建立**GetCategories**方法來擷取使用 LINQ 的類別目錄的清單。 查詢將包含**產品**屬性，好 GridView 可以顯示每個類別目錄的產品數量。 請注意，此方法會傳回未經處理的 IQueryable 物件，代表查詢之後執行網頁生命週期。

    (程式碼片段- *Web Form 實驗室-Ex01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > 在舊版的 ASP.NET Web Form，啟用排序和分頁使用您自己的存放庫邏輯，在物件資料來源內容，才能撰寫您自己的自訂程式碼，並接收所有必要的參數。 現在，當資料繫結方法可以傳回 IQueryable，而這表示查詢仍然是用來執行，ASP.NET 可以處理修改查詢，以便將適當的排序和分頁的參數。
6. 按下**F5**開始偵錯網站並移至 [產品] 頁面。 您應該會看到 GridView 會填入 GetCategories 方法所傳回的類別。

    ![填入使用模型繫結 GridView](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "填入 GridView，使用模型繫結")

    *填入 GridView，使用模型繫結*
7. 按下**SHIFT**+**F5**停止偵錯。

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>工作 3-在模型繫結中的值提供者

模型繫結不僅可讓您指定自訂的方法，以使用您直接在資料繫結控制項中的資料，也可讓您將從頁面的資料對應至從這些方法的參數。 在方法參數，您可以使用值提供者屬性指定值的資料來源。 例如: 

- 在頁面上的控制項
- 查詢字串值
- 檢視資料
- 工作階段狀態
- Cookie
- 已張貼的表單資料
- 檢視狀態
- 此外，也支援自訂值提供者

如果您已使用 ASP.NET MVC 4，您會注意到的模型繫結支援類似。 事實上，這些功能已取自 ASP.NET MVC，並移到**System.Web**可以也在 Web Form 上使用這些組件。

在這個工作中，您將會更新 GridView，以篩選結果的每個類別中的產品數量來使用模型繫結接收 filter 參數。

1. 請返回**Products.aspx**頁面。
2. 在 GridView 的頂端，新增**標籤**並**ComboBox**選取每個類別目錄的產品數目，如下所示。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. 新增**EmptyDataTemplate**至 GridView 與選取的產品數目沒有任何類別時，顯示一則訊息。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. 開啟**Products.aspx.cs**程式碼後置，並新增下列 using 陳述式。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. 修改**GetCategories**方法來接收整數**minProductsCount**引數和篩選傳回的結果。 若要這樣做，請以下列程式碼取代方法。

    (程式碼片段- *Web Form 實驗室-Ex01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    新 **[控制項]** 屬性上**minProductsCount**引數可讓 ASP.NET 知道必須在網頁中使用控制項填入其值。 ASP.NET 會尋找相符的引數 (minProductsCount) 名稱的任何控制項，並執行必要的對應和轉換来填入的控制項值的參數。

    或者，屬性會提供可讓您指定控制項位置，以取得此值的多載建構函式。

    > [!NOTE]
    > 資料繫結功能的其中一個目標是減少需要寫入的頁面互動的程式碼數量。 除了 [控制項] 值提供者，您可以使用其他的模型繫結提供者在方法參數。 其中部分所述工作的簡介。
6. 按下**F5**開始偵錯網站並移至 [產品] 頁面。 選取下拉式清單中的產品數目，並注意如何 GridView 現在已更新。

    ![篩選下拉式清單值 GridView](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "篩選下拉式清單值 GridView")

    *篩選下拉式清單值 GridView*
7. 停止偵錯。

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>工作 4-使用模型繫結進行篩選

在這個工作中，您會將第二個，子 GridView，以顯示所選分類內的產品。

1. 開啟**Products.aspx**頁面上，並更新分類來自動產生 [選取] 按鈕的 GridView。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. 新增第二**GridView**名為**productsGrid**底部。 設定**ItemType**要**WebFormsLab.Model.Product**，則**DataKeyNames**至**ProductId**和**SelectMethod**要**GetProducts**。 設定**AutoGenerateColumns**要**false**和 ProductId、 ProductName、 描述和 UnitPrice 加入資料行。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. 開啟**Products.aspx.cs**程式碼後置檔案。 實作**GetProducts**方法，以獲取 GridView 的類別目錄中的類別目錄識別碼，並篩選產品。 模型繫結會設定使用選取的資料列中的參數值**categoriesGrid**。 因為不相符的引數名稱和控制項名稱，您應該在控制項的值提供者中指定控制項的名稱。

    (程式碼片段- *Web Form 實驗室-Ex01-GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > 這種方法能夠簡化單元測試這些方法。 單元測試內容，其中未執行 Web Form，在 [控制項] 屬性不會執行任何特定動作。
4. 開啟**Products.aspx**頁面上，並找出產品 GridView。 更新 GridView，以顯示連結，以編輯所選的產品的產品。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. 開啟**ProductDetails.aspx**頁面上的程式碼後置，並取代**SelectProduct**為下列程式碼的方法。

    (程式碼片段- *Web Form 實驗室-Ex01-SelectProduct 方法*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > 請注意， **[查詢字串]** 屬性用來填滿的查詢字串中的 productId 參數的方法參數。
6. 按下**F5**開始偵錯網站並移至 [產品] 頁面。 從 GridView 的類別中選取任何分類，並請注意，要更新的產品 GridView。

    ![顯示所選分類的產品](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "顯示所選分類的產品")

    *顯示所選分類的產品*
7. 按一下 [**檢視**產品，以開啟 ProductDetails.aspx] 頁面上的連結。

    請注意頁面與使用 productId 參數從查詢字串 SelectMethod，擷取產品。

    ![檢視產品詳細資料](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "檢視產品詳細資料")

    *檢視產品詳細資料*

    > [!NOTE]
    > 在下一個練習中，將會實作能夠輸入 HTML 的描述。

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>工作 5-使用模型繫結進行更新作業

在先前工作中，您有使用模型繫結主要是針對選取的資料，在這項工作中，您將學習如何在更新作業中使用模型繫結。

您將會更新，讓使用者更新類別目錄的 GridView 的類別。

1. 開啟**Products.aspx**頁面上，並更新分類來自動產生 [編輯] 按鈕，並使用新的 GridView **UpdateMethod**屬性來指定**UpdateCategory**方法來更新選取的項目。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    在 gridview 裡的 DataKeyNames 屬性會定義哪些是可唯一識別的模型繫結物件的成員，因此，這應該至少會收到更新方法的參數。
2. 開啟**Products.aspx.cs**程式碼後置檔案，並實作**UpdateCategory**方法。 此方法應該會收到載入目前的類別填入 GridView 中的值，然後更新分類的類別目錄識別碼。

    (程式碼片段- *Web Form 實驗室-Ex01-UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    新**TryUpdateModel**頁面類別中的方法會負責擴展模型物件使用中的頁面從控制項的值。 在此情況下，它會取代為正在編輯目前的 GridView 資料列的更新的值**分類**物件。

    > [!NOTE]
    > 下一個練習中會說明來驗證使用者輸入時編輯的物件資料 ModelState.IsValid 的使用方式。
3. 執行網站並移至 [產品] 頁面。 編輯類別目錄。 輸入新名稱，然後按一下**更新**來保存變更。

    ![編輯類別目錄](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "編輯類別目錄")

    *編輯類別目錄*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>練習 2： 資料驗證

在此練習中，您將了解 ASP.NET 4.5 中的新資料驗證功能。 您將會檢查在 Web Form 中的新低調驗證功能。 您可以使用資料註解應用程式模型類別中的使用者輸入驗證，以及最後，您將了解如何開啟或關閉在網頁中的個別控制項的要求驗證。

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>工作 1-低調驗證

具有複雜的資料，包括驗證程式的表單通常會在頁面中，可以代表約 60%的程式碼產生太多的 JavaScript 程式碼。 啟用不顯眼的驗證，您的 HTML 程式碼看起來更簡潔且一個更有條理。

在本節中，您將啟用低調驗證在 ASP.NET 中要比較這兩個組態所產生的 HTML 程式碼。

1. 開啟**Visual Studio 2012** ，然後開啟**開始**解決方案位於**Source\Ex2 Validation\Begin**本實驗室的資料夾。 或者，您可以繼續使用您現有的方案，從上一個練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請在 [方案總管] 中，按一下**WebFormsLab**專案**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 按下**F5**啟動 web 應用程式。 請移至客戶頁面，然後按一下**新增客戶**連結。
3. 在瀏覽器 頁面上，以滑鼠右鍵按一下並選取**檢視原始檔**選項來開啟應用程式所產生的 HTML 程式碼。

    ![顯示頁面的 HTML 程式碼](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "顯示網頁的 HTML 程式碼")

    *顯示頁面的 HTML 程式碼*
4. 捲動頁面原始碼，並注意 ASP.NET 已插入 JavaScript 程式碼和資料驗證頁面中執行的驗證，並顯示錯誤清單。

    ![驗證 CustomerDetails 頁面中的 JavaScript 程式碼](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "CustomerDetails 頁面中的 JavaScript 驗證程式碼")

    *驗證 CustomerDetails 頁面中的 JavaScript 程式碼*
5. 關閉瀏覽器並移回至 Visual Studio。
6. 現在您將啟用低調驗證。 開啟**Web.Config**並找出**ValidationSettings:UnobtrusiveValidationMode**中的索引鍵**AppSettings**一節 **。** 將索引鍵的值設定為**WebForms**。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > 您也可以設定這個屬性&quot;**頁\_負載**&quot;萬一您想要啟用低調驗證只會針對某些頁面的事件。
7. 開啟**CustomerDetails.aspx**然後按**F5**啟動 Web 應用程式。
8. 按下 F12 鍵以開啟 IE 開發人員工具。 開發人員工具開啟後，選取 指令碼 索引標籤。選取  **CustomerDetails.aspx**功能表中進行指令碼才能執行頁面上的 jQuery 的附註已經載入瀏覽器的本機站台。

    ![直接從本機 IIS 伺服器載入 jQuery JavaScript 檔案](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "載入 jQuery JavaScript 檔案直接從本機 IIS 伺服器")

    *直接從本機 IIS 伺服器載入 jQuery JavaScript 檔案*
9. 關閉瀏覽器，若要返回 Visual Studio。 開啟**Site.Master**檔案一次，並找出**ScriptManager**。 將屬性加入**加入 EnableCdn**屬性具有值 **，則為 True**。 這會強制 jQuery 載入從線上的 URL，不是從本機站台的 URL。
10. 開啟**CustomerDetails.aspx** Visual Studio 中。 按下 F5 鍵以執行網站。 Internet Explorer 開啟後，請按 F12 鍵以開啟 開發人員工具。 選取 **指令碼**索引標籤，然後再看看下拉式清單。 請注意 jQuery JavaScript 檔案不會再載入本機的站台，但而不是從線上 jQuery CDN。

    ![正在載入 jQuery JavaScript 檔案從 CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "載入 jQuery JavaScript 檔案從 CDN")

    *載入來自 CDN 的 jQuery JavaScript 檔案*
11. 開啟一次使用瀏覽器中的 檢視來源選項的 HTML 頁面程式碼。 請注意，藉由啟用低調驗證 ASP.NET 已取代為插入的 JavaScript 程式碼的資料-\*屬性。

    ![低調驗證程式碼](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "低調驗證程式碼")

    *低調驗證程式碼*

    > [!NOTE]
    > 在此範例中，您會看到如何驗證摘要使用資料註解已簡化，以少數的 HTML 和 JavaScript 行。 先前，而不需低調驗證，更多的驗證控制項新增，愈大 JavaScript 驗證程式碼會變大。

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>工作 2-驗證模型使用資料註解

ASP.NET 4.5 介紹 Web Form 的資料註解驗證。 您不需要在每個輸入的驗證控制項，現在可以在您的模型類別中定義條件約束，並跨所有 web 應用程式中使用它們。 在本節中，您將學習如何使用資料註解驗證新增/編輯客戶表單。

1. 開啟**CustomerDetail.aspx**頁面。 請注意，客戶名字和中的第二個名稱**EditItemTemplate**並**InsertItemTemplate**區段會驗證使用 RequiredFieldValidator 控制項。 每一個驗證程式是關聯至特定的條件，因此您需要包含多個驗證做為要檢查的條件。
2. 新增資料註解，以驗證客戶模型類別。 開啟**Customer.cs**類別中**模型**資料夾以及*裝飾*每個使用資料註解屬性的屬性。

    (程式碼片段- *Web Form 實驗室-Ex02-資料註解*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 已擴充現有的資料註釋集合。 以下是一些您可以使用資料註解: [信用卡]，[Phone]，[EmailAddress]，[範圍]，[比較]，[Url]，[FileExtensions]、 [Required]、[Key]，[RegularExpression]。
    > 
    > 某些使用方式範例：
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > 您也可以定義您自己的錯誤訊息中的每個屬性。
3. 開啟**CustomerDetails.aspx**和 FormView 控制項的 EditItemTemplate 和 InsertItemTemplate 各節中移除所有 RequiredFieldvalidators 名字和姓氏欄位。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > 使用資料註解的優點之一是您的應用程式頁面中沒有重複，驗證邏輯。 您可以定義一次在模型中，並使用操作資料的所有應用程式頁面。
4. 開啟**CustomerDetails.aspx**程式碼後置，並找出 SaveCustomer 方法。 插入新客戶時，會呼叫這個方法，並接收客戶參數從 FormView 控制項的值。 頁面控制項與參數物件就會發生之間的對應，ASP.NET 會在執行時對資料註釋的模型驗證屬性，並填入此 ModelState 字典錯誤發生，如果有的話。

    ModelState.IsValid 只會傳回 true，如果執行驗證之後，您的模型上的所有欄位都都有效。

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. 新增**ValidationSummary** CustomerDetails 頁面以顯示模型錯誤的清單結尾處的控制項。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** ValidationSummary 的新屬性控制，設定為 **，則為 true**，控制項才會顯示從 ModelState 字典的錯誤。 這些錯誤是來自於資料註解驗證。
6. 按下**F5**執行 Web 應用程式。 完成表單與某些錯誤的值，然後按一下**儲存**執行驗證。 請注意底部的摘要時發生錯誤。

    ![使用資料註解驗證](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "使用資料註解驗證")

    *使用資料註解驗證*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>工作 3-ModelState 處理自訂的資料庫錯誤

在舊版的 Web Form 中，處理資料庫錯誤，例如太長的字串或唯一索引鍵違規可能牽涉到在您的存放庫程式碼中擲回例外狀況，並在您的程式碼後置來顯示錯誤的例外狀況，接著處理。 大量的程式碼，才能執行一些相當簡單。

在 Web Form 4.5 中，ModelState 物件可用來以一致的方式顯示在頁面上，從您的模型，或是從資料庫中的錯誤。

在這個工作中，您將加入程式碼，才能正確處理資料庫例外狀況，並使用 ModelState 物件對使用者顯示適當的訊息。

1. 雖然應用程式仍在執行中，嘗試更新使用重複的值分類的名稱。

    ![使用重複的名稱更新分類](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "更新具有重複名稱的類別")

    *更新具有重複名稱的類別*

    請注意，因為擲回例外狀況&quot;唯一&quot;條件約束**CategoryName**資料行。

    ![重複的類別目錄名稱的例外狀況](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "為重複的類別名稱的例外狀況")

    *重複的類別目錄名稱的例外狀況*
2. 停止偵錯。 在  **Products.aspx.cs**程式碼後置檔案、 update **UpdateCategory**方法來處理資料庫擲回例外狀況。Savechanges （） 方法呼叫，並加入錯誤**ModelState**物件。

    新**TryUpdateModel**方法會更新使用使用者所提供的表單資料，從資料庫擷取的類別目錄物件。

    (程式碼片段- *Web Form 實驗室-Ex02-UpdateCategory 處理錯誤*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > 在理想情況下，您必須找出 DbUpdateException 的原因，並檢查根本原因是否是唯一的索引鍵條件約束的違規。
3. 開啟**Products.aspx**並加入**Aggregate**類別 GridView，以顯示的模型錯誤清單 下的控制項。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. 執行網站並移至 [產品] 頁面。 嘗試更新使用重複的值分類的名稱。

    請注意，已處理的例外狀況，並會出現錯誤訊息**ValidationSummary**控制項。

    ![複製類別目錄錯誤](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "複製類別目錄錯誤")

    *重複的類別目錄錯誤*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>工作 4-要求在 ASP.NET Web Form 4.5 中的驗證

在 ASP.NET 要求驗證功能提供特定層級的預設保護來抵禦跨網站指令碼 (XSS) 攻擊。 在舊版 ASP.NET 中，依預設會啟用要求驗證，並只會停用整個頁面。 使用新版本的 ASP.NET Web Form 現在可以停用要求驗證的單一控制項、 執行延遲的要求驗證或存取 （務必小心，如果您這麼做 ！） 的未驗證的要求資料。

1. 按下**Ctrl + F5**啟動網站，但不偵錯，並移至 [產品] 頁面。 選取類別目錄，然後按一下**編輯**連結上的任何產品。
2. 輸入包含有潛在危險的內容，例如包含 HTML 標記的描述。 需要請注意，因為要求驗證擲回的例外狀況。

    ![編輯具有潛在危險的內容之產品](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "編輯產品有潛在危險的內容")

    *編輯的產品有潛在危險的內容*

    ![因為要求的驗證所以擲回的例外狀況](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "因為要求的驗證所以擲回的例外狀況")

    *因為要求的驗證所以擲回的例外狀況*
3. 關閉網頁，並在 Visual Studio 中按**SHIFT + F5**停止偵錯。
4. 開啟**ProductDetails.aspx**頁面上，並找出**描述**文字方塊中。
5. 加入新**ValidateRequestMode**文字方塊中的屬性並將其值設定為**停用**。

    新**ValidateRequestMode**屬性可讓您停用要求驗證，細微的方式在每個控制項上。 當您想要使用的輸入，可能會收到 HTML 程式碼，但想要保留的頁面的其餘部分使用的驗證，這非常有用。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. 按下**F5**執行 web 應用程式。 再次開啟編輯產品頁面上，並完成產品描述，包括 HTML 標記。 請注意，您現在可以將 HTML 內容加入描述。

    ![要求的產品描述停用的驗證](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "要求驗證停用的產品描述")

    *產品說明停用要求驗證*

    > [!NOTE]
    > 在生產環境應用程式中，您應該處理，請確定輸入唯一安全的 HTML 標記的使用者輸入的 HTML 程式碼 (例如，有沒有&lt;指令碼&gt;標記)。 若要這樣做，您可以使用[Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS)。
7. 再次編輯的產品。 在 [名稱] 欄位中輸入 HTML 程式碼，然後按一下**儲存**。 請注意，[描述] 欄位只停用要求驗證和重新欄位的其餘部分仍會驗證有潛在危險的內容。

    ![要求在其餘的欄位中啟用的驗證](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "要求在其餘的欄位中啟用驗證")

    *在其餘的欄位中啟用要求驗證*

    ASP.NET Web Forms 4.5 包含新的要求驗證模式來延遲執行要求驗證。 要求的驗證模式設定為**4.5**，如果一段程式碼存取*Request.Form [&quot;金鑰&quot;]*，ASP.NET 4.5 要求驗證會只有觸發要求驗證該表單集合中的特定元素。

    此外，ASP.NET 4.5 現在會包含從 Microsoft ANTI-XSS 程式庫 v4.0 的核心編碼常式。 ANTI-XSS 編碼常式的實作方式的新*AntiXssEncoder*找到在新的型別**System.Web.Security.AntiXss**命名空間。 具有**encoderType**參數設定為使用*AntiXssEncoder*，所有輸出的編碼方式在 ASP.NET 中自動使用新的編碼常式。
8. ASP.NET 4.5 的要求驗證也支援未驗證的要求資料的存取。 ASP.NET 4.5 新集合將屬性加入至**HttpRequest**呼叫的物件**Unvalidated**。 當您瀏覽到**HttpRequest.Unvalidated**您可以存取所有的要求資料，包括表單、 QueryStrings、 Cookie、 Url 和等等的常見項目。

    ![Request.Unvalidated 物件](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated 物件")

    *Request.Unvalidated 物件*

    > [!NOTE]
    > **請謹慎使用 HttpRequest.Unvalidated 屬性 ！** 請確定您仔細對自訂驗證原始要求資料，以確保危險的文字不往返並呈現遲疑不前的客戶 ！

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>練習 3： 非同步頁面處理在 ASP.NET Web Form

在此練習中，您將介紹新的非同步頁面處理 ASP.NET Web Form 中的功能。

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>工作 1-更新產品詳細資料頁面，即可上傳和顯示的映像

在這個工作中，您將會更新以允許使用者指定產品的影像 URL，並顯示在 [唯讀] 檢視中的產品詳細資料頁面。 您將建立指定的映像的本機複本，下載並以同步方式。 在下一個工作中，您將會更新這項實作，讓它以非同步方式運作。

1. 開啟**Visual Studio 2012**並載入**開始**解決方案位於**Source\Ex3 Async\Begin**從這個實驗室的資料夾。 或者，您可以繼續使用您現有的方案，從之前的練習。

   1. 如果您開啟提供**開始**解決方案中，您必須下載某些缺少的 NuGet 封裝才能繼續。 若要這樣做，請在 [方案總管] 中，按一下**WebFormsLab**專案，然後選取**管理 NuGet 套件**。
   2. 在 [**管理 NuGet 套件**] 對話方塊中，按一下**還原**才能下載遺漏的套件。
   3. 最後，按一下 建置方案**建置** | **建置方案**。

      > [!NOTE]
      > 使用 NuGet 的優點之一是，您不需要寄送您的專案中的所有程式庫專案縮小。 NuGet Power tools，藉由指定封裝版本在 Packages.config 檔案中，您將能夠下載所有必要的程式庫第一次執行專案。 這就是為什麼您必須在您開啟現有的方案從這個實驗室之後，執行這些步驟。
2. 開啟**ProductDetails.aspx**頁面來源，然後將欄位加入 FormView 的 itemtemplate，以顯示產品影像。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. 加入欄位，以在 FormView 的 EditTemplate 指定影像 URL。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. 開啟**ProductDetails.aspx.cs**程式碼後置檔案，並新增下列命名空間指示詞。

    (程式碼片段- *Web Form 實驗室-Ex03-命名空間*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. 建立**UpdateProductImage**方法，以將遠端的映像儲存在本機**映像**資料夾，然後更新為新的映像的位置值的 [產品] 實體。

    (程式碼片段- *Web Form 實驗室-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. 更新**UpdateProduct**方法來呼叫**UpdateProductImage**方法。

    (程式碼片段- *Web Form 實驗室-Ex03-UpdateProductImage 呼叫*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. 執行應用程式，然後再次嘗試上傳一項產品的影像。 例如，您可以使用下列映像 URL 從 Office 剪輯藝術： [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![設定產品的映像](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "設定產品的映像")

    *設定產品的映像*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>工作 2-新增非同步處理產品詳細資料頁面

在這個工作中，您將會更新，讓它以非同步方式運作的產品詳細資料頁面。 您將會增強長時間執行的工作-映像下載程序-使用 ASP.NET 4.5 的非同步頁面處理。

Web 應用程式中的非同步方法可用來最佳化 ASP.NET 執行緒集區使用的方式。 在那里的 ASP.NET 中會有限的數目的執行緒上的執行緒集區中要求，因此時所有執行緒都都忙碌中，, ASP.NET，開始拒絕新的要求、 傳送應用程式錯誤訊息並將您的網站無法使用。

耗時的作業，在您的網站上的非同步程式設計，因為它們在長時間佔用指派的執行緒是絕佳候選項目。 這包括長時間執行的要求，有許多不同的項目頁面，以及需要離線作業，這類查詢資料庫或存取外部 web 伺服器的頁面。 優點是，如果頁面處理時，您可以使用這些作業的非同步方法，執行緒會釋出，並回到執行緒集區，而且可用來處理新的頁面要求。 這表示，網頁會開始處理執行緒集區的一個執行緒中，並可能在非同步處理完成之後完成中的另一處理。

1. 開啟**ProductDetails.aspx**頁面。 新增**非同步**屬性中**頁**項目並將它設定為**true**。 此屬性會告知 ASP.NET 實作 IHttpAsyncHandler 介面。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. 新增標籤以顯示頁面上執行的執行緒的詳細資料頁面的底部。

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. 開啟**ProductDetails.aspx.cs**並新增下列命名空間指示詞。

    (程式碼片段- *Web Form 實驗室-Ex03-命名空間 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. 修改**UpdateProductImage**下載映像與非同步工作的方法。 您將會取代**WebClient** **DownloadFile**方法**DownloadFileTaskAsync**方法，並包含**await**關鍵字。

    (程式碼片段- *Web Form 實驗室-Ex03-UpdateProductImage 非同步*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask 註冊要在不同的執行緒中執行的新頁面非同步工作。 它會接收要執行的工作 (t) 的 lambda 運算式。 **Await**中的關鍵字**DownloadFileTaskAsync**方法會將方法的其餘部分轉換後以非同步方式叫用回呼**DownloadFileTaskAsync**方法完成。 ASP.NET 會繼續執行方法，藉由自動維護所有 HTTP 要求原始值。 在.NET 4.5 中新的非同步程式設計模型可讓您撰寫非同步程式碼看起來非常類似同步程式碼，並讓編譯器處理複雜的回呼函式或接續程式碼。
    > [!NOTE]
    > RegisterAsyncTask PageAsyncTask 都已可使用和自.NET 2.0。 從.NET 4.5 的非同步程式設計模型的新 await 關鍵字，並可搭配新 TaskAsync 方法從.NET WebClient 物件。
5. 加入程式碼顯示程式碼開始和完成執行所在的執行緒。 若要這樣做，更新**UpdateProductImage**為下列程式碼的方法。

    (程式碼片段- *Web Form 實驗室-Ex03-Show 執行緒*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. 開啟網站的**Web.config**檔案。 加入下列的 appSetting 變數。

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. 按下**F5**來執行應用程式，並上傳映像的產品。 請注意，開始和完成的程式碼可能不同的執行緒識別碼。 這是因為個別的執行緒上執行 ASP.NET 執行緒集區中的非同步工作。 工作完成時，ASP.NET 會將工作放回佇列，並指派任何可用的執行緒。

    ![以非同步方式下載映像](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "非同步下載映像")

    *以非同步方式下載映像*

> [!NOTE]
> 此外，您可以在其中部署此應用程式至 Azure 的下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。


* * *

<a id="Summary"></a>
## <a name="summary"></a>總結

在這個實際操作實驗室中，有已解決和示範下列概念：

- 使用強型別資料繫結運算式
- 在 Web Form 中使用模型繫結的新功能
- 將頁面資料對應到程式碼後置方法時，用於值提供者
- 使用資料註解來驗證使用者輸入
- 在 Web Form 中採取 unobstrusive 用戶端驗證，與 jQuery 的 advange
- 實作詳細的要求驗證
- 實作非同步頁面處理 Web Form 中

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>附錄 a： 安裝 Visual Studio Express 2012 for Web

您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;使用版本**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. 下列指示會引導您完成安裝所需的步驟*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。

1. 移至[ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169)。 或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 含 Azure SDK</em>&quot;。
2. 按一下 **立即安裝**。 如果您不需要**Web Platform Installer**您將會重新導向至下載並安裝第一次。
3. 一次**Web Platform Installer**已開啟，按一下**安裝**，啟動安裝程式。

    ![安裝 Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "安裝 Visual Studio Express")

    *安裝 Visual Studio Express*
4. 閱讀所有產品的授權和詞彙，然後按一下**我接受**以繼續。

    ![接受授權條款](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *接受授權條款*
5. 等候完成的下載與安裝程序。

    ![安裝進度](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *安裝進度*
6. 安裝完成時，按一下**完成**。

    ![安裝已完成](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *安裝已完成*
7. 按一下 **結束**關閉 Web Platform Installer。
8. 若要開啟 Visual Studio Express for Web，請前往**開始**畫面，即可開始撰寫&quot; **VS Express**&quot;，然後按一下**VS Express for Web**圖格。

    ![VS Express for Web 圖格](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express for Web 圖格*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy

本附錄將說明如何從 Azure 入口網站中建立新的網站和發行您取得所遵循的實驗室中，利用 Web Deploy 發行所提供的功能由 Azure 應用程式。

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>工作 1： 從 Azure 入口網站中建立新的網站

1. 移至[Azure 管理入口網站](https://manage.windowsazure.com/)並使用您的訂用帳戶相關聯的 Microsoft 認證登入。

    > [!NOTE]
    > 使用 Azure，您可以免費託管 10 個 ASP.NET 網站，並再隨著流量成長而調整。 您可以註冊申請[此處](http://aka.ms/aspnet-hol-azure)。

    ![登入 Windows Azure 入口網站](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "登入 Windows Azure 入口網站")

    *登入入口網站*
2. 按一下 **新增**命令列上。

    ![建立新的 Web 站台](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "建立新的網站")

    *建立新的網站*
3. 按一下 **計算** | **網站**。 然後選取**快速建立**選項。 新的網站提供可用的 URL，然後按**建立網站**。

    > [!NOTE]
    > Azure 是您可以控制和管理雲端中執行的 web 應用程式的主應用程式。 [快速建立] 選項可讓您從 azure 入口網站外部的已完成的 web 應用程式部署。 它不包含設定資料庫的步驟。

    ![建立新的網站上，使用 快速建立](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "建立新的網站上，使用 快速建立")

    *建立新的網站上，使用 快速建立*
4. 等到新**網站**建立。
5. 建立網站後按一下底下的連結**URL**資料行。 檢查新的網站運作。

    ![瀏覽至新的 web 站台](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "瀏覽至新的網站")

    *瀏覽至新的網站*

    ![執行的 web 站台](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "執行的網站")

    *執行的網站*
6. 返回入口網站並按一下底下的網站名稱**名稱**資料行來顯示 [管理] 頁面。

    ![開啟 網站管理頁面](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "開啟的網站管理頁面")

    *開啟 網站管理頁面*
7. 在 **儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。

    > [!NOTE]
    > *發行設定檔*包含所有 web 應用程式發行至 Azure 的每個已啟用的發行方法所需的資訊。 發行設定檔包含 Url、 使用者認證和連接到並對每個發行集的方法已啟用的端點進行驗證所需的資料庫字串。 **Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**並**Microsoft Visual Studio 2012**支援讀取發行設定檔，以自動化的這些程式的組態正在發行至 Azure web 應用程式。

    ![正在下載網站發行設定檔](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "下載網站發行設定檔")

    *正在下載網站發行設定檔*
8. 下載發行設定檔至已知位置。 進一步在這個練習中，您會看到如何使用此檔案來發佈 Azure web 應用程式從 Visual Studio。

    ![儲存發行設定檔](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "儲存發佈設定檔")

    *儲存發行設定檔*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>工作 2-設定資料庫伺服器

如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。 如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。

1. 您將需要 SQL Database 伺服器來儲存應用程式資料庫。 您可以從您的訂用帳戶，在 Azure 管理入口網站中檢視 SQL Database 伺服器**Sql Database** | **伺服器** | **伺服器的儀表板**. 如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。 請記下的**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，如同您會在下一個工作中使用它們。 並未建立資料庫，因為它會建立在稍後的階段。

    ![SQL Database 伺服器儀表板](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "SQL Database 伺服器儀表板")

    *SQL Database 伺服器儀表板*
2. 下一個工作在您將測試資料庫連接，從 Visual Studio 中，因此您需要在伺服器的清單包含您的本機 IP 位址**允許的 IP 位址**。 若要這樣做，請按一下**設定**，選取的 IP 位址**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**並**結束 IP 位址**文字方塊中，然後按一下 [ ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) ] 按鈕。

    ![新增用戶端 IP 位址](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *新增用戶端 IP 位址*
3. 一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。

    ![確認變更](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *確認變更*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy

1. 返回至 ASP.NET MVC 4 的方案。 在 **方案總管**，以滑鼠右鍵按一下網站專案，然後選取**發佈**。

    ![發行應用程式](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "發行應用程式")

    *發行網站*
2. 匯入發行設定檔儲存在第一項工作。

    ![匯入發行設定檔](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "匯入發行設定檔")

    *匯入發行設定檔*
3. 按一下 **驗證連線**。 驗證完成後按一下**下一步**。

    > [!NOTE]
    > 一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。

    ![驗證連接](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "驗證連線")

    *驗證連接*
4. 在 **設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。

    ![Web 部署組態](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Web 部署設定")

    *Web 部署設定*
5. 設定資料庫連線，如下所示：

   - 在 **伺服器名稱**輸入您使用 SQL Database 伺服器的 URL *tcp:* 前置詞。
   - 在 **使用者名**輸入您的伺服器系統管理員身分登入名稱。
   - 在 **密碼**輸入您的伺服器系統管理員身分登入密碼。
   - 輸入新的資料庫名稱。

     ![設定目的地連接字串](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "設定目的地連接字串")

     *設定目的地連接字串*
6. 然後按一下 [確定]。  當系統提示您建立資料庫時，按一下**是**。

    ![建立資料庫](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "建立的資料庫字串")

    *建立資料庫*
7. 您將用來連接到 SQL Database Azure 的連接字串會顯示在文字方塊中的預設連線。 然後按 [下一步] 。

    ![連接字串指向 SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "連接字串指向 SQL Database")

    *連接字串指向 SQL Database*
8. 在  **Preview**頁面上，按一下**發佈**。

    ![Web 應用程式發行](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "發行 web 應用程式")

    *發行 web 應用程式*
9. 當發行程序完成時，預設瀏覽器會開啟已發行的網站。

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>附錄 c： 使用程式碼片段

使用程式碼片段，您會有您需要在隨手可得的所有程式碼。 實驗室課程文件會告訴您完全時您可以使用它們，如下圖所示。

![若要將程式碼插入您的專案使用 Visual Studio 程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "程式碼插入您的專案使用 Visual Studio 程式碼片段")

*若要將程式碼插入您的專案使用 Visual Studio 程式碼片段*

***若要新增的程式碼片段，使用鍵盤 （僅限 C#)***

1. 將游標放在您想要插入的程式碼。
2. 開始輸入程式碼片段名稱 （不含空格或連字號）。
3. 觀看為 IntelliSense 會顯示相符的程式碼片段的名稱。
4. 選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。
5. 按下 Tab 鍵兩次的游標位置插入程式碼片段。

![開始鍵入程式碼片段名稱](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "開始輸入程式碼片段名稱")

*開始鍵入程式碼片段名稱*

![按 Tab 鍵以選取反白顯示的程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "按 Tab 鍵以選取反白顯示的程式碼片段")

*按 Tab 鍵以選取反白顯示的程式碼片段*

![再次按 Tab 鍵和程式碼片段會依序展開](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "再次按 Tab 鍵和程式碼片段會展開")

*再次按 Tab 鍵和程式碼片段會展開*

***若要新增的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。 以滑鼠右鍵按一下您要插入程式碼片段。

1. 選取 **插入程式碼片段**後面**My Code Snippets**。
2. 選擇相關的程式片段，從清單中，對它按一下。

![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")

*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*

![對它按一下挑選清單中，相關程式碼片段](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "對它按一下挑選清單中，相關程式碼片段")

*對它按一下挑選清單中，相關程式碼片段*
