---
title: ASP.NET Core Razor 元件類別庫
author: guardrex
description: 探索如何將元件包含在 Blazor 來自外部元件程式庫的應用程式中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/components/class-libraries
ms.openlocfilehash: ecc9873d7f652f27767df98196786d12789518c9
ms.sourcegitcommit: 490434a700ba8c5ed24d849bd99d8489858538e3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85103630"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>ASP.NET Core Razor 元件類別庫

依[Simon Timms](https://github.com/stimms)

元件可以在各個專案的[ Razor 類別庫（RCL）](xref:razor-pages/ui-class)中共用。 * Razor 元件類別庫*可以包含在：

* 方案中的另一個專案。
* NuGet 套件。
* 參考的 .NET 程式庫。

就像元件是一般的 .NET 類型，RCL 所提供的元件是一般的 .NET 元件。

## <a name="create-an-rcl"></a>建立 RCL

請遵循文章中的指導方針 <xref:blazor/get-started> ，為設定您的環境 Blazor 。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新專案。
1. 選取 [ ** Razor 類別庫**]。 選取 [下一步] 。
1. 在 [**建立新的 Razor 類別庫**] 對話方塊中，選取 [**建立**]。
1. 在 [專案名稱]**** 欄位中提供專案名稱，或接受預設專案名稱。 本主題中的範例會使用專案名稱 `MyComponentLib1` 。 選取 [建立]。
1. 將 RCL 新增至方案：
   1. 以滑鼠右鍵按一下方案。 選取 [**加入**  >  **現有專案**]。
   1. 流覽至 RCL 的專案檔。
   1. 選取 RCL 的專案檔（*.csproj*）。
1. 從應用程式新增參考 RCL：
   1. 以滑鼠右鍵按一下應用程式專案。 選取 [**新增**  >  **參考**]。
   1. 選取 [RCL] 專案。 選取 [確定]。

> [!NOTE]
> 從範本產生 RCL 時，如果已選取 [**支援頁面和視圖**] 核取方塊，則也會將 *_Imports razor*檔案新增至產生之專案的根目錄，並具有下列內容以啟用 Razor 元件撰寫：
>
> ```razor
> @using Microsoft.AspNetCore.Components.Web
> ```
>
> 以手動方式將檔案新增至所產生專案的根目錄。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. 使用** Razor 類別庫**範本（ `razorclasslib` ）搭配命令 shell 中的[dotnet new](/dotnet/core/tools/dotnet-new)命令。 在下列範例中，會建立名為的 RCL `MyComponentLib1` 。 `MyComponentLib1`執行命令時，會自動建立保存的資料夾：

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

   > [!NOTE]
   > 如果 `-s|--support-pages-and-views` 從範本產生 RCL 時使用參數，則也會將 *_Imports razor*檔案新增至所產生專案的根目錄，並具有下列內容以啟用 Razor 元件撰寫：
   >
   > ```razor
   > @using Microsoft.AspNetCore.Components.Web
   > ```
   >
   > 以手動方式將檔案新增至所產生專案的根目錄。

1. 若要將程式庫新增至現有的專案，請在命令 shell 中使用[dotnet add reference](/dotnet/core/tools/dotnet-add-reference)命令。 在下列範例中，會將 RCL 新增至應用程式。 從應用程式的專案資料夾中，使用程式庫的路徑執行下列命令：

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>使用程式庫元件

若要使用另一個專案中的程式庫中所定義的元件，請使用下列其中一種方法：

* 使用命名空間的完整類型名稱。
* 使用 Razor 的指示詞 [`@using`](xref:mvc/views/razor#using) 。 個別元件可以依名稱新增。

在下列範例中， `MyComponentLib1` 是包含元件的元件庫 `SalesReport` 。

`SalesReport`元件可以使用其完整型別名稱和命名空間來加以參考：

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

如果使用指示詞將程式庫帶入範圍，也可以參考此元件 `@using` ：

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

`@using MyComponentLib1`請在最上層的 *_Import razor*檔案中包含指示詞，以將程式庫的元件提供給整個專案。 將指示詞新增至任何層級的 *_Import razor*檔案，以將命名空間套用至資料夾中的單一頁面或一組頁面。

## <a name="create-a-razor-components-class-library-with-static-assets"></a>建立 Razor 具有靜態資產的元件類別庫

RCL 可以包含靜態資產。 使用該程式庫的任何應用程式都可以使用靜態資產。 如需詳細資訊，請參閱 <xref:razor-pages/ui-class#create-an-rcl-with-static-assets> 。

## <a name="build-pack-and-ship-to-nuget"></a>組建、封裝和寄送至 NuGet

因為元件程式庫是標準的 .NET 程式庫，所以封裝和傳送至 NuGet 的方式與將任何程式庫封裝和傳送至 NuGet 的方式並無不同。 封裝是使用命令 shell 中的[dotnet pack](/dotnet/core/tools/dotnet-pack)命令來執行：

```dotnetcli
dotnet pack
```

使用命令 shell 中的[dotnet NuGet push](/dotnet/core/tools/dotnet-nuget-push)命令，將套件上傳至 nuget。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/ui-class>
* [將 XML 連結器設定檔加入至程式庫](xref:blazor/host-and-deploy/configure-linker#add-an-xml-linker-configuration-file-to-a-library)
