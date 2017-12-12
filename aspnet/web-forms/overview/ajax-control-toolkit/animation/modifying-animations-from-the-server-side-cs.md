---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: "修改動畫從伺服器端 (C#) |Microsoft 文件"
author: wenz
description: "動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 也可能動畫..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: e1f9bda03b3e3edf3bbdc591cde9858944d39321
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="ce719-104">修改動畫從伺服器端 (C#)</span><span class="sxs-lookup"><span data-stu-id="ce719-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="ce719-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ce719-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ce719-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce719-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="ce719-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="ce719-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce719-108">可能也會在伺服器端變更動畫</span><span class="sxs-lookup"><span data-stu-id="ce719-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="ce719-109">概觀</span><span class="sxs-lookup"><span data-stu-id="ce719-109">Overview</span></span>

<span data-ttu-id="ce719-110">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="ce719-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce719-111">可能也會在伺服器端變更動畫</span><span class="sxs-lookup"><span data-stu-id="ce719-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="ce719-112">步驟</span><span class="sxs-lookup"><span data-stu-id="ce719-112">Steps</span></span>

<span data-ttu-id="ce719-113">首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：</span><span class="sxs-lookup"><span data-stu-id="ce719-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="ce719-114">動畫會套用到面板中的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="ce719-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="ce719-115">在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：</span><span class="sxs-lookup"><span data-stu-id="ce719-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="ce719-116">程式碼的其餘部分會在伺服器端上執行，而不會使用標記，相反地，它使用程式碼建立`AnimationExtender`控制項：</span><span class="sxs-lookup"><span data-stu-id="ce719-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="ce719-117">不過，此控制項的工具組目前不提供如何建立個別的動畫 API 存取。</span><span class="sxs-lookup"><span data-stu-id="ce719-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="ce719-118">但它是可以設定`AnimationExtender`的動畫屬性設為字串包含時以宣告方式指定動畫所使用的 XML 標記。</span><span class="sxs-lookup"><span data-stu-id="ce719-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="ce719-119">若要建立不能包含 XML`<Animations>`您可以使用.NET Framework 的 XML 項目會支援，或如下列程式碼，只是提供字串：</span><span class="sxs-lookup"><span data-stu-id="ce719-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="ce719-120">最後，會加入`AnimationExtender`內目前的頁面，來控制`<form runat="server">`項目，並確認是否包含動畫，並執行：</span><span class="sxs-lookup"><span data-stu-id="ce719-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="ce719-121">[![使用伺服器端 C# /VB 程式碼來建立動畫](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce719-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="ce719-122">使用伺服器端 C# /VB 程式碼來建立動畫 ([按一下以檢視完整大小的影像](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce719-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ce719-123">[上一頁](triggering-an-animation-in-another-control-cs.md)
[下一頁](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="ce719-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
