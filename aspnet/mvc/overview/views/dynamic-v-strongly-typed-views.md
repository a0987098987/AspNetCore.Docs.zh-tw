---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "動態 v。 強型別檢視 |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a>動態 v。 強型別的檢視
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

有三種方法將從控制器的資訊傳遞至 ASP.NET MVC 3 中的檢視：

1. 為強類型的模型物件。
2. 為動態類型 (使用@model動態)
3. 使用 ViewBag

我所撰寫的簡單比較與對照動態和強型別檢視的 MVC 3 上的部落格應用程式。 在控制器啟動部落格的簡單清單：

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

以滑鼠右鍵按一下 IndexNotStonglyTyped() 方法中，並加入 Razor 檢視。

[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)

請確定**建立強型別檢視**未核取方塊。 產生的檢視不包含許多：

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

因為我們會使用動態和不是強型別的檢視，intellisense 不會協助我們。 已完成的程式碼如下所示：

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)

現在我們將新增強型別的檢視。 將下列程式碼加入至控制器：

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


請注意，它是完全相同傳回 View(topBlogs);呼叫非強型別檢視。 以滑鼠右鍵按一下內*StonglyTypedIndex()*選取**加入檢視**。 這一次選取**部落格**模型類別，然後選取**清單**為 Scaffold 範本。

[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)

在新的檢視範本內得到 intellisense 支援。

[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)

C# 專案可下載[這裡](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip)。
