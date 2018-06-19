---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: 啟用自動化單元測試 |Microsoft 文件
author: microsoft
description: 步驟 12 示範如何開發自動化的單元測試可確認我們 NerdDinner 功能，並讓我們進行變更的信心套件...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: fede08be7e06327c6d04fa5d36f7dd818d79b380
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875626"
---
<a name="enable-automated-unit-testing"></a>啟用自動化的單元測試
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 12 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 12 示範如何開發可確認我們 NerdDinner 功能，並讓我們進行的變更與改進未來的應用程式的信心自動化的單元測試套件。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner 步驟 12： 單元測試

讓我們來開發自動化的單元測試可確認我們 NerdDinner 功能，並讓我們進行的變更與改進未來的應用程式的信心套件。

### <a name="why-unit-test"></a>單元測試為何？

一個早上到磁碟機上，您必須依據您進行應用程式的相關的靈感突然 flash。 您了解變更您可以實作，可讓應用程式大幅更好。 可能是重構的清除程式碼中，加入了新的功能，或修正 bug。

Confronts 您，當您抵達電腦的問題是 – 「 安全方式是，使這項改進？ 」 如果進行變更的副作用或是中斷的項目嗎？ 變更可能會很簡單，而且只需要幾分鐘才能實作，但是如果需要手動測試的所有應用程式案例的時數？ 如果您忘記涵蓋的案例並中斷應用程式進入生產嗎？ 正在進行這項改進真的值得所有投入時間？

自動化的單元測試可以提供一種安全措施，可讓您持續增強您的應用程式，並避免在您處理的程式碼的。 執行具有自動化測試，以快速驗證功能可讓您有信心地-程式碼，並讓您可讓您可能會否則不具有認為舒適的改善。 它們也可以協助建立更容易維護的方案，並以較長的存留期的投資報酬率的將導致更高。

ASP.NET MVC 架構可讓您輕鬆自然的單元測試應用程式功能。 它也可讓測試驅動開發 (TDD) 工作流程，可讓 「 測試先行 」 開發。

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

當我們在本教學課程的開頭建立我們 NerdDinner 應用程式時，我們已收到提示對話方塊，詢問是否我們想要建立單元測試專案中，移以及應用程式專案：

![](enable-automated-unit-testing/_static/image1.png)

我們保留 「 [是]，建立單元測試專案 」 選取的選項按鈕-而產生了 「 NerdDinner.Tests 」 專案加入我們的解決方案：

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests 專案參考 NerdDinner 應用程式專案的組件，並可讓我們以輕鬆地加入自動化的測試，確認應用程式的功能。

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>建立單元測試，我們 Dinner 模型類別

讓我們加入一些測試以確認我們建置我們模型層時，我們建立的 Dinner 類別我們 NerdDinner.Tests 專案。

我們會先建立新資料夾內我們測試專案，稱為 「 模型 」 會放置在我們與模型相關的測試。 我們將在資料夾上按一下滑鼠右鍵，然後選擇**新增-&gt;新測試**功能表命令。 這會顯示 「 加入新測試 」 對話方塊。

我們將選擇要建立 「 單元測試 」 並將它命名為"DinnerTest.cs 」:

![](enable-automated-unit-testing/_static/image3.png)

當我們按一下 「 確定 」 按鈕 Visual Studio 會加入 （會開啟） DinnerTest.cs 檔案加入專案中：

![](enable-automated-unit-testing/_static/image4.png)

預設 Visual Studio 單元測試範本有一大堆鍋爐調色盤中的程式碼它找到有點混亂。 讓我們來清除它只包含下列程式碼：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

在上述的 DinnerTest 類別 [由 TestClass] 屬性識別為類別將包含測試，以及選擇性的測試初始化和清除程式碼。 我們可以透過將在其有 [TestMethod] 屬性的公用方法加入定義內的測試。

以下是兩個測試我們會將新增 Dinner 類別中的第一個。 第一項測試會驗證我們 Dinner 不正確的是否沒有正確設定的所有內容建立新的 Dinner。 第二個測試確認我們 Dinner 有效 Dinner 擁有的所有屬性都具有有效的值設定時：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

您會發現上方，我們的測試名稱都是非常明確 （和比較的詳細資訊）。 因為我們最後可能會建立數百或數千個小型測試中，而且我們想要讓您輕鬆快速判斷每一個函數的行為與意圖 （特別是我們會透過一份測試執行器中的失敗） 時，我們會這樣做。 測試名稱應為名他們所測試的功能。 我們將使用以上所述"名詞\_應該\_指令動詞"的命名模式。

我們會建構使用"AAA"測試模式 – 代表 「 排列、 動作、 判斷提示 」 的測試：

- 排列： 安裝程式的單元測試
- Act： 執行的單元測試和擷取結果
- 判斷提示： 驗證的行為

當我們撰寫我們想要避免讓個別測試的測試執行太多。 改為每個測試應該確認只有一個單一的概念 （如此可讓您更容易找出失敗的原因）。 最好是嘗試只能有單一 assert 陳述式，針對每個測試。 如果您有一個以上的判斷提示陳述式的測試方法中，確定它們所有要用來測試概念相同。 當不確定，請在另一項測試。

### <a name="running-tests"></a>正在執行測試

Visual Studio 2008 Professional （和更高的版本） 包含可以用來執行 Visual Studio 單元測試專案在 IDE 中的內建的測試執行器。 我們可以從中選取**Test-&gt;執行-&gt;方案中的所有測試**功能表命令 （或類型為 Ctrl R、 A） 執行所有的單元測試。 或者我們可以在特定的測試類別或測試方法中我們游標的位置，並使用**Test-&gt;執行-&gt;目前內容中的測試**功能表命令 （或型別 Ctrl R、 T） 來執行單元測試的子集。

讓我們我們 DinnerTest 類別內游標的位置，然後輸入 「 Ctrl R、 T 「 我們剛才定義的兩個執行測試。 當我們執行這個動作 「 測試結果 」 視窗會出現在 Visual Studio 中，我們會看到我們的測試回合中所列的結果：

![](enable-automated-unit-testing/_static/image5.png)

*注意： VS 測試結果視窗未顯示類別名稱資料行的預設值。您可以將它加入以滑鼠右鍵按一下 [測試結果] 視窗中使用 [新增/移除欄位] 功能表命令。*

我們兩項測試所需的第二個執行 –，並為您可以輕鬆擁有兩者傳遞，請參閱。 我們現在可以繼續，並加強它們藉由建立額外的測試，確認特定的規則驗證，以及涵蓋兩個 helper 方法-IsUserHost() 和 IsUserRegisterd() – 我們新增至 Dinner 類別。 就地 Dinner 類別具有所有這些測試會讓更容易，未來對其加入新的商務規則和驗證更安全。 我們可以將我們新的規則邏輯加入至 Dinner，，，然後在數秒內檢查 它尚未中斷任何我們先前的邏輯功能。

請注意如何使用描述性的測試名稱讓您輕鬆地快速了解每項測試會驗證。 建議您使用**Tools-&gt;選項**功能表命令，開啟的測試工具-&gt;測試執行組態 畫面中，並檢查 「 按兩下失敗或結果不明的單元測試結果顯示測試失敗的點 」 核取方塊。 這可讓您在失敗的測試結果視窗中按兩下，然後立即跳到判斷提示失敗。

### <a name="creating-dinnerscontroller-unit-tests"></a>建立 DinnersController 單元測試

我們現在來建立驗證我們 DinnersController 功能有單元測試。 我們將會啟動 「 控制項 」 資料夾，在我們的測試專案上按一下滑鼠右鍵，然後選擇 **新增-&gt;新測試**功能表命令。 我們會建立 「 單元測試 」 並將它命名為"DinnersControllerTest.cs"。

我們將建立確認 DinnersController Details() 動作方法的兩個測試方法。 第一個會確認現有的 Dinner 要求時傳回的檢視。 第二個會確認不存在 Dinner 要求時，會傳回"NotFound"檢視：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

上述程式碼會編譯初始狀態。 當我們執行測試時，不過，它們都失敗：

![](enable-automated-unit-testing/_static/image6.png)

如果我們查看錯誤訊息時，我們會看到測試失敗的原因，是因為我們 DinnersRepository 類別無法連接到資料庫。 我們 NerdDinner 應用程式使用連接字串的本機 SQL Server Express 檔案存留在 \App\_NerdDinner 應用程式專案的資料目錄。 因為我們 NerdDinner.Tests 專案編譯並執行於不同資料夾然後應用程式專案，我們連接字串的相對路徑位置不正確。

我們*無法*修正此問題，SQL Express 資料庫檔案複製到測試專案中，並將適當的測試連接字串到它在我們的測試專案的 App.config 中。 這會取得解除鎖定，而且執行上述測試。

單元測試程式碼使用實際的資料庫，不過，會帶來一些挑戰。 尤其是：

- 它會明顯變慢的單元測試執行時間。 越長它會執行測試時，您會經常執行它們比較不可能。 在理想情況下，您會想要能夠執行的秒數 –，並讓它是您這麼做為自然編譯專案為您的單元測試。
- 它的設定和清除的邏輯中測試變得非常複雜。 您想要隔離的 （具有沒有副作用或相依性） 獨立於其他每個單元測試。 針對實際資料庫使用時您必須留意的狀態重設測試之間。

讓我們看看稱為 「 相依性插入 」 可協助我們解決這些問題，並不需要使用我們的測試與實際資料庫設計模式。

### <a name="dependency-injection"></a>相依性插入

現在 DinnersController 緊密 」 結合 「 DinnerRepository 類別。 「 連接 」 指的是其中一個類別明確依賴另一個類別才能運作的情況下：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

因為 DinnerRepository 類別需要資料庫的存取權，緊密繫結的相依性 DinnersController 類別對我們要測試的 DinnersController 動作方法的順序有資料庫需要 DinnerRepository 結束。

我們可以採用稱為 「 相依性插入 」 – 這是一個方法相依性 （例如提供資料存取的儲存機制類別） 不再隱含建立的位置使用它們的類別內的設計模式，取得解決這個問題。 相反地，相依性可以明確地傳遞給使用它們的類別使用建構函式的引數。 如果使用介面定義的相依性，然後我們更有彈性地在單元測試案例的 「 詐騙 」 的相依性實作中傳遞。 這可讓我們來建立測試特定相依性的實作，實際上不需要資料庫的存取權。

若要查看此動作，讓我們實作使用我們 DinnersController 相依性插入。

#### <a name="extracting-an-idinnerrepository-interface"></a>擷取 IDinnerRepository 介面

我們第一個步驟是建立新封裝儲存機制合約我們控制站需要以擷取和更新 Dinners IDinnerRepository 介面。

我們可以手動定義此介面合約 \Models 資料夾上按一下滑鼠右鍵，然後選擇**新增-&gt;新項目**功能表命令，並建立名為 IDinnerRepository.cs 新介面。

或者我們可以使用重構作業工具內建 Visual Studio Professional （及更高的版本） 會自動擷取，並為我們建立介面，從現有 DinnerRepository 類別。 若要擷取使用 VS 這個介面，只要將游標放在文字編輯器中 DinnerRepository 類別，然後以滑鼠右鍵按一下並選擇**重構-&gt;擷取介面**功能表命令：

![](enable-automated-unit-testing/_static/image7.png)

這將會啟動 「 擷取介面 」 對話方塊，並提示我們要建立的介面名稱。 它會預設為 IDinnerRepository 並自動選取要加入至介面的現有 DinnerRepository 類別上的所有公用方法：

![](enable-automated-unit-testing/_static/image8.png)

當我們按一下 「 確定 」 按鈕時，則 Visual Studio 會將新 IDinnerRepository 介面加入我們的應用程式：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

然後，以便實作介面，就會更新現有 DinnerRepository 類別：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>更新以支援建構函式插入 DinnersController

我們現在會將更新 DinnersController 類別，以使用新的介面。

目前 DinnersController 是硬式編碼，例如其"dinnerRepository 」 欄位一律會 DinnerRepository 類別：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

我們將會使"dinnerRepository 」 欄位的類型而不是 DinnerRepository IDinnerRepository 變更它。 然後，我們會新增兩個公用 DinnersController 建構函式。 其中一個建構函式可讓 IDinnerRepository 傳遞做為引數。 另一個方法是使用我們現有的 DinnerRepository 實作預設建構函式：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

因為預設的 ASP.NET MVC 建立控制器類別使用預設建構函式，所以我們 DinnersController 在執行階段將繼續使用 DinnerRepository 類別來執行資料存取。

我們現在可以更新我們單元測試，不過，在使用參數的建構函式的 「 假"dinner 儲存機制實作中傳遞。 這個 「 假"dinner 儲存機制不需要存取實際的資料庫，並會改為使用記憶體中的範例資料。

#### <a name="creating-the-fakedinnerrepository-class"></a>建立 FakeDinnerRepository 類別

讓我們來建立 FakeDinnerRepository 類別。

我們將開始建立我們 NerdDinner.Tests 專案內的"Fakes"目錄，然後為其新增新的 FakeDinnerRepository 類別 (在資料夾上按一下滑鼠右鍵，然後選擇 **新增-&gt;新類別**):

![](enable-automated-unit-testing/_static/image9.png)

我們將更新程式碼，以便讓 FakeDinnerRepository 類別實作 IDinnerRepository 介面。 我們可以在其上按一下滑鼠右鍵，然後選擇 「 實作介面 IDinnerRepository 」 的操作功能表命令：

![](enable-automated-unit-testing/_static/image10.png)

這會導致 Visual Studio 會自動將所有 IDinnerRepository 介面成員新增至預設 「 虛設常式 」 實作我們 FakeDinnerRepository 類別：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

然後，我們可以更新 FakeDinnerRepository 實作，才能從記憶體中清單&lt;Dinner&gt;集合做為建構函式引數傳遞給它：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

我們現在有假的 IDinnerRepository 實作，不需要使用資料庫，並可以改為使用關閉的 Dinner 物件之記憶體中清單。

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>使用單元測試的 FakeDinnerRepository

讓我們回到稍早失敗，因為資料庫無法使用 DinnersController 單元測試。 我們可以更新使用填入範例記憶體 Dinner 資料以使用下列程式碼 DinnersController FakeDinnerRepository 的測試方法：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

現在當我們執行這些測試都成功：

![](enable-automated-unit-testing/_static/image11.png)

最棒的他們採取只有執行，第二個部分，而且不需要任何複雜的安裝/清除邏輯。 我們現在即可以單元測試所有我們 DinnersController 動作方法的程式碼 （包括清單中，分頁的詳細資訊，建立、 更新及刪除） 不需要連接到實際的資料庫。

| **側邊主題內容： 相依性插入的架構** |
| --- |
| 執行手動的相依性插入 （例如我們都以上） 可正常運作，但沒有變得難以維護的相依性數量為和應用程式中的元件會增加。 可協助提供更多的相依性管理彈性的.NET 存在數個相依性插入的架構。 這些架構，有時也稱為 「 的逆轉控制 」 (IoC) 容器提供的一層額外的組態支援指定並傳遞要在執行階段 （最常使用的建構函式插入的物件相依性). 某些更常用的 OSS 相依性插入 / IOC 架構在.NET 中的包括： AutoFac、 Ninject、 Spring.NET、 StructureMap 和 Windsor。 ASP.NET MVC 會公開擴充性 Api 可讓開發人員參與解析度和具現化的控制站，並讓相依性插入 / IoC 架構以完全整合，此處理序內。 使用 DI/IOC 架構也會讓我們能夠移除我們 DinnersController – 會完全移除它和 DinnerRepositorys 之間的連接中的預設建構函式。 我們將不會使用相依性插入 / IOC 架構與我們 NerdDinner 應用程式。 但是，如果成長 NerdDinner 程式碼基底和功能，我們可以考慮針對未來的某些項目。 |

### <a name="creating-edit-action-unit-tests"></a>建立編輯動作的單元測試

我們現在來建立驗證 DinnersController 的編輯功能有單元測試。 首先我們要測試 HTTP-GET 版本我們編輯動作：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

我們將建立測試，以確認有效 dinner 要求時，會轉譯 DinnerFormViewModel 物件所支援的檢視：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

當我們執行測試時，不過，我們就會發現，它會失敗，因為編輯方法來存取執行 Dinner.IsHostedBy() 檢查 User.Identity.Name 屬性時，會擲回 null 參考例外狀況。

控制器的基底類別上的使用者物件封裝之登入之使用者的相關詳細資訊，ASP.NET MVC 控制器建立在執行階段時填入。 因為我們要測試 web 伺服器環境之外 DinnersController，未設定的使用者物件 （因此 null 參考例外狀況）。

### <a name="mocking-the-useridentityname-property"></a>模擬 User.Identity.Name 屬性

模擬架構會讓測試更為容易讓我們以動態方式建立假支援我們的測試中的相依性物件的版本。 例如，我們可以在我們的編輯動作測試使用模擬架構，以動態方式建立我們 DinnersController 可以用來查閱模擬的使用者名稱的使用者物件。 這樣可避免從我們執行我們的測試時，所擲回的 null 參考。

有許多.NET 模擬可以搭配 ASP.NET MVC 的架構 (您可以看到這些設定的清單這裡： [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/))。 為了測試，我們會使用模擬架構，稱為 「 Moq"的開放原始碼我們 NerdDinner 應用程式，它可以免費下載從[ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq)。

在下載後，我們會參考專案中加入我們 NerdDinner.Tests Moq.dll 組件：

![](enable-automated-unit-testing/_static/image12.png)

然後我們會將"CreateDinnersControllerAs(username)"helper 方法加入我們的測試類別會做為參數，以及其使用者名稱，然後 「 mocks"DinnersController 執行個體上的 User.Identity.Name 屬性：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

上方我們使用 Moq 建立 fakes 的 ControllerContext 物件 （這是 ASP.NET MVC 會傳遞至控制器類別公開的執行階段物件，例如使用者、 要求、 回應和工作階段） 模擬物件。 我們正在撥打"SetupGet"方法上表示的 ControllerContext HttpContext.User.Identity.Name 屬性應該傳回我們傳遞到 helper 方法的使用者名稱字串模擬。

我們可以模擬任何數目的 ControllerContext 屬性和方法。 為了說明這點我也新增了 SetupGet() 呼叫 Request.IsAuthenticated 屬性 （這不實際需要下列 – 測試，但前者可協助說明您可以模擬要求屬性的方式）。 當我們完成我們會指派給我們的 helper 方法會傳回 DinnersController 的 ControllerContext 模擬的執行個體。

現在我們可以撰寫單元測試，使用此 helper 方法來測試編輯案例牽涉到不同的使用者：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

現在我們執行測試時傳遞：

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>測試 UpdateModel() 案例

我們建立了涵蓋編輯動作的 HTTP GET 版本的測試。 我們現在來建立一些測試，確認編輯動作的 HTTP POST 版本：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

我們支援與此動作方法的有趣新測試案例是 UpdateModel() helper 方法，控制站的基底類別上的使用方式。 使用這個 helper 方法來將表單張貼值繫結至我們 Dinner 物件執行個體。

以下是示範如何才能提供表單張貼 UpdateModel() helper 方法，若要使用的值的兩個測試。 我們會建立並擴展 FormCollection 物件，執行這項操作，然後將它指派給 「 ValueProvider"屬性，控制站上。

第一項測試會驗證，在成功儲存瀏覽器會重新導向至詳細資料的動作。 第二個測試會驗證當張貼無效的輸入時的動作又再次編輯檢視並出現錯誤訊息。


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>測試 Wrap-Up

我們已涵蓋參與單元測試控制器類別的的核心概念。 我們可以使用這些技術輕鬆地建立數百個簡單的測試，確認我們的應用程式的行為。

我們的控制器和測試模型不需要實際的資料庫，因為它們是非常快速且容易執行。 我們將能夠以秒為單位，執行數百個自動化測試，並立即取得回應我們所做的變更是否中斷的項目。 這將協助提供我們以持續改善，重構及改善我們的應用程式的信心。

我們涵蓋測試做為最後一個主題在本章中 – 但不是因為測試是您應該執行結尾的開發程序 ！ 相反地，您應該在開發程序中盡早撰寫自動化的測試。 這樣做因此可讓您取得的立即回應您開發，可協助您周全思考應用程式的使用案例，並引導您設計您的應用程式分層和耦合記住初始狀態。

活頁簿中稍後的章節將討論測試驅動開發 (TDD)，以及如何使用 ASP.NET MVC。 TDD 反覆編碼作法是先撰寫測試產生的程式碼將滿足。 使用 TDD 中，您可以開始每項功能藉由建立測試，以確認您要實作的功能。 撰寫單元測試第一次可協助確保您清楚地了解此功能，且它是應該要如何運作。 只會寫入測試 （並確認它失敗之後） 執行您，然後實作測試驗證實際的功能。 因為您已經會花費時間思考該如何運作功能的使用案例，您會有更深入了解需求以及如何妥善地加以實作。 當您完成的實作，您可以重新執行測試 – 並取得與立即回應是否功能正常運作。 我們將討論 TDD 章 10 中的多。

### <a name="next-step"></a>下一個步驟

註解設定某些最終換行。

> [!div class="step-by-step"]
> [上一頁](use-ajax-to-implement-mapping-scenarios.md)
> [下一頁](nerddinner-wrap-up.md)
