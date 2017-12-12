---
uid: single-page-application/overview/templates/breezeknockout-template
title: "幫助您輕鬆/Knockout 範本 |Microsoft 文件"
author: madskristensen
description: "幫助您輕鬆/Knockout 單一網頁應用程式範本"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>幫助您輕鬆/Knockout 範本
====================
由[Mads Kristensen](https://github.com/madskristensen)

> 幫助您輕鬆/Knockout MVC 範本是由行政區鈴鐺寫入
> 
> [下載幫助您輕鬆/Knockout MVC 範本](https://go.microsoft.com/fwlink/?LinkId=282649)


聽說的 「 單一網頁應用程式 」 (SPA) 和不想知道它是什麼。 雖然您無法讀取其相關資訊，您會自行而遇到它。 但是，誰有時間，若要下載範例？ 如果您已有 Visual Studio，您將最多有範例 SPA 和執行在少於 60 秒為單位搭配 ASP.NET MVC 4 「 幫助您輕鬆/Knockout 單一頁面應用程式 」 範本。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>什麼是幫助您輕鬆/Knockout SPA 範本？

大部分的專案範本產生的應用程式的基本架構。 這些添 flesh 打加入您的程式碼，並最終傳遞應用程式運作。 幫助您輕鬆/Knockout SPA 範本會不同。 它會產生要研究的範例應用程式。 它會示範 SPA 應用程式設計和許多建置 SPA 的技術。

幫助您輕鬆/Knockout 範本上是一種變化[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web 工具 2012.2 更新。 幫助您輕鬆 SPA 範本會產生具有相同的使用者經驗的應用程式，但有不同的實作，幫助您輕鬆使用資料管理。

KnockoutJS SPA 範本可讓服務要求未經處理的 jQuery AJAX，即適合簡單的應用程式。 但更複雜的應用程式有較嚴苛的資料管理需求。 例如，大部分的應用程式：

- 查詢並重新擴充的使用者工作階段期間查詢伺服器。
- 加入查詢的篩選、 排序和分頁。
- 跨多個螢幕共用相同的資料。
- 累積變更許多物件，然後將它們儲存為單一交易。
- 驗證用戶端上的變更，因此使用者可以更正錯誤，再認可變更到資料庫。

BreezeJS 程式庫會為您釋出您開發最重要的應用程式邏輯和使用者體驗，來處理這些乏味的雜務。

[**幫助您輕鬆**](http://www.breezejs.com/?utm_source=ms-spa)是開放原始碼程式庫建置豐富的資料以 JavaScript 和 HTML 的應用程式在過去已傳遞做為獨立的桌面應用程式類型的應用程式。

幫助您輕鬆/Knockout 範本可協助您採取更穩固的資料管理基礎結構非常重要的首要步驟。 它會產生等同於面向外部 KnockoutJS SPA 範本範例待辦事項應用程式。 在內部，它會以幫助您輕鬆取代 AJAX 資料層，以便比較兩個方法來並行。 當然，幾乎沒有碰觸到可能會幫助您輕鬆的應用程式。 不過您看到輕而易舉的運作方式以及如何稍有需要，才能將該轉換。

我們現在就開始吧。

## <a name="create-a-breezeknockout-template-project"></a>建立幫助您輕鬆/Knockout 範本專案

下載並安裝的範本，按一下下載按鈕上方。 範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。 您可能需要重新啟動 Visual Studio。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。 在下**Visual C#**，選取**Web**。 在專案範本清單中選取**ASP.NET MVC 4 Web 應用程式**。 為專案名稱，然後按一下**確定**。

在**新專案**精靈中，選取**幫助您輕鬆 Knockout SPA**。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

按 Ctrl + F5 以建置並執行應用程式，但不偵錯，或按 f5 開始執行並偵錯。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

驗證邏輯是由輕而易舉的執行的用戶端。 伺服器模型類別上的驗證屬性傳播至用戶端，而且用戶端連絡伺服器之前，自動執行。

檢閱網路流量。 請注意，沒有呼叫伺服器時沒有幫助您輕鬆偵測到錯誤。 每個有效的變更會導致 POST 要求以"/ api/Todo/SaveChanges"。 幫助您輕鬆包裹在一起所做的變更並將它們傳送一起做為單一要求至 Web API 控制器`SaveChanges`方法。 這不同於 KockoutJS SPA 範本，可讓 PUT、 POST 和個別刪除要求每個項目。

## <a name="peek-inside"></a>查看內部

此應用程式具有用戶端和伺服器端。 用戶端堆疊包含極小的 HTML 和 （在 「 應用程式 」 資料夾中） 的應用程式 JavaScript 模組的組合以及協力廠商的 JavaScript 程式庫 （在 「 指令碼 」 資料夾中）。

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

如果您已經調查 KnockoutJS SPA 範本，這應該看起來很類似。 焦點放在藍色方塊。 UI 架構是模型-檢視-ViewModel (MVVM)，在其中檢視的 HTML widget 會完全支援簡報程式碼分開中檢視模型。 資料繫結系統 (在此情況下 Knockout) 協調的檢視和檢視模型，讓每個可以執行其工作的其他使用者不知情的情況下。

模型封裝 Todo 資料。 模型中的實體可以進行建構幫助您輕鬆使用 Knockout 可觀察的屬性，讓它們可以直接繫結至檢視中的 widget。 檢視模型會要求來取得和儲存模型實體的資料內容。 資料內容會委派來幫助您輕鬆工作的絕大部分。

伺服器端堆疊包含某些開發人員程式碼和三個原則.NET 程式庫： Web API、 Entity Framework 和 Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

基本架構時 KockoutJS SPA 範本相同。 不過，實作會更簡單： DTOs 已刪除，而且大部分的 Entity Framework 的詳細資料已委派給 Breeze.NET。

## <a name="next-steps"></a>後續步驟

我們建議您瀏覽程式碼中，由[廣泛的討論](http://www.breezejs.com/spa-template?utm_source=ms-spa)用戶端和伺服器堆疊，幫助您輕鬆網站上的。

您可能會嘗試播放使用輕而易舉的用戶端查詢;加入一些篩選和排序。 您可以加入更多的模型屬性，以取得更佳的感覺端對端 SPA 程式開發的多個實體。 當您確信設計的時您可以卸除 Todo 功能，並取代它們。

很快就可以輕鬆進行下一個大步驟： 加入用戶端螢幕，並在它們之間巡覽。 您將留下此 SPA 範本，並開啟選項，以更完整的 SPA 堆疊，例如[John Papa 熱毛巾](https://github.com/johnpapa/HotTowel#readme "熱毛巾")，這樣會將 Durandal 十分簡單和 Knockout 混合加入。
