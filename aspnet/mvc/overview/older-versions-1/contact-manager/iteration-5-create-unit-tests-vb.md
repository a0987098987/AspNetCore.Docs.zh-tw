---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: '反覆項目 #5 – 建立單元測試 (VB) |Microsoft Docs'
author: microsoft
description: 在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。 我們模擬我們的資料模型類別，並建置 o 的單元測試...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 302dfc2a26e3f357818570c673eafe44346330c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389998"
---
<a name="iteration-5--create-unit-tests-vb"></a>反覆項目 #5 – 建立單元測試 (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> 在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。 我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>建立連絡人管理 ASP.NET MVC 應用程式 (VB)

在本系列教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 每次反覆運算時，我們會逐漸改善應用程式。 此多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-讓應用程式看起來不錯。 這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。

- 反覆項目 #3-新增表單驗證。 在第三個反覆項目，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-進行鬆散偶合的應用程式。 在此第三個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。 比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。 我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。

- 反覆項目 #6-使用測試導向開發。 在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。 在這個反覆項目，我們會新增連絡人群組。

- 反覆項目 #7-新增 Ajax 功能。 在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。


## <a name="this-iteration"></a>這個反覆項目

在連絡人管理員應用程式的上一個反覆項目，我們可以重構應用程式更鬆散結合。 我們會分隔到不同控制器、 服務和儲存機制層應用程式。 每個圖層是由其下方的圖層，透過介面互動。

我們重構應用程式，讓您更輕鬆地維護及修改應用程式。 例如，如果我們需要使用新的資料存取技術，我們只是可以變更儲存機制層但沒有碰觸的控制器或服務層。 藉由連絡人管理員鬆散偶合的我們已進行應用程式更有彈性，來變更。

但是，我們需要將新的功能新增至連絡人管理員應用程式時，會發生什麼事？ 或者，當我們修正 bug 時，會發生什麼事？ Sad，但也經過證實的老實說，撰寫程式碼的是，每當您在同一個程式碼建立新的 bug 的風險。

比方說，一個美好的一天，您的管理員可能會要求您加入的新功能請連絡管理員。 她想要新增連絡人群組的支援。 她想要讓使用者能夠將他們的連絡人組織成群組，例如朋友、 商務和等等。

若要實作這項新功能，您必須修改連絡人管理員應用程式的所有三個層級。 您必須將新的功能新增至控制器、 服務層和儲存機制。 一旦您開始修改程式碼，可能會中斷運作之前的功能。

重整成個別的多層應用程式，如同我們在先前的反覆項目，是一件好事。 它是一件好事，因為它可讓我們將對整個圖層中的變更，但沒有碰觸的應用程式其餘部分。 不過，如果您想要讓您更容易維護及修改圖層內的程式碼，您需要建立的程式碼的單元測試。

您使用的單元測試的程式碼的個別單位。 小於整個應用程式層級時，則這些單位的程式碼。 一般而言，您可以使用單元測試來驗證您預期的方式中的行為是否在您的程式碼中的特定方法。 例如，您會建立由 ContactManagerService 類別公開的 CreateContact() 方法的單元測試。

只要應用程式工作的單元測試，例如防護機制。 每當您修改應用程式中的程式碼時，您可以執行一組單元測試，以檢查是否修改將會中斷現有的功能。 單元測試可讓您的程式碼安全地修改。 單元測試讓所有的程式碼應用程式中變更更有彈性。

這個反覆項目，在中，我們會將單元測試加入我們的連絡人管理員應用程式。 這樣一來，在下一個反覆項目中，我們可以將連絡人群組加入我們的應用程式而不需擔心小心破壞現有功能。

> [!NOTE] 
> 
> 有各種不同的單元測試架構，包括 NUnit、 xUnit.net 和 MbUnit。 在本教學課程中，我們使用的單元測試架構隨附於 Visual Studio。 不過，您可以輕鬆地使用其中一種替代的架構。


## <a name="what-gets-tested"></a>接受測試的基準

在完美的世界中，會受到您的程式碼的所有單元測試。 在完美的世界中，您會有完美的安全網。 您可以修改任何一行程式碼，在您的應用程式，並立即知道，藉由執行您的單元測試變更是否中斷現有的功能。

不過，我們不完美的世界中的即時 t。 實際上，撰寫單元測試時，您專注於撰寫您的商務邏輯 （例如，驗證邏輯） 的測試。 特別是，您*沒有*撰寫單元測試，為您的資料存取邏輯或檢視邏輯。

若要很有用，單元測試必須非常快速地執行。 您輕鬆地會累積數百個 （或甚至數千個） 的應用程式的單元測試。 如果單元測試需要很長的時間執行，則您將可避免執行它們。 換句話說，長時間執行的單元測試是每日的程式碼撰寫用途沒有幫助的。

基於這個理由，您通常不寫入與資料庫互動的程式碼的單元測試。 針對即時資料庫執行數百個單元測試會太慢。 相反地，您會模擬您的資料庫，並撰寫模擬 （mock） 的資料庫 （我們討論模擬 (mock) 下的資料庫） 進行互動的程式碼。

基於類似的理由，您通常不撰寫檢視的單元測試。 若要測試的檢視，您必須啟動網頁伺服器。 因為分內快速啟動網頁伺服器是一個相當緩慢的程序，不建議為您的檢視建立單元測試。

如果您的檢視包含複雜的邏輯應該考慮將邏輯移至 Helper 方法。 您可以撰寫執行而不會啟動 web 伺服器的協助程式方法的單元測試。

> [!NOTE] 
> 
> 雖然撰寫測試的資料存取邏輯，或檢視邏輯不是個不錯的主意撰寫單元測試時，這些測試可以是非常重要，建置功能或整合測試時。


> [!NOTE] 
> 
> ASP.NET MVC 是 Web Form 檢視引擎。 Web Form 檢視引擎相依於 web 伺服器時，可能不是其他檢視引擎。


## <a name="using-a-mock-object-framework"></a>使用模擬 （mock） 物件架構

當建置單元測試，您幾乎都要利用模擬物件架構。 模擬物件架構可讓您建立應用程式中的模擬 （mock） 和類別的虛設常式。

例如，您可以使用模擬物件架構產生的儲存機制類別的模擬 （mock） 版本。 如此一來，您可以使用模擬 （mock） 的儲存機制類別而不是實際的儲存機制類別在您的單元測試。 使用模擬的儲存機制，可讓您避免執行單元測試時，執行資料庫程式碼。

Visual Studio 不會包含一個模擬物件架構。 不過，有數個商業和開放原始碼模擬物件架構適用於.NET framework:

1. Moq-此架構是開放原始碼 BSD 授權可用。 您可以下載從 Moq [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/)。
2. Rhino Mocks-此架構是開放原始碼 BSD 授權可用。 您可以下載 Rhino Mocks 從[ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx)。
3. Typemock Isolator-這是一個商業的架構。 您可以下載從試用版[ http://www.typemock.com/ ](http://www.typemock.com/)。

在本教學課程中，我決定使用 Moq。 不過，您可以很容易使用 Rhino Mocks 或 Typemock Isolator 建立 Mock 物件，請連絡管理員應用程式。

您可以使用 Moq 之前，您需要完成下列步驟：

1. 。
2. 解壓縮下載之前，請確定您以滑鼠右鍵按一下檔案，然後按一下  按鈕**解除封鎖**（請參閱 圖 1）。
3. 解壓縮下載。
4. 選取功能表選項將 Moq 組件的參考加入您的測試專案**專案中，加入參考**來開啟**加入參考**對話方塊。 在 [瀏覽] 索引標籤中，瀏覽至您解壓縮 Moq 資料夾並選取 Moq.dll 組件。 按一下 **確定**按鈕 （請參閱 圖 2）。


[![解除封鎖 Moq](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**圖 01**： 解除封鎖 Moq ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-vb/_static/image2.png))


[![新增 Moq 之後的參考](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**圖 02**： 新增 Moq 之後的參考 ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-vb/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>建立服務層的單元測試

可讓 s 著手建立一組我們連絡人管理員應用程式的服務層的單元測試。 我們將使用這些測試來驗證我們的驗證邏輯。

建立名為 Models ContactManager.Tests 專案中的新資料夾。 接下來，以滑鼠右鍵按一下 [模型] 資料夾，然後選取**新增]、 [新的測試**。 **加入新測試**[圖 3] 所示的對話方塊隨即出現。 選取 **單元測試**範本並命名您的新測試 ContactManagerServiceTest.vb。 按一下 **確定**按鈕來測試專案中加入新的測試。

> [!NOTE] 
> 
> 一般情況下，您會想測試專案，以符合您的 ASP.NET MVC 專案的資料夾結構的資料夾結構。 例如，您將測試控制器放在 Controllers 資料夾中，在 [模型] 資料夾中，模型測試等等。


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**圖 03**: Models\ContactManagerServiceTest.cs ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-vb/_static/image6.png))


一開始，我們想要測試 ContactManagerService 類別所公開的 CreateContact() 方法。 我們將建立下列五個測試：

- CreateContact()-測試該 CreateContact() 值為 true 時傳回有效的連絡人會傳遞至方法。
- CreateContactRequiredFirstName()-錯誤訊息加入至模型狀態時遺失的第一個名稱與連絡人的測試會傳遞至 CreateContact() 方法。
- CreateContactRequredLastName()-錯誤訊息加入至模型狀態時遺失的最後一個名稱與連絡人的測試會傳遞至 CreateContact() 方法。
- CreateContactInvalidPhone()-錯誤訊息會加入至模型狀態時具有無效電話號碼的連絡人的測試會傳遞至 CreateContact() 方法。
- CreateContactInvalidEmail()-錯誤訊息會加入至模型狀態時無效的電子郵件地址的連絡人的測試會傳遞至 CreateContact() 方法...

第一項測試會驗證有效的連絡人不會產生驗證錯誤。 剩餘的測試會檢查每個驗證規則。

在 列表 1 中包含這些測試的程式碼。

**Listing 1 - Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]


因為我們使用列表 1 中的 Contact 類別，我們需要將 Microsoft Entity Framework 的參考新增至我們的測試專案。 加入 System.Data.Entity 組件的參考。


列表 1 包含一個名為 [TestInitialize] 屬性裝飾的 initialize （） 方法。 每個單元測試執行之前自動呼叫這個方法 （它每個單元測試之前稱為 5 次）。 Initialize （） 方法會使用下列程式碼行建立模擬的儲存機制：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

這行程式碼會使用的 Moq 架構，來產生模擬的儲存機制從 IContactManagerRepository 介面。 模擬儲存機制而不是實際 EntityContactManagerRepository 用以避免存取資料庫，每個單元測試執行時。 模擬儲存機制實作 IContactManagerRepository 介面的方法，但方法 don t 實際執行任何動作。

> [!NOTE] 
> 
> 當使用 Moq 架構，是區分\_mockRepository 和\_mockRepository.Object。 前者是指 Mock (的 IContactManagerRepository) 類別，其中包含方法來指定模擬的儲存機制的行為模式為何。 後者是指實際模擬儲存機制實作 IContactManagerRepository 介面。


模擬儲存機制會在 initialize （） 方法中，建立 ContactManagerService 類別的執行個體時。 所有個別的單元測試使用 ContactManagerService 類別執行個體。

列表 1 包含五個對應至每個單元測試的方法。 每一種方法是以 [TestMethod] 屬性裝飾。 當您執行單元測試時，任何包含這個屬性會呼叫方法。 換句話說，以 [TestMethod] 屬性裝飾的任何方法都是單元測試。

第一個單元測試時，名為 CreateContact()，驗證，呼叫 CreateContact() 時，傳回值 true Contact 類別的有效執行個體傳遞至方法。 測試會建立連絡人類別的執行個體、 呼叫 CreateContact(); 方法，並確認 CreateContact() 傳回值 true。

剩餘的測試確認 CreateContact() 方法呼叫與無效的連絡人時然後方法會傳回 false，且預期的驗證錯誤訊息加入至模型狀態。 比方說，CreateContactRequiredFirstName() 測試會建立連絡人類別的執行個體，取代其 FirstName 屬性為空字串。 接下來，CreateContact() 方法呼叫無效的連絡人。 最後，測試會驗證 CreateContact() 傳回 false，而且模型狀態包含預期的驗證錯誤訊息 「 名字 」 所需。

您也可以選取功能表選項列表 1 中執行單元測試**測試執行時，解決方案 （CTRL + R、 A） 中的所有測試**。 測試的結果會顯示在 測試結果 視窗 （請參閱 圖 4）。


[![測試結果](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**圖 04**： 測試結果 ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-vb/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>建立控制器的單元測試

ASP.NET MVC 應用程式來控制流程的使用者互動。 在測試控制器時，您會想要測試是否控制器會傳回正確的動作結果，並檢視資料。 您也可能會想要測試是否與預期的方式中的模型類別互動的控制器。

例如，列表 2 包含兩個單元測試，請連絡控制器 create （） 方法。 第一次的單元測試會驗證時有效的連絡人傳遞給 create （） 方法，則 create （） 方法會重新導向至 Index 動作。 換句話說，當傳遞有效的連絡人，create （） 方法應該會傳回代表索引動作 RedirectToRouteResult。

我們不想要測試 ContactManager 服務層，我們會測試在控制器層時。 因此，我們會模擬服務層的 Initialize 方法中的下列程式碼：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

在 CreateValidContact() 單元測試中，我們會模擬呼叫服務層使用下列程式碼行 CreateContact() 方法的行為：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

這行程式碼中，會導致模擬 （mock） ContactManager 服務呼叫其 CreateContact() 方法時傳回 true 值。 藉由模擬服務層，我們可以測試控制器的行為，而不需要在服務層中執行任何程式碼。

第二個單元測試會驗證 create （） 動作在無效的連絡人會傳遞至方法時，會傳回建立檢視。 我們會造成服務層 CreateContact() 方法來傳回其值為 false，使用下列程式碼行：

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

如果 create （） 方法的行為如我們所預期它應該傳回建立檢視時的服務層會傳回 false 值。 如此一來，控制器可以顯示驗證錯誤訊息中建立檢視和使用者有機會先行修正該無效的連絡人屬性。


如果您打算建置您的控制站的單元測試您需要從控制器動作傳回明確的檢視表名稱。 比方說，不會傳回如下的檢視：

傳回 View()

相反地，傳回的檢視，像這樣：

傳回 View("Create")

如果您不是明確傳回檢視時 ViewResult.ViewName 屬性就會傳回空字串。


**Listing 2 - Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>總結

這個反覆項目，在中，我們會建立單元測試我們的連絡人管理員應用程式。 我們可以在任何時間，以確認我們的應用程式仍會以我們所預期的方式來執行這些單元測試。 單元測試做為我們的應用程式讓我們安全地修改應用程式未來的安全網。

我們建立兩個一組單元測試。 首先，我們會透過建立我們的服務層的單元測試測試我們的驗證邏輯。 接下來，我們會透過建立我們的控制器層的單元測試測試我們的流程控制邏輯。 在測試我們的服務層時，我們隔離我們的測試我們的服務層從我們的儲存機制層模擬 (mock) 我們的儲存機制層。 在測試控制器層時，我們隔離我們的測試我們控制站的圖層的模擬 (mock) 的服務層。

中的下一個反覆項目中，我們會修改連絡人管理員應用程式，使它支援連絡人群組。 我們會將這項新功能加入我們的應用程式使用稱為 「 測試驅動開發軟體設計程序。

> [!div class="step-by-step"]
> [上一頁](iteration-4-make-the-application-loosely-coupled-vb.md)
> [下一頁](iteration-6-use-test-driven-development-vb.md)
