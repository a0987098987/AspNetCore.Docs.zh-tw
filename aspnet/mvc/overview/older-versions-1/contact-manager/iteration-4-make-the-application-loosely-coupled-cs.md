---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: '反覆項目 #4 – 讓應用程式鬆散耦合 (C#) |Microsoft Docs'
author: microsoft
description: 在此第三個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。 如需...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 8eff8088398d0f7afc020b2bbdf41526ae51591a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829495"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>反覆項目 #4 – 讓應用程式鬆散耦合 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> 在此第三個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。 比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>建立連絡人管理 ASP.NET MVC 應用程式 (C#)

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

在連絡人管理員應用程式的這個第四個反覆項目，我們可以重構應用程式做出更鬆散偶合的應用程式。 當鬆散偶合的應用程式時，您可以修改應用程式的其中一部分中的程式碼，而不需要修改應用程式的其他部分中的程式碼。 鬆散偶合的應用程式是變更更有彈性。

目前，所有的連絡人管理員應用程式所使用的資料存取和驗證邏輯都包含在控制器類別。 這是個好主意。 每當您需要修改您的應用程式的一部分，您可能會引入您的應用程式的另一個組件中的 bug。 比方說，如果您修改您的驗證邏輯時，就可能將您的資料存取] 或 [控制器邏輯引入新的 bug。

> [!NOTE] 
> 
> (SRP)，類別應該永遠不會有一個以上的理由来變更。 混合控制站、 驗證和資料庫的邏輯是大規模 Single Responsibility Principle 的違規情形。


有幾個原因，您可能需要修改您的應用程式。 您可能需要將您的應用程式的新功能，您可能需要修正您的應用程式中的錯誤或您可能需要修改您的應用程式的一項功能的實作方式。 應用程式很少是靜態。 它們通常會成長，並隨著時間變動。

例如，假設您決定要變更您的資料存取層的實作方式。 權限現在，連絡人管理員應用程式會使用 Microsoft Entity Framework 來存取資料庫。 不過，您可能會決定要移轉至新的或替代的資料存取技術，例如 ADO.NET Data Services 或 NHibernate。 不過，因為資料存取程式碼不是分開的驗證和控制站的程式碼，就無法修改資料存取程式碼，在您的應用程式，而不需要修改與資料存取不直接相關的其他程式碼。

當鬆散偶合的應用程式時，相反地，您可以變更應用程式的某一部分但沒有碰觸的應用程式其他部分。 例如，您可以切換資料存取技術，而不需要修改您驗證或控制站的邏輯。

在此反覆項目，我們利用數種軟體設計模式，讓我們重構我們的連絡人管理員應用程式，到更鬆散偶合的應用程式。 當我們完成之後時，贏得 t 的連絡人管理員執行任何動作，它之前執行此嘛 t。 不過，我們可以變更在未來更輕鬆地在應用程式。

> [!NOTE] 
> 
> 重構是重寫的方式，它不會遺失任何現有的功能中的應用程式的程序。


## <a name="using-the-repository-software-design-pattern"></a>使用存放庫的軟體設計模式

我們第一項變更是利用稱為 「 儲存機制模式的軟體設計模式。 若要找出我們的資料存取程式碼，我們的應用程式的其餘部分，我們將使用儲存機制模式。

實作儲存機制模式需要完成下列兩個步驟：

1. 建立介面
2. 建立會實作介面的具象類別

首先，我們需要建立可描述資料存取方法，我們需要執行的所有介面。 在 列表 1 中包含 IContactManagerRepository 介面。 此介面描述五種方法： CreateContact()、 DeleteContact()、 EditContact()、 GetContact 和 ListContacts()。

**列表 1-Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

接下來，我們需要建立可實作 IContactManagerRepository 介面的具象類別。 因為我們會使用 Microsoft Entity Framework 來存取資料庫，我們將建立名為 EntityContactManagerRepository 的新類別。 這個類別包含在 列表 2 中。

**列表 2-Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

請注意 EntityContactManagerRepository 類別會實作 IContactManagerRepository 介面。 此類別會實作全部五個該介面所描述的方法。

您可能會納悶為什麼需要擔心會有一個介面。 為什麼需要建立介面和實作它的類別？

有一個例外狀況，我們的應用程式的其餘部分互動的介面和不具象類別。 而不是呼叫 EntityContactManagerRepository 類別所公開的方法，我們將呼叫 IContactManagerRepository 介面所公開的方法。

如此一來，我們可以實作新類別的介面，而不需要修改應用程式的其餘部分。 例如，在未來的某個日期，我們可能會想要實作 DataServicesContactManagerRepository 類別可實作 IContactManagerRepository 介面。 DataServicesContactManagerRepository 類別可能會使用 ADO.NET Data Services 來存取資料庫，而不是 Microsoft Entity Framework。

如果我們的應用程式程式碼針對 IContactManagerRepository 介面，而不是具體的 EntityContactManagerRepository 類別所編寫然後我們就可以切換具象類別而不需修改任何程式碼的其餘部分。 比方說，我們可以從切換 EntityContactManagerRepository 類別 DataServicesContactManagerRepository 類別而不需修改我們的資料存取或驗證邏輯。

針對介面 （抽象） 而不是具象類別進行程式設計可讓您更有彈性，若要變更應用程式。

> [!NOTE] 
> 
> 您可以選取功能表選項重構，擷取介面，即可從 Visual Studio 內的具象類別中快速建立介面。 比方說，您可以先建立 EntityContactManagerRepository 類別，，然後使用自動產生 IContactManagerRepository 介面的 擷取介面。


## <a name="using-the-dependency-injection-software-design-pattern"></a>使用相依性插入軟體設計模式

既然我們已移轉資料存取程式碼的不同的儲存機制類別，我們需要修改連絡人控制器使用這個類別。 我們會利用呼叫控制器中使用的儲存機制類別的相依性插入軟體設計模式。

列表 3 包含修改過的連絡人控制器。

**列表 3-Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

請注意，請連絡控制器，列表 3 中的有兩個建構函式。 第一個建構函式會將 IContactManagerRepository 介面的具象執行個體傳遞給第二個建構函式。 請連絡控制器類別會使用*建構函式相依性插入*。

以及用於 EntityContactManagerRepository 類別的地方是在第一個建構函式。 類別的其餘部分會使用 IContactManagerRepository 介面，而不是具體的 EntityContactManagerRepository 類別。

這可讓您輕鬆地在未來切換 IContactManagerRepository 類別的實作。 如果您想要使用 DataServicesContactRepository 類別，而不是 EntityContactManagerRepository 類別，只要修改第一個建構函式。

建構函式相依性插入也可以連絡控制器類別非常測試。 在您的單元測試，您可以具現化連絡人控制器傳遞 IContactManagerRepository 類別的模擬 （mock） 實作。 當我們建立的連絡人管理員應用程式的單元測試時，這項功能的相依性插入會對我們的下一個反覆項目中非常重要。

> [!NOTE] 
> 
> 如果您想要完全解除 IContactManagerRepository 介面的特定實作的連絡人控制器類別然後您可以利用架構，可支援例如 StructureMap 或 Microsoft 的相依性插入Entity Framework (MEF)。 利用相依性插入架構，您永遠不需要參考程式碼中的具象類別。


## <a name="creating-a-service-layer"></a>建立服務層

您可能已經注意到，我們將驗證邏輯仍然混用我們列表 3 中修改過的控制器類別中的控制器邏輯。 基於相同理由，最好在其中找出我們的資料存取邏輯，它是個不錯的主意，找出我們的驗證邏輯。

若要修正此問題，我們可以建立個別[*服務層*](http://martinfowler.com/eaaCatalog/serviceLayer.html)。 服務層是獨立的圖層，我們可以將插入之間，我們的控制器和儲存機制類別。 服務層包含商務邏輯包括所有我們的驗證邏輯。

ContactManagerService 都包含在 列表 4 中。 其中包含驗證邏輯，從連絡人控制器類別。

**列表 4-Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

請注意 ContactManagerService 的建構函式要求 ValidationDictionary。 服務層會與透過此 ValidationDictionary 控制器層進行通訊。 當我們討論的裝飾項目模式時，我們會討論在下一節中詳細 ValidationDictionary。

此外，請注意，ContactManagerService 實作 IContactManagerService 介面。 您應該竭盡所能針對介面，而不是具象類別進行程式設計。 請連絡系統管理員應用程式中的其他類別不直接互動 ContactManagerService 類別。 相反地，有一個例外狀況，連絡人管理員應用程式的其餘部分會撰寫程式 IContactManagerService 介面。

列表 5 包含 IContactManagerService 介面。

**列表 5-Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

列表 6 會包含已修改的連絡人控制器類別。 請注意連絡人控制器不會再與 ContactManager 存放庫互動。 相反地，請連絡控制器與 ContactManager 服務互動。 每個圖層隔離盡量從其他圖層。

**列表 6-Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

我們的應用程式不會再執行進攻的單一功能原則 (SRP)。 連絡人控制器列表 6 中的已移除的每個責任以外控制工作流程的應用程式執行。 所有驗證邏輯已移除從連絡人控制器，並推送至服務層。 所有的資料庫邏輯已推送至儲存機制層。

## <a name="using-the-decorator-pattern"></a>使用裝飾項目模式

我們想要能夠完全分離我們的服務層，從我們的控制器層級。 基本上，我們應該可以編譯我們的服務層，從我們的控制器層級的個別組件，而不需要將參考加入至 MVC 應用程式。

不過，我們的服務層必須能夠將驗證錯誤訊息傳回控制器層級。 我們要如何讓通訊而不需要結合程度的控制站和服務層級的驗證錯誤訊息的服務層？ 我們可以利用名為軟體設計模式[裝飾項目模式](http://en.wikipedia.org/wiki/Decorator_pattern)。

控制器會使用名為 ModelState ModelStateDictionary，表示驗證錯誤。 因此，您可能想要從控制器層級的 ModelState 傳遞至服務層。 不過，在服務層中使用 ModelState 會讓您的服務層相依於 ASP.NET MVC framework 的功能。 這會是不正確，因為您總有一天，您可能想要使用 WPF 應用程式，而不是 ASP.NET MVC 應用程式的服務層。 在此情況下，您不想要參考 ASP.NET MVC 架構使用 ModelStateDictionary 類別。

裝飾項目模式可讓您包裝現有類別的新的類別中，若要實作的介面。 我們的連絡人管理員專案包含 ModelStateWrapper 所包含的類別中列表 7。 ModelStateWrapper 類別會實作介面，在 列表 8。

**列表 7-Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**列表 8-Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

如果您仔細看一下 列表 5 會看到 ContactManager 服務層會以獨佔方式使用 IValidationDictionary 介面。 ContactManager 服務不相依於 ModelStateDictionary 類別。 當連絡人控制器建立 ContactManager 服務時，控制器會包裝其 ModelState 像這樣：

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>總結

在此反覆項目，我們沒有新增任何新功能到連絡人管理員應用程式。 這個反覆項目的已重構連絡人管理員應用程式，因此也就是容易維護及修改。

首先，我們會實作儲存機制的軟體設計模式。 我們可以移轉所有的資料存取程式碼的不同 ContactManager 儲存機制類別。

我們也會從我們的控制器邏輯隔離我們的驗證邏輯。 我們會建立個別的服務層，其中包含所有的驗證程式碼。 在控制器層互動的服務層，並與儲存機制層互動的服務層。

當我們建立的服務層時，我們利用從我們的服務層隔離 ModelState 的裝飾項目模式。 在我們的服務層，我們會撰寫程式而不是 ModelState IValidationDictionary 介面。

最後，我們利用一種軟體設計模式，名為相依性插入模式。 此模式可讓我們將針對介面 （抽象） 而不是具象類別進行程式設計。 實作相依性插入設計模式也可讓我們的程式碼進行測試。 中的下一個反覆項目中，我們會將單元測試加入我們的專案。

> [!div class="step-by-step"]
> [上一頁](iteration-3-add-form-validation-cs.md)
> [下一頁](iteration-5-create-unit-tests-cs.md)
