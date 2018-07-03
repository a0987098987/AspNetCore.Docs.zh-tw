---
uid: single-page-application/overview/templates/breezeknockout-template
title: Breeze/Knockout 範本 |Microsoft Docs
author: madskristensen
description: Breeze/Knockout 單一頁面應用程式範本
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 48ee0463fe950c28832523986a2242417411c96a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376860"
---
<a name="breezeknockout-template"></a>Breeze/Knockout 範本
====================
藉由[Mads Kristensen](https://github.com/madskristensen)

> Breeze/Knockout MVC 範本所編寫的 Ward Bell
> 
> [下載 Breeze/Knockout MVC 範本](https://go.microsoft.com/fwlink/?LinkId=282649)


您聽過的 「 單一頁面應用程式 」 (SPA) 和想要知道它是什麼。 雖然您無法讀取其相關資訊，您就是它親身體驗。 但誰可以下載範例的時間？ 如果您有 Visual Studio，您必須的範例 SPA 和執行在低於 60 秒為單位 ASP.NET mvc 4 」 Breeze/Knockout 單一頁面應用程式 範本。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>何謂 Breeze/Knockout SPA 範本

大部分的專案範本產生應用程式基本架構。 這對於置於這些極簡，加上您的程式碼，並最後提供運作中應用程式。 Breeze/Knockout SPA 範本各不相同。 它會產生要研究的範例應用程式。 它會示範 SPA 應用程式的設計和許多技巧來建置 SPA。

Breeze/Knockout 範本上是一種變化[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web 工具 2012.2 Update。 幫助您輕鬆 SPA 範本會產生具有相同的使用者體驗的應用程式，但有不同的實作中，使用資料管理變得輕而易舉。

KnockoutJS SPA 範本可讓服務要求與原始 jQuery AJAX 中，這是適合簡單的應用程式。 但更複雜的應用程式有更高的資料管理需求。 例如，大部分的應用程式：

- 查詢並重新擴充的使用者工作階段期間查詢伺服器。
- 新增查詢篩選器、 排序和分頁。
- 跨多個畫面中共用相同的資料。
- 累積變更許多物件，然後將它們儲存為單一交易。
- 驗證用戶端上的變更，因此使用者可以更正錯誤，再認可變更到資料庫。

BreezeJS 程式庫會為您，您能夠將開發最重要的應用程式邏輯和使用者體驗，來處理這些例行工作。

[**Breeze** ](http://www.breezejs.com/?utm_source=ms-spa)是開放原始碼程式庫來建置豐富的資料應用程式，以 JavaScript 和 HTML，在過去已傳遞做為獨立的桌面應用程式的應用程式的類型。

Breeze/Knockout 範本可協助您執行這個重要的第一個步驟，向更穩固的資料管理基礎結構。 它會產生外表上等同於 KnockoutJS SPA 範本的範例待辦事項應用程式。 在內部，它會以幫助您輕鬆取代 AJAX 資料層，因此您可以比較兩個接近並排顯示。 當然，幾乎沒有碰觸到可能會變得輕而易舉應用程式。 但您會看到變得輕而易舉的運作方式和頻率，才可進行轉換。

我們現在就開始吧。

## <a name="create-a-breezeknockout-template-project"></a>建立 Breeze/Knockout 範本專案

下載並安裝的範本，按一下上方的 [下載] 按鈕。 範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。 您可能需要重新啟動 Visual Studio。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名，然後按一下**確定**。

在 **新的專案**精靈中，選取**幫助您輕鬆 Knockout SPA**。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

按 Ctrl-F5 以建置並執行應用程式，但不偵錯，或按下 F5 以執行並偵錯。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

當第一次執行應用程式時，它會顯示登入畫面。 按一下 「 註冊 」 連結，新的頁面 glides 到檢視中，您可以在其中輸入使用者名稱和密碼。 （登入] 和 [註冊頁面會建置使用 ASP.NET MVC）。當您提交註冊表單時，伺服器就會產生 TodoList 與您帳戶的兩個項目。 然後它將它們呈現給您在黃色便箋上。

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

現在您已經在 land 的 SPA。 所有項目您查看和體驗而操作 Todo 是呈現和管理的 Knockout 和 Breeze 協助用戶端上。 探索應用程式的使用者身分... 但是開發人員的注意。 使用瀏覽器中的開發人員工具，來擷取網路流量。 (在 Internet Explorer： 按 f12 鍵，選取**網路**索引標籤，然後按一下**開始擷取**。)現在請嘗試下列各項：

- 加入新的 Todo 項目。
- 按一下標籤，然後編輯待辦事項項目標題
- 核取方塊來標記項目完成。 請注意，已停用的文字方塊中，因此不再是可編輯的標題。
- 按一下 'x' 標籤的右邊。 項目會消失，而從資料庫中刪除。
- 挑選另一個項目，並清除它的標題。 您會收到標題是必要的驗證錯誤。 在短暫的暫停之後, 就會還原先前的標題。
- 輸入在非常長的標題。 您會收到不同的驗證錯誤，標題會太長。
- 按一下 「 新增待辦事項清單 」 按鈕。 左側的前一份清單會出現新的清單。
- 播放以 TodoList 標題，觸發其必要和長度驗證。
- 按一下以清除錯誤訊息的 [標題] 文字方塊中。
- 按一下 [x] 來刪除 TodoList 和其 todo 右上角圓圈。

驗證邏輯是由幫助您輕鬆的執行的用戶端。 在伺服器的模型類別上的驗證屬性會傳播至用戶端，並自動執行前用戶端會聯繫伺服器。

檢閱網路流量。 請注意，任何對伺服器的呼叫時沒有幫助您輕鬆偵測到錯誤。 每個有效的變更會導致 POST 要求為"/ api/Todo/SaveChanges"。 幫助您輕鬆組合所做的變更並將其傳送一起做為單一要求 Web API 控制器的`SaveChanges`方法。 這是從 KockoutJS SPA 範本，可讓 PUT、 POST 和 DELETE 要求每個項目的個別不同。

## <a name="peek-inside"></a>眼

此應用程式具有用戶端和伺服器端。 用戶端堆疊一些 HTML 和 （在 「 應用程式 」 資料夾中） 的應用程式 JavaScript 模組的組合加上組成第三方 JavaScript 程式庫 （在 「 指令碼 」 資料夾中）。

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

如果您已調查 KnockoutJS SPA 範本，這應該看起來很熟悉。 焦點放在藍色方塊。 UI 架構是 Model View ViewModel (MVVM)，在其中檢視的 HTML widget 完全分開的檢視模型中支援的展示程式碼。 資料繫結系統 (在此情況下 Knockout) 協調的檢視和檢視模型，使每個可以執行其工作，而不需要深入了解其他。

模型封裝 Todo 資料。 模型中的實體會建構方式，幫助您輕鬆使用 Knockout 可觀察的屬性，讓它們可以直接繫結至檢視中的小工具。 檢視模型會要求要取得和儲存模型實體的資料內容。 資料內容委派 (delegate) 大部分的工作變得輕而易舉。

伺服器端堆疊包含一些開發人員程式碼和三個主要.NET 程式庫： Web API、 Entity Framework 和 Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

基本架構是 KockoutJS SPA 範本相同。 不過，實作是更簡單： 已刪除的 Dto，和大部分的 Entity Framework 細節已委派給 Breeze.NET。

## <a name="next-steps"></a>後續步驟

我們建議您瀏覽程式碼，藉由指引[廣泛討論](http://www.breezejs.com/spa-template?utm_source=ms-spa)的用戶端和伺服器堆疊，幫助您輕鬆網站上的。

您可以嘗試播放使用變得輕而易舉的用戶端查詢;加入一些篩選和排序。 您可以加入更多的模型屬性並取得端對端 SPA 開發更加了解多個實體。 自信的設計時，您可以卸除的 Todo 功能，並取代它們。

您很快就準備好大一步： 新增用戶端螢幕，並在它們之間瀏覽。 您將此 SPA 範本拋諸腦後，並開啟更完整的 SPA 堆疊中，這類[John Papa 的熱毛巾](https://github.com/johnpapa/HotTowel#readme "熱毛巾")，幫助您輕鬆與 Knockout 在混合加入 Durandal。
