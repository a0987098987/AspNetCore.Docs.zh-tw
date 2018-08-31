---
title: 使用 Visual Studio 中的 ASP.NET Core 使用 LibMan
author: scottaddie
description: 了解如何使用 Visual Studio 的 ASP.NET Core 專案中使用 LibMan。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: a653b1a5c07feca8672ba38e0cda3ddc30482c5a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312175"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a>使用 Visual Studio 中的 ASP.NET Core 使用 LibMan

作者：[Scott Addie](https://twitter.com/Scott_Addie)

Visual Studio 的內建支援[LibMan](xref:client-side/libman/index)在 ASP.NET Core 專案中，包括：

* 設定和執行組建 LibMan 還原作業的支援。
* 觸發 LibMan 還原和清除作業的功能表項目。
* 尋找程式庫及將檔案新增至專案的 [搜尋] 對話方塊。
* 編輯支援*libman.json*&mdash;LibMan 資訊清單檔案。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)

## <a name="prerequisites"></a>必要條件

* Visual Studio 2017 版 15.8 或更新版本以及**ASP.NET 和 web 開發**工作負載

## <a name="add-library-files"></a>新增程式庫檔案

程式庫檔案可以新增至 ASP.NET Core 專案中，兩個不同的方式：

1. [使用 [新增用戶端程式庫] 對話方塊](#use-the-add-client-side-library-dialog)
1. [手動設定 LibMan 資訊清單檔案項目](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a>使用 [新增用戶端程式庫] 對話方塊

請遵循下列步驟來安裝用戶端程式庫：

* 在 [**方案總管] 中**，以滑鼠右鍵按一下專案資料夾中的檔案應該新增。 選擇**新增** > **用戶端程式庫**。 **新增用戶端程式庫**對話方塊隨即出現：

  ![新增用戶端程式庫 對話方塊](_static/add-library-dialog.png)

* 選取 從程式庫提供者**提供者**下拉式清單。 CDNJS 是預設提供者。
* 類型程式庫名稱，在擷取**程式庫**文字方塊。 IntelliSense 提供一份文件庫開始提供的文字。
* 從 [IntelliSense] 清單中選取程式庫。 請注意，程式庫名稱會加上`@`符號和已知的所選的提供者的最新穩定版本。
* 決定要包含哪些檔案：
  * 選取 **包括所有的程式庫檔**選項按鈕，以包含所有的程式庫的檔案。
  * 選取 **選擇特定的檔案**選項按鈕，以包含程式庫檔案的子集。 選取選項按鈕時，會啟用檔案選取器樹狀目錄。 請檢查要下載的檔案名稱的左邊的方塊。
* 指定儲存在檔案的專案資料夾**目標位置**文字方塊。 建議，另一個資料夾中儲存每個程式庫。

  建議**目標位置**資料夾以啟動 [] 對話方塊的位置為基礎：

  * 如果從專案根目錄中啟動：
    * *wwwroot/lib*如果使用*wwwroot*存在。
    * *lib*如果使用*wwwroot*不存在。
  * 如果啟動專案資料夾中，會使用對應的資料夾名稱。

  程式庫名稱會加上資料夾的建議。 下表說明資料夾建議，Razor 頁面專案中安裝 jQuery 時。
  
  |啟動位置                           |建議的資料夾      |
  |------------------------------------------|----------------------|
  |專案根目錄 (如果*wwwroot*存在)        |*wwwroot/lib/jquery /* |
  |專案根目錄 (如果*wwwroot*不存在) |*lib/jquery /*         |
  |*頁面*專案中的資料夾                 |*頁面/jquery /*       |

* 按一下 [**安裝**] 按鈕，下載的檔案，每個在組態*libman.json*。
* 檢閱**程式庫管理員**摘要**輸出**安裝詳細資料 視窗。 例如: 

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

### <a name="manually-configure-libman-manifest-file-entries"></a>手動設定 LibMan 資訊清單檔案項目

在 Visual Studio 中的所有 LibMan 作業為都基礎的專案根目錄 LibMan 資訊清單的內容 (*libman.json*)。 您可以手動編輯*libman.json*設定專案的程式庫檔案。 Visual Studio 中還原所有的程式庫檔案一次*libman.json*儲存。

若要開啟  *libman.json*進行編輯，有下列選項：

* 按兩下*libman.json*中的檔案**方案總管 中**。
* 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**管理用戶端程式庫**。 **&#8224;**
* 選取 **管理的用戶端程式庫**從 Visual Studio**專案**功能表。 **&#8224;**

**&#8224;** 如果*libman.json*檔案不存在的專案根目錄中，將會使用預設項目範本內容建立。

Visual Studio 提供豐富編輯支援，像是顏色標示、 格式、 IntelliSense 和結構描述驗證的 JSON。 LibMan 資訊清單的 JSON 結構描述位於[ http://json.schemastore.org/libman ](http://json.schemastore.org/libman)。

下列資訊清單檔案時，LibMan 擷取檔案中定義的組態每`libraries`屬性。 物件常值中定義的說明`libraries`遵循：

* 子集[jQuery](https://jquery.com/) 3.3.1 版會從 CDNJS 提供者。 中所定義的子集`files`屬性&mdash;*jquery.min.js*， *jquery.js*，以及*jquery.min.map*。 檔案會放在專案的*wwwroot/lib/jquery*資料夾。
* 將整個[Bootstrap](https://getbootstrap.com/) 4.1.3 版會擷取並放置於*wwwroot/lib/啟動程序*資料夾。 物件常值`provider`屬性會覆寫`defaultProvider`屬性值。 LibMan 擷取 unpkg 提供者的啟動程序的檔案。
* 子集[Lodash](https://lodash.com/)已核准的組織內的控管主體。 *Lodash.js*並*lodash.min.js*檔案會從本機檔案系統中擷取*c:\\temp\\lodash\\*。 檔案會複製到專案的*wwwroot/lib/lodash*資料夾。

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> LibMan 僅支援一個版本從每個提供者的每個程式庫。 *Libman.json*檔案無法通過結構描述驗證，如果它包含具有相同的程式庫名稱，指定提供者的兩個程式庫。

## <a name="restore-library-files"></a>將程式庫檔案還原

若要還原從 Visual Studio 內的程式庫檔案，必須有有效*libman.json*專案根目錄中的檔案。 已還原的檔案會放置在專案中指定的每個程式庫的位置。

您可以在 ASP.NET Core 專案中有兩種還原程式庫檔案：

1. [在建置時還原檔案](#restore-files-during-build)
1. [以手動方式還原檔案](#restore-files-manually)

### <a name="restore-files-during-build"></a>在建置時還原檔案

LibMan 可以還原的已定義的程式庫檔案做為建置程序的一部分。 根據預設，*還原上建置*停用行為。

若要啟用並測試還原上建置行為：

* 以滑鼠右鍵按一下*libman.json*中**方案總管**，然後選取**啟用還原用戶端程式庫建置**從內容功能表。
* 按一下 **是**按鈕時提示您安裝 NuGet 套件。 [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet 封裝加入至專案：

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* 建置專案，以確認 LibMan 檔案還原，就會發生。 `Microsoft.Web.LibraryManager.Build`套件插入專案的建立作業期間執行 LibMan MSBuild 目標。
* 檢閱**建置**摘要**輸出**LibMan 活動記錄 視窗：

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

啟用還原上建置行為時， *libman.json*操作功能表會顯示**停用還原用戶端程式庫建置**選項。 選取此選項會移除`Microsoft.Web.LibraryManager.Build`套件從專案檔的參考。 因此，用戶端程式庫不再會在每個組建上還原。

不論還原上組建設定中，您可以手動還原的任何時候*libman.json*操作功能表。 如需詳細資訊，請參閱 <<c0> [ 以手動方式還原檔案](#restore-files-manually)。

### <a name="restore-files-manually"></a>以手動方式還原檔案

若要以手動方式還原程式庫檔案：

* 在方案中的所有專案：
  * 中的方案名稱上按一下滑鼠右鍵**方案總管 中**。
  * 選取 **還原用戶端程式庫**選項。
* 針對特定專案：
  * 以滑鼠右鍵按一下*libman.json*中的檔案**方案總管 中**。
  * 選取 **還原用戶端程式庫**選項。

雖然還原作業正在執行：

* 在 Visual Studio 的 [狀態] 列上的工作狀態中心 (TSC) 圖示會變成動畫，以及將讀取*還原作業已開始*。 按一下圖示即可開啟列出的已知的背景工作的工具提示。
* 會將訊息傳送至 [狀態] 列與**程式庫管理員**摘要**輸出**視窗。 例如: 

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

## <a name="delete-library-files"></a>刪除程式庫檔案

若要執行*全新*作業，這會刪除先前還原 Visual Studio 中的程式庫檔案：

* 以滑鼠右鍵按一下*libman.json*中的檔案**方案總管 中**。
* 選取 **全新的用戶端程式庫**選項。

若要防止意外移除程式庫檔案，清除作業並不會刪除整個目錄。 它只會移除已包含在前一個還原的檔案。

當正在執行清除的作業：

* Visual Studio 的 [狀態] 列 TSC 圖示項目建立動畫，並將讀取*用戶端程式庫作業啟動*。 按一下圖示即可開啟列出的已知的背景工作的工具提示。
* 訊息會傳送至 [狀態] 列與**程式庫管理員**摘要**輸出**視窗。 例如: 

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

清除作業只會刪除從專案的檔案。 程式庫檔案保持在未來的還原作業上更快速地擷取的快取中。 若要管理儲存在本機電腦的快取中的程式庫檔案，請使用[LibMan CLI](xref:client-side/libman/libman-cli)。

## <a name="uninstall-library-files"></a>解除安裝程式庫檔案

若要解除安裝程式庫檔案：

* 開啟*libman.json*。
* 放置在對應的插入號`libraries`物件常值。
* 按一下左邊界中，會出現燈泡圖示，然後選取**解除安裝\<library_name > @\<library_version >**:

  ![解除安裝程式庫操作功能表選項](_static/uninstall-menu-option.png)

或者，您可以手動編輯並儲存 LibMan 資訊清單 (*libman.json*)。 [還原作業](#restore-library-files)時儲存檔案時執行。 不會再定義中的程式庫檔案*libman.json*從專案中移除。

## <a name="update-library-version"></a>更新程式庫版本

若要檢查有更新的程式庫版本：

* 開啟*libman.json*。
* 放置在對應的插入號`libraries`物件常值。
* 按一下燈泡圖示出現在左邊界中。 將滑鼠停留**檢查是否有更新**。

LibMan 會檢查程式庫版本比安裝的版本還新。 可能會發生下列結果：

* A**找不到更新**如果已安裝最新版本，就會顯示訊息。
* 最新穩定版本會顯示如果沒有已安裝。

  ![檢查有更新快顯功能表選項](_static/update-menu-option.png)

* 如果可用的發行前版本比已安裝的版本還新，則會顯示發行前版本。

若要降級至較舊的程式庫版本，請以手動方式編輯*libman.json*檔案。 當儲存檔案時，LibMan[還原作業](#restore-library-files):

* 從先前版本中移除多餘的檔案。
* 從新的版本中加入全新和更新的檔案。

## <a name="additional-resources"></a>其他資源

* <xref:client-side/libman/libman-cli>
* [LibMan GitHub 存放庫](https://github.com/aspnet/LibraryManager)
