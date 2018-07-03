---
uid: single-page-application/overview/introduction/other-libraries
title: 知道 Knockout 以外的程式庫？ | Microsoft Docs
author: madskristensen
description: 知道 Knockout 以外的程式庫？
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 0424d209cbd24756d1a840788bb3dc5b48d905ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375236"
---
<a name="know-a-library-other-than-knockout"></a>知道 Knockout 以外的程式庫？
====================
藉由[Mads Kristensen](https://github.com/madskristensen)

[單一頁面應用程式 (SPA) 範本](knockoutjs-template.md)是若要開始撰寫單一頁面應用程式的好方法。 此範本會使用[KnockoutJS](http://knockoutjs.com/)應用程式資料繫結至 DOM 項目。

但 Knockout 不是唯一的 JavaScript 程式庫，來建立豐富型用戶端應用程式。 其他程式庫會以不同的方式解決類似的挑戰。 讓我們了數個社群建立的範本可供下載，您可能會偏好透過另一個程式庫。 每個範本會使用不同的用戶端 JavaScript 程式庫組合。

若要安裝的社群建立的範本，請造訪的範本頁面下面所列，然後按一下 [下載] 按鈕。 提供的範本為 VSIX 檔案。

## <a name="backbonejs"></a>BackboneJS

[Backbone.js SPA 範本](../templates/backbonejs-template.md)。 此範本會提供初始的基本架構來開發[Backbone.js](http://backbonejs.org/) ASP.NET MVC 應用程式。 根據預設，它會提供基本的使用者登入功能，包括使用者註冊、 登入的密碼重設，以及使用基本的電子郵件範本的使用者確認。

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)是用於管理豐富的資料，JavaScript 用戶端中的開放原始碼程式庫。 幫助您輕鬆處理查詢、 快取、 變更追蹤、 驗證，等等。 兩個範本功能幫助您輕鬆：

- [Breeze/Knockout](../templates/breezeknockout-template.md)範本延伸 Knockout SPA 範本中，顯示您可以在如何輕鬆地建置單一頁面應用程式以幫助您輕鬆的資料管理和 KnockoutJS 進行資料繫結。
- [Breeze/Angular](../templates/breezeangular-template.md)範本也會擴充變得輕而易舉，但是使用 Knockout SPA 範本[AngularJS](http://angularjs.org)適用於資料繫結、 相依性插入和畫面管理程式庫。

颾魤 ㄛ[熱毛巾 SPA 範本](../templates/hottowel-template.md)使用 BreezeJS。

## <a name="emberjs"></a>EmberJS

[EmberJS SPA 範本](../templates/emberjs-template.md)。 此範本使用[Ember](http://emberjs.com/)，功能強大的 MVC JavaScript 程式庫，可解決各式各樣的建置豐富型用戶端應用程式的挑戰。

Ember SPA 範本是 Knockout SPA 範本中，使用 EmberJS 和 Handlebars 樣板化的重新實作。

## <a name="hot-towel"></a>最忙碌的毛巾

[最忙碌的毛巾 SPA 範本](../templates/hottowel-template.md)。 此範本會在數個 JavaScript 程式庫，包括幫助您輕鬆、 Knockout、 RequireJS 和 Twitter Bootstrap。

最忙碌的毛巾 teample 相較於此處所列的其他範本，提供更完整的應用程式，您可以用來建置您自己。 有多個概念要注意的但是一旦您了解它們，此範本可能只是您要尋找。 如果您想要建置 SPA，但無法決定在何處開始，請使用 最忙碌的毛巾以秒為單位，您將會有 SPA 和所有工具，而且您需要在其上建置。

## <a name="feature-table"></a>功能資料表

以下是每個 SPA 範本所提供的功能：


|                        | ASP.NET SPA | 骨幹 | Breeze/Angular | Breeze/KO |  Ember   | 最忙碌的毛巾 |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      待辦事項範例       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     裸機範本      |             | &#10003; |                |           |          | &#10003;  |
| 瀏覽和歷程記錄 |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        庫        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         幫助您輕鬆         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

