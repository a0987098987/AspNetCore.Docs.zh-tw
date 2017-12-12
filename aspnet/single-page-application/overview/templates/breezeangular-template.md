---
uid: single-page-application/overview/templates/breezeangular-template
title: "幫助您輕鬆/Angular 範本 |Microsoft 文件"
author: madskristensen
description: "幫助您輕鬆/Angular 單一網頁應用程式範本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a>幫助您輕鬆/Angular 範本
====================
由[Mads Kristensen](https://github.com/madskristensen)

> 幫助您輕鬆/Angular MVC 範本是由行政區鈴鐺寫入
> 
> [下載幫助您輕鬆/角度的 MVC 範本](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org)建置單一頁面應用程式 (SPAs) 會將來自 Google 的開放原始碼程式庫。 它提供資料繫結、 相依性插入和螢幕管理。 搭配[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)，另一個開放原始碼程式庫的資料模型和資料管理，且您有很棒的 HTML/JavaScript 用戶端應用程式的重要元素。

幫助您輕鬆/Angular SPA 範本上是一種變化[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web 工具 2012.2 更新。 如果您已有 Visual Studio，您必須範例 SPA 啟動並執行在少於 60 秒。

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

面向外部，應用程式看起來非常類似 KnockoutJS SPA 範本。 但實際上相當不同。 KnockoutJS 範本，Knockout 用於資料繫結與原始 AJAX 資料存取。 幫助您輕鬆/Angular 範本用於 Angular 資料繫結和幫助您輕鬆存取資料。 這些庫啟用額外的功能，包括頁面導覽和歷程記錄。

以下是應用程式的相關頁面：

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

此頁面會顯示目前的使用者工作階段期間的執行記錄檔事件包括：

- 分頁。 請注意在 #2 與快照 #7 Todo 控制站建立。
- 遠端查詢 (#3) 和本機快取查詢 (#7)。
- 儲存新的 （#5、 #6），修改 (#4) 的實體。
- 驗證用戶端 (#9)，讓使用者可以在認可變更之前，更正錯誤，資料庫的變更。

沒有進一步探索此範本中包括：

- 動態載入的 HTML 檢視範本。
- 自訂資料繫結，透過 Angular 「 指示詞。 」
- 在模組化和相依性插入式攻擊。
- 篩選、 排序、 分頁、 預測和包含相關實體的查詢。
- 跨多個螢幕共用資料。
- 將多個變更儲存為單一交易。
- 驗證規則會自動從伺服器傳送的 JavaScript 用戶端。

我們現在就開始吧。

## <a name="create-a-breezeangular-template-project"></a>建立幫助您輕鬆/角度範本專案

下載並安裝的範本，按一下下載按鈕上方。 範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。 您可能需要重新啟動 Visual Studio。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。 為專案名稱，然後按一下**確定**。

在**新專案**精靈中，選取**幫助您輕鬆角度 SPA**。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

按 Ctrl + F5 以建置並執行應用程式，但不偵錯，或按 f5 開始執行並偵錯。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

當第一次執行應用程式時，它會顯示登入畫面。 按一下 「 註冊 」 連結，新的頁面 glides 到檢視，您可以在其中輸入使用者名稱和密碼。 （登入和註冊網頁會建置使用 ASP.NET MVC）。當您提交註冊表單時，伺服器會產生 TodoList 與您帳戶的兩個項目。 然後它呈現給您一個黃色的提示。

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

現在您已在土地的 SPA。 所有項目您會看到及體驗時操作 Todos 轉譯，而且藉助 Knockout 和輕而易舉的用戶端上管理。 探索應用程式的使用者身分... 但與開發人員的紅眼。 使用瀏覽器中的開發人員工具，來擷取網路流量。 (在 Internet Explorer： 按 F12，選取**網路**索引標籤，然後按一下**開始擷取**。)現在請嘗試下列各項：

- 加入新的 Todo 項目。
- 按一下標籤，然後編輯 Todo 項目標題
- 請檢查的核取方塊，來標記項目完成。 請注意，文字方塊將會停用，因此不再是可編輯的標題。
- 按一下右邊的標籤 'x'。 項目就會消失，並且從資料庫中刪除。
- 挑選另一個項目，並清除其標題。 您會取得的標題是必要的驗證錯誤。 短暫的暫停之後, 就會還原先前的標題。
- 輸入將為非常長的標題。 您會取得不同的驗證錯誤的標題是太長。
- 按一下 加入 Todo 清單 」 按鈕。 上述清單的左邊會出現新的清單。
- 播放以 TodoList 標題，觸發其必要和長度驗證。
- 按一下 [標題] 文字方塊中，若要清除的錯誤訊息中。
- 按一下右上角刪除 TodoList 和其 todos 圓形中的"x"。
- 按一下右上方，以查看這些活動的記錄檔的 「 關於 」 連結。

驗證邏輯是由輕而易舉的執行的用戶端。 伺服器模型類別上的驗證屬性傳播至用戶端，而且用戶端連絡伺服器之前，自動執行。

檢閱網路流量。 請注意，沒有呼叫伺服器時沒有幫助您輕鬆偵測到錯誤。 每個有效的變更會導致 POST 要求以"/ api/Todo/SaveChanges"。 幫助您輕鬆包裹在一起所做的變更並將它們傳送一起做為單一要求至 Web API 控制器`SaveChanges`方法。 這不同於 KockoutJS SPA 範本，可讓 PUT、 POST 和個別刪除要求每個項目。

另外而且請注意沒有網路流量時沒有您切換 TodoList 之間，以及有關頁面。 這是因為查詢已被限於本機幫助您輕鬆快取。

## <a name="peek-inside"></a>查看內部

此應用程式具有用戶端和伺服器端。 用戶端堆疊包含極小的 HTML 和 （在 「 應用程式 」 資料夾中） 的應用程式 JavaScript 模組的組合以及協力廠商的 JavaScript 程式庫 （在 「 指令碼 」 資料夾中）。

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

UI 架構分隔檢視的 HTML 的 widget 支援簡報中的程式碼控制站。 角度的資料繫結系統協調檢視和控制器，讓每個可以執行其他相當深不知情的情況下其工作。

控制器會要求來取得和儲存模型實體的資料內容。 資料內容會委派十分簡單，建構自我追蹤模型物件從 JSON 查詢結果來工作的絕大部分。

伺服器端堆疊包含某些開發人員程式碼和三個原則.NET 程式庫： Web API、 Entity Framework 和 Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

基本架構時 KockoutJS SPA 範本相同。 不過，實作會更簡單： DTOs 已刪除，而且大部分的 Entity Framework 的詳細資料已委派給 Breeze.NET。

## <a name="next-steps"></a>後續步驟

我們建議您瀏覽程式碼中，由[廣泛的討論](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)用戶端和伺服器堆疊，幫助您輕鬆網站上的。

您可能會嘗試播放使用輕而易舉的用戶端查詢;加入一些篩選和排序。 您可以加入更多的模型屬性，以取得更佳的感覺端對端 SPA 程式開發的多個實體。 當您確信設計的時您可以卸除 Todo 功能，並取代它們。

祝各位寫程式愉快！
