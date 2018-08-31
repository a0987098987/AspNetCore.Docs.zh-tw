---
title: 使用 ASP.NET Core 使用 LibMan 命令列介面 (CLI)
author: scottaddie
description: 了解如何在 ASP.NET Core 專案中使用 LibMan 命令列介面 (CLI)。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: ad81af2e789a31382f50ed37754bfc94469eb197
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336033"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>使用 ASP.NET Core 使用 LibMan 命令列介面 (CLI)

作者：[Scott Addie](https://twitter.com/Scott_Addie)

[LibMan](xref:client-side/libman/index) CLI 是一種跨平台工具，支援.NET Core 支援的每個地方。

## <a name="prerequisites"></a>必要條件

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>安裝

若要安裝 LibMan CLI:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

A [.NET Core 全域工具](/dotnet/core/tools/global-tools#install-a-global-tool)從安裝[Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet 套件。

若要從特定的 NuGet 套件來源安裝 LibMan CLI:

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

在上述範例中，從本機的 Windows 電腦安裝.NET Core 全域工具*C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*檔案。

## <a name="usage"></a>使用量

安裝成功後的 cli，您可以使用下列命令：

```console
libman
```

若要檢視已安裝的 CLI 版本：

```console
libman --version
```

若要檢視可用的 CLI 命令：

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

下列各節說明可用的 CLI 命令。

## <a name="initialize-libman-in-the-project"></a>在專案中初始化 LibMan

`libman init`命令會建立*libman.json*檔案如果不存在。 建立檔案是使用預設項目範本內容。

### <a name="synopsis"></a>概要

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>選項

下列選項可供`libman init`命令：

* `-d|--default-destination <PATH>`

  相對於目前的資料夾路徑。 如果沒有，將會安裝在此位置的程式庫檔案`destination`屬性定義在程式庫*libman.json*。 `<PATH>`值會寫入至`defaultDestination`屬性*libman.json*。

* `-p|--default-provider <PROVIDER>`

  如果沒有提供者已定義特定的程式庫所使用的提供者。 `<PROVIDER>`值會寫入至`defaultProvider`屬性*libman.json*。 取代`<PROVIDER>`具有下列值之一：

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

若要建立*libman.json* ASP.NET Core 專案中的檔案：

* 瀏覽至專案根目錄。
* 執行下列命令：

  ```console
  libman init
  ```

* 輸入名稱的預設提供者或按`Enter`使用預設 CDNJS 提供者。 有效值包括：

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init 命令-預設提供者](_static/libman-init-provider.png)

A *libman.json*檔案新增至專案根目錄中，以下列內容：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>新增程式庫檔案

`libman install`命令會下載並安裝至專案的程式庫檔案。 A *libman.json*檔案會新增，如果不存在。 *Libman.json*檔案遭到修改儲存的程式庫檔案的組態詳細資料。

### <a name="synopsis"></a>概要

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

若要安裝的程式庫名稱。 此名稱可能包含版本號碼的標記法 (例如`@1.2.0`)。

### <a name="options"></a>選項

下列選項可供`libman install`命令：

* `-d|--destination <PATH>`

  要安裝的程式庫的位置。 如果未指定，則會使用預設位置。 如果沒有`defaultDestination`屬性中指定*libman.json*，這是必要選項。

* `--files <FILE>`

  指定要從程式庫安裝的檔案名稱。 如果未指定，從程式庫的所有檔案會都安裝。 提供一個`--files`安裝每個檔案的選項。 也支援相對路徑。 例如：`--files dist/browser/signalr.js`。

* `-p|--provider <PROVIDER>`

  若要使用程式庫取得提供者的名稱。 取代`<PROVIDER>`具有下列值之一：
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  如果未指定，`defaultProvider`中的屬性*libman.json*用。 如果沒有`defaultProvider`屬性中指定*libman.json*，這是必要選項。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

請考慮下列*libman.json*檔案：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

若要安裝的 jQuery 版本 3.2.1 *jquery.min.js*的檔案*wwwroot\scripts\jquery*使用 CDNJS 提供者的資料夾：

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot\scripts\jquery --files jquery.min.js
```

*Libman.json*檔案如下所示：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

若要安裝*calendar.js*並*calendar.css*從檔案*c:\\temp\\contosoCalendar\\* 使用檔案系統提供者：

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

有兩個原因，顯示下列提示：

* *Libman.json*檔案未包含`defaultDestination`屬性。
* `libman install`命令不包含`-d|--destination`選項。

![libman 安裝命令-目的地](_static/libman-install-destination.png)

接受預設的目的地之後, *libman.json*檔案如下所示：

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot\\lib\\contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a>將程式庫檔案還原

`libman restore`命令會在安裝程式庫檔案中定義*libman.json*。 可套用下列規則：

* 如果沒有*libman.json*檔案位於專案根目錄，則會傳回錯誤。
* 如果程式庫指定提供者，`defaultProvider`中的屬性*libman.json*會被忽略。
* 如果程式庫指定目的地`defaultDestination`中的屬性*libman.json*會被忽略。

### <a name="synopsis"></a>概要

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>選項

下列選項可供`libman restore`命令：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

若要還原程式庫檔案中定義*libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>刪除程式庫檔案

`libman clean`命令會刪除先前還原透過 LibMan 的程式庫檔案。 這項作業後會變成空的資料夾被刪除。 相關聯的程式庫檔案中的組態`libraries`的屬性*libman.json*不會被移除。

### <a name="synopsis"></a>概要

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>選項

下列選項可供`libman clean`命令：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

若要刪除透過 LibMan 安裝的程式庫檔案：

```console
libman clean
```

## <a name="uninstall-library-files"></a>解除安裝程式庫檔案

`libman uninstall`命令：

* 刪除指定的程式庫中的目的地從相關聯的所有檔案*libman.json*。
* 移除相關聯的文件庫設定*libman.json*。

發生錯誤時：

* 否*libman.json*檔案位於專案根目錄。
* 指定的程式庫不存在。

如果已安裝具有相同名稱的多個程式庫，系統會提示您選擇其中一個。

### <a name="synopsis"></a>概要

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

若要解除安裝程式庫的名稱。 此名稱可能包含版本號碼的標記法 (例如`@1.2.0`)。

### <a name="options"></a>選項

下列選項可供`libman uninstall`命令：

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

請考慮下列*libman.json*檔案：

[!code-json[](samples/LibManSample/libman.json)]

* 若要解除安裝 jQuery，其中一個下列的命令會成功：

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* 若要解除安裝安裝透過 Lodash 檔案`filesystem`提供者：

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>更新程式庫版本

`libman update`命令會更新文件庫透過 LibMan 安裝至指定的版本。

發生錯誤時：

* 否*libman.json*檔案位於專案根目錄。
* 指定的程式庫不存在。

如果已安裝具有相同名稱的多個程式庫，系統會提示您選擇其中一個。

### <a name="synopsis"></a>概要

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>引數

`LIBRARY`

要更新的程式庫名稱。

### <a name="options"></a>選項

下列選項可供`libman update`命令：

* `-pre`

  取得程式庫的最新發行前版本。

* `--to <VERSION>`

  取得特定版本的程式庫。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

* 若要更新為最新版本的 jQuery:

  ```console
  libman update jquery
  ```

* 若要更新 jQuery 版本 3.3.1:

  ```console
  libman update jquery --to 3.3.1
  ```

* 若要更新為最新的發行前版本的 jQuery:

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>管理程式庫快取

`libman cache`命令管理 LibMan 程式庫快取。 `filesystem`提供者不使用程式庫快取。

### <a name="synopsis"></a>概要

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>引數

`PROVIDER`

僅能搭配`clean`命令。 指定提供者快取清除。 有效值包括：

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>選項

下列選項可供`libman cache`命令：

* `--files`

  列出快取的檔案的名稱。

* `--libraries`

  列出快取程式庫名稱。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>範例

* 若要檢視每個提供者的快取程式庫名稱，使用下列命令之一：

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

* 若要檢視每個提供者的快取程式庫檔案的名稱：

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

  請注意上述的輸出顯示的 jQuery 版本 3.2.1 和 3.3.1 CDNJS 提供者下快取。

* 若要清空 CDNJS 提供者的程式庫快取：

  ```console
  libman cache clean cdnjs
  ```

  之後清空 CDNJS 提供者快取，`libman cache list`命令會顯示下列：

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

* 若要清空快取所有支援的提供者：

  ```console
  libman cache clean
  ```

  之後清空所有提供者快取，`libman cache list`命令會顯示下列：

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>其他資源

* [安裝的通用工具](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [LibMan GitHub 存放庫](https://github.com/aspnet/LibraryManager)
