---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: 建置本章下載 EF 5 MVC 4 的教學課程 |Microsoft 文件
author: Rick-Anderson
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878512"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>建置本章下載 EF 5 MVC 4 的教學課程
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


## <a name="building-the-chapter-downloads"></a>建置章下載

1. 下載並解壓縮專案範例 zip 檔案。 解壓縮的下載封裝中，您會發現額外的 zip 檔案，其中的每一章完成作業。
2. 以滑鼠右鍵按一下所需的 zip 檔案，然後按一下**屬性**，然後按一下**解除封鎖** 按鈕。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. 解壓縮檔案。
4. 按兩下*CUx.sln*檔案即可啟動 Visual Studio。
5. 從**工具**功能表上，按一下 **程式庫套件管理員**，然後**Package Manager Console**。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. 在 封裝管理員主控台 (PMC)，按一下**還原**。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. 結束 Visual Studio。
8. 重新啟動 Visual Studio，您關閉上述步驟中開啟方案檔。
9. 在 封裝管理員主控台 (PMC)，輸入`Update-Database`命令：  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > 如果您收到下列錯誤：  
    >   
    >  *'Update-database' 詞彙無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。請檢查名稱拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。*  
    > 結束並重新啟動 Visual Studio。

    每個移轉將會執行，然後種子方法會執行。 您現在可以執行應用程式。

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [上一步](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
