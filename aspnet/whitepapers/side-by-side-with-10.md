---
uid: whitepapers/side-by-side-with-10
title: .NET framework 1.0 和 1.1 版的 ASP.NET 的並存執行 |Microsoft Docs
author: rick-anderson
description: 本白皮書會說明如何在您的電腦，讓畫面任一版本上執行的 ASP.NET Web 應用程式上安裝.NET 1.0 與.NET 1.1...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808391"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>ASP.NET-並存執行.NET Framework 1.0 和 1.1 版
====================
> 本白皮書會說明如何在您允許在任一版本的架構上執行的 ASP.NET Web 應用程式的電腦上安裝.NET 1.0 與.NET 1.1。
> 
> 適用於 ASP.NET 1.0 及 ASP.NET 1.1。


在 ASP.NET 中，應用程式是指安裝在相同電腦上，但使用不同版本的.NET Framework 並存執行。 下列主題說明如何設定並排顯示執行的 ASP.NET 應用程式，並提供詳細的步驟：

- [維護期間安裝的 Web 應用程式的對應至.NET Framework 1.0 版](#1)
- [對應至特定版本的.NET framework 的 Web 應用程式](#2)
- [尋找網站使用的.NET framework 版本](#3)

傳統上，當更新的電腦上的元件或應用程式時，較舊的版本會移除並取代為較新版本。 如果新的版本不是與舊版相容，這通常會中斷其他應用程式使用的元件或應用程式。 .NET Framework 來並行執行，這可讓多個版本的組件或同時在相同電腦上安裝的應用程式提供支援。 可以同時安裝多個版本，因為受管理的應用程式可以選取要使用而不會影響使用不同版本的應用程式的版本。

根據預設，.NET Framework 1.1 版在安裝期間所有現有的 ASP.NET 應用程式會自動重新設定為使用最新版的.NET framework。 如果不想使用預設的.NET Framework 1.1 ASP.NET 應用程式，請按一下[此處](#1)以了解如何避免這個問題在安裝期間。

如果您更新您的 Web 伺服器，以.NET Framework 1.1，而且想要執行.NET Framework 1.0 的一或多個 Web 應用程式，您需要更新網際網路資訊服務 (IIS) 指令碼對應。 指令碼對應是將對應的.NET framework 的版本特定的 Web 應用程式的.aspx 檔案副檔名的機制。 按一下 [此處](#2)以了解如何對應至特定版本的.NET framework 的 Web 應用程式。

您可以使用網際網路資訊 Manger] 或 [ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe) 來尋找執行特定的 Web 應用程式的.NET Framework 版本。 按一下 [此處](#3)以了解如何尋找網站使用的.NET framework 版本。

移轉至.NET Framework 1.1 是每個版本的.NET Framework 會使用自己的 Machine.config 檔案時，出現一個匯入考量。 如此一來，如果 Web 系統管理員已對 Machine.config 檔案的變更，這些變更必須移轉至.NET Framework 1.1 Machine.config 檔案。

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>維護期間安裝的 Web 應用程式的對應至.NET Framework 1.0

根據預設，所有現有的 ASP.NET 應用程式會自動重新設定期間使用較新版本的.NET framework 的安裝。 使用較新版本的.NET framework，應用程式可以充分利用改善和新的版本中隨附的新功能。 在此同時，可能會想要更精確地控制哪些應用程式的 Web 系統管理員會更新，可防止自動重新對應所有現有的 ASP.NET 應用程式的.NET framework 的安裝期間。

若要避免的自動重新對應到較新版的.NET framework 的整個 ASP.NET 應用程式，Web 系統管理員可以使用 /noaspupgrade 命令列選項與 Dotnetfx.exe 安裝程式。

**若要避免總計重新對應到較新版本的 ASP.NET 應用程式**

1. 移至**啟動**。
2. 按一下 **執行**。
3. 輸入 **cmd**。
4. 按一下 [確定 **Deploying Office Solutions**]。  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. 從命令提示字元中，輸入下列這一行加入開始的.NET framework 安裝： **Dotnetfx.exe /c: 「 安裝 /noaspupgrade？**。  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. 按一下 **是**中 Microsoft.NET Framework 1.1 版安裝程式。 這會啟動安裝程序的.NET Framework 1.1。  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>對應至特定版本的.NET framework 的 Web 應用程式

每個版本的.NET Framework 包含 ASP.NET IIS 註冊工具的版本 (Aspnet\_regiis.exe)。 這項工具可讓系統管理員指定特定版本的.NET Framework 下執行 Web 應用程式。 這被指對應的.NET framework 版本的 Web 應用程式。 系統管理員必須選取 Aspnet\_regiis.exe 對應於要與 Web 應用程式相關聯的.NET framework 版本。 例如，想要指定網站上使用.NET Framework 1.1 的系統管理員必須使用 Aspnet\_regiis.exe 隨附於.NET Framework 1.1。

Aspnet\_1.0 版的 regiis.exe 位於：

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

Aspnet\_regiis.exe 版本 1，1 是位於：

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

Aspnet\_regiis.exe 提供兩個對應的 Web 應用程式的指令碼選項：

- **-s**設定指令碼對應的路徑以及其子系的目錄。
- **-sn**只在路徑中設定指令碼對應。

此路徑定義 W3SVC/根項目的形式定義 Web 應用程式 IIS 中繼資料路徑 / {WebSiteNumber} / {應用程式\_名稱}。 例如，稱為位於預設網站上的入口網站的 Web 應用程式，metabase 路徑是 W3SVC/1/ROOT/入口網站。

![](side-by-side-with-10/_static/image4.gif)

請注意，您也可以使用名為 Metabase 編輯工具，若要取得的 metabase 路徑。 您可以從 Microsoft 支援服務網站，在下載此工具[ https://support.microsoft.com/default.aspx?scid=kb; en-us-我們; 232068。](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- 執行 Aspnet\_regiis.exe-s W3SVC/1/ROOT/入口網站來更新在入口網站的 IIS 指令碼對應和其 subapplication。  
  
    ![](side-by-side-with-10/_static/image5.gif)

- 執行 Aspnet\_regiis.exe-sn W3SVC/1/ROOT/入口網站更新入口網站的 IIS 指令碼對應，而不會影響在入口網站中的應用程式？ s 子目錄。  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>尋找 Web 應用程式使用的.NET Framework 版本

系統管理員可以使用網際網路服務管理員來尋找執行網站的.NET framework 版本。 不同的作業系統版本會以不同的方式啟動網際網路服務管理員。 若要啟動 service manager，請遵循下列步驟。

**若要啟動網際網路服務管理員**

1. 移至**啟動**。
2. 按一下 **執行**。
3. 型別**inetmgr**。  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. 從網際網路服務管理員中，選取您想要知道其版本的.NET framework 的 Web 應用程式。  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. 以滑鼠右鍵按一下 Web 應用程式，然後按一下 **屬性。**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. 從 屬性 視窗中，選取 **組態。**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. 從應用程式對應資料表中，選取 **.aspx**，然後按一下**編輯**。  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. 從**可執行檔** 文字方塊中，看看捲動版本目錄。 如果版本目錄是 v.1.1.4322，應用程式會對應至.NET Framework 1.1。 相反地，v1.0.3705 版本目錄時，應用程式的動作就被對應至.NET Framework 1.0。  
  
    ![](side-by-side-with-10/_static/image12.gif)
