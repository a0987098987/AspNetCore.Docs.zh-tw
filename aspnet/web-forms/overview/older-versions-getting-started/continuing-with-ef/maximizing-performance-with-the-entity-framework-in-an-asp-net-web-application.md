---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: 將使用 Entity Framework 4.0 ASP.NET 4 Web 應用程式中的效能最大化 |Microsoft Docs
author: tdykstra
description: 本教學課程系列由開始使用 Entity Framework 4.0 的教學課程系列的 Contoso 大學 web 應用程式為基礎。 我...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7d7c66289f09179a98e09532172477d5b06c70bd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823693"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>將使用 Entity Framework 4.0 ASP.NET 4 Web 應用程式中的效能最大化
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> 本教學課程系列是根據所建立的 Contoso 大學 web 應用程式[Getting Started with Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，為本教學課程的起始點即可[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整的教學課程系列。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


在上一個教學課程中，您會看到如何處理並行衝突。 本教學課程會示範提升使用 Entity Framework 的 ASP.NET web 應用程式效能的選項。 您將了解幾種方法，將效能最大化或診斷效能問題。

下列各節所述的資訊很可能在各式各樣的案例很有用：

- 有效率地載入相關的資料。
- 管理檢視狀態。

下列各節所述的資訊可能很有用，如果您有個別的查詢該出現的效能問題：

- 使用`NoTracking`合併選項。
- 先行編譯 LINQ 查詢。
- 檢查傳送至資料庫的查詢命令。

下一節所述的資訊是有極大的資料模型的應用程式很有用：

- 預先產生檢視表。

> [!NOTE]
> Web 應用程式效能會受到許多因素，包括像是要求和回應資料的大小、 資料庫查詢，伺服器可以排入佇列的要求數目和速度它提供的服務，以及甚至任何效率的速度您也可以使用用戶端指令碼程式庫。 如果效能嚴重不足，在您的應用程式，或測試或經驗會顯示應用程式的效能不滿意，您應該遵循標準的通訊協定進行效能微調。 量值來判斷發生效能瓶頸，並找出對整體應用程式效能會有最大的影響的區域。
> 
> 本主題主要著重在其中您可以改善效能特別的 ASP.NET 中的 Entity Framework 的方式。 此處的建議是很有用，如果您判斷資料存取是其中一個應用程式中的效能瓶頸。 除了這裡所說明的方法如所述，不應該被視為&quot;最佳做法&quot;一般情況下，有許多都是適當只有在例外狀況的情況下，或到地址非常特定的效能瓶頸的種類。


若要開始本教學課程，請啟動 Visual Studio，並開啟您已在上一個教學課程中使用的 Contoso 大學 web 應用程式。

## <a name="efficiently-loading-related-data"></a>有效率地載入相關的資料

有數種方式，Entity Framework，可將相關的資料載入實體的導覽屬性：

- *消極式載入*。 第一次讀取實體時，不會擷取相關資料。 不過，第一次嘗試存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。 這會導致多個查詢傳送至資料庫，一個用於實體本身，一個必須擷取每個相關實體資料的時間。 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*積極式載入*。 讀取實體時，將會同時擷取其相關資料。 這通常會導致單一聯結查詢，其可擷取所有需要的資料。 使用指定積極式載入`Include`方法中，您已了解已在這些教學課程。

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *明確式載入*。 這是類似於消極式載入，不同之處在於您明確地擷取相關的資料，在程式碼當您存取導覽屬性時，它不會發生自動。 您將使用以手動方式的相關的資料的載入`Load`導覽屬性的集合，或您的方法使用`Load`保存單一物件屬性的參考屬性的方法。 (例如，您呼叫`PersonReference.Load`方法以載入`Person`導覽屬性`Department`實體。)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

因為它們不立即擷取的屬性值，消極式載入和明確式載入同時所謂*延後載入*。

消極式載入是已經在設計工具所產生的物件內容的預設行為。 如果您開啟*SchoolModel.Designer.cs*檔案，定義物件內容類別，您會發現三個建構函式方法，並每個包含下列陳述式：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

一般情況下，如果您知道您需要相關的資料的每個實體擷取，積極式載入可提供最佳效能，因為傳送至資料庫的單一查詢通常比擷取每個實體的個別查詢更有效率。 另一方面，如果您需要只不常存取實體的導覽屬性，還是只能用於小型組實體、 消極式載入或明確式載入可能更有效率，因為積極式載入會擷取比您所需的更多資料。

Web 應用程式中消極式載入可能相當低的價值，因為在未連線至物件內容呈現頁面的瀏覽器中的使用者動作會影響相關資料的需求進行。 相反地，當您資料繫結控制項，您通常知道什麼資料，您需要並讓它通常是最佳選擇積極式載入或根據延後的載入什麼是適用於每個案例。

此外，資料繫結控制項在處置物件內容之後，可能會使用實體物件。 在此情況下，延遲載入的導覽屬性嘗試會失敗。 您收到的錯誤訊息很明確的： &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource`控制項預設會停用消極式載入。 針對`ObjectDataSource`控制您目前的教學課程中使用 （或如果您從網頁程式碼存取物件的內容），有數種方式，您可以進行消極式載入預設停用。 當您具現化物件內容，您可以關閉它。 比方說，您可以將下面這一行新增至建構函式方法`SchoolRepository`類別：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Contoso 大學應用程式中，您要進行自動停用消極式載入，所以這個屬性不需要設定每當內容會具現化物件內容。

開啟*SchoolModel.edmx*資料模型，按一下 設計介面，然後在 屬性 窗格將**已啟用消極式載入**屬性設`False`。 儲存並關閉資料模型。

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>管理檢視狀態

為了提供更新功能，ASP.NET 網頁必須轉譯頁面時儲存實體的原始屬性值。 處理控制項的回傳期間可以重新建立實體的原始狀態，並呼叫的實體`Attach`方法，然後再套用變更，並呼叫`SaveChanges`方法。 根據預設，ASP.NET Web Form 資料控制項可以用於檢視狀態在儲存原始值。 不過，檢視狀態可能會影響效能，因為它儲存在網頁瀏覽器來回傳送的大小會大幅增加的隱藏欄位。

讓本教學課程不會進入此主題的詳細資料，不是 Entity Framework 中，唯一方法來管理的檢視狀態或工作階段狀態之類的替代方案。 如需詳細資訊請參閱在本教學課程結尾處的連結。

不過，第 4 版的 ASP.NET 提供有關使用 Web Forms 應用程式的每位 ASP.NET 開發人員應該要注意的檢視狀態的新方法：`ViewStateMode`屬性。 這個新屬性可以設定在頁面或控制項的層級，以及它可讓您根據頁面的預設停用檢視狀態，並啟用只針對需要它的控制項。

對於效能的關鍵應用程式，最好是一律停用頁面層級的檢視狀態，並啟用只針對需要它的控制項。 Contoso 大學頁面中的檢視狀態大小不會大幅減少由這個方法，但若要查看其運作方式，您將會替*Instructors.aspx*頁面。 該頁面包含許多控制項，包括`Label`的停用檢視狀態的控制項。 無此頁面上的控制項實際上需要已啟用狀態的檢視。 (`DataKeyNames`的屬性`GridView`控制項指定之間的回傳中，必須維持的狀態，但這些值會保留在控制項狀態，不會受到`ViewStateMode`屬性。)

`Page`指示詞和`Label`控制項標記目前會類似下列範例：

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

進行下列變更：

- 新增`ViewStateMode="Disabled"`至`Page`指示詞。
- 移除`ViewStateMode="Disabled"`從`Label`控制項。

標記現在會類似下列範例：

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

所有控制項現在已停都用檢視狀態。 如果您後來加入需要使用檢視狀態的控制項，您只需要為包含`ViewStateMode="Enabled"`該控制項的屬性。

## <a name="using-the-notracking-merge-option"></a>使用 NoTracking 合併選項

當物件內容會擷取資料庫的資料列，並建立代表他們的實體物件時，預設情況下它也會追蹤這些實體物件，使用它的物件狀態管理員。 這個追蹤資料做為快取，並更新實體時，會使用。 因為 web 應用程式通常會有短暫的物件內容執行個體，查詢通常會傳回不需要追蹤，因為之前的任何實體，它會讀取一次使用，將會處置物件內容，讀取的資料，或更新。

在 Entity Framework 中，您可以指定物件的內容是否會追蹤實體物件，藉由設定*合併選項*。 您可以設定個別查詢或實體集的合併選項。 如果您設定實體集，這表示您正在設定預設的合併選項，會建立該實體集的所有查詢。

Contoso 大學應用程式中，追蹤不需要針對任何您從儲存機制，因此您可以設定的合併選項存取實體集`NoTracking`這些實體集，當您具現化的存放庫類別中的物件內容。 （請注意，在本教學課程中，設定的合併選項不會有明顯的影響，對應用程式的效能。 `NoTracking`選項很有可能只在特定的高資料量案例中讓可觀察的效能改進。)

在 DAL 資料夾中，開啟*SchoolRepository.cs*檔案，然後新增實體可讓您設定存放庫的存取設定的合併選項的建構函式方法：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>預先編譯的 LINQ 查詢

Entity Framework 執行 Entity SQL 查詢的生命週期內的第一次指定`ObjectContext`執行個體，需要一些時間來編譯查詢。 快取的編譯結果，這表示查詢的後續執行會更快。 LINQ 查詢會遵循類似的模式，不同之處在於有些來編譯查詢所需的工作是每次執行查詢。 換句話說，LINQ 查詢中，依預設不是所有的編譯結果會快取。

如果您有您預期要在物件內容的生命週期中重複執行 LINQ 查詢時，您可以撰寫程式碼，會導致所有快取執行 LINQ 查詢的第一次編譯的結果。

舉例來說，您需要這兩個`Get`中的方法`SchoolRepository`類別，其中未採用任何參數 (`GetInstructorNames`方法)，而另一個需要參數 (`GetDepartmentsByAdministrator`方法)。 這些方法如下： 它們現在實際上不需要編譯，因為它們不是 LINQ 查詢：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

不過，以便您可以試試看已編譯查詢，您將繼續如同這些已寫入下列 LINQ 查詢為：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

您可以變更這些方法中的程式碼項目已如上所示，並執行應用程式，以確認它運作再繼續進行。 但是，下列指示直接跳到建立它們的先行編譯的版本。

建立類別檔案中的*DAL*資料夾，其命名*SchoolEntities.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

此程式碼會建立擴充自動產生的物件內容類別的部分類別。 部分類別包含兩個已編譯的 LINQ 查詢使用`Compile`方法的`CompiledQuery`類別。 它也會建立可用來呼叫查詢的方法。 儲存並關閉此檔案。

接下來，在*SchoolRepository.cs*，變更現有`GetInstructorNames`和`GetDepartmentsByAdministrator`存放庫中的方法類別，使其呼叫已編譯的查詢：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

執行*Departments.aspx*頁面，確認其運作方式跟之前一樣。 `GetInstructorNames`會呼叫方法，以填入系統管理員的下拉式清單，而`GetDepartmentsByAdministrator`當您按一下時，會呼叫方法**更新**以確認沒有講師是一個以上的系統管理員部門。

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

您只能以不是因為它會適當地提升效能，請參閱如何這麼做，Contoso 大學應用程式中已預先編譯的查詢。 先行編譯 LINQ 查詢，並將某種程度的複雜性新增至您的程式碼，因此請確定您執行僅供實際代表您的應用程式中的效能瓶頸的查詢。

## <a name="examining-queries-sent-to-the-database"></a>檢查傳送至資料庫的查詢

當您要調查效能問題時，有時最好知道確切 Entity Framework 會傳送至資料庫的 SQL 命令。 如果您正在使用`IQueryable`物件，若要這樣做的一個方式是使用`ToTraceString`方法。

在  *SchoolRepository.cs*，變更中的程式碼`GetDepartmentsByName`方法，以符合下列範例：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments`變數必須轉換成`ObjectQuery`型別，只因為`Where`方法前一行的結尾會建立`IQueryable`物件，而不需要`Where`方法，轉換就不一定需要。

上設定中斷點`return`行，然後再執行*Departments.aspx*偵錯工具中的頁面。 當您遇到中斷點時，檢查`commandText`變數中**區域變數**視窗並使用文字視覺化檢視 (在放大鏡**值**資料行) 以顯示其值在**文字視覺化檢視**視窗。 您可以看到整個 SQL 命令所產生的這段程式碼：

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

或者，在 Visual Studio Ultimate 中的 IntelliTrace 功能會提供檢視不需要您變更您的程式碼，或甚至是設定中斷點的 Entity Framework 所產生的 SQL 命令的方式。

> [!NOTE]
> 只有當您擁有 Visual Studio Ultimate，您可以執行下列程序。


還原原始的程式碼中`GetDepartmentsByName`方法，然後執行*Departments.aspx*偵錯工具中的頁面。

在 Visual Studio 中，選取**偵錯**功能表，然後**IntelliTrace**，然後**IntelliTrace 事件**。

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

在 [ **IntelliTrace** ] 視窗中，按一下**全部中斷**。

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace**視窗會顯示一份最近的事件：

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

按一下  **ADO.NET**列。 它會展開以顯示您的命令文字：

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

您可以將整個命令文字字串複製到從剪貼簿**區域變數**視窗。

假設您已使用具有更多的資料表、 關聯性和資料行比簡單的資料庫`School`資料庫。 您可能會發現，查詢會收集所有資訊，您需要在單一`Select`包含多個陳述式`Join`子句變得太複雜的工作效率。 在此情況下，您可以切換的積極式載入來明確載入以簡化查詢。

例如，請嘗試變更中的程式碼`GetDepartmentsByName`方法中的*SchoolRepository.cs*。 目前，方法有物件查詢已`Include`方法`Person`和`Courses`導覽屬性。 取代`return`陳述式加上程式碼來執行明確式載入，如下列範例所示：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

執行*Departments.aspx*頁面上的偵錯工具並檢查**IntelliTrace**視窗一次為您先前一樣。 現在，在存在單一查詢之前，您會看到一長串。

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

按一下第一個**ADO.NET**折線圖，看看發生了什麼變化複雜的查詢您稍早檢視。

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

部門的查詢已經成為簡單`Select`查詢沒有`Join`子句，但後面擷取相關的課程和系統管理員的個別查詢，則每個部門使用一組兩個查詢傳回的原始查詢。

> [!NOTE]
> 如果您將保留消極式載入已啟用，您在這裡看到具有重複許多次，相同的查詢模式可能是因為消極式載入。 您通常想要避免的模式是主資料表的每個資料列的消極式載入相關的資料。 除非您已驗證的單一聯結查詢太過複雜，能夠有效率，您通常可以改善效能，在此情況下，藉由變更主要查詢來使用積極式載入。


## <a name="pre-generating-views"></a>預先產生檢視

當`ObjectContext`物件第一次建立新的應用程式定義域中時，Entity Framework 會產生一組類別，用來存取資料庫。 這些類別統稱*檢視*，且如果您有非常大的資料模型，產生這些檢視會延遲網站的回應頁面的第一個要求之後初始化新的應用程式定義域。 您可以建立檢視，在編譯時期，而不在執行階段，以減少此第一個要求延遲。

> [!NOTE]
> 如果您的應用程式沒有的極大的資料模型，或如果它確實有大型的資料模型，但您不必擔心會影響第一個網頁要求，IIS 會回收之後的效能問題，您可以略過本節。 每次您具現化，並不會建立檢視`ObjectContext`物件，因為檢視會快取應用程式定義域中。 因此，除非您經常會回收您的應用程式在 IIS 中，很少的頁面要求受益於預先產生檢視。


您可以預先產生檢視使用*EdmGen.exe*命令列工具或使用*文字範本轉換工具組*(T4) 範本。 在本教學課程中，您將使用 T4 範本。

在*DAL*資料夾中，新增檔案，使用**文字範本**範本 (其低於**一般**中的節點**已安裝的範本**清單)，並將它命名*SchoolModel.Views.tt*。 取代為下列程式碼中的檔案中現有的程式碼：

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

此程式碼會產生檢視 *.edmx*檔案，其位於相同的資料夾，做為範本，並為範本檔案具有相同的名稱。 例如，如果您的範本檔案的名稱為*SchoolModel.Views.tt*，它會尋找名為的資料模型檔案*SchoolModel.edmx*。

儲存檔案，然後在檔案按一下滑鼠右鍵**方案總管**，然後選取**執行自訂工具**。

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio 產生的程式碼檔案會建立檢視表，也稱為*SchoolModel.Views.cs*以範本為基礎。 (您可能會產生的程式碼檔案，即使您選取已經發覺**執行自訂工具**，只要儲存範本檔案。)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

您現在可以執行應用程式，並確認其運作方式跟之前一樣。

如需有關預先產生檢視的詳細資訊，請參閱下列資源：

- [如何： 預先產生檢視，以改善查詢效能](https://msdn.microsoft.com/library/bb896240.aspx)MSDN 網站上。 說明如何使用`EdmGen.exe`命令列工具來預先產生檢視表。
- [隔離與預先編譯/預先產生檢視 Entity Framework 4 中的效能](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx)Windows Server AppFabric 客戶諮詢小組部落格上。

如此即完成簡介改善使用 Entity Framework 的 ASP.NET web 應用程式中的效能。 如需詳細資訊，請參閱下列資源：

- [效能考量 (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) MSDN 網站上。
- [Entity Framework 小組部落格上的效能相關文章](https://blogs.msdn.com/b/adonet/archive/tags/performance/)。
- [EF 合併選項和編譯的查詢](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx)。 部落格文章，說明非預期的行為，已編譯的查詢和合併選項，例如`NoTracking`。 如果您想要使用已編譯的查詢，或操作應用程式中的合併選項設定，請閱讀此第一次。
- [Entity Framework 相關文章中的資料和模型化客戶諮詢小組部落格](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/)。 包含已編譯的查詢和使用 Visual Studio 2010 Profiler 找出效能問題的文章。
- [Entity Framework 論壇往來文章，以改善相當複雜的查詢效能的建議](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6)。
- [ASP.NET 狀態管理建議](https://msdn.microsoft.com/library/z1hkazw7.aspx)。
- [使用 Entity Framework 和 ObjectDataSource： 自訂分頁](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx)。 建置 ContosoUniversity 建立的應用程式在這些教學課程說明如何實作中的分頁的部落格文章*Departments.aspx*頁面。

下一個教學課程會檢閱一些 Entity framework 第 4 版中新的重要增強功能。

> [!div class="step-by-step"]
> [上一頁](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [下一頁](what-s-new-in-the-entity-framework-4.md)
