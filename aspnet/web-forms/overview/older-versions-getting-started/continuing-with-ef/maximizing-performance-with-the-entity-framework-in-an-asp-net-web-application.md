---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: "有了 Entity Framework 4.0 ASP.NET 4 Web 應用程式的效能最大化 |Microsoft 文件"
author: tdykstra
description: "此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 我..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 9e257f5061f6bdf14ad0776ff6385fb526d6dcb1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>有了 Entity Framework 4.0 ASP.NET 4 Web 應用程式而言，發揮最佳效能
====================
由[Tom Dykstra](https://github.com/tdykstra)

> 此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程中，您可了解如何處理並行存取衝突。 本教學課程示範改善使用 Entity Framework 的 ASP.NET web 應用程式的效能選項。 您將學習數種方法，將效能最大化或診斷效能問題。

下列各節中所顯示的資訊很可能是各種案例中很有用：

- 有效率地載入相關的資料。
- 管理檢視狀態。

下列各節中所顯示的資訊可能是適用於該出現效能問題有個別的查詢：

- 使用`NoTracking`合併選項。
- 先行編譯的 LINQ 查詢。
- 檢查傳送至資料庫的查詢命令。

在下一節中所呈現的資訊會有極大的資料模型的應用程式很有用：

- 預先產生檢視。

> [!NOTE]
> Web 應用程式的效能會受到許多因素，包括下列作業要求和回應資料大小、 資料庫查詢，伺服器可以排入佇列的要求數目和速度它提供的服務，以及甚至任何效率的速度您也可以使用用戶端指令碼程式庫。 如果效能嚴重不足應用程式中，或測試或經驗會顯示應用程式效能不滿意，您應該遵循標準通訊協定進行效能微調。 量值來判斷發生效能瓶頸的位置，然後解決將對整體應用程式效能有重大影響的區域。
> 
> 本主題主要著重在其中您可以提升的效能，特別是 Entity Framework 的 ASP.NET 中的方式。 此處的建議會很有用，如果您判斷資料存取是一個應用程式中的效能瓶頸。 除了這裡所說明的方法如所述，不應視為&quot;最佳做法&quot;一般情況下，其中有多少適當只在例外的情況，或者位址非常特定種類的效能瓶頸。


若要開始本教學課程，請啟動 Visual Studio 並開啟您在上一個教學課程使用 Contoso 大學 web 應用程式。

## <a name="efficiently-loading-related-data"></a>有效率地載入相關的資料

有數種方式，Entity Framework 可以將實體的導覽屬性載入相關的資料：

- *消極式載入*。 第一次讀取實體時，不被擷取相關的資料。 不過，第一次您嘗試存取的導覽屬性，該導覽屬性所需的資料自動擷取。 這會導致多個查詢傳送至資料庫，一個用於實體本身，一個必須擷取每個相關實體資料的時間。 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*積極式載入*。 實體讀取時，同時擷取的相關的資料。 這通常會導致單一聯結查詢以擷取所有所需的資料。 使用指定積極式載入`Include`方法，為您上文所述在這些教學課程。

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *明確式載入*。 這是類似於延遲載入，不同之處在於您明確地擷取相關的資料中的程式碼;當您存取導覽屬性時，它不會發生自動。 您使用以手動方式的相關的資料載入`Load`導覽屬性的集合，或您的方法使用`Load`保存單一物件的屬性參考屬性的方法。 (例如，您呼叫`PersonReference.Load`方法以載入`Person`導覽屬性`Department`實體。)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

因為它們不會立即擷取的屬性值，消極式載入和明確載入也都稱為*延後載入*。

延遲載入的物件內容已經在設計工具所產生的預設行為。 如果您開啟*SchoolModel.Designer.cs*檔案，定義物件內容類別，您會發現三個建構函式方法，並每個包含下列陳述式：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

一般情況下，如果您知道您需要相關的資料針對每個實體，擷取、 積極式載入提供最佳效能，因為傳送至資料庫的單一查詢通常比針對擷取每個實體個別查詢更有效率。 另一方面，如果您需要只不常存取的實體導覽屬性，或是只為一個小型實體、 消極式載入或明確載入的集合可能更有效率，因為積極式載入會擷取比您所需的更多資料。

Web 應用程式中消極式載入的相對較小的值，因為可能在未連線至物件內容呈現網頁瀏覽器中的使用者動作將會影響相關資料的需求進行。 相反地，當您進行資料繫結控制項，您通常知道哪些資料，也因此它通常是最佳選擇積極式載入或延後的載入，根據什麼是適用於每個案例。

此外，資料繫結控制項在處置物件內容之後，可能會使用實體物件。 在此情況下，延遲載入的導覽屬性嘗試會失敗。 您收到的錯誤訊息會很明確：&quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource`控制項預設會停用延遲載入。 如`ObjectDataSource`控制您使用目前的教學課程 （或如果您從網頁程式碼存取的物件內容），有數種方式，您可以進行延遲載入預設停用。 當您具現化物件內容，您可以停止它。 例如，您可以加入下行建構函式方法的`SchoolRepository`類別：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Contoso 大學應用程式，您要進行自動停用延遲載入的所以這個屬性不需要設定每當具現化內容的物件內容。

開啟*SchoolModel.edmx*資料模型，按一下設計介面，然後在 [屬性] 窗格中設定**延遲載入啟用**屬性`False`。 儲存並關閉 資料模型。

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>管理狀態檢視

為了提供更新功能，ASP.NET web 網頁必須轉譯頁面時儲存原始屬性值的實體。 處理控制項的回傳期間才能重新建立實體的原始狀態，以及呼叫的實體`Attach`方法，然後再套用變更，然後呼叫`SaveChanges`方法。 根據預設，ASP.NET Web Form 資料控制項可使用檢視狀態來儲存原始值。 不過，檢視狀態可能會影響效能，，因為它儲存在可以大幅增加，會傳送到以及從瀏覽器的頁面大小的隱藏欄位。

管理檢視狀態或工作階段狀態，這類替代方案的技術不是唯一 Entity Framework 中，讓本教學課程不會進入此主題的詳細資料。 如需詳細資訊請參閱本教學課程結尾處的連結。

不過，第 4 版的 ASP.NET 提供新的方式來使用 ASP.NET Web Form 應用程式的開發人員應該要注意的檢視狀態：`ViewStateMode`屬性。 這個新屬性可以設定在網頁或控制項層級，它可讓您檢視狀態中頁面的預設停用和啟用只針對需要它的控制項。

對於在效能很重要的應用程式，最好是一律停用的頁面層次的檢視狀態，並啟用只針對需要它的控制項。 Contoso 大學頁面中的檢視狀態的大小不會使用這個方法，大幅減少，但若要查看其運作方式，您將會執行它*Instructors.aspx*頁面。 該頁面包含許多控制項，包括`Label`已停用檢視狀態的控制項。 無此頁面上的控制項實際需要已啟用狀態的檢視。 (`DataKeyNames`屬性`GridView`控制項指定必須回傳，之間維護狀態，但這些值存放在控制項狀態，不會受到`ViewStateMode`屬性。)

`Page`指示詞和`Label`控制項標記目前類似下列範例：

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

進行下列變更：

- 新增`ViewStateMode="Disabled"`至`Page`指示詞。
- 移除`ViewStateMode="Disabled"`從`Label`控制項。

標記就會類似下列的範例如下：

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

所有控制項現在已停都用檢視狀態。 如果您稍後將需要使用檢視狀態的控制項時，您只需要為包含`ViewStateMode="Enabled"`該控制項的屬性。

## <a name="using-the-notracking-merge-option"></a>使用 NoTracking 合併選項

當物件內容擷取資料庫資料列，並建立代表的實體物件時，依預設它也會追蹤使用其物件狀態管理員這些實體物件。 這個追蹤資料做為快取，並更新實體時，會使用。 由於 web 應用程式通常具有存留較短的物件內容的執行個體，查詢通常會傳回不需要追蹤，因為一次使用的任何實體，它會讀取前，會處置讀取物件內容的資料，或更新。

在 Entity Framework 中，您可以指定是否物件內容追蹤實體物件的設定*合併選項*。 您可以設定個別的查詢，或是實體集的合併選項。 如果您設定實體集，這表示您正在設定實體集所建立的所有查詢的預設合併選項。

Contoso 大學應用程式的追蹤不需要任何您從儲存機制，因此您可以設定的合併選項存取的實體集`NoTracking`的這些實體集，當您具現化物件內容中的儲存機制類別。 （請注意，在本教學課程中，設定的合併選項不會明顯影響應用程式的效能。 `NoTracking`時可能會讓特定大量的資料的案例中的可觀察的效能改進選項。)

在 DAL 資料夾中，開啟*SchoolRepository.cs*檔案，然後加入設定的合併選項，實體集會儲存機制存取的建構函式方法：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>預先編譯的 LINQ 查詢

Entity Framework 執行 Entity SQL 查詢的生命週期內的第一次指定`ObjectContext`執行個體，請花一些時間來編譯查詢。 編譯的結果會快取，而這表示查詢的後續執行作業更快。 LINQ 查詢會依照類似的模式，不同之處在於每次執行查詢時完成以上某些編譯查詢所需的工作。 換句話說，LINQ 查詢中，依預設不是所有的編譯結果會快取。

如果您希望重複執行物件內容的生命週期中的 LINQ 查詢，您可以撰寫程式碼會導致所有快取執行 LINQ 查詢的第一次編譯的結果。

舉例來說，您將會執行兩個這`Get`方法`SchoolRepository`類別，其中不接受任何參數 (`GetInstructorNames`方法)，而另一個需要參數 (`GetDepartmentsByAdministrator`方法)。 這些方法如下： 它們現在實際上不需要編譯，因為它們不在 LINQ 查詢：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

不過，以便您可以試用已編譯的查詢，您將繼續如同這些已寫入下列 LINQ 查詢：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

您可以變更這些方法中的程式碼功能具有如上所示，並執行應用程式來確認它運作再繼續進行。 但下列指示直接跳到建立它們的先行編譯的版本。

建立類別檔案中的*DAL*資料夾中，其命名*SchoolEntities.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

此程式碼會建立部分類別延伸會自動產生的物件內容類別。 部分類別包含兩個已編譯的 LINQ 查詢使用`Compile`方法`CompiledQuery`類別。 它也會建立可用來呼叫查詢的方法。 儲存並關閉此檔案。

接下來，在*SchoolRepository.cs*，變更現有`GetInstructorNames`和`GetDepartmentsByAdministrator`方法儲存機制中的類別，以便讓這些呼叫的已編譯的查詢：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

執行*Departments.aspx*頁面，以驗證它的運作與以前一樣。 `GetInstructorNames`填入系統管理員的下拉式清單，才能呼叫方法和`GetDepartmentsByAdministrator`呼叫方法時，您按一下**更新**以確認沒有講師是一個以上的系統管理員部門。

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Contoso 大學應用程式中只有不是因為它會適當地提升效能，請參閱 < 如何這麼做，在預先編譯的查詢。 預先編譯的 LINQ 查詢不將層級的複雜性加入至您的程式碼，因此請確定您執行只會針對實際代表您的應用程式中的效能瓶頸的查詢。

## <a name="examining-queries-sent-to-the-database"></a>檢查查詢傳送至資料庫

當您想調查效能問題時，有時候很有幫助知道確切 Entity Framework 傳送至資料庫的 SQL 命令。 如果您正在使用`IQueryable`物件，若要這樣做的一種方式為使用`ToTraceString`方法。

在*SchoolRepository.cs*，在變更程式碼`GetDepartmentsByName`方法，以符合下列範例：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments`變數必須轉換成`ObjectQuery`類型只因為`Where`方法前一行的結尾會建立`IQueryable`物件; 而不`Where`方法，轉型就不一定需要。

上設定中斷點`return`行，然後再執行*Departments.aspx*偵錯工具中的頁面。 當您叫用中斷點時，檢查`commandText`變數中**區域變數**視窗並使用文字視覺化檢視 (中的放大鏡**值**資料行) 以顯示其值在**文字視覺化檢視**視窗。 您可以看到這個程式碼會產生整個 SQL 命令：

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

或者，在 Visual Studio Ultimate 中的 IntelliTrace 功能會提供方法來檢視不需要您變更您的程式碼，或甚至設定中斷點的 Entity Framework 所產生的 SQL 命令。

> [!NOTE]
> 只有當您擁有的 Visual Studio Ultimate，您可以執行下列程序。


還原原始的程式碼中`GetDepartmentsByName`方法，這個方法，然後執行*Departments.aspx*偵錯工具中的頁面。

在 Visual Studio 中，選取**偵錯**功能表，然後**IntelliTrace**，然後**IntelliTrace 事件**。

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

在**IntelliTrace**視窗中，按一下 **全部中斷**。

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace**視窗會顯示一份新的事件：

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

按一下**ADO.NET**列。 它會展開以顯示您的命令文字：

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

您可以將整個命令文字字串複製到剪貼簿 從**區域變數**視窗。

假設您已使用具有多個資料表、 關聯性和資料行，比簡單的資料庫`School`資料庫。 您可能會發現，查詢會收集所有資訊，您需要在單一`Select`包含多個陳述式`Join`子句變得太複雜的工作效率。 在此情況下您可以切換 eager 載入明確載入以簡化查詢。

例如，請嘗試變更中的程式碼`GetDepartmentsByName`方法中的*SchoolRepository.cs*。 目前，方法有物件查詢具有`Include`方法`Person`和`Courses`導覽屬性。 取代`return`執行明確式載入，如下列範例所示的程式碼陳述式：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

執行*Departments.aspx*偵錯工具在頁面上，並檢查**IntelliTrace**先前執行一次為您的視窗。 現在，在先前存在單一查詢之前，您會看到一長串。

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

按一下第一個**ADO.NET**線條，以查看發生了什麼複雜的查詢您稍早檢視。

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

從部門查詢變得簡單`Select`查詢沒有`Join`子句，但後面跟著擷取相關的課程和系統管理員的個別查詢，針對每個部門使用兩個查詢的一組傳回的原始查詢。

> [!NOTE]
> 如果您離開延遲載入啟用，您會發現在這裡，重複許多次，相同的查詢模式可能是因為消極式載入。 您通常想要避免的模式是主資料表的每個資料列的消極式載入相關的資料。 除非您已驗證的單一聯結查詢太過複雜，能夠有效率，您一般會改善效能，在此情況下變更主要查詢來使用積極式載入。


## <a name="pre-generating-views"></a>預先產生檢視表

當`ObjectContext`物件第一次建立新的應用程式定義域中時，Entity Framework 會產生一組類別，用來存取資料庫。 這些類別稱為 「*檢視*，如果您有非常大的資料模型，產生這些檢視可以延遲後網站的回應頁面的第一個要求初始化新的應用程式定義域之後。 您可以減少這個第一個要求的延遲建立檢視，在編譯時期，而不是在執行階段。

> [!NOTE]
> 如果您的應用程式不會有極大的資料模型中，或如果沒有大型資料模型，但是您不必擔心回收 IIS 後，會影響第一個頁面要求的效能問題，您可以略過本節。 每次您具現化，不會建立檢視`ObjectContext`物件，因為檢視會快取應用程式定義域中。 因此，除非您正在經常回收 IIS 應用程式，很少的頁面要求後獲益預先產生檢視。


您可以預先產生檢視使用*EdmGen.exe*命令列工具或使用*文字範本轉換工具組*(T4) 範本。 在本教學課程中，您將使用 T4 範本。

在*DAL*資料夾中，新增檔案使用**文字範本**範本 (下**一般**節點**已安裝的範本**清單)，並將其命名*SchoolModel.Views.tt*。 取代下列程式碼檔案中的現有程式碼：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

此程式碼產生檢視*.edmx*檔案，而且位於與範本相同的資料夾，並為範本檔案具有相同的名稱。 例如，如果您的範本檔案命名為*SchoolModel.Views.tt*，它會尋找名為的資料模型檔案*SchoolModel.edmx*。

儲存檔案，然後以滑鼠右鍵按一下 [檔案中的**方案總管] 中**選取**執行自訂工具**。

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio 會產生的程式碼檔案會建立檢視表中，名為*SchoolModel.Views.cs*範本為基礎。 (您可能已注意到您選取之前，甚至產生的程式碼檔案**執行自訂工具**，如儲存範本檔案。)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

您現在可以執行應用程式，並確認它的運作與以前一樣。

如需有關預先產生檢視的詳細資訊，請參閱下列資源：

- [如何： 預先產生檢視，以改善查詢效能](https://msdn.microsoft.com/en-us/library/bb896240.aspx)MSDN 網站上。 說明如何使用`EdmGen.exe`命令列工具來預先產生檢視。
- [隔離效能與先行編譯/前 generated 檢視 Entity Framework 4 中](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx)Windows Server AppFabric 客戶諮詢團隊部落格上。

如此即完成簡介改善使用 Entity Framework 的 ASP.NET web 應用程式中的效能。 如需詳細資訊，請參閱下列資源：

- [效能考量 (Entity Framework)](https://msdn.microsoft.com/en-us/library/cc853327.aspx) MSDN 網站上。
- [Entity Framework 小組部落格上的效能相關文章](https://blogs.msdn.com/b/adonet/archive/tags/performance/)。
- [EF 的合併選項和已編譯的查詢](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx)。 說明非預期的行為，已編譯的查詢和合併的部落格文章之類的選項`NoTracking`。 如果您打算使用已編譯的查詢，或管理應用程式中的合併選項設定，讀取此第一次。
- [Entity Framework 相關張貼資料和模型化客戶諮詢小組部落格中](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/)。 包含已編譯的查詢和使用 Visual Studio 2010 分析工具找出效能問題的文章。
- [具有提升高複雜的查詢效能的建議的實體架構論壇執行緒](https://social.msdn.microsoft.com/Forums/en-US/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6)。
- [ASP.NET 狀態管理建議](https://msdn.microsoft.com/en-us/library/z1hkazw7.aspx)。
- [使用 Entity Framework 和 ObjectDataSource： 自訂分頁](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx)。 建置 ContosoUniversity 建立的應用程式在這些教學課程說明如何實作中的分頁的部落格文章*Departments.aspx*頁面。

下一個教學課程會檢閱一些至 Entity Framework 版本 4 中新的重要增強功能。

>[!div class="step-by-step"]
[上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
[下一頁](what-s-new-in-the-entity-framework-4.md)
