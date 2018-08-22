---
uid: single-page-application/overview/templates/breezeangular-template
title: Breeze/Angular 範本 |Microsoft Docs
author: madskristensen
description: Breeze/Angular 單一頁面應用程式範本
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: a3e8b42cdadf99df6971a278834b1429e129ce72
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825160"
---
<a name="breezeangular-template"></a>Breeze/Angular 範本
====================
藉由[Mads Kristensen](https://github.com/madskristensen)

> Breeze/Angular MVC 範本所編寫的 Ward Bell
> 
> [Breeze/Angular 的 MVC 範本下載](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org)是來自 Google 的開放原始碼程式庫來建置單一頁面應用程式 (Spa)。 它提供資料繫結、 相依性插入和畫面管理。 搭配使用[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)，適用於資料模型化和資料管理，而您的另一個開放原始碼程式庫有很棒的 HTML/JavaScript 用戶端應用程式不可或缺的要素。

Breeze/Angular SPA 範本上是一種變化[KnockoutJS SPA 範本](../introduction/knockoutjs-template.md)包含在 ASP.NET 和 Web 工具 2012.2 Update。 如果您有 Visual Studio，您必須範例 SPA 啟動並執行在少於 60 秒。

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

面向外部，應用程式看起來非常類似 KnockoutJS SPA 範本。 但實際上相當不同。 KnockoutJS 範本使用 Knockout 來資料繫結和資料存取的未經處理的 AJAX。 Breeze/Angular 範本會使用 Angular 資料繫結和幫助您輕鬆進行資料存取。 這些庫啟用額外的功能，包括頁面導覽和歷程記錄。

以下是應用程式的 About 頁面：

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

此頁面會顯示執行記錄檔的事件，在目前的使用者工作階段，包括：

- 分頁。 請注意在 #2 與快照 #7 在 Todo 控制站建立。
- 遠端查詢 (3) 和本機快取查詢 (#7)。
- 儲存新的 （#5，6），修改 (#4) 的實體。
- 驗證用戶端 (#9)，讓使用者可以更正錯誤，然後再認可變更到資料庫的變更。

沒有進一步探索此範本中，包括：

- 動態 HTML 檢視範本的方式載入。
- 自訂資料繫結，透過 Angular"指示詞。 」
- 模組化和相依性插入。
- 查詢篩選、 排序、 分頁、 投影和包含相關實體。
- 多個畫面間的資料共用。
- 當做單一交易中儲存多個變更。
- 驗證規則會自動從伺服器傳送至 JavaScript 用戶端。

我們現在就開始吧。

## <a name="create-a-breezeangular-template-project"></a>建立 Breeze/Angular 範本專案

下載並安裝的範本，按一下上方的 [下載] 按鈕。 範本會封裝成 Visual Studio 擴充功能 (VSIX) 檔案。 您可能需要重新啟動 Visual Studio。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Web**。 在專案範本清單中，選取**ASP.NET MVC 4 Web 應用程式**。 將專案命名，然後按一下**確定**。

在 **新的專案**精靈中，選取**幫助您輕鬆 Angular SPA**。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

按 Ctrl-F5 以建置並執行應用程式，但不偵錯，或按下 F5 以執行並偵錯。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- 按一下右上方，若要查看這些活動的記錄檔上的 「 關於 」 連結。

驗證邏輯是由幫助您輕鬆的執行的用戶端。 在伺服器的模型類別上的驗證屬性會傳播至用戶端，並自動執行前用戶端會聯繫伺服器。

檢閱網路流量。 請注意，任何對伺服器的呼叫時沒有幫助您輕鬆偵測到錯誤。 每個有效的變更會導致 POST 要求為"/ api/Todo/SaveChanges"。 幫助您輕鬆組合所做的變更並將其傳送一起做為單一要求 Web API 控制器的`SaveChanges`方法。 這是從 KockoutJS SPA 範本，可讓 PUT、 POST 和 DELETE 要求每個項目的個別不同。

此外，請注意沒有網路流量切換 TodoList 之間，以及有關頁面時。 這是因為查詢限定於本機的幫助您輕鬆快取。

## <a name="peek-inside"></a>眼

此應用程式具有用戶端和伺服器端。 用戶端堆疊一些 HTML 和 （在 「 應用程式 」 資料夾中） 的應用程式 JavaScript 模組的組合加上組成第三方 JavaScript 程式庫 （在 「 指令碼 」 資料夾中）。

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

UI 架構會將檢視的 HTML widget 分開的控制器中支援的展示程式碼。 Angular 的資料繫結系統會協調，讓每個可以執行其工作，而不需要深入了解其他檢視和控制器。

控制器會要求要取得和儲存模型實體的資料內容。 資料內容會將大部分的工作變得輕而易舉，建構 JSON 查詢結果中的自我追蹤模型物件的委派。

伺服器端堆疊包含一些開發人員程式碼和三個主要.NET 程式庫： Web API、 Entity Framework 和 Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

基本架構是 KockoutJS SPA 範本相同。 不過，實作是更簡單： 已刪除的 Dto，和大部分的 Entity Framework 細節已委派給 Breeze.NET。

## <a name="next-steps"></a>後續步驟

我們建議您瀏覽程式碼，藉由指引[廣泛討論](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)的用戶端和伺服器堆疊，幫助您輕鬆網站上的。

您可以嘗試播放使用變得輕而易舉的用戶端查詢;加入一些篩選和排序。 您可以加入更多的模型屬性並取得端對端 SPA 開發更加了解多個實體。 自信的設計時，您可以卸除的 Todo 功能，並取代它們。

祝各位寫程式愉快！
