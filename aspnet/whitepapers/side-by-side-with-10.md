---
uid: whitepapers/side-by-side-with-10
title: .NET framework 1.0 和 1.1 版的 ASP.NET-並存執行 |Microsoft 文件
author: rick-anderson
description: 本白皮書說明如何在允許的 fram 任一版本上執行的 ASP.NET Web 應用程式的電腦上安裝.NET 1.0 和.NET 1.1...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530177"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>.NET Framework 1.0 和 1.1 版的 ASP.NET-並存執行
====================
> 本白皮書說明如何在任一版本的架構上執行的 ASP.NET Web 應用程式可讓您的電腦上安裝.NET 1.0 和.NET 1.1。
> 
> 適用於 ASP.NET 1.0 和 1.1 的 ASP.NET。


在 ASP.NET 中，可視為應用程式時安裝在相同電腦上，但使用不同版本的.NET Framework 並存執行。 下列主題描述如何設定 ASP.NET 應用程式的並存執行，並提供詳細的步驟：

- [維護期間安裝的 Web 應用程式的對應至.NET Framework 1.0 版](#1)
- [對應至特定版本的.NET framework 的 Web 應用程式](#2)
- [尋找網站使用的.NET framework 版本](#3)

傳統上，當更新的電腦上的元件或應用程式時，較舊版本移除和取代成較新版本。 如果新的版本不相容的舊版本，這通常會中斷其他元件或應用程式所使用的應用程式。 .NET Framework 的並存執行，可讓多個版本的組件或同時在相同電腦上安裝的應用程式提供支援。 可以同時安裝多個版本，因為 managed 應用程式可以選取要使用而不會影響使用不同版本的應用程式的版本。

根據預設，.NET Framework 1.1 版的安裝期間所有現有的 ASP.NET 應用程式會自動重新設定成使用最新版本的.NET Framework。 如果不想使用預設的.NET Framework 1.1 的 ASP.NET 應用程式，請按一下[這裡](#1)以了解如何防止這在安裝期間。

如果您為.NET Framework 1.1 更新您的 Web 伺服器，並將執行.NET Framework 1.0 的一或多個 Web 應用程式，您需要更新網際網路資訊服務 (IIS) 指令碼對應。 指令碼對應是將對應.aspx 檔案延伸模組特定的 Web 應用程式的.NET Framework 版本的機制。 按一下[這裡](#2)以了解如何對應至特定版本的.NET framework 的 Web 應用程式。

您可以使用網際網路資訊管理員或 ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe) 來尋找執行特定的 Web 應用程式的.NET Framework 版本。 按一下[這裡](#3)以了解如何尋找網站使用的.NET framework 版本。

移轉至.NET Framework 1.1 就是每一版.NET Framework 會使用自己的 Machine.config 檔案時，就會有一個匯入考量。 如此一來，如果 Web 系統管理員變更至 Machine.config 檔案，這些變更必須移轉至.NET Framework 1.1 Machine.config 檔。

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>維護期間安裝.NET Framework 1.0 Web 應用程式的對應

根據預設，所有現有的 ASP.NET 應用程式會自動重新設定期間使用較新版本的.NET framework 安裝。 使用.NET framework 的較新版本，應用程式可以利用完整的改良措施和新的版本中所包含的新功能。 在相同的時間，可能會想要更精確地控制哪些應用程式的 Web 系統管理員會更新，可防止自動重新對應所有現有的 ASP.NET 應用程式的.NET framework 的安裝期間。

若要避免整個 ASP.NET 應用程式的.NET framework 的較新版本的自動重新對應，Web 系統管理員可以使用 /noaspupgrade 命令列選項與 Dotnetfx.exe 安裝程式。

**若要避免總計重新對應至較新版本的 ASP.NET 應用程式**

1. 移至**啟動**。
2. 按一下**執行**。
3. 輸入 **cmd**。
4. 按一下 [確定]。  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. 從命令提示字元中，輸入下列行加入開始安裝的.NET framework: **Dotnetfx.exe 包裝 」 安裝 /noaspupgrade？**。  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. 按一下**是**中 Microsoft.NET Framework 1.1 安裝程式。 這會啟動.NET Framework 1.1 的安裝程序。  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>對應至特定版本的.NET framework 的 Web 應用程式

.NET Framework 的每個版本都包括某個版本的 ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)。 此工具可讓系統管理員指定的特定版本的.NET Framework 下執行 Web 應用程式。 這被指對應的.NET Framework 版本的 Web 應用程式。 系統管理員必須選取 Aspnet\_regiis.exe 對應於要與 Web 應用程式相關聯的.NET framework 版本。 例如，想要指定的網站使用.NET Framework 1.1 的系統管理員必須使用 Aspnet\_regiis.exe 隨附於.NET Framework 1.1。

Aspnet\_版本 1.0 regiis.exe 位於：

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis

Aspnet\_regiis.exe 版本 1，1 是位於：

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis

Aspnet\_regiis.exe 提供兩個對應的 Web 應用程式的指令碼選項：

- **-s**設定指令碼對應和其子系的路徑中目錄。
- **-sn**只在路徑中設定指令碼對應。

此路徑定義中 W3SVC/ROOT 表單定義 Web 應用程式 IIS 中繼資料路徑 / {WebSiteNumber} / {應用程式\_名稱}。 例如，呼叫預設的網站之下的入口網站 Web 應用程式，metabase 路徑是 W3SVC/1/ROOT/入口網站。

![](side-by-side-with-10/_static/image4.gif)

請注意，您也可以使用稱為 Metabase 編輯器工具，以取得的 metabase 路徑。 您可以從 「 Microsoft 支援服務 」 網站下載此工具[https://support.microsoft.com/default.aspx?scid=kb;en-us;232068。](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- 執行 Aspnet\_regiis.exe-s W3SVC/1/ROOT/入口網站更新入口網站 IIS 的指令碼對應和其 subapplication。  
  
    ![](side-by-side-with-10/_static/image5.gif)

- 執行 Aspnet\_regiis.exe-sn W3SVC/1/ROOT/入口網站更新的入口網站的 IIS 指令碼對應，而不會影響入口網站中的應用程式？ s 子目錄。  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>尋找 Web 應用程式使用的.NET Framework 版本

系統管理員可以使用網際網路服務管理員來尋找.NET Framework 版本會執行的網站。 不同的作業系統版本會以不同的方式啟動網際網路服務管理員。 若要啟動 service manager，請遵循下面所列的步驟。

**若要啟動網際網路服務管理員**

1. 移至**啟動**。
2. 按一下**執行**。
3. 型別**inetmgr**。  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. 從網際網路服務管理員中，選取您想要知道其版本的.NET framework 的 Web 應用程式。  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. 以滑鼠右鍵按一下 Web 應用程式，然後按一下**屬性。**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. 從 [屬性] 視窗中，選取**組態。**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. 從應用程式對應資料表中，選取 **.aspx**，然後按一下**編輯**。  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. 從**可執行檔** 文字方塊中，捲動的版本目錄檢視。 如果版本目錄 v.1.1.4322，應用程式會對應到.NET Framework 1.1。 相反地，如果 v1.0.3705 版本目錄，應用程式的動作就會對應到.NET Framework 1.0。  
  
    ![](side-by-side-with-10/_static/image12.gif)
