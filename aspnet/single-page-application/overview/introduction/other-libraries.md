---
uid: single-page-application/overview/introduction/other-libraries
title: 了解 Knockout 以外的程式庫？ | Microsoft Docs
author: madskristensen
description: 了解 Knockout 以外的程式庫？
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 6ac260e88fd156bad4b414e93325d5a04c490c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872581"
---
<a name="know-a-library-other-than-knockout"></a>了解 Knockout 以外的程式庫？
====================
由[Mads Kristensen](https://github.com/madskristensen)

[單一頁面應用程式 (SPA) 範本](knockoutjs-template.md)是若要開始撰寫單一頁面應用程式的好方法。 範本使用[KnockoutJS](http://knockoutjs.com/)將應用程式資料繫結至 DOM 項目。

但是 Knockout 不是只建立豐富型用戶端應用程式的 JavaScript 程式庫。 其他文件庫不同的方式解決類似的挑戰。 讓我們讓數個社群建立的範本可供下載，您可能會想透過另一個程式庫。 用戶端 JavaScript 程式庫不同混用每個範本。

若要安裝社群建立的範本，請造訪的範本頁面下面所列，然後按一下 [下載] 按鈕。 提供的範本為 VSIX 檔案。

## <a name="backbonejs"></a>BackboneJS

[Backbone.js SPA 範本](../templates/backbonejs-template.md)。 此範本來開發提供初始的基本架構[Backbone.js](http://backbonejs.org/)在 ASP.NET MVC 應用程式。 根據預設，它會提供基本的使用者登入功能，包括註冊、 登入、 重設使用者密碼，以及具有基本的電子郵件範本的使用者確認。

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)是用於管理豐富的資料，JavaScript 用戶端中的開放原始碼程式庫。 幫助您輕鬆處理查詢、 快取，變更追蹤、 驗證，等等。 兩個範本功能十分簡單：

- [幫助您輕鬆/Knockout](../templates/breezeknockout-template.md)範本延伸 Knockout SPA 範本，顯示如何輕鬆地您可以建立的單一頁面應用程式以幫助您輕鬆資料管理和 KnockoutJS 資料繫結。
- [幫助您輕鬆/Angular](../templates/breezeangular-template.md)範本也會擴充十分簡單，但使用 Knockout SPA 範本[AngularJS](http://angularjs.org)適用於資料繫結、 相依性插入和螢幕管理程式庫。

此外，[熱毛巾 SPA 範本](../templates/hottowel-template.md)使用 BreezeJS。

## <a name="emberjs"></a>EmberJS

[EmberJS SPA 範本](../templates/emberjs-template.md)。 此範本使用[Ember](http://emberjs.com/)、 功能強大的 MVC JavaScript 程式庫，可以解決各式各樣的挑戰，來建立豐富型用戶端應用程式。

Ember SPA 範本是使用 EmberJS 和 Handlebars 樣板化的 Knockout SPA 範本的重新實作。

## <a name="hot-towel"></a>熱毛巾

[熱毛巾 SPA 範本](../templates/hottowel-template.md)。 此範本會在數個的 JavaScript 程式庫，包括幫助您輕鬆、 Knockout、 RequireJS 和 Twitter 啟動程序。

熱毛巾 teample 相較於此處所列的其他範本，提供更完整的應用程式，從中您可以建立您自己。 有更多的概念，要注意的但是一旦您了解它們，此範本可能只是您要尋找。 如果您想要建置 SPA，但無法決定在何處開始，請使用熱毛巾以秒為單位，您會有 SPA 和所有工具，而且您需要在其上建置。

## <a name="feature-table"></a>在功能資料表

以下是每個 SPA 範本所提供的功能：


|                        | ASP.NET SPA | Backbone | 幫助您輕鬆/角度 | 幫助您輕鬆/KO |  Ember   | 熱毛巾 |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      ToDo 範例       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     裸機範本      |             | &#10003; |                |           |          | &#10003;  |
| 瀏覽和歷程記錄 |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        庫        |             |          |                |           |          |           |
|        角度         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         幫助您輕鬆         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

