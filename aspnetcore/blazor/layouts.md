---
title: ASP.NET Core Blazor 版面配置
author: guardrex
description: 瞭解如何為 Blazor 應用程式建立可重複使用的版面配置元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/layouts
ms.openlocfilehash: 5b6e1c7ceb4a6e41230e31bbe379bde1bb0a8286
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447135"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a>ASP.NET Core Blazor 版面配置

By [Rainer Stropek](https://www.timecockpit.com)和[Luke Latham](https://github.com/guardrex)

某些應用程式專案（例如功能表、著作權訊息和公司標誌）通常是應用程式整體版面配置的一部分，並由應用程式中的每個元件所使用。 將這些專案的程式碼複製到應用程式的所有元件，並不是有效率的方法&mdash;每次元素需要更新時，每個元件都必須更新。 這種複製很容易維護，而且可能會在一段時間後導致不一致的內容。 *版面*配置會解決此問題。

就技術上而言，版面配置只是另一個元件。 版面配置定義于 Razor 範本或程式碼中C# ，而且可以使用[資料](xref:blazor/data-binding)系結、相依性[插入](xref:blazor/dependency-injection)和其他元件案例。

若要將*元件*轉換成*版面*配置，元件：

* 繼承自 `LayoutComponentBase`，其定義配置內轉譯內容的 `Body` 屬性。
* 會使用 Razor 語法 `@Body`，在版面配置標記中，指定轉譯內容的位置。

下列程式碼範例顯示*MainLayout*版面配置元件的 razor 範本。 版面配置會繼承 `LayoutComponentBase` 並設定導覽列和頁尾之間的 `@Body`：

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

在以其中一個 Blazor 應用程式範本為基礎的應用程式中，`MainLayout` 元件（*MainLayout*）位於應用程式的*共用*資料夾中。

## <a name="default-layout"></a>預設版面配置

在應用程式的*razor*檔案中，于 `Router` 元件中指定預設的應用程式佈建。 下列 `Router` 元件（由預設 Blazor 範本提供）會將預設版面配置設定為 `MainLayout` 元件：

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

若要提供 `NotFound` 內容的預設版面配置，請指定 `NotFound` 內容的 `LayoutView`：

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

如需 `Router` 元件的詳細資訊，請參閱 <xref:blazor/routing>。

將配置指定為路由器中的預設版面配置是很有用的作法，因為它可以根據每個元件或每個資料夾來覆寫。 偏好使用路由器來設定應用程式的預設版面配置，因為這是最常見的技術。

## <a name="specify-a-layout-in-a-component"></a>在元件中指定版面配置

使用 Razor 指示詞 `@layout` 將版面配置套用至元件。 編譯器會將 `@layout` 轉換成 `LayoutAttribute`，這會套用至元件類別。

下列 `MasterList` 元件的內容會插入至 `MasterLayout` 的 `@Body`位置：

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

直接在元件中指定版面配置會覆寫路由器中的*預設*組態集，或從 *_Imports*匯入的 `@layout` 指示詞。

## <a name="centralized-layout-selection"></a>集中式版面配置選取

應用程式的每個資料夾都可以選擇性地包含名為 *_Imports*的範本檔案。 編譯器包含相同資料夾中所有 Razor 範本的匯入檔案中所指定的指示詞，並且會以遞迴方式在其所有子資料夾中進行。 因此，包含 `@layout MyCoolLayout` 的 *_Imports razor*檔案可確保資料夾中的所有元件都使用 `MyCoolLayout`。 不需要重複將 `@layout MyCoolLayout` 新增至資料夾和子資料夾內的所有*razor*檔案。 `@using` 指示詞也會以同樣的方式套用至元件。

下列 *_Imports razor*檔案匯入：

* `MyCoolLayout`第 1 課：建立 Windows Azure 儲存體物件{2}。
* 相同資料夾和任何子資料夾中的所有 Razor 元件。
* `BlazorApp1.Data` 命名空間。
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

*_Imports razor*檔案類似[razor 視圖和頁面的 _ViewImports. cshtml](xref:mvc/views/layout#importing-shared-directives)檔案，但特別適用于 razor 元件檔案。

在 *_Imports*中指定版面配置，會覆寫指定為路由器*預設版面*配置的版面配置。

## <a name="nested-layouts"></a>嵌套版面配置

應用程式可以包含嵌套的版面配置。 元件可以參考配置，而該配置會轉而參考另一個版面配置。 例如，用來建立多層級功能表結構的嵌套配置。

下列範例顯示如何使用嵌套的配置。 *EpisodesComponent. razor*檔案是要顯示的元件。 元件會參考 `MasterListLayout`：

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

*MasterListLayout razor*檔案提供 `MasterListLayout`。 版面配置會參考另一個版面配置，也就是其轉譯位置的 `MasterLayout`。 `EpisodesComponent` 會在 `@Body` 出現的位置呈現：

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

最後，在*MasterLayout*中 `MasterLayout` 包含最上層的版面配置元素，例如頁首、主功能表和頁尾。 `EpisodesComponent` 的 `MasterListLayout` 會在 `@Body` 出現的位置呈現：

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a>與整合式元件共用 Razor Pages 的版面配置

當可路由的元件整合到 Razor Pages 應用程式時，應用程式的共用配置可以與元件搭配使用。 如需詳細資訊，請參閱 <xref:blazor/integrate-components>。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/layout>
