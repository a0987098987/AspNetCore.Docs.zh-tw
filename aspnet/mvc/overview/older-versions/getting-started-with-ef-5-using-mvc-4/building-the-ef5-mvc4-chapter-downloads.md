---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: 建置章節下載 EF 5 MVC 4 的教學課程 |Microsoft Docs
author: Rick-Anderson
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: fa018ea12929efa742a323d96938d372b634fdd7
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577058"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>建置章節下載 EF 5 MVC 4 的教學課程
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


## <a name="building-the-chapter-downloads"></a>建置章節下載

1. 下載並解壓縮專案範例 zip 檔案。 解壓縮的下載封裝中，您會發現其他的 zip 檔案，其中的每一章完成作業。
2. 以滑鼠右鍵按一下所需的 zip 檔案，然後按一下**屬性**，然後按一下**解除封鎖** 按鈕。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. 將檔案解壓縮。
4. 按兩下*CUx.sln*檔案以啟動 Visual Studio。
5. 從**工具**功能表上，按一下**程式庫套件管理員**，然後**Package Manager Console**。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. 在 套件管理員主控台 (PMC)，按一下**還原**。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. 結束 Visual Studio。
8. 重新啟動 Visual Studio 中，在上述步驟中開啟方案檔，您已關閉。
9. 在 套件管理員主控台 (PMC)，請輸入`Update-Database`命令：  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > 如果您收到下列錯誤：  
    >   
    >  *詞彙 更新資料庫 ' 無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。請檢查名稱的拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。*  
    > 結束並重新啟動 Visual Studio。

    每個移轉將會執行，則會執行 seed 方法。 您現在可以執行應用程式。

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [上一步](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
