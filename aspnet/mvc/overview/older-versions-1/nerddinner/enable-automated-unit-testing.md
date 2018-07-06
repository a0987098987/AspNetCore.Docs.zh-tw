---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: 啟用自動化單元測試 |Microsoft Docs
author: microsoft
description: 步驟 12 示範如何開發自動化的單元測試，以便驗證我們 NerdDinner 的功能，並可將讓我們能夠放心進行變更的套件...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 2247dc2e6d22cc0d5ddba97dfe6c7d2d1b0e49be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819403"
---
<a name="enable-automated-unit-testing"></a>啟用自動化的單元測試
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 12 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 12 示範如何開發自動化的單元測試，以便驗證我們 NerdDinner 的功能，並可將讓我們能夠放心進行變更和改進未來的應用程式套件。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner 步驟 12： 單元測試

讓我們來開發自動化的單元測試，以便驗證我們 NerdDinner 的功能，並可將讓我們能夠放心進行變更和改進未來的應用程式套件。

### <a name="why-unit-test"></a>為什麼要進行單元測試？

工作某天早上的磁碟機上，您必須依據您進行應用程式相關的靈感突然 flash。 您了解變更您可以實作可讓應用程式大幅更好。 它可能可以進行重整，清除程式碼，加入新的功能，或修正 bug。

當您抵達您的電腦 confronts 您的問題是 – 「 安全性程度 」，藉此改進功能？ 如果您進行變更有副作用或中斷的項目嗎？ 變更可能很簡單，而且只需要幾分鐘的時間來實作，但如果需要手動測試的應用程式案例的所有時數？ 如果您忘記涵蓋的案例，並中斷的應用程式會進入生產環境嗎？ 進行這項改善，真的值得下所有投入時間？

自動化的單元測試可以提供防護機制，可讓您持續增強您的應用程式，並可避免您正在使用的程式碼的風險。 執行具有自動化測試來快速驗證功能可讓您放心 – 程式碼，並讓您能夠讓您可能會否則不會覺得舒適的改善。 此外，它們也會協助建立更容易維護的解決方案，並具有較長的存留期-投資報酬以更高的潛在客戶。

ASP.NET MVC 架構能夠以簡單且自然的單元測試應用程式功能。 它也可讓測試導向開發 (TDD) 工作流程，可讓 「 測試先行 」 開發。

### <a name="nerddinnertests-project"></a>NerdDinner.Tests 專案

當我們建立我們的 NerdDinner 應用程式在本教學課程開頭時，我們已收到提示對話方塊，詢問是否要建立單元測試專案來搭配應用程式專案：

![](enable-automated-unit-testing/_static/image1.png)

我們保留 「 是的建立單元測試專案 」 選項按鈕已選取-，導致 「 NerdDinner.Tests"專案新增至我們的解決方案：

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests 專案參考 NerdDinner 應用程式專案組件，且讓我們能夠輕鬆地加入自動化的測試，確認應用程式的功能。

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>建立單元測試 Dinner 模型類別

讓我們加入一些測試來確認 Dinner 類別，我們建立我們的模型層時，我們建立我們 NerdDinner.Tests 專案。

我們先建立新資料夾我們稱為 「 模型 」 的測試專案中我們將在其中放置我們模型相關的測試。 我們將在資料夾上按一下滑鼠右鍵，然後選擇**Add-&gt;新的測試**功能表命令。 這會顯示 [加入新測試] 對話方塊。

我們將選擇要建立 「 單元測試 」 並將它命名為"DinnerTest.cs 」:

![](enable-automated-unit-testing/_static/image3.png)

當我們按一下 [確定] 按鈕時 Visual Studio 會加入 （會開啟） DinnerTest.cs 檔案加入專案：

![](enable-automated-unit-testing/_static/image4.png)

預設 Visual Studio 單元測試範本有一大堆樣板程式碼在其中找到有點繁瑣。 讓我們來清除它只包含下列程式碼：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

在上述 DinnerTest 類別上的 [TestClass] 屬性將其識別為測試，以及選擇性的測試初始化和清除程式碼將會包含的類別。 我們可以藉由新增在其有 [TestMethod] 屬性的公用方法，定義其內的測試。

以下是兩個測試，我們將新增，以執行我們的 Dinner 類別中的第一個。 第一項測試會驗證我們吃晚餐不正確的是否在建立新的 Dinner 時未正確設定的所有屬性。 第二項測試會驗證我們吃晚餐吃晚餐具有有效的值設定的所有屬性時才會有效：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

您會注意到上面，我們的測試名稱都是非常明確 （和比較的詳細資訊）。 因為我們可能會得到建立數百或數千個小型的測試，而且我們想要讓您輕鬆快速判斷每一個函數的行為與意圖 （特別是當我們會仔細檢查失敗的測試執行器中的清單），我們會這麼做。 測試名稱應該測試的功能之後命名。 以上所述，我們會使用 「 名詞\_應該\_動詞"命名模式。

我們會建構使用測試模式 – 代表 「 排列、 Act、 判斷提示 」"AAA"的測試：

- 排列： 設定單元外待測試
- Act： 執行單元測試，並擷取結果
- 判斷提示： 驗證的行為

當我們撰寫我們想要避免個別測試的測試執行太多作業。 改為每個測試應該確認只有一個單一的概念 （這會讓您更容易找出失敗的原因）。 理想的指導方針，就是嘗試只能有單一 assert 陳述式，針對每個測試。 如果您有多個測試方法中的陳述式的判斷提示，請確定它們是所有要用來測試相同的概念。 如有疑問，請另一項測試。

### <a name="running-tests"></a>正在執行測試

Visual Studio 2008 Professional （及更新版本） 包含可用來執行 Visual Studio 單元測試專案，在 IDE 中的內建的測試執行器。 我們可以選取**Test-&gt;執行位&gt;方案中的所有測試**功能表命令 （或類型 Ctrl R、 A） 以執行所有的單元測試。 或者我們可以在 特定的測試類別或測試方法中我們游標的位置，並使用**Test-&gt;執行位&gt;目前內容中的測試**功能表命令 （或型別 Ctrl R、 T） 來執行單元測試的子集。

讓我們我們 DinnerTest 類別內游標的位置，並輸入"Ctrl R、 T"來執行這兩個我們剛才定義的測試。 當我們執行此動作的 [測試結果] 視窗會出現在 Visual Studio 中，我們會看到我們的測試回合中所列的結果：

![](enable-automated-unit-testing/_static/image5.png)

*注意： VS 測試結果 視窗不會預設顯示的類別名稱資料行。您可以將它加入以滑鼠右鍵按一下 測試結果 視窗內使用 新增/移除欄位 功能表命令。*

我們的兩個測試時間只有一小部分的第二個執行 –，並為您可以兩者都傳遞，請參閱。 我們現在可以在 上移，並建立其他的測試，確認特定的規則驗證，以及涵蓋兩個 helper 方法-IsUserHost() 和 IsUserRegisterd() – 我們新增至 Dinner 類別，藉此強化它們。 備妥，Dinner 類別的所有這些測試可讓您更容易、 更安全地在未來將新的商務規則和驗證加入至它。 我們可以加入我們新的規則邏輯至 Dinner，，，然後在數秒內確認 它尚未中斷任何我們先前的邏輯功能。

請注意如何使用描述性的測試名稱容易快速了解每項測試會驗證。 我建議使用**工具-&gt;選項**功能表命令，開啟 測試工具-&gt;測試執行組態 畫面中，並檢查 「 按兩下失敗或結果不明的單元測試結果顯示在測試中的失敗點 」 核取方塊。 這可讓您按兩下測試結果 視窗中的失敗，並立即跳到判斷提示失敗。

### <a name="creating-dinnerscontroller-unit-tests"></a>建立 DinnersController 單元測試

我們現在來建立單元測試，確認我們 DinnersController 的功能。 我們將會啟動我們的測試專案中的 「 控制項 」 資料夾上按一下滑鼠右鍵，然後選擇  **Add-&gt;新的測試**功能表命令。 我們將建立 「 單元測試 」 並將它命名為"DinnersControllerTest.cs 」。

我們將建立確認 DinnersController Details() 動作方法的兩個測試方法。 第一個會確認現有的 Dinner 要求時，會傳回檢視。 第二個會確認不存在 Dinner 要求時，會傳回"NotFound"檢視：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

上述程式碼會清除編譯。 當我們執行測試時，不過，它們都失敗：

![](enable-automated-unit-testing/_static/image6.png)

如果我們查看錯誤訊息時，我們會看到測試失敗的原因是因為我們 DinnersRepository 類別無法連接到資料庫。 我們的 NerdDinner 應用程式連接字串使用本機的 SQL Server Express 檔案下 \App,\_NerdDinner 應用程式專案的資料目錄。 因為我們 NerdDinner.Tests 專案會編譯並執行在不同的目錄然後應用程式專案，我們連接字串的相對路徑位置不正確。

我們*無法*修正此問題，將 SQL Express 資料庫檔案複製到我們的測試專案，然後加入適當的測試連接字串，在我們的測試專案的 App.config 中。 這會取得上述解除鎖定，而且執行測試。

單元測試程式碼使用實際的資料庫，不過，帶來一些挑戰。 尤其是：

- 大幅降低的單元測試執行時間。 時間越長，就能執行測試時，您會經常執行它們比較不可能。 在理想情況下，您會想要能夠執行的秒數 – 並讓它成為您這麼做當然做為編譯專案單元測試。
- 它在測試設定和清除邏輯變得非常複雜。 您想要做到隔離或獨立於其他 （沒有副作用或相依性） 的每個單元測試。 使用時，針對實際資料庫，您必須留意的狀態，並測試之間進行重設。

讓我們看看設計模式，稱為 「 相依性插入 」 可協助我們解決這些問題，並不需要與我們的測試中使用實際的資料庫。

### <a name="dependency-injection"></a>相依性插入

現在 DinnersController 緊密 「 結合 」 DinnerRepository 類別。 「 結合 」 指的是其中一個類別明確依賴另一個類別才能運作的情況：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

DinnerRepository 類別需要資料庫的存取權，因為緊密繫結的相依性就會有 DinnersController 類別具有 DinnerRepository 端點需要我們要測試的 DinnersController 動作方法的順序有一個資料庫中。

我們可以採用的設計模式，稱為 「 相依性插入 」 – 這是一種方法 （例如提供資料存取的儲存機制類別） 的相依性會不再隱含建立的內使用它們的類別，取得解決這個問題。 相反地，相依性可以明確地傳遞至使用它們的類別使用建構函式的引數。 如果使用介面定義的相依性，然後我們將在 「 假 」 的相依性實作中進行單元測試案例的彈性。 這可讓我們建立測試特定相依性的實作，實際上不需要資料庫的存取權。

若要查看此動作，請讓我們來實作我們 DinnersController 使用相依性插入。

#### <a name="extracting-an-idinnerrepository-interface"></a>擷取 IDinnerRepository 介面

我們的第一個步驟是建立新封裝存放庫合約我們控制站需要以擷取和更新 Dinners IDinnerRepository 介面。

我們可以手動定義此介面合約上 \Models 資料夾中，按一下滑鼠右鍵，然後選擇**Add-&gt;新的項目**功能表命令，並建立名為 IDinnerRepository.cs 新介面。

或者我們可以使用重構工具內建於 Visual Studio Professional （及更新版本） 會自動擷取，並為我們建立介面，從現有 DinnerRepository 類別。 若要擷取使用與此介面，只是將游標放在文字編輯器中 DinnerRepository 類別，然後以滑鼠右鍵按一下並選擇**重構-&gt;擷取介面**功能表命令：

![](enable-automated-unit-testing/_static/image7.png)

這會啟動 「 擷取介面 」 對話方塊，並提示我們輸入要建立的介面名稱。 它會預設為 IDinnerRepository，並自動選取 現有 DinnerRepository 来加入的類別介面上的所有公用方法：

![](enable-automated-unit-testing/_static/image8.png)

當我們按一下 [確定] 按鈕時，Visual Studio 會加入我們的應用程式的新 IDinnerRepository 介面：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

使其實作介面，將會更新現有 DinnerRepository 類別：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>更新以支援建構函式插入 DinnersController

我們現在將更新 DinnersController 類別，以使用新的介面。

目前 DinnersController 是硬式編碼，其 「 dinnerRepository"欄位一律為 DinnerRepository 類別：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

使 「 dinnerRepository"欄位的型別而不是 DinnerRepository IDinnerRepository，我們會將它變更。 然後，我們將新增兩個公用 DinnersController 建構函式。 其中一個建構函式可讓 IDinnerRepository 傳遞做為引數。 另一個則是使用我們現有的 DinnerRepository 實作預設建構函式：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

ASP.NET MVC 預設會建立使用預設建構函式的控制器類別，因為我們 DinnersController 在執行階段將繼續使用 DinnerRepository 類別來執行資料存取。

我們現在可以更新我們單元測試，不過，在使用參數的建構函式的 「 假 」 dinner 存放庫實作中傳遞。 這個 「 假 」 dinner 存放庫不需要存取實際的資料庫，並改為將使用記憶體中的範例資料。

#### <a name="creating-the-fakedinnerrepository-class"></a>建立 FakeDinnerRepository 類別

讓我們建立 FakeDinnerRepository 類別。

我們將開始先建立我們 NerdDinner.Tests 專案內的"Fakes"目錄，然後加入新的 FakeDinnerRepository 類別，(在資料夾上按一下滑鼠右鍵，然後選擇  **Add-&gt;新的類別**):

![](enable-automated-unit-testing/_static/image9.png)

我們將更新程式碼，以便 FakeDinnerRepository 類別會實作 IDinnerRepository 介面。 然後，我們可以在其上按一下滑鼠右鍵，並選擇 實作介面 IDinnerRepository 」 內容功能表命令：

![](enable-automated-unit-testing/_static/image10.png)

這會導致 Visual Studio 自動將所有 IDinnerRepository 介面成員新增至預設 「 虛設常式 」 實作 FakeDinnerRepository 類別：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

然後，我們可以更新 FakeDinnerRepository 實作，才能從記憶體中清單&lt;Dinner&gt;集合做為建構函式引數傳遞給它：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

我們現在有假的 IDinnerRepository 實作不需要使用資料庫，並改為可關閉 Dinner 物件的記憶體中清單。

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>FakeDinnerRepository 使用單元測試

讓我們回到先前失敗，因為資料庫無法使用 DinnersController 單元測試。 我們可以更新使用填入範例記憶體 Dinner 資料，以使用下列程式碼 DinnersController FakeDinnerRepository 測試方法：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

然後，現在當我們執行這些測試都通過：

![](enable-automated-unit-testing/_static/image11.png)

最棒的是，它們會採取只有一小部分來執行，第二個，而且不需要任何複雜的安裝/清除邏輯。 我們現在可以將單元測試所有我們 DinnersController 動作方法的程式碼 （包括清單中，分頁的詳細資訊，建立、 更新和刪除） 不需要連線到實際的資料庫。

| **側邊的主題內容： 相依性插入架構** |
| --- |
| 執行手動的相依性插入 （例如我們都以上） 可正常運作，但會成為難以維護的相依性，並增加應用程式中的元件。 適用於.NET 可協助提供更多的相依性管理彈性，存在數個相依性插入架構。 這些架構，有時也稱為 「 反向控制 」 (IoC) 容器，提供適當機制，讓其他組態的支援層級指定，並將相依性傳遞至物件在執行階段 （通常使用建構函式插入). 一些較受歡迎的 OSS 相依性插入在.NET 中的 IOC 架構包含： AutoFac、 Ninject、 Spring.NET、 StructureMap 和 Windsor。 ASP.NET MVC 會公開擴充性 Api 可讓開發人員參與的解析度和具現化的控制站，並讓相依性插入 / IoC 架構，此處理序內完全整合。 使用 DI/IOC 架構還能讓我們移除我們 DinnersController – 這會完全移除其與 DinnerRepositorys 結合的預設建構函式。 我們將不會使用新的相依性插入 / IOC NerdDinner 應用程式架構。 但如果 NerdDinner 的程式碼基底和功能的成長，我們可以認為未來的某些項目。 |

### <a name="creating-edit-action-unit-tests"></a>建立編輯動作的單元測試

我們現在來建立單元測試驗證 DinnersController 的編輯功能。 首先我們要測試我們的編輯動作的 HTTP GET 版本：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

我們將建立測試來驗證有效 dinner 要求時，轉譯 DinnerFormViewModel 物件所支援的檢視：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

當我們執行測試時，不過，我們就會發現，它會失敗，因為編輯方法來存取執行 Dinner.IsHostedBy() 檢查 User.Identity.Name 屬性時，會擲回 null 參考例外狀況。

控制器的基底類別上的使用者物件封裝登入的使用者相關的詳細資訊，並在執行階段建立控制器時，會將 ASP.NET mvc 填入。 因為我們要測試 web 伺服器環境之外 DinnersController，未設定的使用者物件 （因此 null 參考例外狀況）。

### <a name="mocking-the-useridentityname-property"></a>模擬 (mock) User.Identity.Name 屬性

（mock） 架構，讓測試更容易讓我們可以動態建立支援我們的測試中的相依性物件的假的版本。 比方說，我們可以使用在我們的編輯動作測試的模擬架構，以動態方式建立使用者物件，我們 DinnersController 可用來查閱模擬的使用者名稱。 這可避免當我們執行我們的測試時擲回 null 參考。

有許多模擬 (mock) 架構，可以搭配 ASP.NET MVC 的.NET (您可以看到這些設定的清單如下： [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/))。 為了測試我們的 NerdDinner 應用程式，我們將使用模擬 (mock) 架構，稱為 「 Moq"的開放原始碼，這可以免費從下載[ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq)。

下載完成後，我們會參考專案中加入我們 NerdDinner.Tests Moq.dll 組件：

![](enable-automated-unit-testing/_static/image12.png)

然後我們會將 「 CreateDinnersControllerAs(username)"helper 方法加入我們的測試類別當作參數，以及其中的使用者名稱，然後 「 模擬 」 DinnersController 執行個體上的 User.Identity.Name 屬性：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

以上所述，我們會建立 fakes 的 ControllerContext 物件 （這是 ASP.NET MVC 會傳遞至控制器類別，來公開執行階段物件，例如使用者、 要求、 回應和工作階段） 的 Mock 物件使用 Moq。 我們會呼叫將 Mock 切換成指定的 ControllerContext HttpContext.User.Identity.Name 屬性應該會傳回我們傳遞至 helper 方法的使用者名稱字串"SetupGet 」 方法。

我們可以模擬任意數目的 ControllerContext 屬性和方法。 為了說明這點我在裡面加入 SetupGet() 呼叫 Request.IsAuthenticated 屬性 （這實際上不需要針對下列測試 – 但幫助說明您可以模擬要求屬性的方式）。 當我們完成時我們會指派給我們的 helper 方法會傳回 DinnersController 的 ControllerContext mock 的執行個體。

我們現在可以撰寫單元測試來測試編輯案例牽涉到不同的使用者使用這個 helper 方法：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

然後，現在當我們執行測試時所傳遞：

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>測試 UpdateModel() 案例

我們建立了測試，涵蓋編輯動作的 HTTP GET 版本。 我們現在來建立一些測試來驗證編輯動作的 HTTP POST 版本：

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

讓我們支援與此動作方法的有趣新測試案例是 UpdateModel() helper 方法，控制站的基底類別上的使用方式。 表單張貼值繫結至我們的 Dinner 物件執行個體，我們會使用這個 helper 方法。

以下是兩項測試會示範如何，我們可以提供表單發佈 UpdateModel() 協助程式方法，若要使用的值。 我們將建立並擴展 FormCollection 物件，執行這項操作，並再將它指派給 「 ValueProvider"屬性，控制站上。

第一項測試會驗證，於成功儲存瀏覽器重新導向至詳細資料的動作。 第二項測試會驗證，張貼無效的輸入時的動作會重新顯示一次編輯檢視並出現錯誤訊息。


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>測試總結

我們所討論的單元測試的控制器類別中所涉及的核心概念。 我們可以使用這些技術人員輕鬆地建立數百個簡單的測試來驗證我們的應用程式的行為。

因為我們的控制器和測試模型不需要實際的資料庫，也就是極度快速且輕鬆地執行。 我們可以執行數百個自動化測試，以秒為單位，並立即取得有關我們所做的變更是否中斷內容的意見反應。 這將協助提供我們持續改善，重構，並改進我們的應用程式的信心。

我們涵蓋測試與最後一個主題，在這一章 – 但不是因為測試是您應該在開發程序結束 ！ 相反地，您應該在開發程序中盡早撰寫自動化的測試。 這麼做因此可讓您取得立即意見反應開發時，可協助您填寫完成思考您的應用程式使用案例，並引導您設計您的應用程式清除分層，並記住。

活頁簿中的稍後章節會討論測試導向開發 (TDD)，以及如何使用它來搭配 ASP.NET MVC。 TDD 是反覆執行的程式碼撰寫慣例，先撰寫的測試，都能符合您產生的程式碼。 使用 TDD 中，您會開始每項功能所建立的測試，確認您要實作的功能。 撰寫單元測試前可協助確保您清楚了解此功能，以及應該如何運作。 只寫入測試 （並確認它失敗之後） 執行您，然後實作實際功能測試會驗證。 因為您已花時間來思考運作的功能有何的使用案例，您會有進一步了解需求與如何加以實作。 當您完成的實作，您可以重新執行測試工作，並取得立即意見反應，而時是否功能正常運作。 我們將討論更多在第 10 章 TDD。

### <a name="next-step"></a>下一個步驟

註解一些最終總結。

> [!div class="step-by-step"]
> [上一頁](use-ajax-to-implement-mapping-scenarios.md)
> [下一頁](nerddinner-wrap-up.md)
