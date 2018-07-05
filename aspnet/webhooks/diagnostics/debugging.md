---
uid: webhooks/diagnostics/debugging
title: ASP.NET Webhook 偵錯 |Microsoft Docs
author: rick-anderson
description: 如何偵錯 ASP.NET Webhook。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801984"
---
# <a name="aspnet-webhooks-debugging"></a>偵錯 ASP.NET Webhook  

## <a name="debugging-in-azure"></a>在 Azure 中偵錯

若要偵錯 Web 應用程式在 Azure 中執行時，請參閱本教學課程[疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。

## <a name="debugging-with-source-and-symbols"></a>使用來源和符號偵錯

除了您自己的程式碼偵錯，就可以直接在 Microsoft ASP.NET Webhook，事實上，偵錯所有.NET。 這適用於無論是否您在本機或遠端的偵錯。 首先，設定 Visual Studio 用來尋找來源和符號移至**偵錯**，然後**選項和設定**。 設定選項如下：

![選項和設定](_static/SourceSymbols.png)

然後新增連結[symbolsource.org](http://symbolsource.org)下載的來源和符號。 移至**符號**上方選單 索引標籤，並新增下列做為符號的位置：

```
http://srv.symbolsource.org/pdb/Public
```

此外，請確定快取目錄有簡短的名稱;否則的檔案名稱可以變得太長導致不載入符號。 範例路徑為：

```
C:\SymCache
```

設定看起來應該如下所示：

![選項符號檔位置範例](_static/SymSource.png)
