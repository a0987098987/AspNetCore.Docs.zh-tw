---
uid: webhooks/diagnostics/debugging
title: "偵錯 ASP.NET Webhook |Microsoft 文件"
author: rick-anderson
description: "如何偵錯 ASP.NET Webhook。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a>偵錯 ASP.NET Webhook  

## <a name="debugging-in-azure"></a>在 Azure 中偵錯

若要偵錯 Web 應用程式在 Azure 中執行時，請參閱本教學課程[疑難排解 web 應用程式中使用 Visual Studio 的 Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。

## <a name="debugging-with-source-and-symbols"></a>使用原始檔和符號偵錯

除了您自己的程式碼進行偵錯，就可以直接在 Microsoft ASP.NET Webhook，事實上偵錯所有.NET。 這適用於無論是否您在本機或遠端的偵錯。 首先，設定 Visual Studio 以尋找原始檔和符號，請前往**偵錯**然後**選項和設定**。 設定選項如下：

![選項和設定](_static/SourceSymbols.png)

然後加入要連結[symbolsource.org](http://symbolsource.org)下載的來源和符號。 移至**符號**上述功能表 索引標籤並做為符號位置加入下列：

```
http://srv.symbolsource.org/pdb/Public
```

此外，請確定快取目錄的簡短名稱;否則檔案名稱可以變得太長導致不載入符號。 範例路徑為：

```
C:\SymCache
```

設定看起來應該如下所示：

![選項符號檔位置範例](_static/SymSource.png)
