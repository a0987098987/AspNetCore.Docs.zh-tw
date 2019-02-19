---
title: 開始使用 Blazor
author: guardrex
description: 了解如何開始使用 Blazor 建立和修改 Blazor 專案。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2019
uid: spa/blazor/get-started
ms.openlocfilehash: 26336f73f6c8976ed5de819cebc3c5c50274ab03
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410004"
---
# <a name="get-started-with-blazor"></a>開始使用 Blazor

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

必要條件：

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

若要在 Visual Studio 中建立第一個 Blazor 專案：

1. 安裝最新[Blazor 語言服務延伸模組](https://go.microsoft.com/fwlink/?linkid=870389)從 Visual Studio Marketplace。 此步驟會 Blazor 範本提供給 Visual Studio。
1. 在命令殼層中執行下列命令，請 Blazor 範本適用於.NET Core CLI:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.8.0-preview-19104-04
   ```

1. 選取 **檔案** > **新專案** > **Web** > **ASP.NET Core Web 應用程式**。
1. 請確定 **.NET Core**並**ASP.NET Core 3.0**選取頂端。
1. 選擇 **Blazor** 範本，然後選取 [確定]。
1. 按下 **F5** 即可執行應用程式。

恭喜您！ 您剛剛執行第一個 Blazor 應用程式 ！

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

必要條件：

* [.NET core SDK 3.0 預覽](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. 在命令殼層中執行下列命令，將新增 Blazor 範本：

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.8.0-preview-19104-04
   ```

1. 在命令殼層中建立第一個 Blazor 專案：

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. 在瀏覽器中，巡覽至 `https://localhost:5001`。

恭喜您！ 您剛剛執行第一個 Blazor 應用程式 ！

---

## <a name="blazor-project"></a>Blazor 專案

執行應用程式時，多個頁面都是從資訊看板中的索引標籤：

* 首頁
* 計數器
* 擷取資料

在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。 遞增計數器，以在網頁上通常需要撰寫 JavaScript，但 Blazor 提供更好的方法使用C#。

*Pages/Counter.cshtml*：

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

申請`/counter`在瀏覽器中所指定`@page`指示詞，在頂端，導致計數器元件來呈現其內容。 元件會轉譯成接著可用來更新 UI 富彈性又有效率的方式呈現樹狀結構的記憶體中表示法。

每次**Click me**按鈕已選取：

* `onclick`引發事件。
* 已呼叫 `IncrementCount` 方法。
* `currentCount`會遞增。
* 元件會轉譯一次。

執行階段會比較新的內容來將舊內容，並只會套用已變更的內容與文件物件模型 (DOM)。

您可以將元件加入另一個元件使用 HTML 的類似語法。 元件參數會指定使用屬性或子內容。 比方說，將計數器元件可以加入至應用程式的首頁加`<Counter />`索引元件的項目。

在  *pages/Index.cshtml*，問卷提示元件取代計數器元件：

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

執行應用程式。 在首頁上都有它自己的計數器。

若要將參數加入至計數器的元件，更新 元件的`@functions`區塊：

* 新增的屬性`IncrementAmount`附有`[Parameter]`屬性。
* 將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。

*Pages/Counter.cshtml*：

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

使用屬性在「首頁」元件的 `<Counter>` 元素中指定 `IncrementAmount` 參數。

*Pages/Index.cshtml*：

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

執行應用程式。 在首頁上有它自己的十個在每次增加的計數器**Click me**按鈕已選取。

## <a name="next-steps"></a>後續步驟

<xref:tutorials/first-razor-components-app>
