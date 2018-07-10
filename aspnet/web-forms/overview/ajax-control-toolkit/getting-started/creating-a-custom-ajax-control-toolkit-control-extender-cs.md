---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: 建立自訂的 AJAX 控制項 Toolkit 控制項擴充項 (C#) |Microsoft Docs
author: microsoft
description: 自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不需要建立新的類別。
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 525975c9e0bddc6ef539b477e9f5db3cfd7ee8c3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814261"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="f5890-103">建立自訂的 AJAX Control Toolkit 控制項擴充項 (C#)</span><span class="sxs-lookup"><span data-stu-id="f5890-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="f5890-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f5890-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f5890-105">自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不需要建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="f5890-106">在本教學課程中，您將了解如何建立自訂的 AJAX Control Toolkit 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="f5890-107">我們會建立一項簡單、 但變更按鈕的狀態從停用，以啟用在文字方塊中輸入文字時的實用、 新的擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="f5890-108">閱讀本教學課程中之後, 您將能夠擴充 ASP.NET AJAX 工具組，使用您自己的控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="f5890-109">您可以建立使用 Visual Studio 或 Visual Web Developer 的自訂控制項擴充項 （請確定您有最新版的 Visual Web Developer）。</span><span class="sxs-lookup"><span data-stu-id="f5890-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="f5890-110">DisabledButton 擴充項的概觀</span><span class="sxs-lookup"><span data-stu-id="f5890-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="f5890-111">我們新的控制項擴充項是名為 DisabledButton 擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="f5890-112">此擴充項將會有三個屬性：</span><span class="sxs-lookup"><span data-stu-id="f5890-112">This extender will have three properties:</span></span>

- <span data-ttu-id="f5890-113">TargetControlID-擴充控制項的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="f5890-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="f5890-114">TargetButtonIID-已停用或啟用的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5890-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="f5890-115">DisabledText-最初顯示在按鈕的文字。</span><span class="sxs-lookup"><span data-stu-id="f5890-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="f5890-116">當您開始輸入時，按鈕會顯示按鈕文字屬性的值。</span><span class="sxs-lookup"><span data-stu-id="f5890-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="f5890-117">您連接至文字方塊和按鈕控制項 DisabledButton 擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="f5890-118">輸入任何文字之前，會停用按鈕和文字方塊和按鈕看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="f5890-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="f5890-119">([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="f5890-120">在您開始輸入文字之後，已啟用 按鈕和文字方塊和按鈕看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="f5890-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="f5890-121">([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="f5890-122">若要建立我們控制擴充項，我們需要建立下列三個檔案：</span><span class="sxs-lookup"><span data-stu-id="f5890-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="f5890-123">DisabledButtonExtender.cs-這個檔案是伺服器端控制項類別，會管理建立您的擴充項，並可讓您在設計階段設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="f5890-124">它也會定義可以設定在 extender 的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="f5890-125">這些屬性可透過程式碼，並在設計階段，而且符合 DisableButtonBehavior.js 檔案中定義的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="f5890-126">DisabledButtonBehavior.js-這個檔案是會加入所有的用戶端指令碼邏輯的位置。</span><span class="sxs-lookup"><span data-stu-id="f5890-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="f5890-127">DisabledButtonDesigner.cs-此類別可讓設計階段功能。</span><span class="sxs-lookup"><span data-stu-id="f5890-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="f5890-128">如果您想要控制擴充項，才能正確地使用 Visual Studio/Visual Web Developer 設計工具，您會需要此類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="f5890-129">因此控制項擴充項所組成的伺服器端控制項、 用戶端行為，以及伺服器端的設計工具類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="f5890-130">您了解如何建立下列各節中的所有三個檔案。</span><span class="sxs-lookup"><span data-stu-id="f5890-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="f5890-131">建立自訂擴充項網站和專案</span><span class="sxs-lookup"><span data-stu-id="f5890-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="f5890-132">第一個步驟是在 Visual Studio/Visual Web Developer 中建立類別庫專案和網站。</span><span class="sxs-lookup"><span data-stu-id="f5890-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="f5890-133">我們將在類別庫專案中建立自訂的擴充性和網站中測試自訂的擴充性。</span><span class="sxs-lookup"><span data-stu-id="f5890-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="f5890-134">可讓網站啟動的 s。</span><span class="sxs-lookup"><span data-stu-id="f5890-134">Let�s start with the website.</span></span> <span data-ttu-id="f5890-135">請遵循下列步驟來建立網站：</span><span class="sxs-lookup"><span data-stu-id="f5890-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="f5890-136">選取功能表選項**檔案中，新的網站**。</span><span class="sxs-lookup"><span data-stu-id="f5890-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="f5890-137">選取  **ASP.NET 網站**範本。</span><span class="sxs-lookup"><span data-stu-id="f5890-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="f5890-138">命名新的網站*Website1*。</span><span class="sxs-lookup"><span data-stu-id="f5890-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="f5890-139">按一下 [**確定**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5890-139">Click the **OK** button.</span></span>

<span data-ttu-id="f5890-140">接下來，我們需要建立類別庫專案將包含控制項擴充項的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f5890-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="f5890-141">選取功能表選項**檔案中，加入新的專案**。</span><span class="sxs-lookup"><span data-stu-id="f5890-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="f5890-142">選取 **類別庫**範本。</span><span class="sxs-lookup"><span data-stu-id="f5890-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="f5890-143">命名新的類別程式庫同名**CustomExtenders**。</span><span class="sxs-lookup"><span data-stu-id="f5890-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="f5890-144">按一下 [**確定**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5890-144">Click the **OK** button.</span></span>

<span data-ttu-id="f5890-145">完成這些步驟之後，您的方案總管 視窗看起來應該像圖 1。</span><span class="sxs-lookup"><span data-stu-id="f5890-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="f5890-146">[![與網站和類別庫專案的方案](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f5890-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="f5890-147">**圖 01**： 網站和類別程式庫專案的方案 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="f5890-148">接下來，您需要新增所有必要的組件的參考類別庫專案：</span><span class="sxs-lookup"><span data-stu-id="f5890-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="f5890-149">CustomExtenders 專案上按一下滑鼠右鍵，然後選取功能表選項**加入參考**。</span><span class="sxs-lookup"><span data-stu-id="f5890-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="f5890-150">選取 [.NET] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f5890-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="f5890-151">加入下列組件的參考：</span><span class="sxs-lookup"><span data-stu-id="f5890-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="f5890-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="f5890-152">System.Web.dll</span></span>
    2. <span data-ttu-id="f5890-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="f5890-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="f5890-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="f5890-154">System.Design.dll</span></span>
    4. <span data-ttu-id="f5890-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="f5890-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="f5890-156">選取 [瀏覽] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f5890-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="f5890-157">新增 AjaxControlToolkit.dll 組件的參考。</span><span class="sxs-lookup"><span data-stu-id="f5890-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="f5890-158">這個組件位於您用來下載 AJAX Control Toolkit 中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f5890-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="f5890-159">完成這些步驟之後，您的類別庫專案參考資料夾看起來應該像圖 2。</span><span class="sxs-lookup"><span data-stu-id="f5890-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="f5890-160">[![必要的參考，[參考] 資料夾](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="f5890-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="f5890-161">**圖 02**： 必要的參考，[參考] 資料夾 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="f5890-162">建立自訂控制項擴充項</span><span class="sxs-lookup"><span data-stu-id="f5890-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="f5890-163">既然我們已經有類別庫，我們可以開始建置我們的擴充項控制項。</span><span class="sxs-lookup"><span data-stu-id="f5890-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="f5890-164">可讓開始使用最基本的自訂擴充項控制項類別 （請參閱列表 1） s。</span><span class="sxs-lookup"><span data-stu-id="f5890-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="f5890-165">**列表 1-MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="f5890-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="f5890-166">有幾件事，您會注意到關於清單 1 控制項擴充項類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="f5890-167">首先，請注意，類別可以繼承自基底 ExtenderControlBase 類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="f5890-168">所有的 AJAX Control Toolkit 擴充項控制項是衍生自這個基底類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="f5890-169">比方說，基底類別包含必要的屬性，每個控制項擴充項的 TargetID 屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="f5890-170">接下來，要請您注意的是類別，包含用戶端指令碼相關的下列兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="f5890-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="f5890-171">WebResource-會使要做為內嵌資源的組件中包含的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5890-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="f5890-172">ClientScriptResource-會導致指令碼資源要擷取的組件。</span><span class="sxs-lookup"><span data-stu-id="f5890-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="f5890-173">WebResource 屬性用來將 MyControlBehavior.js JavaScript 檔案內嵌至組件中，自訂的擴充性在編譯時。</span><span class="sxs-lookup"><span data-stu-id="f5890-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="f5890-174">ClientScriptResource 屬性用來在網頁中使用自訂的擴充性時，從組件擷取 MyControlBehavior.js 指令碼。</span><span class="sxs-lookup"><span data-stu-id="f5890-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="f5890-175">為了 WebResource 與 ClientScriptResource 屬性運作，您必須編譯 JavaScript 檔案做為內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="f5890-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="f5890-176">在 [方案總管] 視窗中選取的檔案、 開啟屬性工作表及值指派*內嵌的資源*要**建置動作**屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="f5890-177">請注意，控制項擴充項也會包含 TargetControlType 屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="f5890-178">這個屬性用來指定擴充控制項擴充項的控制項型別。</span><span class="sxs-lookup"><span data-stu-id="f5890-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="f5890-179">列表 1，如果控制項擴充項用來擴充的文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="f5890-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="f5890-180">最後，要請您注意的是自訂的擴充項包含名為 MyProperty 的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="f5890-181">屬性標記著 ExtenderControlProperty 屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="f5890-182">GetPropertyValue() 和 SetPropertyValue() 方法用來從伺服器端控制項擴充項的屬性值傳遞到用戶端的行為。</span><span class="sxs-lookup"><span data-stu-id="f5890-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="f5890-183">可讓繼續及實作我們 DisabledButton 擴充項的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f5890-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="f5890-184">可以在 列表 2 中找到這個擴充項的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f5890-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="f5890-185">**列表 2-DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="f5890-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="f5890-186">DisabledButton 擴充項列表 2 中的會有兩個名為 TargetButtonID 和 DisabledText 的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="f5890-187">套用至 TargetButtonID 屬性 IDReferenceProperty 會防止您將按鈕控制項的 ID 以外的任何項目指派給這個屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="f5890-188">WebResource 與 ClientScriptResource 屬性建立關聯位於名為 DisabledButtonBehavior.js 這個擴充項檔案中的用戶端行為。</span><span class="sxs-lookup"><span data-stu-id="f5890-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="f5890-189">我們會討論這個 JavaScript 檔案下, 一節。</span><span class="sxs-lookup"><span data-stu-id="f5890-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="f5890-190">建立自訂的擴充項行為</span><span class="sxs-lookup"><span data-stu-id="f5890-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="f5890-191">控制項擴充項的用戶端端元件稱為 「 行為 」。</span><span class="sxs-lookup"><span data-stu-id="f5890-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="f5890-192">停用和啟用按鈕的實際邏輯都包含在 DisabledButton 行為。</span><span class="sxs-lookup"><span data-stu-id="f5890-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="f5890-193">在 列表 3 包含行為的 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="f5890-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="f5890-194">**列表 3-DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="f5890-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="f5890-195">在 列表 3 中的 JavaScript 檔案包含名為 DisabledButtonBehavior 的用戶端類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="f5890-196">像其伺服器端對應項中，此類別包含兩個屬性，名為 TargetButtonID 和取得您可以使用來存取 DisabledText\_TargetButtonID/set\_TargetButtonID 並取得\_DisabledText/set\_DisabledText。</span><span class="sxs-lookup"><span data-stu-id="f5890-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="f5890-197">Initialize （） 方法會將 keyup 事件處理常式關聯的目標項目行為。</span><span class="sxs-lookup"><span data-stu-id="f5890-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="f5890-198">每當您在這項行為，與相關文字方塊中輸入字母的 keyup 處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="f5890-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="f5890-199">Keyup 處理常式會啟用或停用根據與行為相關聯的文字方塊是否包含任何文字的按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5890-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="f5890-200">請記住，您必須編譯 JavaScript 檔案做為內嵌資源的列表 3。</span><span class="sxs-lookup"><span data-stu-id="f5890-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="f5890-201">在 [方案總管] 視窗中選取的檔案、 開啟屬性工作表及值指派*內嵌資源*要**建置動作**屬性 （請參閱 [圖 3]）。</span><span class="sxs-lookup"><span data-stu-id="f5890-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="f5890-202">Visual Studio 和 Visual Web Developer 中使用此選項。</span><span class="sxs-lookup"><span data-stu-id="f5890-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="f5890-203">[![新增 JavaScript 檔案做為內嵌資源](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="f5890-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="f5890-204">**圖 03**： 加入 JavaScript 檔案做為內嵌資源 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="f5890-205">建立自訂的擴充性設計工具</span><span class="sxs-lookup"><span data-stu-id="f5890-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="f5890-206">沒有一個我們需要建立完成我們的擴充項的最後一個類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="f5890-207">我們需要在 列表 4 中建立設計工具類別。</span><span class="sxs-lookup"><span data-stu-id="f5890-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="f5890-208">這個類別，才能讓正常運作，Visual Studio/Visual Web Developer 設計工具與擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="f5890-209">**列表 4-DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="f5890-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="f5890-210">列表 4 中的設計工具關聯 DisabledButton 擴充項與設計工具屬性上。您需要將設計工具屬性套用至 DisabledButtonExtender 類別就像這樣：</span><span class="sxs-lookup"><span data-stu-id="f5890-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="f5890-211">使用自訂的擴充性</span><span class="sxs-lookup"><span data-stu-id="f5890-211">Using the Custom Extender</span></span>

<span data-ttu-id="f5890-212">既然我們已經完成建立 DisabledButton 控制項擴充項，就可以在我們的 ASP.NET 網站中使用它。</span><span class="sxs-lookup"><span data-stu-id="f5890-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="f5890-213">首先，我們需要將自訂的擴充性加入至工具箱。</span><span class="sxs-lookup"><span data-stu-id="f5890-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="f5890-214">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="f5890-214">Follow these steps:</span></span>

1. <span data-ttu-id="f5890-215">按兩下 [方案總管] 視窗中的頁面，以開啟 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="f5890-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="f5890-216">以滑鼠右鍵按一下 [工具箱]，然後選取功能表選項**選擇項目**。</span><span class="sxs-lookup"><span data-stu-id="f5890-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="f5890-217">在 [選擇工具箱項目] 對話方塊中，瀏覽至 CustomExtenders.dll 組件。</span><span class="sxs-lookup"><span data-stu-id="f5890-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="f5890-218">按一下 **確定**按鈕以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="f5890-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="f5890-219">完成這些步驟之後，DisabledButton 控制項擴充項應該會出現在工具箱] 中 （請參閱 [圖 4）。</span><span class="sxs-lookup"><span data-stu-id="f5890-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="f5890-220">[![在 [工具箱] 的 DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="f5890-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="f5890-221">**圖 04**： 在 [工具箱] 的 DisabledButton ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="f5890-222">接下來，我們需要建立新的 ASP.NET 頁面。</span><span class="sxs-lookup"><span data-stu-id="f5890-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="f5890-223">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="f5890-223">Follow these steps:</span></span>

1. <span data-ttu-id="f5890-224">建立名為 ShowDisabledButton.aspx 的新 ASP.NET 頁面。</span><span class="sxs-lookup"><span data-stu-id="f5890-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="f5890-225">將 ScriptManager 拖曳至頁面。</span><span class="sxs-lookup"><span data-stu-id="f5890-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="f5890-226">將 TextBox 控制項拖曳至頁面。</span><span class="sxs-lookup"><span data-stu-id="f5890-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="f5890-227">將按鈕控制項拖曳至頁面。</span><span class="sxs-lookup"><span data-stu-id="f5890-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="f5890-228">在 屬性 視窗中，將按鈕的 ID 屬性變更的值<em>btnSave</em>和 文字屬性設為值*儲存\**。</span><span class="sxs-lookup"><span data-stu-id="f5890-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="f5890-229">我們建立使用標準 ASP.NET 文字方塊和按鈕控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="f5890-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="f5890-230">接下來，我們需要擴充 DisabledButton 擴充項與 TextBox 控制項：</span><span class="sxs-lookup"><span data-stu-id="f5890-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="f5890-231">選取 **加入擴充項**工作選項來開啟 擴充性精靈 對話方塊 （請參閱 圖 5）。</span><span class="sxs-lookup"><span data-stu-id="f5890-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="f5890-232">請注意對話方塊包含我們自訂的 DisabledButton 擴充性。</span><span class="sxs-lookup"><span data-stu-id="f5890-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="f5890-233">選取 DisabledButton 擴充項，然後按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f5890-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="f5890-234">[![擴充項精靈對話方塊](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="f5890-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="f5890-235">**圖 05**: [擴充性精靈] 對話方塊 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="f5890-236">最後，我們可以設定 DisabledButton 擴充項的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="f5890-237">您可以修改 TextBox 控制項的屬性來修改 DisabledButton 擴充項的屬性：</span><span class="sxs-lookup"><span data-stu-id="f5890-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="f5890-238">在設計工具選取文字方塊。</span><span class="sxs-lookup"><span data-stu-id="f5890-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="f5890-239">在 屬性 視窗中，展開 Extender 節點 （請參閱 圖 6）。</span><span class="sxs-lookup"><span data-stu-id="f5890-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="f5890-240">將值指派*儲存*DisabledText 屬性和值*btnSave* TargetButtonID 屬性。</span><span class="sxs-lookup"><span data-stu-id="f5890-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="f5890-241">[![設定擴充項屬性](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="f5890-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="f5890-242">**圖 06**： 設定擴充項屬性 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="f5890-243">當您執行的頁面 （藉由按下 F5） 時，一開始會停用的按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="f5890-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="f5890-244">一旦您開始在文字方塊中輸入文字，控制項的按鈕會啟用 （請參閱 圖 7）。</span><span class="sxs-lookup"><span data-stu-id="f5890-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="f5890-245">[![在 動作 DisabledButton 擴充項](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="f5890-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="f5890-246">**圖 07**: DisabledButton 作用中的擴充項 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="f5890-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="f5890-247">總結</span><span class="sxs-lookup"><span data-stu-id="f5890-247">Summary</span></span>

<span data-ttu-id="f5890-248">本教學課程的目標是要說明如何擴充 AJAX Control Toolkit 之自訂擴充項控制項。</span><span class="sxs-lookup"><span data-stu-id="f5890-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="f5890-249">在本教學課程中，我們會建立簡單的 DisabledButton 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="f5890-250">我們透過建立 DisabledButtonExtender 類別、 DisabledButtonBehavior JavaScript 行為和 DisabledButtonDesigner 類別實作這個擴充項。</span><span class="sxs-lookup"><span data-stu-id="f5890-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="f5890-251">每當您建立自訂控制項擴充項時，您就會遵循一組類似的步驟。</span><span class="sxs-lookup"><span data-stu-id="f5890-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f5890-252">[上一頁](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [下一頁](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f5890-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
