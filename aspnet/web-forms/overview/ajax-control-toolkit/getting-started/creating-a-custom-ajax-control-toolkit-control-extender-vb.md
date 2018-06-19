---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: 建立自訂的 AJAX 控制項 Toolkit 控制項擴充項 (VB) |Microsoft 文件
author: microsoft
description: 自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不必建立新的類別。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 06950770bf788fff4a03e9d41fd448ea675a8bce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875132"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a><span data-ttu-id="cfe93-103">建立自訂的 AJAX 控制項 Toolkit 控制項擴充項 (VB)</span><span class="sxs-lookup"><span data-stu-id="cfe93-103">Creating a Custom AJAX Control Toolkit Control Extender (VB)</span></span>
====================
<span data-ttu-id="cfe93-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cfe93-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="cfe93-105">自訂擴充項可讓您自訂和擴充 ASP.NET 控制項的功能，而不必建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="cfe93-106">在本教學課程中，您會學習如何建立自訂的 AJAX Control Toolkit 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="cfe93-107">我們會建立簡單，但很有用，新的按鈕狀態變更從停用，以您在文字方塊中輸入文字時，啟用的擴充項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="cfe93-108">閱讀本教學課程之後, 您可以擴充 ASP.NET AJAX 工具組，使用您自己的控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="cfe93-109">您可以建立使用 Visual Studio 或 Visual Web Developer 中的自訂控制項擴充項 （請確定您有最新版本的 Visual Web Developer）。</span><span class="sxs-lookup"><span data-stu-id="cfe93-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="cfe93-110">DisabledButton Extender 的概觀</span><span class="sxs-lookup"><span data-stu-id="cfe93-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="cfe93-111">我們新的控制項擴充項名為 DisabledButton 擴充項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="cfe93-112">此擴充項會有三個屬性：</span><span class="sxs-lookup"><span data-stu-id="cfe93-112">This extender will have three properties:</span></span>

- <span data-ttu-id="cfe93-113">TargetControlID-擴充控制項的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="cfe93-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="cfe93-114">TargetButtonIID-已停用或啟用的按鈕。</span><span class="sxs-lookup"><span data-stu-id="cfe93-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="cfe93-115">DisabledText-最初顯示在按鈕的文字。</span><span class="sxs-lookup"><span data-stu-id="cfe93-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="cfe93-116">當您開始輸入時，按鈕會顯示按鈕文字屬性的值。</span><span class="sxs-lookup"><span data-stu-id="cfe93-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="cfe93-117">您連接至文字方塊和按鈕控制項 DisabledButton extender。</span><span class="sxs-lookup"><span data-stu-id="cfe93-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="cfe93-118">您輸入的任何文字之前，會停用按鈕和文字方塊和按鈕看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="cfe93-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

<span data-ttu-id="cfe93-119">([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span></span>


<span data-ttu-id="cfe93-120">您開始輸入文字後，已啟用 按鈕和文字方塊和按鈕看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="cfe93-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

<span data-ttu-id="cfe93-121">([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="cfe93-122">若要建立我們控制項擴充項，我們需要建立下列三個檔案：</span><span class="sxs-lookup"><span data-stu-id="cfe93-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="cfe93-123">DisabledButtonExtender.vb-這個檔案是伺服器端控制項類別會管理建立您的擴充性，並可讓您在設計階段設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-123">DisabledButtonExtender.vb - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="cfe93-124">它也會定義可以在 extender 設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="cfe93-125">這些屬性是透過程式碼和設計階段存取，且比對 DisableButtonBehavior.js 檔案中定義的屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="cfe93-126">DisabledButtonBehavior.js-這個檔案是供您用來新增所有您的用戶端指令碼邏輯。</span><span class="sxs-lookup"><span data-stu-id="cfe93-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="cfe93-127">DisabledButtonDesigner.vb-此類別可讓設計階段功能。</span><span class="sxs-lookup"><span data-stu-id="cfe93-127">DisabledButtonDesigner.vb - This class enables design-time functionality.</span></span> <span data-ttu-id="cfe93-128">如果您想控制項擴充項 Visual Studio/Visual Web Developer 設計工具中正常運作，您會需要此類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="cfe93-129">因此控制項擴充項組成的伺服器端控制項、 用戶端行為，以及伺服器端的設計工具類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="cfe93-130">您了解如何在下列各節中建立這些檔案的所有三個。</span><span class="sxs-lookup"><span data-stu-id="cfe93-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="cfe93-131">建立自訂 Extender 網站和專案</span><span class="sxs-lookup"><span data-stu-id="cfe93-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="cfe93-132">第一個步驟是在 Visual Studio/Visual Web Developer 中建立類別庫專案和網站。</span><span class="sxs-lookup"><span data-stu-id="cfe93-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="cfe93-133">我們 ll 類別庫專案中建立自訂的擴充性，並在網站中測試自訂的擴充性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="cfe93-134">可讓網站啟動 s。</span><span class="sxs-lookup"><span data-stu-id="cfe93-134">Let�s start with the website.</span></span> <span data-ttu-id="cfe93-135">請遵循下列步驟來建立網站：</span><span class="sxs-lookup"><span data-stu-id="cfe93-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="cfe93-136">選取功能表選項**檔案、 新的網站**。</span><span class="sxs-lookup"><span data-stu-id="cfe93-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="cfe93-137">選取**ASP.NET 網站**範本。</span><span class="sxs-lookup"><span data-stu-id="cfe93-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="cfe93-138">命名新的網站*Website1*。</span><span class="sxs-lookup"><span data-stu-id="cfe93-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="cfe93-139">按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cfe93-139">Click the **OK** button.</span></span>

<span data-ttu-id="cfe93-140">接下來，我們需要建立類別庫專案將包含控制項擴充項的程式碼：</span><span class="sxs-lookup"><span data-stu-id="cfe93-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="cfe93-141">選取功能表選項**檔案，加入新的專案**。</span><span class="sxs-lookup"><span data-stu-id="cfe93-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="cfe93-142">選取**類別庫**範本。</span><span class="sxs-lookup"><span data-stu-id="cfe93-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="cfe93-143">命名新的類別庫的名稱**CustomExtenders**。</span><span class="sxs-lookup"><span data-stu-id="cfe93-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="cfe93-144">按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cfe93-144">Click the **OK** button.</span></span>

<span data-ttu-id="cfe93-145">完成這些步驟之後，您的方案總管 視窗看起來應該像圖 1。</span><span class="sxs-lookup"><span data-stu-id="cfe93-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="cfe93-146">[![網站和類別庫專案與方案](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cfe93-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="cfe93-147">**圖 01**： 與網站和類別庫專案的方案 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span></span>


<span data-ttu-id="cfe93-148">接下來，您要新增的所有必要的組件參考類別庫專案：</span><span class="sxs-lookup"><span data-stu-id="cfe93-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="cfe93-149">CustomExtenders 專案上按一下滑鼠右鍵，然後選取功能表選項**加入參考**。</span><span class="sxs-lookup"><span data-stu-id="cfe93-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="cfe93-150">選取 [.NET] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="cfe93-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="cfe93-151">加入下列組件的參考：</span><span class="sxs-lookup"><span data-stu-id="cfe93-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="cfe93-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="cfe93-152">System.Web.dll</span></span>
    2. <span data-ttu-id="cfe93-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="cfe93-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="cfe93-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="cfe93-154">System.Design.dll</span></span>
    4. <span data-ttu-id="cfe93-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="cfe93-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="cfe93-156">選取 [瀏覽] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="cfe93-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="cfe93-157">新增 AjaxControlToolkit.dll 組件的參考。</span><span class="sxs-lookup"><span data-stu-id="cfe93-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="cfe93-158">這個組件位於您下載 AJAX Control Toolkit 所在的資料夾。</span><span class="sxs-lookup"><span data-stu-id="cfe93-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="cfe93-159">您可以確認您已用滑鼠右鍵按一下您的專案、 選取屬性，然後按一下 [參考] 索引標籤新增所有正確的參考 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="cfe93-159">You can verify that you have added all of the right references by right-clicking your project, selecting Properties, and clicking the References tab (see Figure 2).</span></span>


<span data-ttu-id="cfe93-160">[![必要的參考，[參考] 資料夾](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="cfe93-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span></span>

<span data-ttu-id="cfe93-161">**圖 02**： 所需的參考，[參考] 資料夾 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="cfe93-162">建立自訂控制項擴充項</span><span class="sxs-lookup"><span data-stu-id="cfe93-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="cfe93-163">現在我們有我們類別程式庫，我們可以開始建置我們擴充項控制項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="cfe93-164">可讓 s 以最基本自訂的擴充項控制項類別 （請參閱清單 1） 的開頭。</span><span class="sxs-lookup"><span data-stu-id="cfe93-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="cfe93-165">**Listing 1 - MyCustomExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="cfe93-165">**Listing 1 - MyCustomExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

<span data-ttu-id="cfe93-166">有幾件事，您會注意到控制項擴充項類別清單 1 中的相關。</span><span class="sxs-lookup"><span data-stu-id="cfe93-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="cfe93-167">首先，請注意，類別可以繼承自基底 ExtenderControlBase 類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="cfe93-168">所有的 AJAX Control Toolkit 擴充項控制項衍生自這個基底類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="cfe93-169">例如，基底類別包含 TargetID 屬性是必要的每個控制項擴充項屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="cfe93-170">接下來，請注意此類別包括下列用戶端指令碼相關的兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="cfe93-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="cfe93-171">WebResource-會使要做為內嵌資源的組件中包含的檔案。</span><span class="sxs-lookup"><span data-stu-id="cfe93-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="cfe93-172">ClientScriptResource-會使指令碼資源要擷取的組件。</span><span class="sxs-lookup"><span data-stu-id="cfe93-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="cfe93-173">WebResource 屬性用來編譯自訂 extender 時 MyControlBehavior.js JavaScript 檔案嵌入組件。</span><span class="sxs-lookup"><span data-stu-id="cfe93-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="cfe93-174">ClientScriptResource 屬性用來在網頁中使用自訂的擴充性時，從組件擷取 MyControlBehavior.js 指令碼。</span><span class="sxs-lookup"><span data-stu-id="cfe93-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="cfe93-175">為了讓 WebResource 和 ClientScriptResource 屬性運作，您必須做為內嵌資源編譯 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="cfe93-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="cfe93-176">在 [方案總管] 視窗中選取檔案、 開啟屬性工作表和指派值*內嵌資源*至**建置動作**屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="cfe93-177">請注意控制項擴充項也包含 TargetControlType 屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="cfe93-178">這個屬性用來指定所擴充的控制項擴充項控制項類型。</span><span class="sxs-lookup"><span data-stu-id="cfe93-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="cfe93-179">在程式碼範例 1，控制項擴充項用於擴充的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="cfe93-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="cfe93-180">最後，請注意，自訂的擴充性包含名為 MyProperty 的屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="cfe93-181">屬性會標示 ExtenderControlProperty 屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="cfe93-182">GetPropertyValue() 和 SetPropertyValue() 方法可用來從伺服器端控制項擴充項的屬性值傳遞到用戶端的行為。</span><span class="sxs-lookup"><span data-stu-id="cfe93-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="cfe93-183">可讓 s 請繼續並實作我們 DisabledButton 擴充項的程式碼。</span><span class="sxs-lookup"><span data-stu-id="cfe93-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="cfe93-184">此程式碼可以列表 2 中找到。</span><span class="sxs-lookup"><span data-stu-id="cfe93-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="cfe93-185">**列出 2-DisabledButtonExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="cfe93-185">**Listing 2 - DisabledButtonExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

<span data-ttu-id="cfe93-186">DisabledButton 擴充項清單 2 中的有兩個名為 TargetButtonID 和 DisabledText 的屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="cfe93-187">套用至 TargetButtonID 屬性 IDReferenceProperty 會防止您將按鈕控制項的識別碼以外的任何項目指派給這個屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="cfe93-188">WebResource 和 ClientScriptResource 屬性產生關聯名 DisabledButtonBehavior.js 為此擴充項的檔案中的用戶端行為。</span><span class="sxs-lookup"><span data-stu-id="cfe93-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="cfe93-189">我們將討論這個 JavaScript 檔案，在下一節。</span><span class="sxs-lookup"><span data-stu-id="cfe93-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="cfe93-190">建立自訂的擴充性行為</span><span class="sxs-lookup"><span data-stu-id="cfe93-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="cfe93-191">控制項擴充項的用戶端元件會呼叫行為。</span><span class="sxs-lookup"><span data-stu-id="cfe93-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="cfe93-192">停用和啟用按鈕的實際邏輯會包含在 DisabledButton 行為。</span><span class="sxs-lookup"><span data-stu-id="cfe93-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="cfe93-193">列出的 3 包含行為的 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="cfe93-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="cfe93-194">**列出 3-DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="cfe93-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

<span data-ttu-id="cfe93-195">範例 3 中的 JavaScript 檔案包含名為 DisabledButtonBehavior 的用戶端類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="cfe93-196">這個類別，例如其伺服器端兩個，包含名為 TargetButtonID 的兩個屬性，並可以使用存取 DisabledText 取得\_TargetButtonID/set\_TargetButtonID 並取得\_DisabledText/set\_DisabledText。</span><span class="sxs-lookup"><span data-stu-id="cfe93-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="cfe93-197">Initialize （） 方法會將 keyup 事件處理常式關聯之行為的目標項目。</span><span class="sxs-lookup"><span data-stu-id="cfe93-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="cfe93-198">每當您在這種行為，與相關聯的文字方塊中輸入的字母 keyup 處理常式會執行。</span><span class="sxs-lookup"><span data-stu-id="cfe93-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="cfe93-199">Keyup 處理常式會啟用或停用根據與行為相關聯的文字方塊中是否包含任何文字按鈕。</span><span class="sxs-lookup"><span data-stu-id="cfe93-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="cfe93-200">請記住，您必須編譯 JavaScript 檔案中列出的 3，做為內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="cfe93-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="cfe93-201">在 [方案總管] 視窗中選取檔案、 開啟屬性工作表和指派值*內嵌資源*至**建置動作**屬性 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="cfe93-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="cfe93-202">在 Visual Studio 和 Visual Web Developer 中使用此選項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="cfe93-203">[![若要加入 JavaScript 檔案做為內嵌資源](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="cfe93-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span></span>

<span data-ttu-id="cfe93-204">**圖 03**： 加入 JavaScript 檔案做為內嵌資源 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="cfe93-205">建立自訂的擴充性設計工具</span><span class="sxs-lookup"><span data-stu-id="cfe93-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="cfe93-206">沒有一個我們需要建立完成我們 extender 的最後一個類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="cfe93-207">我們需要在列表 4 中建立設計工具類別。</span><span class="sxs-lookup"><span data-stu-id="cfe93-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="cfe93-208">這個類別才能讓正常運作，與 Visual Studio/Visual Web Developer 設計工具的擴充項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="cfe93-209">**列出 4-DisabledButtonDesigner.vb**</span><span class="sxs-lookup"><span data-stu-id="cfe93-209">**Listing 4 - DisabledButtonDesigner.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

<span data-ttu-id="cfe93-210">您可以在設計工具中列出的 4 關聯 DisabledButton extender 使用 Designer 屬性。您需要將設計工具屬性套用至 DisabledButtonExtender 類別就像這樣：</span><span class="sxs-lookup"><span data-stu-id="cfe93-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="cfe93-211">使用自訂的擴充性</span><span class="sxs-lookup"><span data-stu-id="cfe93-211">Using the Custom Extender</span></span>

<span data-ttu-id="cfe93-212">既然我們已經完成建立 DisabledButton 控制項擴充項，就可以使用我們的 ASP.NET 網站中開始。</span><span class="sxs-lookup"><span data-stu-id="cfe93-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="cfe93-213">首先，我們需要 [工具箱] 加入自訂的擴充性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="cfe93-214">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="cfe93-214">Follow these steps:</span></span>

1. <span data-ttu-id="cfe93-215">按兩下 [方案總管] 視窗中的頁面中開啟 ASP.NET 頁面。</span><span class="sxs-lookup"><span data-stu-id="cfe93-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="cfe93-216">以滑鼠右鍵按一下 [工具箱]，然後選取功能表選項**選擇項目**。</span><span class="sxs-lookup"><span data-stu-id="cfe93-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="cfe93-217">在 [選擇工具箱項目] 對話方塊中，瀏覽至 CustomExtenders.dll 組件。</span><span class="sxs-lookup"><span data-stu-id="cfe93-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="cfe93-218">按一下**確定**按鈕以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="cfe93-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="cfe93-219">完成這些步驟之後，DisabledButton 控制項擴充項應該會出現在工具箱中 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="cfe93-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="cfe93-220">[![在 [工具箱] 的 DisabledButton](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="cfe93-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span></span>

<span data-ttu-id="cfe93-221">**圖 04**： 在 [工具箱] 的 DisabledButton ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span></span>


<span data-ttu-id="cfe93-222">接下來，我們需要建立新的 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="cfe93-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="cfe93-223">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="cfe93-223">Follow these steps:</span></span>

1. <span data-ttu-id="cfe93-224">建立名為 ShowDisabledButton.aspx 的新 ASP.NET 頁面。</span><span class="sxs-lookup"><span data-stu-id="cfe93-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="cfe93-225">將 ScriptManager 拖曳至頁面。</span><span class="sxs-lookup"><span data-stu-id="cfe93-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="cfe93-226">將文字方塊控制項拖曳至頁面。</span><span class="sxs-lookup"><span data-stu-id="cfe93-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="cfe93-227">將按鈕控制項拖曳至頁面。</span><span class="sxs-lookup"><span data-stu-id="cfe93-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="cfe93-228">在 [屬性] 視窗中變更的按鈕 ID 屬性的值<em>btnSave</em>和文字屬性的值*儲存\**。</span><span class="sxs-lookup"><span data-stu-id="cfe93-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="cfe93-229">我們建立標準 ASP.NET 文字方塊和按鈕控制項的頁面。</span><span class="sxs-lookup"><span data-stu-id="cfe93-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="cfe93-230">接下來，我們必須擴充 DisabledButton extender 與文字方塊控制項：</span><span class="sxs-lookup"><span data-stu-id="cfe93-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="cfe93-231">選取**加入擴充項**工作以開啟 [擴充性精靈] 對話方塊的選項 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="cfe93-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="cfe93-232">請注意，對話方塊會包含自訂的 DisabledButton 擴充性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="cfe93-233">選取 DisabledButton 擴充性，然後按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cfe93-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="cfe93-234">[![[擴充性精靈] 對話方塊](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="cfe93-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span></span>

<span data-ttu-id="cfe93-235">**圖 05**: Extender 精靈對話方塊 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span></span>


<span data-ttu-id="cfe93-236">最後，我們可以設定 DisabledButton 擴充項的屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="cfe93-237">您可以藉由修改文字方塊控制項的內容修改 DisabledButton 擴充項的屬性：</span><span class="sxs-lookup"><span data-stu-id="cfe93-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="cfe93-238">在設計工具選取文字方塊。</span><span class="sxs-lookup"><span data-stu-id="cfe93-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="cfe93-239">在 屬性 視窗中，展開 Extender 節點 （請參閱圖 6）。</span><span class="sxs-lookup"><span data-stu-id="cfe93-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="cfe93-240">將值指派*儲存*DisabledText 屬性和值*btnSave* TargetButtonID 屬性。</span><span class="sxs-lookup"><span data-stu-id="cfe93-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="cfe93-241">[![設定擴充項屬性](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="cfe93-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span></span>

<span data-ttu-id="cfe93-242">**圖 06**： 設定擴充項屬性 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span></span>


<span data-ttu-id="cfe93-243">當您執行的頁面 （藉由按下 F5） 時，一開始會停用按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="cfe93-244">當您啟動的文字方塊中輸入文字時，控制項是這個按鈕會啟用 （請參閱圖 7）。</span><span class="sxs-lookup"><span data-stu-id="cfe93-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="cfe93-245">[![DisabledButton 擴充項中的動作](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="cfe93-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span></span>

<span data-ttu-id="cfe93-246">**圖 07**: DisabledButton extender 中動作 ([按一下以檢視完整大小的影像](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="cfe93-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="cfe93-247">總結</span><span class="sxs-lookup"><span data-stu-id="cfe93-247">Summary</span></span>

<span data-ttu-id="cfe93-248">本教學課程的目的是要說明如何擴充 AJAX Control Toolkit，含有自訂的擴充項控制項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="cfe93-249">在本教學課程中，我們會建立簡單的 DisabledButton 控制項擴充項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="cfe93-250">我們透過建立 DisabledButtonExtender 類別、 DisabledButtonBehavior JavaScript 行為，以及 DisabledButtonDesigner 類別實作此擴充項。</span><span class="sxs-lookup"><span data-stu-id="cfe93-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="cfe93-251">每當您建立自訂控制項擴充項時，您可以遵循一組類似的步驟。</span><span class="sxs-lookup"><span data-stu-id="cfe93-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="cfe93-252">上一步</span><span class="sxs-lookup"><span data-stu-id="cfe93-252">Previous</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
