---
title: ASP.NET核心剃刀元件類庫
author: guardrex
description: 瞭解如何從外部元件庫中將元件Blazor包含在應用中。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
no-loc:
- Blazor
- SignalR
uid: blazor/class-libraries
ms.openlocfilehash: f2cc57638922bd1f6ab036adb2ed37209d14c5b0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80218762"
---
# <a name="aspnet-core-razor-components-class-libraries"></a>ASP.NET核心剃刀元件類庫

由[西蒙·蒂姆斯](https://github.com/stimms)

元件可以在跨專案的[Razor 類庫 (RCL)](xref:razor-pages/ui-class)中共用。 *Razor 元件類庫*可以從:

* 解決方案中的另一個專案。
* NuGet 包。
* 引用的 .NET 庫。

正如元件是一般的 .NET 類型一樣,RCL 提供的元件是普通 .NET 程式集。

## <a name="create-an-rcl"></a>建立 RCL

按照本文中的<xref:blazor/get-started>指南為 Blazor 配置環境。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 建立新專案。
1. 選擇**Razor 函式庫**。 選取 [下一步]  。
1. 在 **"創建新 Razor 類庫**"對話框中,選擇 **"創建**"。
1. 在 [專案名稱]**** 欄位中提供專案名稱，或接受預設專案名稱。 這個主題中的範例使用項目名稱`MyComponentLib1`。 選取 [建立]  。
1. 新增 RCL 新增到解決方案:
   1. 右鍵單擊解決方案。 選擇 **「添加** > **現有專案**」。
   1. 導航到 RCL 的專案檔。
   1. 選擇 RCL 的專案檔 *(.csproj*)。
1. 從應用程式新增 RCL 的參考:
   1. 右鍵單擊應用專案。 選擇 **「添加** > **參考**」。
   1. 選擇 RCL 專案。 選取 [確定]  。

> [!NOTE]
> 如果在從樣本產生 RCL 時選擇了 **「支援頁面和檢視**」複選框,則還會將 *_Imports.razor*檔案添加到生成的專案的根目錄,其中包含以下內容,以啟用 Razor 元件創作:
>
> ```razor
> @using Microsoft.AspNetCore.Components.Web
> ```
>
> 手動將檔添加到生成的專案的根目錄。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. 在命令 shell`razorclasslib`中使用**Razor 函式庫**樣本 ( ) 與[dotnet 新](/dotnet/core/tools/dotnet-new)命令一起。 在下面的示例中,將創建名為`MyComponentLib1`的 RCL。 執行命令時自動`MyComponentLib1`建立保留的資料夾:

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

   > [!NOTE]
   > 如果從`-s|--support-pages-and-views`樣本產生 RCL 時使用交換機,則還會將 *_Imports.razor*檔案加入到產生的專案的根目錄,其中包含以下內容,以啟用 Razor 元件創作:
   >
   > ```razor
   > @using Microsoft.AspNetCore.Components.Web
   > ```
   >
   > 手動將檔添加到生成的專案的根目錄。

1. 要將函式庫加入到現有專案,請使用[dotnet 在](/dotnet/core/tools/dotnet-add-reference)命令 shell 加入引用命令。 在下面的示例中,RCL將添加到應用中。 使用函式庫的路徑從應用程式的項目資料夾中執行以下指令:

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a>使用函式庫元件

為了使用在另一個專案中的庫中定義的元件,請使用以下任一方法:

* 使用完整類型名稱與命名空間。
* 使用 Razor[\@的使用](xref:mvc/views/razor#using)指令。 可以按名稱添加單個元件。

在以下範例中,`MyComponentLib1`是包含元件的`SalesReport`元件庫。

可以使用`SalesReport`使用一個命名空間的全類型名稱引用此元件:

```razor
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

如果函式庫被`@using`引入具有指令的範圍,也可以參考該元件:

```razor
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

將`@using MyComponentLib1`指令包含在頂級 *_Import.razor*檔中,以使庫的元件可用於整個專案。 將指令添加到任何級別的 *_Import.razor*檔,以將命名空間應用於資料夾中的單個頁面或頁面集。

## <a name="create-a-razor-components-class-library-with-static-assets"></a>使用靜態資產建立 Razor 元件類庫

RCL 可以包括靜態資產。 靜態資產可用於使用庫的任何應用。 如需詳細資訊，請參閱 <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>。

## <a name="build-pack-and-ship-to-nuget"></a>建置、打包及運送到 NuGet

因為元件庫是標準的 .NET 庫,因此打包並將其運送到 NuGet 與打包和將任何庫運送到 NuGet 沒有什麼不同。 使用命令 shell 中的[dotnet pack](/dotnet/core/tools/dotnet-pack)命令執行封包:

```dotnetcli
dotnet pack
```

使用命令 shell 中的[dotnet nuget 推送](/dotnet/core/tools/dotnet-nuget-push)指令將包上載到 NuGet。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/ui-class>
* [新增 XML 連結器設定檔到函式庫](xref:host-and-deploy/blazor/configure-linker#add-an-xml-linker-configuration-file-to-a-library)
