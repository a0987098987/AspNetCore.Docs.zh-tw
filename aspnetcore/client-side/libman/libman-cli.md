---
title: 將 LibMan CLI 與ASP.NET核心
author: scottaddie
description: 瞭解如何在ASP.NET核心專案中使用 LibMan CLI。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/libman/libman-cli
ms.openlocfilehash: 02d88d09805bd23a86ef924766373245fec7ff52
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664630"
---
# <a name="use-the-libman-cli-with-aspnet-core"></a>將 LibMan CLI 與ASP.NET核心

作者：[Scott Addie](https://twitter.com/Scott_Addie)

[LibMan](xref:client-side/libman/index) CLI 是一個跨平臺工具,支援無處不在 .NET Core。

## <a name="prerequisites"></a>Prerequisites

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>安裝

要安裝 LibMan CLI:

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

從[Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet 套件中安裝了[.NET 核心全域工具](/dotnet/core/tools/global-tools#install-a-global-tool)。

要從特定的 NuGet 套件來源安裝 LibMan CLI,請執行以下規定:

```dotnetcli
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

在前面的示例中,從本地 Windows 計算機的*C:\Temp_Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*檔中安裝了 .NET 核心全域工具。

## <a name="usage"></a>使用量

成功安裝 CLI 後,可以使用以下指令:

```console
libman
```

要檢視已安裝的 CLI 版本:

```console
libman --version
```

要檢視可用的 CLI 指令:

```console
libman --help
```

前面的指令顯示類似於以下內容的輸出:

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

## <a name="initialize-libman-in-the-project"></a>在項目中初始化 LibMan

如果`libman init`不存在,該命令將創建一個*libman.json*檔。 該檔使用預設專案範本內容創建。

### <a name="synopsis"></a>概要

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>選項。

以下是使用 `libman init` 命令時可用的選項：

* `-d|--default-destination <PATH>`

  相對於當前資料夾的路徑。 如果沒有`destination`為*libman.json*中的庫定義屬性,則庫檔將安裝在此位置。 該`<PATH>`值寫`defaultDestination`入*利曼.json*的屬性。

* `-p|--default-provider <PROVIDER>`

  如果未為給定庫定義提供程式,則要使用的提供程式。 該`<PROVIDER>`值寫`defaultProvider`入*利曼.json*的屬性。 取代為`<PROVIDER>`以下值之一:

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

在ASP.NET核心項目中建立*libman.json*檔,請進行:

* 導航到專案根目錄。
* 執行以下命令：

  ```console
  libman init
  ```

* 鍵入預設提供程式的名稱,或按`Enter`使用預設 CDNJS 提供程式。 有效值包括：

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init 指令 ─ 預設提供者](_static/libman-init-provider.png)

*libman.json*檔已添加到專案根中,內容如下:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>新增函式庫檔案

該`libman install`命令下載庫檔並將其安裝到專案中。 如果不存在,將添加*libman.json*檔。 *libman.json*檔被修改以儲存庫檔的配置詳細資訊。

### <a name="synopsis"></a>概要

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

要安裝的庫的名稱。 此名稱可能包含版本號表示法(例如, `@1.2.0`。

### <a name="options"></a>選項。

以下是使用 `libman install` 命令時可用的選項：

* `-d|--destination <PATH>`

  要安裝庫的位置。 如果未指定,則使用預設位置。 如果在`defaultDestination`*libman.json*中未指定任何屬性,則需要此選項。

* `--files <FILE>`

  指定要從庫中安裝的檔案的名稱。 如果未指定,則安裝庫中的所有檔。 每個要`--files`安裝的檔案提供一個選項。 也支持相對路徑。 例如： `--files dist/browser/signalr.js` 。

* `-p|--provider <PROVIDER>`

  用於庫採集的提供程式的名稱。 取代為`<PROVIDER>`以下值之一:
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  如果未指定,`defaultProvider`則使用*libman.json*中的屬性。 如果在`defaultProvider`*libman.json*中未指定任何屬性,則需要此選項。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

請考慮以下*利伯曼.json*檔:

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

要使用 CDNJS 提供者將 jQuery 版本 3.2.1 *jquery.min.js*檔案安裝到*wwwroot/文稿/jquery*資料夾:

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

*libman.json*檔案類似於以下內容:

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

要使用檔案系統提供者從*\\C:\\暫時\\contosoCalendar*安裝*calendar.js*與*calendar.css*檔案:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

發生以下提示有兩個原因:

* *libman.json*檔不包含`defaultDestination`屬性 。
* 該`libman install`命令不包含`-d|--destination`該 選項。

![利伯曼安裝命令 - 目標](_static/libman-install-destination.png)

在接受預設目標後 *,libman.json*檔案類似於以下內容:

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

## <a name="restore-library-files"></a>復原庫檔案

該`libman restore`命令安裝在*libman.json*中定義的庫檔。 適用的規則如下：

* 如果專案根中不存在*libman.json*檔,則傳回錯誤。
* 如果函式庫指定提供`defaultProvider`者, 則忽略*libman.json*中的屬性。
* 如果函式庫指定了`defaultDestination`目標, 則忽略*libman.json*中的屬性。

### <a name="synopsis"></a>概要

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>選項。

以下是使用 `libman restore` 命令時可用的選項：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

要回復*Libman.json*中定義的庫檔:

```console
libman restore
```

## <a name="delete-library-files"></a>刪除庫檔案

該`libman clean`指令刪除以前透過 LibMan 還原的庫檔。 此操作後變為空的資料夾將被刪除。 `libraries` *libman.json*屬性中的庫文件關聯配置不會被刪除。

### <a name="synopsis"></a>概要

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>選項。

以下是使用 `libman clean` 命令時可用的選項：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

要刪除透過 LibMan 安裝的庫檔,

```console
libman clean
```

## <a name="uninstall-library-files"></a>卸載庫檔案

指令`libman uninstall`:

* 從*libman.json*中的目標中刪除與指定庫關聯的所有檔。
* 從*libman.json*中刪除關聯的庫配置。

在以下情況時發生錯誤:

* 專案根中不存在*libman.json*檔。
* 指定的庫不存在。

如果安裝了多個同名庫,系統將提示您選擇一個庫。

### <a name="synopsis"></a>概要

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

要卸載的庫的名稱。 此名稱可能包含版本號表示法(例如, `@1.2.0`。

### <a name="options"></a>選項。

以下是使用 `libman uninstall` 命令時可用的選項：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

請考慮以下*利伯曼.json*檔:

[!code-json[](samples/LibManSample/libman.json)]

* 要卸載 jQuery,以下任一命令都成功:

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* 要卸載透過提供程式安裝的 Lodash`filesystem`檔,請執行以下操作:

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>更新函式庫版本

該`libman update`命令將透過 LibMan 安裝的庫更新為指定的版本。

在以下情況時發生錯誤:

* 專案根中不存在*libman.json*檔。
* 指定的庫不存在。

如果安裝了多個同名庫,系統將提示您選擇一個庫。

### <a name="synopsis"></a>概要

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

要更新的庫的名稱。

### <a name="options"></a>選項。

以下是使用 `libman update` 命令時可用的選項：

* `-pre`

  獲取庫的最新預發行版本。

* `--to <VERSION>`

  獲取庫的特定版本。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

* 要將 jQuery 更新到最新版本:

  ```console
  libman update jquery
  ```

* 要將 jQuery 更新到版本 3.3.1:

  ```console
  libman update jquery --to 3.3.1
  ```

* 要將 jQuery 更新到最新的預發行版本:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>管理庫快取

該`libman cache`命令管理 LibMan 庫緩存。 提供程式`filesystem`不使用庫緩存。

### <a name="synopsis"></a>概要

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>引數

`PROVIDER`

僅與命令`clean`一起使用。 指定要清理的提供程式快取。 有效值包括：

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>選項。

以下是使用 `libman cache` 命令時可用的選項：

* `--files`

  列出快取的檔案名稱。

* `--libraries`

  列出緩存的庫的名稱。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

* 要檢視每個提供程式快取庫的名稱,請使用以下指令之一:

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

* 要查看每個提供程式快取的庫文件的名稱,

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

  請注意,前面的輸出顯示 jQuery 版本 3.2.1 和 3.3.1 緩存在 CDNJS 提供程式下。

* 要清空 CDNJS 提供者的庫快取,可以:

  ```console
  libman cache clean cdnjs
  ```

  清空 CDNJS 提供程式快取後`libman cache list`,這個指令會顯示以下內容:

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

* 要清空所有受支援的提供程式的快取,可以:

  ```console
  libman cache clean
  ```

  清空所有提供程式快取後,`libman cache list`這個命令會顯示以下內容:

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
