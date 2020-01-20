---
title: ASP.NET Core Razor 元件類別庫
author: guardrex
description: 探索如何將元件包含在來自外部元件程式庫的 Blazor 應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/class-libraries
ms.openlocfilehash: f8e8688cdb3d1aef0d470e0e2d8c3857140ef65f
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160024"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>ASP.NET Core Razor 元件類別庫

依[Simon Timms](https://github.com/stimms)

元件可以在跨專案的[Razor 類別庫（RCL）](xref:razor-pages/ui-class)中共用。 *Razor 元件類別庫*可以包含在：

* 方案中的另一個專案。
* NuGet 套件。
* 參考的 .NET 程式庫。

就像元件是一般的 .NET 類型，RCL 所提供的元件是一般的 .NET 元件。

## <a name="create-an-rcl"></a>建立 RCL

遵循 <xref:blazor/get-started> 文章中的指導方針，設定您的環境以進行 Blazor。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新的專案。
1. 選取 [ **Razor 類別庫**]。 選取 [下一步]。
1. 在 [**建立新的 Razor 類別庫**] 對話方塊中，選取 [**建立**]。
1. 在 [專案名稱] 欄位中提供專案名稱，或接受預設專案名稱。 本主題中的範例會使用專案名稱 `MyComponentLib1`。 選取 [建立]。
1. 將 RCL 新增至方案：
   1. 以滑鼠右鍵按一下方案。 選取 [**新增** > **現有專案**]。
   1. 流覽至 RCL 的專案檔。
   1. 選取 RCL 的專案檔（ *.csproj*）。
1. 從應用程式新增參考 RCL：
   1. 以滑鼠右鍵按一下應用程式專案。 選取 [**新增** > **參考**]。
   1. 選取 [RCL] 專案。 選取 [確定]。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. 使用**Razor 類別庫**範本（`razorclasslib`）搭配命令 shell 中的[dotnet new](/dotnet/core/tools/dotnet-new)命令。 在下列範例中，會建立名為 `MyComponentLib1`的 RCL。 執行命令時，會自動建立保存 `MyComponentLib1` 的資料夾：

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. 若要將程式庫新增至現有的專案，請在命令 shell 中使用[dotnet add reference](/dotnet/core/tools/dotnet-add-reference)命令。 在下列範例中，會將 RCL 新增至應用程式。 從應用程式的專案資料夾中，使用程式庫的路徑執行下列命令：

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>使用程式庫元件

若要使用另一個專案中的程式庫中所定義的元件，請使用下列其中一種方法：

* 使用命名空間的完整類型名稱。
* 使用 Razor 的[\@using](xref:mvc/views/razor#using)指示詞。 個別元件可以依名稱新增。

在下列範例中，`MyComponentLib1` 是包含 `SalesReport` 元件的元件庫。

您可以使用命名空間的完整型別名稱來參考 `SalesReport` 元件：

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

如果程式庫是使用 `@using` 指示詞帶入範圍中，則也可以參考此元件：

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

在最上層的 *_Import razor*檔案中包含 `@using MyComponentLib1` 指示詞，讓程式庫的元件可供整個專案使用。 將指示詞新增至任何層級的 *_Import razor*檔案，以將命名空間套用至資料夾中的單一頁面或一組頁面。

## <a name="build-pack-and-ship-to-nuget"></a>組建、封裝和寄送至 NuGet

因為元件程式庫是標準的 .NET 程式庫，所以封裝和傳送至 NuGet 的方式與將任何程式庫封裝和傳送至 NuGet 的方式並無不同。 封裝是使用命令 shell 中的[dotnet pack](/dotnet/core/tools/dotnet-pack)命令來執行：

```dotnetcli
dotnet pack
```

使用命令 shell 中的[dotnet NuGet push](/dotnet/core/tools/dotnet-nuget-push)命令，將套件上傳至 nuget。

## <a name="create-a-razor-components-class-library-with-static-assets"></a>建立具有靜態資產的 Razor 元件類別庫

RCL 可以包含靜態資產。 使用該程式庫的任何應用程式都可以使用靜態資產。 如需詳細資訊，請參閱<xref:razor-pages/ui-class#create-an-rcl-with-static-assets>。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/ui-class>
