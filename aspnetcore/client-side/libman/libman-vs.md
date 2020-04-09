---
title: 在視覺工作室中將 LibMan 與ASP.NET核心一起使用
author: scottaddie
description: 瞭解如何在視覺工作室的ASP.NET核心專案中使用 LibMan。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: e92e6bc28ec58b26785dd6c79e71512368202a26
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658309"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>在視覺工作室中將 LibMan 與ASP.NET核心一起使用

作者：[Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio 在 ASP.NET 核心專案中為[LibMan](xref:client-side/libman/index)提供了內建支援,包括:

* 支援在生成時配置和運行 LibMan 還原操作。
* 用於觸發 LibMan 還原和清潔操作的功能表項。
* 搜尋對話框以查找庫並將檔添加到專案中。
* 編輯對*Libman.json*&mdash;的Libman清單檔的支援。

[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/client-side/libman/samples/)[(如何下載)](xref:index#how-to-download-a-sample)

## <a name="prerequisites"></a>Prerequisites

* [視覺工作室 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)與**ASP.NET和網路開發**工作負載

## <a name="add-library-files"></a>新增函式庫檔案

函式庫檔案可以透過兩種不同的方式加入 ASP.NET 核心專案中:

1. [使用「新增用戶端-側庫」對話框](#use-the-add-client-side-library-dialog)
1. [手動設定 LibMan 清單檔項目](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>使用「新增用戶端-側庫」對話框

依以下步驟安裝用戶端庫:

* 在**解決方案資源管理器**中,右鍵單擊應在其中添加檔的項目資料夾。 選擇 **「添加** > **用戶端庫**」。 將顯示 **'新增用戶端-端庫**'對話框:

  ![新增客戶端函式庫對話框](_static/add-library-dialog.png)

* 從 **「提供程式**」下拉清單中選擇庫提供程式。 CDNJS 是預設提供程式。
* 在 **「庫**」 文字框中鍵入要提取的庫名稱。 IntelliSense 提供以提供的文本開頭的庫清單。
* 從"感知"列表中選擇庫。 請注意,庫名稱後綴在`@`符號和所選提供者已知的最新穩定版本中。
* 決定要包括哪些檔案:
  * 選擇「**包括所有庫檔案**單選按鈕」 以包括庫的所有檔案。
  * 選擇 **「選擇特定檔案**單選按鈕」 以包括庫檔案的子集。 選擇單選按鈕後,將啟用檔案選擇器樹。 選中要下載的檔名左側的複選框。
* 指定用於在 **「目標位置」** 文字框中儲存檔案的項目資料夾。 作為建議,將每個庫存儲在單獨的資料夾中。

  建議**的目標位置**資料夾基於對話框啟動的位置:

  * 如果從專案根部啟動:
    * 如果*存在 wwwroot,* 則使用*wwwroot/lib。*
    * 如果*wwwroot*不存在,則使用*lib。*
  * 如果從項目資料夾啟動,則使用相應的資料夾名稱。

  資料夾建議後綴在庫名稱中。 下表說明了在 Razor Pages 專案中安裝 jQuery 時的資料夾建議。
  
  |啟動位置                           |建議資料夾      |
  |------------------------------------------|----------------------|
  |專案根(如果存在*wwwroot)*        |*wwwroot/lib/jquery/* |
  |專案根(如果*wwwroot*不存在) |*lib/jquery/*         |
  |專案中*的頁面*資料夾                 |*頁面/jquery/*       |

* 按下 **「安裝**」按鈕下載檔案,根據*libman.json*中的配置。
* 檢視 **「輸出**」視窗的**庫管理員**,瞭解安裝詳細資訊。 例如：

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a>手動設定 LibMan 清單檔項目

Visual Studio 中的所有 LibMan 操作都基於專案根的 LibMan 清單 *(libman.json)* 的內容。 您可以手動編輯*libman.json*來設定專案的庫檔。 可視化工作室在*保存 libman.json*後恢復所有庫檔。

要開啟*libman.json*進行編輯,存在以下選項:

* 雙擊**解決方案資源管理員**中的*libman.json*檔案。
* 右鍵單擊**解決方案資源管理員**中的專案,然後選擇 **「管理用戶端庫**」。 **&#8224;**
* 從可視化工作室**項目**功能表中選擇 **「管理用戶端端庫**」。 **&#8224;**

**&#8224;** 如果*libman.json*檔在專案根中不存在,它將使用預設項範本內容創建。

Visual Studio 提供豐富的 JSON 編輯支援,如著色、格式設置、IntelliSense 和架構驗證。 LibMan 清單的 JSON[https://json.schemastore.org/libman](https://json.schemastore.org/libman)架構位於 。

使用以下清單檔,LibMan`libraries`根據 屬性中定義的設定檢索檔。 在以下範圍內`libraries`定義的物件文本的說明如下:

* 從 CDNJS 提供程式檢索[jQuery](https://jquery.com/)版本 3.3.1 的子集。 `files`子集在屬性&mdash;*jquery.min.js、jquery.js*和*jquery.min.map*中定義。 *jquery.js* 這些檔被放置在專案的*wwwroot/lib/jquery*資料夾中。
* 將檢索整個[Bootstrap](https://getbootstrap.com/)版本 4.1.3 並將其放置在*wwwroot/lib/引導資料夾中*。 物件文字的屬性`provider``defaultProvider`覆蓋 屬性值。 LibMan 從 unpkg 提供程式檢索引導檔。
* [Lodash](https://lodash.com/)的子集得到了組織內的一個理事機構的批准。 *lodash.js*和*lodash.min.js*檔從*C:\\臨時\\lodash\\*的本地文件系統中檢索。 這些檔案將複製到專案的*wwwroot/lib/lodash*資料夾。

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan 僅支援每個提供程式中每個庫的一個版本。 如果*libman.json*檔包含兩個庫具有相同的給定提供程式的庫名,則它無法進行架構驗證。

## <a name="restore-library-files"></a>復原庫檔案

要從 Visual Studio 中還原庫檔,專案根中必須有一個有效的*libman.json*檔。 還原的檔案放置在為每個庫指定的位置的專案中。

函式庫檔案可以通過兩種方式在ASP.NET核心專案中還原:

1. [產生期間還原檔案](#restore-files-during-build)
1. [手動回復檔案](#restore-files-manually)

### <a name="restore-files-during-build"></a>產生期間還原檔案

LibMan 可以還原定義的庫文件作為生成過程的一部分。 默認情況下,將禁用*生成時還原*行為。

要啟用和測試產生時還原行為,請:

* 右鍵按一下**解決方案資源管理程式**中的*libman.json,* 然後從上下文選單中選擇 **「在生成上啟用還原用戶端端庫**」。
* 當提示安裝 NuGet 包時,按下「**是**」按鈕。 [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet 包被添加到專案中:

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* 生成專案以確認 LibMan 檔還原發生。 該`Microsoft.Web.LibraryManager.Build`包注入一個 MSBuild 目標,該目標在專案的生成操作期間運行 LibMan。
* 檢視 LibMan 活動紀錄的**輸出「視窗**的**產生**來源:

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

啟用生成還原行為後 *,libman.json*上下文菜單**會在生成上顯示"禁用還原用戶端端庫"** 選項。 選擇此選項會`Microsoft.Web.LibraryManager.Build`從專案檔中刪除包引用。 因此,用戶端庫不再在每個生成上還原。

無論生成時還原設置如何,您都可以隨時從*libman.json*上下文菜單手動還原。 有關詳細資訊,請參閱[手動回復檔](#restore-files-manually)。

### <a name="restore-files-manually"></a>手動回復檔案

手動還原函式庫檔:

* 對解決方案中的所有專案:
  * 右鍵單擊**解決方案資源管理器**中的解決方案名稱。
  * 選擇 **「還原用戶端庫」** 選項。
* 對特定項目:
  * 右鍵按一下**解決方案資源管理程式**中的*libman.json*檔。
  * 選擇 **「還原用戶端庫」** 選項。

回復執行時:

* "視覺工作室"狀態列上的任務狀態中心 (TSC) 圖示將設置動畫,並將讀取 *"恢復操作已啟動*"。 按一下該圖示將打開一個工具提示,其中列出了已知的後台任務。
* 訊息將發送到**輸出視窗的狀態**列和**庫管理員**來源。 例如：

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a>刪除庫檔案

要執行*乾淨*操作,請刪除以前在 Visual Studio 中還原的庫檔,

* 右鍵按一下**解決方案資源管理程式**中的*libman.json*檔。
* 選擇 **「清理用戶端-側庫」** 選項。

為了防止意外刪除非庫檔,清理操作不會刪除整個目錄。 它僅刪除上一個還原中包含的檔。

執行清潔操作時:

* Visual Studio 狀態列上的 TSC 圖示會設定動畫,並將讀取*客戶端庫操作啟動*。 按一下該圖示將打開一個工具提示,其中列出了已知的後台任務。
* 訊息將發送到**輸出視窗的狀態**列和**庫管理員**來源。 例如：

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

清除操作僅從項目中刪除檔。 庫檔保留在緩存中,以便在未來還原操作中更快地檢索。 要管理儲存在本地電腦快取中的函式庫檔案,請使用[LibMan CLI](xref:client-side/libman/libman-cli)。

## <a name="uninstall-library-files"></a>卸載庫檔案

要卸載庫檔:

* 開啟*利伯曼.json*。
* 將 caret 放置`libraries`在相應的 物件文本中。
* 按下左邊邊界中顯示的燈泡圖示,然後選擇 **「卸\<載\<library_name>* library_version>:**

  ![卸載庫上下文選單選項](_static/uninstall-menu-option.png)

或者,您可以手動編輯和保存 LibMan 清單 *(libman.json*)。 儲存檔案時,[還原操作](#restore-library-files)將執行。 不再在*libman.json*中定義的庫檔將從專案中刪除。

## <a name="update-library-version"></a>更新函式庫版本

要檢查更新的庫版本,

* 開啟*利伯曼.json*。
* 將 caret 放置`libraries`在相應的 物件文本中。
* 按一下左側邊距中顯示的燈泡圖示。 將滑鼠懸停在 **「檢查更新**」。

LibMan 檢查庫版本比安裝的版本更新。 可能會出現以下結果:

* 如果已安裝最新版本,則顯示 **「未找到的更新**」消息。
* 如果未安裝,將顯示最新的穩定版本。

  ![檢查更新內容選單選項](_static/update-menu-option.png)

* 如果具有比已安裝版本更新的預先發佈可用,則顯示預先發佈。

要降級為較舊的庫版本,請手動編輯*libman.json*檔。 儲存檔案後, LibMan[回復操作](#restore-library-files):

* 從以前的版本中刪除冗餘檔。
* 從新版本添加新和更新的檔。

## <a name="additional-resources"></a>其他資源

* <xref:client-side/libman/libman-cli>
* [LibMan GitHub 存放庫](https://github.com/aspnet/LibraryManager)
