---
title: Razor 元件類別庫
author: guardrex
description: 探索如何包含元件，外部元件程式庫中的 Razor 元件應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515391"
---
# <a name="razor-components-class-libraries"></a>Razor 元件類別庫

藉由[Simon Timms](https://github.com/stimms)

元件可以在專案之間共用 Razor 類別庫中。 元件可以包含：

* 在方案中的另一個專案。
* NuGet 套件。
* 參考的.NET 程式庫。

元件是一般的.NET 型別，如同 Razor 類別庫所提供的元件就會是一般的.NET 組件。

使用`razorclasslib`（Razor 類別程式庫） 範本，內含[dotnet 新](/dotnet/core/tools/dotnet-new)命令：

```console
dotnet new razorclasslib -o MyComponentLib1
```

新增 Razor 元件檔案 (*.razor*) Razor 類別庫。

若要加入現有的專案程式庫，請使用[dotnet sln](/dotnet/core/tools/dotnet-sln)命令：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> Razor 類別庫與不相容的 ASP.NET Core 的 Preview 3 的 Blazor 應用程式。
>
> 若要建立可以與 Blazor 和 Razor 元件的應用程式共用程式庫中的元件，請使用 Blazor 類別庫所建立`blazorlib`範本。
>
> Razor 類別庫不支援 ASP.NET Core 的 Preview 3 中的靜態資產。 元件程式庫使用`blazorlib`範本可以包含靜態檔案，例如影像、 JavaScript 和樣式表。 在建置時，靜態檔案內嵌到建置組件檔案 (*.dll*)，可讓元件的耗用量，而不必擔心如何包含其資源。 包含的任何檔案`content`目錄標示為內嵌資源。

## <a name="consume-a-library-component"></a>使用程式庫元件

若要使用定義在另一個專案中，文件庫中的元件[ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label)必須使用指示詞。 個別元件可能會依名稱加入。

指示詞一般格式為：

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

例如，新增下列指示詞`Component1`的`MyComponentLib1`:

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

不過，通常會包含所有的元件，從組件使用萬用字元 (`*`):

```cshtml
@addTagHelper *, MyComponentLib1
```

`@addTagHelper`指示詞可以包含在 *_ViewImport.cshtml*使元件適用於整個專案或套用至單一頁面或一組資料夾內的頁面。 使用`@addTagHelper`就地指示詞時，元件程式庫的元件可以使用，彷彿應用程式相同的組件中。

## <a name="build-pack-and-ship-to-nuget"></a>組建、 套件及出貨至 NuGet

因為元件程式庫是標準的.NET 程式庫，並無不同封裝和傳送任何程式庫 nuget 封裝和傳送至 NuGet。 使用執行封裝[dotnet pack](/dotnet/core/tools/dotnet-pack)命令：

```console
dotnet pack
```

使用 NuGet 封裝上傳[dotnet nuget 發行](/dotnet/core/tools/dotnet-nuget-push)命令：

```console
dotnet nuget publish
```

當使用`blazorlib`範本中，靜態資源會包含在 NuGet 套件。 程式庫取用者會自動接收指令碼和樣式表，讓取用者不一定要以手動方式安裝的資源。
