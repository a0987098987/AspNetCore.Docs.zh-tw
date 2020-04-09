---
title: ASP.NET核心Blazor佈局
author: guardrex
description: 瞭解如何為應用創建可重用的Blazor佈局元件。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/layouts
ms.openlocfilehash: 5b6e1c7ceb4a6e41230e31bbe379bde1bb0a8286
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78660409"
---
# <a name="aspnet-core-opno-locblazor-layouts"></a>ASP.NET核心Blazor佈局

由[雷納·斯特羅佩克](https://www.timecockpit.com)和[盧克·萊瑟姆](https://github.com/guardrex)

某些應用元素(如功能表、版權消息和公司徽標)通常是應用整體佈局的一部分,並且由應用中的每個元件使用。 每次其中一個元素需要更新時,將這些元素的代碼複製到應用的所有元件中並不是一種有效&mdash;的方法,因此必須更新每個元件。 這種重複很難維護,並且隨著時間的推移可能導致內容不一致。 *佈局*解決了這個問題。

從技術上講,佈局只是另一個元件。 佈局在 Razor 樣本或 C# 代碼中定義,可以使用[資料綁定](xref:blazor/data-binding)、[依賴項注入](xref:blazor/dependency-injection)和其他元件方案。

要將*元件*轉換為*佈局*,該元件:

* 繼承,`LayoutComponentBase`該繼承定義佈局`Body`中 呈現的內容的屬性。
* 使用 Razor`@Body`語法指定呈現內容的布局標記中的位置。

以下代碼示例顯示了佈局元件*MainLayout.razor*的 Razor 範本。 佈局繼承`LayoutComponentBase`並設定導覽`@Body`列和 頁腳之間的設定:

[!code-razor[](layouts/sample_snapshot/3.x/MainLayout.razor?highlight=1,13)]

在Blazor基於其中一個應用樣本的應用中,`MainLayout`元件(*MainLayout.razor)* 位於應用的 *「共用」* 資料夾中。

## <a name="default-layout"></a>預設佈局

在應用的`Router`*App.razor*檔中的元件中指定預設應用佈局。 預設`Router`Blazor樣本提供的以下元件將預設佈局設定到`MainLayout`元件:

[!code-razor[](layouts/sample_snapshot/3.x/App1.razor?highlight=3)]

要為內容提供預設佈局`NotFound`,請`NotFound`為`LayoutView`內容指定 a:

[!code-razor[](layouts/sample_snapshot/3.x/App2.razor?highlight=6-9)]

有關元件的詳細資訊,`Router`請參閱。 <xref:blazor/routing>

在路由器中指定布局作為默認佈局是一種有用的做法,因為它可以按元件或每個資料夾覆蓋。 更喜歡使用路由器來設置應用的默認佈局,因為它是最通用的技術。

## <a name="specify-a-layout-in-a-component"></a>在元件中指定佈局

使用 Razor`@layout`指令將佈局應用於元件。 編譯器轉換`@layout`為套用元件`LayoutAttribute`類別類別的 。

以下`MasterList`元件的內容插入 :`MasterLayout`的位置`@Body`:

[!code-razor[](layouts/sample_snapshot/3.x/MasterList.razor?highlight=1)]

直接在元件中指定佈局將覆蓋路由器中的*默認佈局*集或`@layout`從 *_Imports.razor*導入的指令。

## <a name="centralized-layout-selection"></a>集中佈局選擇

應用的每個資料夾可以選擇包含名為 *_Imports.razor 的*範本檔。 編譯器包括導入檔中指定的指令,這些指令位於同一資料夾中的所有 Razor 範本中,並遞歸於其所有子資料夾中。 因此,`@layout MyCoolLayout`包含 *_Imports.razor*檔案可確保資料夾中的所有元件`MyCoolLayout`都使用 。 無需重複加入`@layout MyCoolLayout`資料夾或子資料夾中的所有 *.razor*檔案。 `@using`指令也以同樣的方式應用於元件。

以下 *_Imports.razor*檔案匯入:

* `MyCoolLayout`.
* 同一資料夾中的所有 Razor 元件和任何子資料夾。
* `BlazorApp1.Data` 命名空間。
 
[!code-razor[](layouts/sample_snapshot/3.x/_Imports.razor)]

*_Imports.razor*檔案類似於Razor[檢視和頁面的_ViewImports.cshtml檔](xref:mvc/views/layout#importing-shared-directives),但專門應用於Razor元件檔。

在 *_Imports.razor*中指定佈局將覆蓋指定為路由器*預設佈局的佈局*。

## <a name="nested-layouts"></a>巢狀佈局

應用可以由嵌套布局組成。 元件可以引用佈局,而佈局又引用另一個佈局。 例如,嵌套布局用於創建多級功能表結構。

下面的範例示範如何使用嵌套佈局。 *「單集元件.razor」* 檔案是要顯示的元件。 元件參照`MasterListLayout`:

[!code-razor[](layouts/sample_snapshot/3.x/EpisodesComponent.razor?highlight=1)]

*主清單佈局.razor*檔案`MasterListLayout`提供 。 佈局引用另一個佈局`MasterLayout`,在其中呈現佈局。 `EpisodesComponent`呈現的位置`@Body`出現:

[!code-razor[](layouts/sample_snapshot/3.x/MasterListLayout.razor?highlight=1,9)]

最後,`MasterLayout`在*MasterLayout.razor*中包含頂級佈局元素,如標題、主選單和頁腳。 `MasterListLayout``EpisodesComponent`與 顯示`@Body`位置顯示:

[!code-razor[](layouts/sample_snapshot/3.x/MasterLayout.razor?highlight=6)]

## <a name="share-a-razor-pages-layout-with-integrated-components"></a>與整合元件分享剃刀頁面配置

當可路由元件集成到 Razor Pages 應用中時,該應用的共用佈局可與元件一起使用。 如需詳細資訊，請參閱 <xref:blazor/integrate-components>。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/layout>
