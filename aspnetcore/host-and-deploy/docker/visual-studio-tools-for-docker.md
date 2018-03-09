---
title: "搭配 ASP.NET Core 使用 Visual Studio Tools for Docker"
author: spboyer
description: "了解如何使用適用於 Windows 的 Visual Studio 2017 工具和 Docker 對 ASP.NET Core 應用程式進行容器化。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: 590d32342b1724a0cbc937655c35631938eb09b2
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2018
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>搭配 ASP.NET Core 使用 Visual Studio Tools for Docker

[Visual Studio 2017](https://www.visualstudio.com/) 支援建置、偵錯和執行以 .NET Framework 或 .NET Core 為目標的 ASP.NET Core 應用程式。 同時支援 Windows 和 Linux 容器。

## <a name="prerequisites"></a>必要條件

* 已安裝「.NET Core 跨平台開發」工作負載的 [Visual Studio 2017](https://www.visualstudio.com/)
* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>安裝和設定

若要安裝 Docker，請檢閱 [Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) (Docker for Windows：安裝須知) 的資訊，並安裝 [Docker For Windows](https://docs.docker.com/docker-for-windows/install/)。

Docker for Windows 中的 **[Shared Drives](https://docs.docker.com/docker-for-windows/#shared-drives)** (共用磁碟機) 必須設定為支援磁碟區對應和偵錯。 以滑鼠右鍵按一下系統匣 Docker 圖示，選取**設定...**，然後選取**共用磁碟機**。 選取 Docker 儲存檔案的磁碟機。 選取**套用**。

![共用磁碟機](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> 15.6 和更新版本的 visual Studio 2017 版本提示時**共用磁碟機**未設定。

## <a name="add-docker-support-to-an-app"></a>將 Docker 支援新增至應用程式

若要將 Docker 的支援加入 ASP.NET Core 專案時，專案必須為目標.NET Core。 支援的 Linux 和 Windows 容器。

將 Docker 的支援加入至專案，選擇 Windows 或 Linux 容器。 Docker 主機必須執行相同的容器類型。 若要變更執行中 Docker 執行個體中的容器類型，請以滑鼠右鍵按一下系統匣的 Docker 圖示，然後選擇 [Switch to Windows containers...] (切換至 Windows 容器...) 或 [Switch to Linux containers] (切換至 Linux 容器...)。

### <a name="new-app"></a>新增應用程式

使用 **ASP.NET Core Web 應用程式**專案範本來建立新的應用程式時，請選取 [Enable Docker Support] ( (啟用 Docker 支援) 核取方塊：

![啟用 Docker 支援核取方塊](visual-studio-tools-for-docker/_static/enable-docker-support-checkbox.png)

如果目標 framework 是.NET Core **OS**下拉式清單可讓容器類型選取項目。

### <a name="existing-app"></a>現有應用程式

Visual Studio Tools for Docker 不支援將 Docker 新增至以 .NET Framework 為目標的現有 ASP.NET Core 專案。 針對以 .NET Core 為目標的 ASP.NET Core 專案，有兩個選項可透過工具來新增 Docker 支援。 在 Visual Studio 中開啟專案，然後選擇下列其中一個選項：

* 選取 [專案] 功能表的 [Docker 支援]。
* 以滑鼠右鍵按一下方案總管中的專案，然後選取 [新增] > [Docker 支援]。

## <a name="docker-assets-overview"></a>Docker 資產概觀

Visual Studio Tools for Docker 會將 *docker-compose* 專案新增至方案，包含下列項目：

* *.dockerignore*：包含要在產生組建內容時排除的檔案和目錄模式清單。
* *docker-compose.yml*︰基礎 [Docker Compose](https://docs.docker.com/compose/overview/) 檔案，用於定義要分別使用 `docker-compose build` 和 `docker-compose run` 建置並執行的映像集合。
* *docker-compose.override.yml*：透過 Docker Compose 讀取的選擇性檔案，內含服務的組態覆寫。 Visual Studio 會執行 `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` 來合併這些檔案。

*Dockerfile*，是用於建立最終 Docker 映像的配方，會新增至專案根目錄。 請參閱 [Dockerfile 參考](https://docs.docker.com/engine/reference/builder/)，以了解其內的命令。 這個特定 *Dockerfile* 使用[多階段建置](https://docs.docker.com/engine/userguide/eng-image/multistage-build/)，內含四個不同的具名建置階段：

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Dockerfile* 是根據 [microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) 映像。 此基底映像包含 ASP.NET Core NuGet 套件，已經過預先 JIT 編譯改善啟動效能。

*Docker compose.yml*檔案包含執行專案時所建立的映像的名稱：

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

在上述範例中，`image: hellodockertools` 會在以**偵錯**模式執行應用程式時產生 `hellodockertools:dev` 映像。 以**發行**模式執行應用程式時，會產生 `hellodockertools:latest` 映像。

使用映像名稱的首碼[Docker Hub](https://hub.docker.com/)使用者名稱 (例如， `dockerhubusername/hellodockertools`) 如果映像將會推送至登錄。 或者，變更 映像名稱，包括私用登錄 URL (例如， `privateregistry.domain.com/hellodockertools`) 視組態而定。

## <a name="debug"></a>偵錯

從工具列的偵錯下拉式清單中選取 [Docker]，然後開始對應用程式進行偵錯。 [輸出] 視窗的 **Docker** 檢視會顯示下列將採取的動作：

* *Microsoft/aspnetcore*取得執行階段影像 （如果尚未在快取）。
* *Microsoft/aspnetcore 建置*編譯/發佈映像會取得 （如果尚未在快取）。
* 在容器內，*ASPNETCORE_ENVIRONMENT* 環境變數設定為 `Development`。
* 連接埠 80 會公開並對應至本機主機的動態指派連接埠。 連接埠是由 Docker 主機所決定，並且可以使用 `docker ps` 命令進行查詢。
* 應用程式會複製到容器。
* 預設的瀏覽器就會啟動偵錯工具附加至使用動態指派連接埠的容器。 

產生的 Docker 映像*dev*的應用程式與映像*microsoft/aspnetcore*做為基底映像的映像。 在 [套件管理員主控台] (PMC) 視窗中，執行 `docker images` 命令。 會顯示在電腦上的映像：

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> 開發人員映像缺少應用程式內容，做為**偵錯**設定使用磁碟區掛接提供反覆的體驗。 若要推送映像，請使用 [發行] 組態。

在 PMC 中執行 `docker ps` 命令。 請注意是使用容器來執行應用程式：

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>編輯後繼續

針對靜態檔案和 Razor 檢視所做的變更會自動更新，而不需要編譯步驟。 進行變更並儲存，然後重新整理瀏覽器來檢視更新。  

程式碼檔案的修改需要編譯以及重新啟動容器內的 Kestrel。 完成變更之後，請使用 CTRL + F5 來執行程序，並啟動容器內的應用程式。 Docker 容器不重建或停止。 在 PMC 中執行 `docker ps` 命令。 請注意，原始容器在 10 分鐘前仍在執行：

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>發行 Docker 映像

應用程式的開發和偵錯週期完成後，可協助 Visual Studio Tools for Docker 建立的應用程式的實際執行映像。 將組態下拉式清單變更為 [發行] 並建置應用程式。 工具會產生影像的*最新*標記，可以推送到 Docker Hub 或私人登錄。 

在 PMC 中執行 `docker images` 命令，以查看映像清單：

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` 命令會傳回存放庫名稱和標記識別為 \<無> (上面未列出) 的中繼映像。 這些未命名映像是由[多階段建置](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile* 所產生。 它們可以改善最終映像的建置效率 &mdash; 發生變更時只會重建必要層。 當不再需要的中繼映像時，刪除它們使用[docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/)命令。

相較於 *dev* 映像，生產或發行映像的大小可能需要更小。 因為磁碟區對應中，偵錯工具和應用程式已從本機電腦，而且不在容器內執行。 「最新」映像已封裝在主機上執行應用程式所需的應用程式碼。 因此，差異是應用程式程式碼的大小。
