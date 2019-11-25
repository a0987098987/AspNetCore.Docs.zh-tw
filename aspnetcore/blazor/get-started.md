---
title: Get started with ASP.NET Core Blazor
author: guardrex
description: Get started with Blazor by building a Blazor app with the tooling of your choice.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 198093b37cb4f440eb7b520d18004304aea570a5
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239711"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a>Get started with ASP.NET Core Blazor

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Get started with Blazor:

::: moniker range=">= aspnetcore-3.1"

1. Install the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by running the following command in a command shell. The [Microsoft.AspNetCore.Blazor.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) package has a preview version while Blazor WebAssembly is in preview.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. Follow the guidance for your choice of tooling:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload.

   2\. 建立新的專案。

   3\. Select **Blazor App**. 選取 [下一步]。

   4\. 在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。 Confirm the **Location** entry is correct or provide a location for the project. 選取 [建立]。

   5\. For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template. For a Blazor Server experience, choose the **Blazor Server App** template. 選取 [建立]。 For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   6\. 按下 **Ctrl**+**F5** 即可執行應用程式。

   > [!NOTE]
   > If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension. Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. 安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。

   2\. Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3\. For a Blazor WebAssembly experience, execute the following command in a command shell:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      For a Blazor Server experience, execute the following command in a command shell:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   4\. Open the *WebApplication1* folder in Visual Studio Code.

   5\. For a Blazor Server project, the IDE requests that you add assets to build and debug the project. 選取 [是]。

   6\. If using a Blazor Server app, run the app using the Visual Studio Code debugger. If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.

   7\. 在瀏覽器中，巡覽至 `https://localhost:5001`。

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   For a Blazor WebAssembly experience, execute the following commands in a command shell:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   For a Blazor Server experience, execute the following commands in a command shell:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   在瀏覽器中，巡覽至 `https://localhost:5001`。

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. Install the latest [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) release.

1. Optionally install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template by installing the [.NET Core 3.1 Preview SDK](https://dotnet.microsoft.com/download/dotnet-core/3.1) and then running the following command in a command shell:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview3.19555.2
   ```

1. Follow the guidance for your choice of tooling:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Install the latest [Visual Studio](https://visualstudio.com/vs/) with the **ASP.NET and web development** workload.

   2\. Optionally install [Visual Studio 16.4 Preview 2 or later](https://visualstudio.microsoft.com/vs/preview/) with the **ASP.NET and web development** workload for Blazor WebAssembly app development.

   3\. 建立新的專案。

   4\. Select **Blazor App**. 選取 [下一步]。

   5\. 在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。 Confirm the **Location** entry is correct or provide a location for the project. 選取 [建立]。

   6\. For a Blazor WebAssembly experience, choose the **Blazor WebAssembly App** template. For a Blazor Server experience, choose the **Blazor Server App** template. 選取 [建立]。 For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   7\. 按下 **F5** 即可執行應用程式。

   > [!NOTE]
   > If you installed the Blazor Visual Studio extension for a prior preview release of ASP.NET Core Blazor (Preview 6 or earlier), you can uninstall the extension. Installing the Blazor templates in a command shell is now sufficient to surface the templates in Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. 安裝 [Visual Studio Code (英文)](https://code.visualstudio.com/)。

   2\. Install the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3\. For a Blazor WebAssembly experience, execute the following command in a command shell:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      For a Blazor Server experience, execute the following command in a command shell:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   4\. Open the *WebApplication1* folder in Visual Studio Code.

   5\. For a Blazor Server project, the IDE requests that you add assets to build and debug the project. 選取 [是]。

   6\. If using a Blazor Server app, run the app using the Visual Studio Code debugger. If using a Blazor WebAssembly app, execute `dotnet run` from the app's project folder.

   7\. 在瀏覽器中，巡覽至 `https://localhost:5001`。

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   For a Blazor WebAssembly experience, execute the following commands in a command shell:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   For a Blazor Server experience, execute the following commands in a command shell:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   在瀏覽器中，巡覽至 `https://localhost:5001`。

   ---

::: moniker-end

Multiple pages are available from tabs in the sidebar:

* 首頁
* 計數器
* Fetch data

在 [計數器] 頁面上，選取 [按我] 按鈕以在不重新整理頁面的情況下讓計數器遞增。 Incrementing a counter in a webpage normally requires writing JavaScript, but with Blazor you can use C#.

*Pages/Counter.razor*：

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

A request for `/counter` in the browser, as specified by the `@page` directive at the top, causes the `Counter` component to render its content. Components render into an in-memory representation of the render tree that can then be used to update the UI in a flexible and efficient way.

Each time the **Click me** button is selected:

* The `onclick` event is fired.
* 已呼叫 `IncrementCount` 方法。
* The `currentCount` is incremented.
* The component is rendered again.

The runtime compares the new content to the previous content and only applies the changed content to the Document Object Model (DOM).

Add a component to another component using HTML syntax. For example, add the `Counter` component to the app's homepage by adding a `<Counter />` element to the `Index` component.

*Pages/Index.razor*：

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

執行應用程式。 The homepage has its own counter provided by the `Counter` component.

Component parameters are specified using attributes or [child content](xref:blazor/components#child-content), which allow you to set properties on the child component. To add a parameter to the `Counter` component, update the component's `@code` block:

* Add a public property for `IncrementAmount` with a `[Parameter]` attribute.
* 將 `IncrementCount` 方法變更為在增加 `currentCount`的值時使用 `IncrementAmount`。

*Pages/Counter.razor*：

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Specify the `IncrementAmount` in the `Index` component's `<Counter>` element using an attribute.

*Pages/Index.razor*：

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

執行應用程式。 The `Index` component has its own counter that increments by ten each time the **Click me** button is selected. The `Counter` component (*Counter.razor*) at `/counter` continues to increment by one.

## <a name="next-steps"></a>後續步驟

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>其他資源

* <xref:signalr/introduction>
