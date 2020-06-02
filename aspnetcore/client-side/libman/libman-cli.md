---
title: 搭配 ASP.NET Core 使用 LibMan CLI
author: scottaddie
description: 瞭解如何在 ASP.NET Core 專案中使用 LibMan CLI。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: 1a42d162e28d4bb4cce284b8b5e37f1be6ff64c6
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82770548"
---
# <a name="use-the-libman-cli-with-aspnet-core"></a>搭配 ASP.NET Core 使用 LibMan CLI

作者：[Scott Addie](https://twitter.com/Scott_Addie)

[LibMan](xref:client-side/libman/index) CLI 是一種跨平臺工具，支援 .net Core 的任何位置。

## <a name="prerequisites"></a>Prerequisites

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>安裝

若要安裝 LibMan CLI：

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

[.Net Core 通用工具](/dotnet/core/tools/global-tools#install-a-global-tool)是從[LibraryManager](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/)安裝而來。

若要從特定的 NuGet 套件來源安裝 LibMan CLI：

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

在上述範例中，.NET Core 通用工具是從本機 Windows 電腦的*C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*檔案進行安裝。

## <a name="usage"></a>使用方式

成功安裝 CLI 之後，您可以使用下列命令：

```console
libman
```

若要查看已安裝的 CLI 版本：

```console
libman --version
```

若要查看可用的 CLI 命令：

```console
libman --help
```

上述命令會顯示類似下列的輸出：

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

下列各節將概述可用的 CLI 命令。

## <a name="initialize-libman-in-the-project"></a>初始化專案中的 LibMan

此 `libman init` 命令會建立*libman*檔案（如果不存在的話）。 使用預設專案範本內容建立檔案。

### <a name="synopsis"></a>概要

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>選項

以下是使用 `libman init` 命令時可用的選項：

* `-d|--default-destination <PATH>`

  相對於目前資料夾的路徑。 如果未 `destination` 針對*libman*中的程式庫定義任何屬性，則會在此位置安裝程式庫檔案。 此 `<PATH>` 值會寫入至 `defaultDestination` *libman*的屬性。

* `-p|--default-provider <PROVIDER>`

  未針對指定的程式庫定義提供者時，所要使用的提供者。 此 `<PROVIDER>` 值會寫入至 `defaultProvider` *libman*的屬性。 取代 `<PROVIDER>` 為下列其中一個值：

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

若要在 ASP.NET Core 專案中建立*libman json*檔案：

* 流覽至專案根目錄。
* 執行以下命令：

  ```console
  libman init
  ```

* 輸入預設提供者的名稱，或按 `Enter` 以使用預設的 CDNJS 提供者。 有效值包括：

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init 命令-預設提供者](_static/libman-init-provider.png)

*Libman*會使用下列內容新增至專案根目錄：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>新增程式庫檔案

命令會將連結 `libman install` 庫檔案下載並安裝到專案中。 如果*libman*檔案不存在，則會新增它。 已修改*libman*檔案以儲存程式庫檔案的設定詳細資料。

### <a name="synopsis"></a>概要

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

要安裝之程式庫的名稱。 此名稱可能包含版本號碼標記法（例如 `@1.2.0` ）。

### <a name="options"></a>選項

以下是使用 `libman install` 命令時可用的選項：

* `-d|--destination <PATH>`

  要安裝程式庫的位置。 如果未指定，則會使用預設位置。 如果 `defaultDestination` 在*libman*中未指定任何屬性，則此為必要選項。

* `--files <FILE>`

  指定要從程式庫安裝的檔案名。 如果未指定，則會安裝媒體櫃中的所有檔案。 `--files`針對要安裝的每個檔案提供一個選項。 也支援相對路徑。 例如： `--files dist/browser/signalr.js` 。

* `-p|--provider <PROVIDER>`

  要用於取得程式庫的提供者名稱。 取代 `<PROVIDER>` 為下列其中一個值：
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  如果未指定，則 `defaultProvider` 會使用*libman*中的屬性。 如果 `defaultProvider` 在*libman*中未指定任何屬性，則此為必要選項。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

請考慮下列*libman json*檔案：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

若要使用 CDNJS 提供者，將 jQuery 版本 3.2.1 *jquery. min .js*檔案安裝至*wwwroot/scripts/jQuery*資料夾：

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

*Libman*類似下列內容：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

若要使用檔案系統提供者，從*C： \\ temp \\ contosoCalendar \\ *安裝行事*曆*和*calendar*檔案：

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

有兩個原因會出現下列提示：

* *Libman*不包含 `defaultDestination` 屬性。
* 此 `libman install` 命令不包含 `-d|--destination` 選項。

![libman install 命令-目的地](_static/libman-install-destination.png)

接受預設目的地之後， *libman*會如下所示：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a>還原程式庫檔案

`libman restore`命令會安裝*libman*中定義的程式庫檔案。 適用的規則如下：

* 如果專案根目錄中沒有*libman*檔案存在，則會傳回錯誤。
* 如果程式庫指定了提供者，則 `defaultProvider` 會忽略*libman*中的屬性。
* 如果程式庫指定了目的地，則 `defaultDestination` 會忽略*libman*中的屬性。

### <a name="synopsis"></a>概要

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>選項

以下是使用 `libman restore` 命令時可用的選項：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

若要還原*libman*中定義的程式庫檔案：

```console
libman restore
```

## <a name="delete-library-files"></a>刪除程式庫檔案

`libman clean`命令會刪除先前透過 LibMan 還原的程式庫檔案。 刪除此作業後變成空白的資料夾。 程式庫檔案在 libman 的屬性中的關聯設定 `libraries` 不會被移除。 *libman.json*

### <a name="synopsis"></a>概要

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>選項

以下是使用 `libman clean` 命令時可用的選項：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

若要刪除透過 LibMan 安裝的程式庫檔案：

```console
libman clean
```

## <a name="uninstall-library-files"></a>卸載程式庫檔案

`libman uninstall`命令：

* 從*libman*中的目的地，刪除與指定的程式庫相關聯的所有檔案。
* 從*libman*移除相關聯的程式庫設定。

當下列情況發生錯誤：

* 專案根目錄中沒有*libman*檔案存在。
* 指定的程式庫不存在。

如果安裝了多個具有相同名稱的程式庫，系統會提示您選擇其中一個。

### <a name="synopsis"></a>概要

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

要卸載之程式庫的名稱。 此名稱可能包含版本號碼標記法（例如 `@1.2.0` ）。

### <a name="options"></a>選項

以下是使用 `libman uninstall` 命令時可用的選項：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

請考慮下列*libman json*檔案：

[!code-json[](samples/LibManSample/libman.json)]

* 若要卸載 jQuery，下列任一命令都會成功：

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* 若要卸載透過提供者安裝的 Lodash 所檔案 `filesystem` ：

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>更新程式庫版本

命令會將透過 `libman update` LibMan 安裝的程式庫更新為指定的版本。

當下列情況發生錯誤：

* 專案根目錄中沒有*libman*檔案存在。
* 指定的程式庫不存在。

如果安裝了多個具有相同名稱的程式庫，系統會提示您選擇其中一個。

### <a name="synopsis"></a>概要

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

要更新之程式庫的名稱。

### <a name="options"></a>選項

以下是使用 `libman update` 命令時可用的選項：

* `-pre`

  取得程式庫的最新發行前版本。

* `--to <VERSION>`

  取得特定版本的程式庫。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

* 若要將 jQuery 更新為最新版本：

  ```console
  libman update jquery
  ```

* 若要將 jQuery 更新為版本3.3.1：

  ```console
  libman update jquery --to 3.3.1
  ```

* 若要將 jQuery 更新為最新的發行前版本：

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>管理程式庫快取

`libman cache`命令會管理 LibMan 程式庫快取。 `filesystem`提供者不會使用程式庫快取。

### <a name="synopsis"></a>概要

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>引數

`PROVIDER`

僅搭配命令使用 `clean` 。 指定要清除的提供者快取。 有效值包括：

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>選項

以下是使用 `libman cache` 命令時可用的選項：

* `--files`

  列出快取的檔案名稱。

* `--libraries`

  列出快取的程式庫名稱。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

* 若要查看每個提供者的快取程式庫名稱，請使用下列其中一個命令：

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  會顯示類似下列的輸出：

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* 若要查看每個提供者的快取程式庫檔案的名稱：

  ```console
  libman cache list --files
  ```

  會顯示類似下列的輸出：

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  請注意，上述輸出顯示 jQuery 版本3.2.1 和3.3.1 會在 CDNJS 提供者底下快取。

* 若要清空 CDNJS 提供者的程式庫快取：

  ```console
  libman cache clean cdnjs
  ```

  清空 CDNJS 提供者快取之後，此 `libman cache list` 命令會顯示下列內容：

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* 若要清空所有支援之提供者的快取：

  ```console
  libman cache clean
  ```

  清空所有提供者快取之後，此 `libman cache list` 命令會顯示下列內容：

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>其他資源

* [安裝通用工具](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [LibMan GitHub 存放庫](https://github.com/aspnet/LibraryManager)
