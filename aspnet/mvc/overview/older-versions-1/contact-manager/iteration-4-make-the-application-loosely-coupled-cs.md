---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: '反覆項目 #4 – 讓鬆散偶合的應用程式 (C#) |Microsoft 文件'
author: microsoft
description: 在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。 如需...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 33221c6c3326c7034fe013f152579828e2fc8a3a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>反覆項目 #4 – 讓鬆散偶合的應用程式 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> 在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。 例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。


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

連絡人管理員應用程式的這個第四個反覆項目中重構應用程式，讓較鬆散偶合的應用程式。 當鬆散偶合的應用程式時，您可以修改應用程式的一個部分中的程式碼，而不需要修改應用程式的其他部分中的程式碼。 鬆散偶合的應用程式是變更更有彈性。

目前，所有連絡人管理員應用程式所使用的資料存取和驗證邏輯包含在控制器類別。 這是個好主意。 每當您需要修改您的應用程式的一個部分，就可能會引入 bug 到另一個應用程式的一部分。 例如，如果您修改您的驗證邏輯，就可能會進入您的資料存取或控制站邏輯引進新的 bug。

> [!NOTE] 
> 
> (SRP)，一個類別應該永遠不會有一個以上的理由来變更。 混合控制站、 驗證和資料庫的邏輯是大量的單一責任原則違規。


有多種原因所造成，您可能需要修改您的應用程式。 您可能需要您的應用程式中加入新功能，您可能需要在應用程式中修正 bug 或您可能需要修改您的應用程式的一項功能的實作方式。 應用程式很少是靜態。 他們傾向於成長及變動經過一段時間。

例如，假設您決定要變更 資料存取層的實作方式。 權限，連絡人管理員應用程式使用 Microsoft Entity Framework 來存取資料庫。 不過，您可能決定要移轉至新的或替代的資料存取技術如 ADO.NET Data Services 或 NHibernate。 不過，資料存取程式碼是分開的驗證和控制器程式碼，因為沒有方法可以修改應用程式中的資料存取程式碼，而不需修改與資料存取直接相關的其他程式碼。

當鬆散偶合的應用程式時，相反地，您可以變更應用程式的一個部分但沒有碰觸的應用程式其他部分。 比方說，您可以切換資料存取技術，而不需要修改您驗證或控制站的邏輯。

在這個反覆項目，我們利用數種軟體設計模式，讓我們重構連絡人管理員應用程式到較鬆散的應用程式。 當我們完成之後時，贏了 t 連絡人管理員執行任何動作，它 professionals t 之前執行。 不過，我們可以更輕鬆地在未來的應用程式變更。

> [!NOTE] 
> 
> 重構是重寫的方式，它不會遺失任何現有功能的應用程式的程序。


## <a name="using-the-repository-software-design-pattern"></a>使用儲存機制的軟體設計模式

我們第一項變更是利用稱為 「 儲存機制模式的軟體設計模式。 我們會使用儲存機制模式來隔離應用程式的其餘部分我們資料存取程式碼。

實作儲存機制模式需要我們完成下列兩個步驟：

1. 建立介面
2. 建立實作介面的具象類別

首先，我們需要建立說明資料存取方法，我們需要執行的所有介面。 IContactManagerRepository 介面包含在程式碼範例 1。 此介面描述五種方法： CreateContact()、 DeleteContact()、 EditContact()、 GetContact 和 ListContacts()。

**列出 1-Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

接下來，我們必須建立實作 IContactManagerRepository 介面的具象類別。 因為我們使用 Microsoft Entity Framework 來存取資料庫，我們將建立名為 EntityContactManagerRepository 的新類別。 此類別被包含在清單 2。

**Listing 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

請注意 EntityContactManagerRepository 類別實作 IContactManagerRepository 介面。 類別會實作所有五個介面所描述的方法。

您可能會懷疑為何需要擔心介面。 我們要建立介面和實作它的類別為何？

有一個例外狀況，我們的應用程式的其餘部分將互動的介面，以及不具象類別。 而不是呼叫 EntityContactManagerRepository 類別所公開的方法，我們稱 IContactManagerRepository 介面所公開的方法。

這樣一來，我們可以實作新的類別與介面，而不需要修改應用程式的其餘部分。 例如，在未來的某個日期，我們可能需要實作 DataServicesContactManagerRepository 類別可實作 IContactManagerRepository 介面。 DataServicesContactManagerRepository 類別可能會使用 ADO.NET Data Services 來存取資料庫，而不是 Microsoft Entity Framework。

如果應用程式的程式碼針對 IContactManagerRepository 介面，而不是具象 EntityContactManagerRepository 類別編寫然後我們就可以切換具象類別而不需修改任何的程式碼的其餘部分。 例如，我們可以從切換 EntityContactManagerRepository 類別 DataServicesContactManagerRepository 類別而不需修改我們的資料存取或驗證邏輯。

程式設計介面 （抽象） 而不是具象類別針對可讓您更有彈性，來變更應用程式。

> [!NOTE] 
> 
> 您可以選取功能表選項重構，擷取介面，從 Visual Studio 中的具象類別上快速建立介面。 比方說，您可以先建立 EntityContactManagerRepository 類別，，然後使用自動產生 IContactManagerRepository 介面的 擷取介面。


## <a name="using-the-dependency-injection-software-design-pattern"></a>使用相依性插入軟體設計模式

既然我們已移轉資料存取程式碼的不同的儲存機制類別，我們需要修改連絡人 controller 使用這個類別。 我們會利用呼叫使用的儲存機制類別我們控制器中的相依性插入的軟體設計模式。

修改過的連絡人控制器被包含在列出的 3。

**Listing 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

請注意，連絡人控制器中列出的 3 會有兩個建構函式。 第一個建構函式會將 IContactManagerRepository 介面的具象執行個體傳遞給第二個建構函式。 請連絡控制器類別會使用*建構函式相依性插入*。

以及 EntityContactManagerRepository 類別使用的位置是在第一個建構函式。 類別的其餘部分將使用 IContactManagerRepository 介面而不是具象 EntityContactManagerRepository 類別。

這可讓您易於未來切換 IContactManagerRepository 類別的實作。 如果您想要使用 DataServicesContactRepository 類別，而不是 EntityContactManagerRepository 類別，只需修改第一個建構函式。

建構函式相依性插入也可讓連絡人控制器類別非常測試。 在單元測試中，您可以藉由傳遞 IContactManagerRepository 類別的模擬實作初始化連絡人控制站。 當我們建立連絡人管理員應用程式的單元測試時，這項功能的相依性插入會對我們的下一個反覆運算中非常重要。

> [!NOTE] 
> 
> 如果您想要完全解除連絡人控制器類別從 IContactManagerRepository 介面的特定實作的聯結然後您可以利用支援例如 StructureMap 或 Microsoft 的相依性插入的架構Entity Framework (MEF)。 利用相依性插入架構，您永遠不需要參考程式碼中的具象類別。


## <a name="creating-a-service-layer"></a>建立服務層

您可能已注意到，我們將驗證邏輯仍混合我們控制器中的邏輯中列出的 3 修改的控制器類別。 基於相同理由，則建議您區隔我們的資料存取邏輯，最好隔離我們驗證邏輯。

若要修正這個問題，我們可以建立個別[*服務層*](http://martinfowler.com/eaaCatalog/serviceLayer.html)。 服務層是我們的控制站和儲存機制類別之間，我們可以插入個別圖層。 服務層包含商務流程包括所有我們驗證邏輯。

ContactManagerService 都包含在列出的 4。 其中包含驗證邏輯，從連絡人控制器類別。

**Listing 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

請注意 ContactManagerService 的建構函式需要 ValidationDictionary。 服務層會與透過此 ValidationDictionary 控制器層進行通訊。 當我們討論的裝飾項目的模式時，我們會討論在下一節中詳細 ValidationDictionary。

此外，請注意，ContactManagerService 實作 IContactManagerService 介面。 您應一律盡可能進行程式設計的介面，而不是具象類別。 連絡人管理員應用程式中的其他類別不會與互動 ContactManagerService 類別直接。 相反地，有一個例外狀況，連絡人管理員應用程式的其餘部分被編寫 IContactManagerService 介面。

列出 5 包含 IContactManagerService 介面。

**列出 5-Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

修改過的連絡人控制器類別包含在程式碼範例 6。 請注意，連絡人控制站不會再互動 ContactManager 儲存機制。 相反地，請連絡控制器與 ContactManager 服務互動。 每個圖層的隔離之盡可能從其他圖層。

**Listing 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

我們的應用程式不會再執行與的單一責任原則 (SRP)。 連絡控制器，列出 6 中的已移除的每個非控制應用程式執行的工作流程的責任。 所有驗證邏輯具有已從連絡人控制器中移除並推送至服務層。 所有的資料庫邏輯已推入儲存機制的圖層。

## <a name="using-the-decorator-pattern"></a>使用裝飾項目模式

我們想要能夠完全分離我們從我們的控制器層的服務層。 基本上，我們應該可以透過我們的服務層中不同的組件從我們的控制器層編譯而不需要加入我們的 MVC 應用程式的參考。

不過，我們的服務層必須能夠將驗證錯誤訊息傳回控制器層級。 我們要如何啟用服務層不耦合的控制站和服務層進行通訊的驗證錯誤訊息？ 我們可以利用一種軟體設計模式，名為[裝飾項目的模式](http://en.wikipedia.org/wiki/Decorator_pattern)。

控制器會使用名為 ModelState ModelStateDictionary，表示驗證錯誤。 因此，您可能會想要 ModelState 從控制器層傳遞至服務層。 不過，在服務層中使用 ModelState 會讓服務層相依於 ASP.NET MVC 架構的功能。 這會是不正確，因為吧，您可能想要使用 WPF 應用程式，而不是 ASP.NET MVC 應用程式的服務層。 在此情況下，您不想要參考 ASP.NET MVC 架構使用 ModelStateDictionary 類別。

裝飾項目的模式可讓您將現有的類別中的新類別，才能實作介面。 我們的連絡管理員專案會包含列出 7 中所含的 ModelStateWrapper 類別。 ModelStateWrapper 類別會實作介面中列出 8。

**Listing 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**列出 8-Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

如果您列出 5 仔細查看您會看到 ContactManager 服務層，只會使用 IValidationDictionary 介面。 ContactManager 服務不相依於 ModelStateDictionary 類別。 當連絡人控制器建立 ContactManager 服務時，控制器會包裝其 ModelState 像這樣：

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>總結

在這個反覆項目，我們沒有新增任何新功能到連絡人管理員應用程式。 這個反覆項目的已重構連絡人管理員應用程式，好讓它更容易維護及修改。

首先，我們會實作儲存機制軟體設計模式。 我們可以移轉所有資料存取程式碼的不同 ContactManager 儲存機制類別。

我們也會從控制器邏輯隔離我們驗證邏輯。 我們建立個別的服務層，其中包含所有驗證程式碼。 控制器層級互動服務層，以及服務層與互動的儲存機制的圖層。

當我們建立的服務層時，我們利用從我們的服務層隔離 ModelState 的裝飾項目的模式。 在服務層中，我們所撰寫的 IValidationDictionary 介面，而不是 ModelState。

最後，我們利用一種軟體設計模式，名為相依性插入模式。 此模式可讓我們進行程式設計的介面 （抽象） 而不是具象類別。 實作相依性插入的設計模式也可讓我們的程式碼更多測試。 中的下一個反覆項目中，我們加入受測專案的單元測試。

> [!div class="step-by-step"]
> [上一頁](iteration-3-add-form-validation-cs.md)
> [下一頁](iteration-5-create-unit-tests-cs.md)
