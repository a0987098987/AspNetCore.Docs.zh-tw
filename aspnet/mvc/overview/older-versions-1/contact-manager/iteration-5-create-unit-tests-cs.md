---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: "反覆項目 #5 – 建立單元測試 (C#) |Microsoft 文件"
author: microsoft
description: "第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試的 o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: f9b2d05ec8756d68f6bd2f387c85faf03abd167e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="iteration-5--create-unit-tests-c"></a>反覆項目 #5 – 建立單元測試 (C#)
====================
由[Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> 第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>建立連絡人管理 ASP.NET MVC 應用程式 (C#)

在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 與每個反覆項目，我們會逐漸改善應用程式。 這個多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-請看起來很棒的應用程式。 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。

- 反覆項目 #3-加入表單驗證。 第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-請鬆散偶合的應用程式。 在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。 例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。

- 反覆項目 #6-使用測試為導向的開發。 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，我們會加入連絡人群組。

- 反覆項目 #7： 加入 Ajax 功能。 在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。


## <a name="this-iteration"></a>這個反覆項目

在連絡人管理員應用程式的上一個反覆項目，我們重構是更鬆散偶合的應用程式。 我們分隔到不同的控制站、 服務和儲存機制的圖層的應用程式。 每個圖層會與下圖層，透過介面互動。

我們重構應用程式，讓您更輕鬆地維護及修改應用程式。 例如，如果我們需要使用新的資料存取技術，我們只是可以變更儲存機制層但沒有碰觸的控制站或服務層。 藉由連絡人管理員鬆散結合且我們 ve 進行更有彈性，來變更應用程式。

但是，我們需要將新功能新增至連絡人管理員應用程式時，會發生什麼事？ 或者，當我們修正 bug 時，會發生什麼事？ 撰寫程式碼的悲傷，但也經過驗證，真是每當您修改程式碼建立引入新的 bug 的風險。

例如，一個美好的一天，您的管理員可能會要求您新增到連絡人管理員的新功能。 她想要新增連絡人群組的支援。 她想要讓使用者將他們的連絡人組織成群組，例如朋友、 商務和等等。

若要實作這項新功能，您必須修改連絡人管理員應用程式的所有三個層級。 您必須將新功能加入至控制器、 服務層和儲存機制。 一旦您開始修改程式碼時，就可能會中斷運作正常的功能。

重構至個別圖層，應用程式，如同先前的反覆項目，是個好方法。 因為它可讓我們對整個圖層中的變更，但沒有碰觸其餘的應用程式，它會是個好方法。 不過，如果您想要讓您更輕鬆地維護及修改圖層內的程式碼，您需要建立單元測試的程式碼。

您使用單元測試的程式碼個別單位。 這些程式碼的單位是小於整個應用程式層級。 一般而言，您可以使用單元測試來驗證您預期的方式中的行為是否在程式碼中的特定方法。 例如，您會建立由 ContactManagerService 類別公開的 CreateContact() 方法的單元測試。

只要應用程式工作的單元測試，例如安全網。 每當您修改應用程式中的程式碼，您可以執行一組要檢查此修改是否中斷現有功能的單元測試。 單元測試讓安全地修改程式碼。 單元測試讓所有的程式碼應用程式中變更的更有彈性。

在這個反覆項目，我們加入我們的連絡人管理員應用程式的單元測試。 這樣一來，在下一個反覆項目，我們可以將連絡人群組加入我們的應用程式而不需擔心中斷現有的功能。

> [!NOTE] 
> 
> 有各種不同的單元測試架構，包含適用於 NUnit、 xUnit.net 和 MbUnit。 在本教學課程中，我們使用的單元測試架構隨附於 Visual Studio。 不過，您可以輕鬆地使用其中一種替代的架構。


## <a name="what-gets-tested"></a>取得測試的項目

在理想的世界裡，所有的程式碼會涵蓋單元測試。 在理想的世界裡，您必須完美的安全網。 您可以修改任何應用程式中的程式碼行，即可立即知道，藉由執行單元測試，變更是否中斷現有的功能。

不過，我們不完美的世界中的即時 t。 實際上，撰寫單元測試時，您專注於商務邏輯 （例如，驗證邏輯） 為撰寫測試。 特別是，您*不*撰寫單元測試您的資料存取邏輯或檢視邏輯。

若要很有用，單元測試必須非常快速地執行。 您輕鬆地會累積數百個 （或甚至數千） 的應用程式的單元測試。 如果單元測試需要很長的時間來執行您可避免執行它們。 換句話說，長時間執行的單元測試是沒有幫助，每天程式碼撰寫用途的。

基於這個理由，您通常不要撰寫與資料庫互動的程式碼的單元測試。 針對即時資料庫，執行數百個單元測試很太慢。 相反地，您會模擬您的資料庫，並撰寫與模擬的資料庫 （我們討論模擬下的資料庫） 互動的程式碼。

針對類似的理由，您通常不要撰寫單元測試的檢視。 若要測試的檢視，您必須備妥 web 伺服器。 因為網頁伺服器，大致是相對比較慢的處理程序，所以不建議建立單元測試您的檢視。

如果您的檢視包含複雜的邏輯應該考慮將邏輯移至 Helper 方法。 您可以撰寫單元測試的 Helper 方法，以執行不含才能組織好 web 伺服器。

> [!NOTE] 
> 
> 寫入測試的資料存取邏輯或檢視邏輯時，不建議您撰寫單元測試，而這些測試可以是建置功能或整合測試時非常有用。


> [!NOTE] 
> 
> ASP.NET MVC 是 Web Form 檢視引擎。 Web Form 檢視引擎相依於 web 伺服器時，可能不是其他檢視引擎。


## <a name="using-a-mock-object-framework"></a>使用模擬物件架構

當建立單元測試時，您幾乎都需要利用模擬物件架構。 模擬物件架構可讓您在 您的應用程式中建立模擬和虛設常式類別。

例如，您可以使用模擬物件架構來產生模擬的儲存機制類別版本。 這樣一來，您可以使用模擬儲存機制類別而不是實際的儲存機制類別在您的單元測試。 使用模擬的儲存機制，可讓您避免執行單元測試時，執行資料庫程式碼。

Visual Studio 不包含模擬物件架構。 不過，有數個商業和開放原始碼模擬物件架構適用於.NET framework:

1. Moq-此架構可開放原始碼 BSD 授權。 您可以下載從 Moq [https://code.google.com/p/moq/](https://code.google.com/p/moq/)。
2. Rhino Mocks-此架構才可使用的開放原始碼 BSD 授權。 您可以下載從 Mocks Rhino [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx)。
3. Typemock 絕緣器-這是商業架構。 您可以下載從試用版[http://www.typemock.com/](http://www.typemock.com/)。

在本教學課程中，我決定使用 Moq。 不過，您可以輕鬆地使用 Rhino Mocks 或 Typemock 絕緣器來建立模擬物件連絡人管理員應用程式。

您可以使用 Moq 之前，您需要完成下列步驟：

1. 。
2. 解壓縮下載之前，請確定您以滑鼠右鍵按一下檔案，然後按一下 [] 按鈕**解除封鎖**（請參閱圖 1）。
3. 解壓縮下載。
4. Moq 組件的參考加入 ContactManager.Tests 專案中的 [參考] 資料夾上按一下滑鼠右鍵，然後選取**加入參考**。 在 [瀏覽] 索引標籤中，瀏覽至您解壓縮 Moq 資料夾並選取 Moq.dll 組件。 按一下**確定** 按鈕。
5. 完成這些步驟之後，參考資料夾看起來應該像圖 2。


[![解除封鎖 Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**圖 01**： 解除封鎖 Moq ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image2.png))


[![加入 Moq 之後的參考](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**圖 02**： 參考加入 Moq 之後 ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>建立服務層的單元測試

可讓 s 開始建立一組我們連絡人管理員應用程式 s 的服務層的單元測試。 若要確認我們驗證邏輯，我們會使用這些測試。

建立新資料夾，名為 ContactManager.Tests 專案中的模型。 接下來，以滑鼠右鍵按一下 [模型] 資料夾，然後選取**新增]、 [新的測試**。 **加入新測試**圖 3 所示的對話方塊隨即出現。 選取**單元測試**範本，並命名新的測試 ContactManagerServiceTest.cs。 按一下**確定** 按鈕，測試專案中加入新的測試。

> [!NOTE] 
> 
> 一般情況下，您想要比對您的 ASP.NET MVC 專案的資料夾結構的測試專案的資料夾結構。 例如，您會置於 Controllers 資料夾，在 [模型] 資料夾中，模型測試的測試控制器，並以此類推。


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**圖 03**: Models\ContactManagerServiceTest.cs ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image6.png))


一開始，我們想要測試 ContactManagerService 類別所公開的 CreateContact() 方法。 我們將建立下列五個測試：

- CreateContact()-測試該 CreateContact() 在值為 true 時傳回有效的連絡人會傳遞至方法。
- CreateContactRequiredFirstName()-錯誤訊息會加入至模型狀態時遺失的名字與連絡人的測試會傳遞至 CreateContact() 方法。
- CreateContactRequredLastName()-測試錯誤訊息會加入至模型狀態時具有遺失的姓氏的連絡人會傳遞至 CreateContact() 方法。
- CreateContactInvalidPhone()-測試錯誤訊息會加入至模型狀態時具有無效電話號碼的連絡人會傳遞至 CreateContact() 方法。
- CreateContactInvalidEmail()-CreateContact() 方法傳遞錯誤訊息會加入至模型狀態時具有無效的電子郵件地址的連絡人的測試...

第一項測試會驗證有效的連絡人不會產生驗證錯誤。 其餘的測試會檢查每個驗證規則。

這些測試的程式碼會包含在程式碼範例 1。

**列出 1-Models\ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]


因為我們使用連絡人類別清單 1 中，我們需要將 Microsoft Entity Framework 的參考加入至我們的測試專案。 新增 System.Data.Entity 組件的參考。


清單 1 包含名為 initialize （），以 [TestInitialize] 屬性裝飾的方法。 每個單元測試執行之前自動呼叫這個方法 （它每個單元測試之前稱為 5 次）。 Initialize （） 方法會建立模擬的儲存機制，與下列程式碼行：

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

這行程式碼會產生模擬的儲存機制從 IContactManagerRepository 介面使用 Moq 架構。 模擬儲存機制而不是實際 EntityContactManagerRepository 用於避免每個單元測試執行時存取資料庫。 模擬儲存機制實作介面方法的 IContactManagerRepository，但是方法 don t 實際執行任何動作。

> [!NOTE] 
> 
> 當使用 Moq 架構，沒有區分\_mockRepository 和\_mockRepository.Object。 前者是指模擬&lt;IContactManagerRepository&gt;類別，其中包含方法來指定模擬的儲存機制的行為方式。 後者是指實際模擬儲存機制實作 IContactManagerRepository 介面。


建立 ContactManagerService 類別的執行個體時，會模擬儲存機制使用 initialize （） 方法中。 所有的個別單元測試使用 ContactManagerService 類別執行個體。

清單 1 包含五個對應至每個單元測試的方法。 每一種方法是以 [TestMethod] 屬性裝飾。 當您執行單元測試時，包含這個屬性的任何方法呼叫。 換句話說，任何以 [TestMethod] 屬性裝飾的方法的單元測試。

名為 CreateContact()，第一個單元測試會驗證呼叫 CreateContact() 值 true 當時，傳回連絡人類別的有效執行個體傳遞給方法。 測試建立連絡人類別的執行個體，呼叫 CreateContact() 方法時，並確認 CreateContact() 傳回值 true。

剩餘的測試確認 CreateContact() 方法呼叫含有無效的連絡人時然後方法會傳回 false 的預期的驗證錯誤訊息加入至模型狀態。 比方說，CreateContactRequiredFirstName() 測試會以空字串的 FirstName 屬性建立連絡人類別的執行個體。 接下來，CreateContact() 方法稱為含有無效的連絡人。 最後，測試會 CreateContact() 傳回 false，而且模型狀態包含預期的驗證錯誤訊息 「 名字 」 所需。

您也可以選取功能表選項清單 1 中執行單元測試**，執行測試，（CTRL + R、 A） 的方案中的所有測試**。 測試結果會顯示在 [測試結果] 視窗中 （請參閱圖 4）。


[![測試結果](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**圖 04**： 測試結果 ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>建立單元測試控制器

ASP.NETMVC 應用程式會控制流程的使用者互動。 在測試控制器時，您會想要測試是否控制器傳回正確的動作結果，並檢視資料。 您也可能會想要測試是否與以預期的方式的模型類別互動的控制器。

例如，列出 2 包含連絡人控制器 create （） 方法的兩個單元測試。 第一個單元測試會確認有效的連絡人給 create （） 方法，則 create （） 方法將重新導向至索引動作。 換句話說，當傳遞有效的連絡人，則 create （） 方法應該傳回 RedirectToRouteResult 代表索引動作。

我們不想要測試 ContactManager 服務層，我們所測試的控制器層時。 因此，我們會模擬服務層的初始化方法中的下列程式碼：

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

在 CreateValidContact() 單元測試中，我們會模擬呼叫服務層與下列程式碼行 CreateContact() 方法的行為：

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

這行程式碼會導致模擬 ContactManager 服務呼叫其 CreateContact() 方法時，傳回值 true。 藉由模擬服務層，我們可以測試我們控制站的行為，而不需要在服務層中執行的任何程式碼。

第二個單元測試會驗證當無效的連絡人會傳遞至方法時，create （） 動作，傳回建立檢視。 我們會造成服務層 CreateContact() 方法傳回的值為 false，且下列程式碼行：

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

如果 create （） 方法的行為，如我們所預期它應傳回建立檢視時的服務層會傳回其值為 false。 這樣一來，控制器可顯示驗證錯誤訊息中建立檢視和使用者有機會先行修正該無效的連絡人屬性。


如果您計劃要建置單元測試您的控制站您需要從控制器動作傳回明確的檢視表名稱。 例如，不會傳回如下的檢視：

傳回 View();

相反地，傳回的檢視就像這樣：

傳回 View("Create");

如果您不明確傳回檢視時 ViewResult.ViewName 屬性會傳回空字串。


**列出 2-Controllers\ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>總結

在這個反覆項目，我們會建立單元測試連絡人管理員應用程式。 我們可以在任何時間，以確認我們的應用程式仍然運作我們所預期的方式執行這些單元測試。 單元測試做為應用程式讓我們來安全地修改應用程式在未來的安全網。

我們建立兩個集合的單元測試。 首先，我們會透過建立單元測試，我們的服務層的測試我們驗證邏輯。 接下來，我們會透過建立單元測試，我們的控制器層的測試我們的流程控制邏輯。 在測試我們的服務層時，我們隔離我們的測試為我們的服務層從我們的儲存機制層模擬我們的儲存機制層。 在測試控制器的圖層時，我們隔離我們的測試我們控制器層級的模擬服務層。

中的下一個反覆項目中，我們會修改連絡人管理員應用程式，使它支援連絡人群組。 我們會將這項新功能新增至我們的應用程式使用稱為 「 測試驅動開發軟體設計程序。

>[!div class="step-by-step"]
[上一頁](iteration-4-make-the-application-loosely-coupled-cs.md)
[下一頁](iteration-6-use-test-driven-development-cs.md)
