---
uid: single-page-application/overview/templates/backbonejs-template
title: Backbone 範本 |Microsoft Docs
author: madskristensen
description: Backbone.js SPA 範本
ms.author: aspnetcontent
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: f483a7ef840b7bce71187fd929e9667ee1eaf053
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811000"
---
<a name="backbone-template"></a>Backbone 範本
====================
藉由[Mads Kristensen](https://github.com/madskristensen)

> Kazi Manzur Rashid 寫入骨幹 SPA 範本
> 
> [下載 Backbone.js SPA 範本](https://go.microsoft.com/fwlink/?LinkId=293631)


Backbone.js SPA 範本設計來協助您開始快速建置互動式用戶端 web 應用程式使用[Backbone.js。](http://backbonejs.org/)

此範本會針對開發 ASP.NET MVC 中的 Backbone.js 應用程式提供初始的基本架構。 根據預設，它會提供基本的使用者登入功能，包括使用者註冊、 登入的密碼重設，以及使用基本的電子郵件範本的使用者確認。

需求：

- [ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>建立中樞範本專案

下載並安裝的範本，按一下上方的 [下載] 按鈕。 範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。 您可能需要重新啟動 Visual Studio。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名，然後按一下**確定**。

![](backbonejs-template/_static/image1.png)

在 [**新的專案**] 精靈，選取 Backbone.js SPA 專案。

![](backbonejs-template/_static/image2.png)

按 Ctrl-F5 以建置並執行應用程式，但不偵錯，或按下 F5 以執行並偵錯。

![](backbonejs-template/_static/image3.png)

按一下 [我的帳戶] 會顯示登入頁面：

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>逐步解說： 用戶端程式碼

讓我們開始在用戶端。 用戶端應用程式指令碼位於 ~/Scripts/application 資料夾中。 應用程式以[TypeScript](http://www.typescriptlang.org/) （.ts 檔案） 這會編譯成 JavaScript （.js 檔案）。

**應用程式**

`Application` 定義於 application.ts。 此物件會初始化應用程式，並做為根命名空間。 它會維護組態和狀態會在應用程式之間共用的資訊，例如是否將使用者登入。

`application.start`方法建立強制回應的檢視，並將附加事件處理常式的應用程式層級的事件，例如使用者登入。 接下來，它會建立預設路由器，並檢查是否已指定任何用戶端的 URL。 如果不是，它會重新導向至預設的 url (# ！ /)。

**事件**

開發鬆散聯繫元件時，是很重要的事件。 應用程式通常會執行多個作業以回應使用者動作。 骨幹提供內建事件的元件，例如模型、 集合和檢視。 而不是建立這些元件間的交互相依性，此範本會使用"pub/sub"模型： `events` events.ts 中, 定義的物件做為事件中樞以供發佈和訂閱的應用程式事件。 `events`物件是單一值。 下列程式碼示範如何訂閱事件，然後再觸發事件：

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**路由器**

在 Backbone.js，路由器會提供方法讓路由用戶端頁面並將它們連接至動作和事件。 範本會定義單一的路由器，router.ts 中。 路由器會建立 activable 檢視表，並切換檢視時，會維護狀態。 （下一節會描述 activable 檢視）。專案一開始，有兩個虛擬的檢視，家用以及有關。 它也有找不到檢視中，如果不知道路由就會顯示。

**檢視**

~/Scripts/application/檢視表中定義的檢視。 有兩種類型的檢視、 activable 檢視和強制回應對話方塊檢視。 路由器會叫用 activable 的檢視。 當 activable 檢視顯示時，所有其他 activable 檢視就會變成非使用中。 若要建立 activable 的檢視，延伸檢視`Activable`物件：

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

使用擴充`Activable`將兩個新方法加入至檢視中，`activate`和`deactivate`。 路由器會呼叫這些方法來啟用和停用仍檢視。

強制回應檢視會實作為[Twitter Bootstrap](http://twitter.github.com/bootstrap/)強制回應對話方塊。 `Membership`和`Profile`檢視是強制回應的檢視。 模型檢視可以叫用任何應用程式事件。 例如，在`Navigation`檢視中，按一下 [我的帳戶] 連結會顯示其中一個`Membership`檢視或`Profile`檢視中的，根據使用者是否登入。 `Navigation`將 click 事件處理常式，有任何子項目`data-command`屬性。 以下是 HTML 標記：

[!code-html[Main](backbonejs-template/samples/sample3.html)]

以下是 navigation.ts 來連結事件中的程式碼：

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**模型**

~/Scripts/application/模型中定義的模型。 這些模型都具有三個基本項目： 預設屬性，驗證規則和伺服器端結束點。 以下是典型的範例：

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**外掛程式**

~/Scripts/application/lib 資料夾包含一些好用的 jQuery 外掛程式。Form.ts 檔案會定義外掛程式，可使用的表單資料。 通常您要序列化或還原序列化表單資料，並顯示任何模型驗證錯誤。 具有方法，例如外掛程式 form.ts `serializeFields`， `deserializeFields`，和`showFieldErrors`。 下列範例示範如何序列化至模型的表單。

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

外掛程式 flashbar.ts 提供各種意見反應訊息給使用者。 方法會`$.showSuccessbar`，`$.showErrorbar`和`$.showInfobar`。 在幕後，它會使用 Twitter Bootstrap 警示顯示良好動畫的訊息。

外掛程式 confirm.ts 取代瀏覽器的確認對話方塊中，雖然 API 是有點不同：

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>逐步解說： 伺服端程式碼

現在讓我們看看伺服器端。

**控制器**

在單一頁面應用程式中，伺服器會扮演小型角色在使用者介面。 一般來說，伺服器會呈現初始網頁，然後傳送和接收 JSON 資料。

範本有兩個 MVC 控制器：`HomeController`呈現初始頁面，並`SupportsController`用來確認新的使用者帳戶和密碼重設。 在範本中的所有控制站是 ASP.NET Web API 控制器，傳送與接收 JSON 資料。 根據預設，新的控制器使用`WebSecurity`類別來執行與使用者相關的工作。 不過，他們也有選擇性建構函式，可讓您傳入委派來執行這些工作。 這可讓測試更為容易，並讓您取代`WebSecurity`與其他項目，使用 IoC 容器。 請看以下範例：

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>檢視

檢視設計為模組化： 頁面的每個區段都有自己專用的檢視。 在單一頁面應用程式中，常會包含沒有任何對應控制器的檢視。 您可以藉由呼叫包含檢視`@Html.Partial('myView')`，但這會取得單調乏味。 為了簡化起見，範本會定義協助程式方法， `IncludeClientViews`，呈現所有指定的資料夾中的檢視：

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

如果未指定資料夾名稱，預設的資料夾名稱會是"ClientViews 」。 如果您的用戶端檢視也會使用部分檢視，名稱以底線字元的部分檢視 (例如`_SignUp`)。 `IncludeClientViews`方法不包括其名稱開頭為底線的任何檢視。 若要併入用戶端檢視中的部分檢視，請呼叫`Html.ClientView('SignUp')`而不是`Html.Partial('_SignUp')`。

**傳送電子郵件**

若要傳送電子郵件，此範本會使用[郵遞](http://aboutcode.net/postal)。 不過，程式碼的其餘部分抽離郵遞`IMailer`介面，以便您可以使用另一個實作，輕鬆地取代它。 電子郵件範本位於 [檢視/電子郵件] 資料夾中。 寄件者的電子郵件地址在 web.config 檔案中，指定在`sender.email`的索引鍵**appSettings**一節。 也，當`debug="true"`在 web.config 中，應用程式不需要使用者電子郵件確認，加速開發。

## <a name="github"></a>GitHub

您也可以找到 Backbone.js SPA 範本上[GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa)。
