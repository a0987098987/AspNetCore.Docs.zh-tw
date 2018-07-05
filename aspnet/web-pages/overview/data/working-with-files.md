---
uid: web-pages/overview/data/working-with-files
title: 使用 ASP.NET Web Pages (Razor) 網站中的檔案 |Microsoft Docs
author: tfitzmac
description: 本章說明如何讀取、 寫入、 附加、 刪除和上傳檔案。
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 3778bc0041c56fe7164265fe565996aea5564bdf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841600"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="c0f2f-103">使用 ASP.NET Web Pages (Razor) 網站中的檔案</span><span class="sxs-lookup"><span data-stu-id="c0f2f-103">Working with Files in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="c0f2f-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c0f2f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c0f2f-105">這篇文章說明如何讀取、 寫入、 附加、 刪除和上傳的 ASP.NET Web Pages (Razor) 網站中的檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-105">This article explains how to read, write, append, delete, and upload files in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c0f2f-106">如果您想要將影像上傳及管理它們 （例如，翻轉或調整其大小），請參閱[使用 ASP.NET Web Pages 網站中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-106">If you want to upload images and manipulate them (for example, flip or resize them), see [Working with Images in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202897).</span></span>
> 
> 
> <span data-ttu-id="c0f2f-107">**您將學到什麼：**</span><span class="sxs-lookup"><span data-stu-id="c0f2f-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="c0f2f-108">如何建立文字檔，並寫入其中的資料。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-108">How to create a text file and write data to it.</span></span>
> - <span data-ttu-id="c0f2f-109">如何將資料附加到現有的檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-109">How to append data to an existing file.</span></span>
> - <span data-ttu-id="c0f2f-110">如何讀取檔案，並從它顯示。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-110">How to read a file and display from it.</span></span>
> - <span data-ttu-id="c0f2f-111">如何從網站中刪除檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-111">How to delete files from a website.</span></span>
> - <span data-ttu-id="c0f2f-112">如何讓使用者上傳一或多個檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-112">How to let users upload one file or multiple files.</span></span>
> 
> <span data-ttu-id="c0f2f-113">這些是 ASP.NET 程式設計文章中所引進的功能：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-113">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="c0f2f-114">`File`物件，可用來管理檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-114">The `File` object, which provides a way to manage files.</span></span>
> - <span data-ttu-id="c0f2f-115">`FileUpload`協助程式。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-115">The `FileUpload` helper.</span></span>
> - <span data-ttu-id="c0f2f-116">`Path`物件，提供方法，讓您管理的路徑和檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-116">The `Path` object, which provides methods that let you manipulate path and file names.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c0f2f-117">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="c0f2f-117">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c0f2f-118">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="c0f2f-118">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="c0f2f-119">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="c0f2f-119">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="c0f2f-120">本教學課程也適用於 WebMatrix 3。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-120">This tutorial also works with WebMatrix 3.</span></span>


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a><span data-ttu-id="c0f2f-121">建立文字檔案，並對其寫入資料</span><span class="sxs-lookup"><span data-stu-id="c0f2f-121">Creating a Text File and Writing Data to It</span></span>

<span data-ttu-id="c0f2f-122">除了在您的網站使用的資料庫，您可能會使用檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-122">In addition to using a database in your website, you might work with files.</span></span> <span data-ttu-id="c0f2f-123">比方說，您可能會將文字檔案做簡單的方式來儲存站台資料。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-123">For example, you might use text files as a simple way to store data for the site.</span></span> <span data-ttu-id="c0f2f-124">(有時稱為用來儲存資料的文字檔*一般檔案*。)文字檔可以是不同的格式，例如 *.txt*， *.xml*，或 *.csv* （以逗號分隔值）。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-124">(A text file that's used to store data is sometimes called a *flat file*.) Text files can be in different formats, like *.txt*, *.xml*, or *.csv* (comma-delimited values).</span></span>

<span data-ttu-id="c0f2f-125">如果您想要將資料儲存在文字檔中，您可以使用`File.WriteAllText`方法，以指定要建立的檔案和要寫入其中的資料。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-125">If you want to store data in a text file, you can use the `File.WriteAllText` method to specify the file to create and the data to write to it.</span></span> <span data-ttu-id="c0f2f-126">在此程序中，您將建立包含三個簡單的表單的頁面`input`項目 （名字、 姓氏和電子郵件地址） 和**送出** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-126">In this procedure, you'll create a page that contains a simple form with three `input` elements (first name, last name, and email address) and a **Submit** button.</span></span> <span data-ttu-id="c0f2f-127">當使用者提交表單時，您將會儲存使用者的輸入文字檔案中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-127">When the user submits the form, you'll store the user's input in a text file.</span></span>

1. <span data-ttu-id="c0f2f-128">建立新的資料夾，名為*應用程式\_資料*，如果尚不存在。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-128">Create a new folder named *App\_Data*, if it doesn't exist already.</span></span>
2. <span data-ttu-id="c0f2f-129">在您的網站根目錄中，建立新的檔案，名為*UserData.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-129">At the root of your website, create a new file named *UserData.cshtml*.</span></span>
3. <span data-ttu-id="c0f2f-130">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-130">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    <span data-ttu-id="c0f2f-131">HTML 標記會建立具有三個文字方塊的表單。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-131">The HTML markup creates the form with the three text boxes.</span></span> <span data-ttu-id="c0f2f-132">在程式碼中，您會使用`IsPost`屬性來判斷頁面是否已提交之前開始處理。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-132">In the code, you use the `IsPost` property to determine whether the page has been submitted before you start processing.</span></span>

    <span data-ttu-id="c0f2f-133">第一項工作是取得使用者輸入，並將它指派給變數。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-133">The first task is to get the user input and assign it to variables.</span></span> <span data-ttu-id="c0f2f-134">然後，程式碼會將個別變數的值串連成一個逗號分隔的字串，然後儲存在不同的變數。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-134">The code then concatenates the values of the separate variables into one comma-delimited string, which is then stored in a different variable.</span></span> <span data-ttu-id="c0f2f-135">請注意，逗號分隔符號字串，包含在引號 （"，"），因為您實際上內嵌逗號到您要建立大型字串。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-135">Notice that the comma separator is a string contained in quotation marks (","), because you're literally embedding a comma into the big string that you're creating.</span></span> <span data-ttu-id="c0f2f-136">在您將串連在一起的資料結尾，您將新增`Environment.NewLine`。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-136">At the end of the data that you concatenate together, you add `Environment.NewLine`.</span></span> <span data-ttu-id="c0f2f-137">這會將換行符號 （新行字元）。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-137">This adds a line break (a newline character).</span></span> <span data-ttu-id="c0f2f-138">您要建立與所有這種串連是看起來像這樣的字串：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-138">What you're creating with all this concatenation is a string that looks like this:</span></span>

    [!code-css[Main](working-with-files/samples/sample2.css)]

    <span data-ttu-id="c0f2f-139">（含不可見行結尾的符號。）</span><span class="sxs-lookup"><span data-stu-id="c0f2f-139">(With an invisible line break at the end.)</span></span>

    <span data-ttu-id="c0f2f-140">接著，您建立的變數 (`dataFile`)，其中包含要將資料儲存在檔案名稱與位置。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-140">You then create a variable (`dataFile`) that contains the location and name of the file to store the data in.</span></span> <span data-ttu-id="c0f2f-141">設定位置需要一些特殊處理。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-141">Setting the location requires some special handling.</span></span> <span data-ttu-id="c0f2f-142">在網站中，並不適合在程式碼中參考為絕對路徑，例如*C:\Folder\File.txt* web 伺服器上的檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-142">In websites, it's a bad practice to refer in code to absolute paths like *C:\Folder\File.txt* for files on the web server.</span></span> <span data-ttu-id="c0f2f-143">如果移動網站時，絕對路徑會是錯誤。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-143">If a website is moved, an absolute path will be wrong.</span></span> <span data-ttu-id="c0f2f-144">此外，裝載站台 （而非您自己的電腦上） 您通常不甚至知道正確的路徑為何您要編寫指令碼時。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-144">Moreover, for a hosted site (as opposed to on your own computer) you typically don't even know what the correct path is when you're writing the code.</span></span>

    <span data-ttu-id="c0f2f-145">但有時 （如 now、 寫入檔案)，您需要完整的路徑。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-145">But sometimes (like now, for writing a file) you do need a complete path.</span></span> <span data-ttu-id="c0f2f-146">解決方法是使用`MapPath`方法的`Server`物件。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-146">The solution is to use the `MapPath` method of the `Server` object.</span></span> <span data-ttu-id="c0f2f-147">這是您的網站傳回的完整路徑。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-147">This returns the complete path to your website.</span></span> <span data-ttu-id="c0f2f-148">若要取得的網站根目錄中，您的使用者路徑`~`運算子 (represen 站台的虛擬根) 到`MapPath`。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-148">To get the path for the website root, you user the `~` operator (to represen the site's virtual root) to `MapPath`.</span></span> <span data-ttu-id="c0f2f-149">(您也可以傳遞給它，子資料夾名稱，例如 *~/App\_Data /*，以取得該資料夾的路徑。)然後，您可以串連至任何方法以建立完整的路徑會傳回的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-149">(You can also pass a subfolder name to it, like *~/App\_Data/*, to get the path for that subfolder.) You can then concatenate additional information onto whatever the method returns in order to create a complete path.</span></span> <span data-ttu-id="c0f2f-150">在此範例中，您可以新增的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-150">In this example, you add a file name.</span></span> <span data-ttu-id="c0f2f-151">(您可以深入了解如何使用中的檔案和資料夾路徑[ASP.NET Web Pages 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)</span><span class="sxs-lookup"><span data-stu-id="c0f2f-151">(You can read more about how to work with file and folder paths in [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)</span></span>

    <span data-ttu-id="c0f2f-152">檔案會儲存在*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-152">The file is saved in the *App\_Data* folder.</span></span> <span data-ttu-id="c0f2f-153">這個資料夾是用來儲存資料檔案中所述的 ASP.NET 中的特殊資料夾[使用 ASP.NET Web Pages 網站中的資料庫簡介](https://go.microsoft.com/fwlink/?LinkId=195209)。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-153">This folder is a special folder in ASP.NET that's used to store data files, as described in [Introduction to Working with a Database in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=195209).</span></span>

    <span data-ttu-id="c0f2f-154">`WriteAllText`方法的`File`物件會將資料寫入檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-154">The `WriteAllText` method of the `File` object writes the data to the file.</span></span> <span data-ttu-id="c0f2f-155">這個方法會採用兩個參數： 要寫入的檔案和要寫入的實際資料的名稱 （使用路徑）。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-155">This method takes two parameters: the name (with path) of the file to write to, and the actual data to write.</span></span> <span data-ttu-id="c0f2f-156">請注意，第一個參數的名稱具有`@`字元作為前置詞。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-156">Notice that the name of the first parameter has an `@` character as a prefix.</span></span> <span data-ttu-id="c0f2f-157">這會告訴的 ASP.NET 提供逐字字串常值，且該字元像是"/"不應該以特殊方式解譯。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-157">This tells ASP.NET that you're providing a verbatim string literal, and that characters like "/" should not be interpreted in special ways.</span></span> <span data-ttu-id="c0f2f-158">(如需詳細資訊，請參閱 < [ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)</span><span class="sxs-lookup"><span data-stu-id="c0f2f-158">(For more information, see [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)</span></span>

    > [!NOTE]
    > <span data-ttu-id="c0f2f-159">為了讓您儲存檔案中的程式碼*應用程式\_資料*資料夾中，應用程式需要讀取-寫入該資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-159">In order for your code to save files in the *App\_Data* folder, the application needs read-write permissions for that folder.</span></span> <span data-ttu-id="c0f2f-160">在您的開發電腦上不通常發生問題。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-160">On your development computer this is not typically an issue.</span></span> <span data-ttu-id="c0f2f-161">不過，當您發行您的網站以裝載提供者的 web 伺服器時，您可能需要明確地設定這些權限。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-161">However, when you publish your site to a hosting provider's web server, you might need to explicitly set those permissions.</span></span> <span data-ttu-id="c0f2f-162">如果您裝載提供者的伺服器上執行此程式碼，而且發生錯誤，請與主機服務提供者，了解如何設定這些權限。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-162">If you run this code on a hosting provider's server and get errors, check with the hosting provider to find out how to set those permissions.</span></span>

- <span data-ttu-id="c0f2f-163">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-163">Run the page in a browser.</span></span> 

    ![](working-with-files/_static/image1.jpg)
- <span data-ttu-id="c0f2f-164">在欄位中輸入值，然後按一下**送出**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-164">Enter values into the fields and then click **Submit**.</span></span>
- <span data-ttu-id="c0f2f-165">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-165">Close the browser.</span></span>
- <span data-ttu-id="c0f2f-166">返回專案並重新整理檢視。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-166">Return to the project and refresh the view.</span></span>
- <span data-ttu-id="c0f2f-167">開啟*data.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-167">Open the *data.txt* file.</span></span> <span data-ttu-id="c0f2f-168">您提交表單中的資料位於檔案中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-168">The data you submitted in the form is in the file.</span></span> 

    ![[影像]](working-with-files/_static/image2.jpg)
- <span data-ttu-id="c0f2f-170">關閉*data.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-170">Close the *data.txt* file.</span></span>

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a><span data-ttu-id="c0f2f-171">將資料附加到現有的檔案</span><span class="sxs-lookup"><span data-stu-id="c0f2f-171">Appending Data to an Existing File</span></span>

<span data-ttu-id="c0f2f-172">在上述範例中，您已使用`WriteAllText`建立一個文字檔，裡面有一個資料片段。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-172">In the previous example, you used `WriteAllText` to create a text file that's got just one piece of data in it.</span></span> <span data-ttu-id="c0f2f-173">如果您再次呼叫方法，並將它傳遞相同的檔案名稱，會完全覆寫現有的檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-173">If you call the method again and pass it the same file name, the existing file is completely overwritten.</span></span> <span data-ttu-id="c0f2f-174">不過，您已建立檔案之後您通常想要將新資料加入至檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-174">However, after you've created a file you often want to add new data to the end of the file.</span></span> <span data-ttu-id="c0f2f-175">您可以執行的方法使用`AppendAllText`方法的`File`物件。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-175">You can do that using the `AppendAllText` method of the `File` object.</span></span>

1. <span data-ttu-id="c0f2f-176">在網站中，建立一份*UserData.cshtml*檔案，並命名為複製*UserDataMultiple.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-176">In the website, make a copy of the *UserData.cshtml* file and name the copy *UserDataMultiple.cshtml*.</span></span>
2. <span data-ttu-id="c0f2f-177">取代程式碼區塊，才能開啟`<!DOCTYPE html>`以下列程式碼區塊的標記：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-177">Replace the code block before the opening `<!DOCTYPE html>` tag with the following code block:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    <span data-ttu-id="c0f2f-178">此程式碼中有一項變更，從先前的範例。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-178">This code has one change in it from the previous example.</span></span> <span data-ttu-id="c0f2f-179">而不是使用`WriteAllText`，它會使用`the AppendAllText`方法。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-179">Instead of using `WriteAllText`, it uses `the AppendAllText` method.</span></span> <span data-ttu-id="c0f2f-180">方法很相似，不同之處在於`AppendAllText`將資料加入至檔案結尾。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-180">The methods are similar, except that `AppendAllText` adds the data to the end of the file.</span></span> <span data-ttu-id="c0f2f-181">如同`WriteAllText`，`AppendAllText`不存在時，會建立檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-181">As with `WriteAllText`, `AppendAllText` creates the file if it doesn't already exist.</span></span>
3. <span data-ttu-id="c0f2f-182">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-182">Run the page in a browser.</span></span>
4. <span data-ttu-id="c0f2f-183">輸入欄位的值，然後按一下**送出**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-183">Enter values for the fields and then click **Submit**.</span></span>
5. <span data-ttu-id="c0f2f-184">新增更多資料，並再次提交表單。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-184">Add more data and submit the form again.</span></span>
6. <span data-ttu-id="c0f2f-185">返回您的專案，以滑鼠右鍵按一下專案資料夾，然後按一下**重新整理**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-185">Return to your project, right-click the project folder, and then click **Refresh**.</span></span>
7. <span data-ttu-id="c0f2f-186">開啟*data.txt*檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-186">Open the *data.txt* file.</span></span> <span data-ttu-id="c0f2f-187">它現在會包含您剛才輸入的新資料。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-187">It now contains the new data that you just entered.</span></span> 

    ![[影像]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a><span data-ttu-id="c0f2f-189">讀取和顯示資料從檔案</span><span class="sxs-lookup"><span data-stu-id="c0f2f-189">Reading and Displaying Data from a File</span></span>

<span data-ttu-id="c0f2f-190">即使您不需要將資料寫入到文字檔案，可能有時您必須從一個讀取的資料。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-190">Even if you don't need to write data to a text file, you'll probably sometimes need to read data from one.</span></span> <span data-ttu-id="c0f2f-191">若要這樣做，您可以再次使用`File`物件。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-191">To do this, you can again use the `File` object.</span></span> <span data-ttu-id="c0f2f-192">您可以使用`File`物件來個別讀取每一行 （以分行符號分隔），或是讀取個別的項目，不論分隔的方式。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-192">You can use the `File` object to read each line individually (separated by line breaks) or to read individual item no matter how they're separated.</span></span>

<span data-ttu-id="c0f2f-193">此程序會示範如何讀取並顯示您在上一個範例中建立的資料。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-193">This procedure shows you how to read and display the data that you created in the previous example.</span></span>

1. <span data-ttu-id="c0f2f-194">在您的網站根目錄中，建立新的檔案，名為*DisplayData.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-194">At the root of your website, create a new file named *DisplayData.cshtml*.</span></span>
2. <span data-ttu-id="c0f2f-195">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-195">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    <span data-ttu-id="c0f2f-196">程式碼會藉由讀取到變數，名為在前一個範例中所建立的檔案開始`userData`，使用這個方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-196">The code starts by reading the file that you created in the previous example into a variable named `userData`, using this method call:</span></span>

    [!code-css[Main](working-with-files/samples/sample5.css)]

    <span data-ttu-id="c0f2f-197">若要這樣做的程式碼位於`if`陳述式。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-197">The code to do this is inside an `if` statement.</span></span> <span data-ttu-id="c0f2f-198">當您想要讀取的檔案時，它是個不錯的主意，若要使用`File.Exists`方法，先判斷檔案是否可用。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-198">When you want to read a file, it's a good idea to use the `File.Exists` method to determine first whether the file is available.</span></span> <span data-ttu-id="c0f2f-199">程式碼也會檢查檔案是否是空的。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-199">The code also checks whether the file is empty.</span></span>

    <span data-ttu-id="c0f2f-200">頁面的主體包含兩個`foreach`迴圈巢狀方式置於另一個。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-200">The body of the page contains two `foreach` loops, one nested inside the other.</span></span> <span data-ttu-id="c0f2f-201">外部`foreach`迴圈一次取得一行，從資料檔。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-201">The outer `foreach` loop gets one line at a time from the data file.</span></span> <span data-ttu-id="c0f2f-202">在此情況下，檔案中的分行符號所定義的程式行&#8212;也就是每個資料項目位於它自己的那一行。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-202">In this case, the lines are defined by line breaks in the file &#8212; that is, each data item is on its own line.</span></span> <span data-ttu-id="c0f2f-203">外部迴圈會建立新的項目 (`<li>`項目) 內的已排序清單 (`<ol>`項目)。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-203">The outer loop creates a new item (`<li>` element) inside an ordered list (`<ol>` element).</span></span>

    <span data-ttu-id="c0f2f-204">內部迴圈會將每個資料列分割成使用逗號做為分隔符號項目 （欄位）。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-204">The inner loop splits each data line into items (fields) using a comma as a delimiter.</span></span> <span data-ttu-id="c0f2f-205">(根據先前的範例，這表示每一行都包含三個欄位&#8212;名字、 姓氏和電子郵件地址，每個以逗號分隔。)內部迴圈也會建立`<ul>`清單和顯示一個清單項目中的資料線，每個欄位。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-205">(Based on the previous example, this means that each line contains three fields &#8212; the first name, last name, and email address, each separated by a comma.) The inner loop also creates a `<ul>` list and displays one list item for each field in the data line.</span></span>

    <span data-ttu-id="c0f2f-206">程式碼說明如何使用兩種資料類型、 陣列和`char`資料型別。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-206">The code illustrates how to use two data types, an array and the `char` data type.</span></span> <span data-ttu-id="c0f2f-207">陣列是必要的因為`File.ReadAllLines`方法會傳回資料當做陣列。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-207">The array is required because the `File.ReadAllLines` method returns data as an array.</span></span> <span data-ttu-id="c0f2f-208">`char`資料類型是必要的因為`Split`方法會傳回`array`所在每個項目型別的`char`。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-208">The `char` data type is required because the `Split` method returns an `array` in which each element is of the type `char`.</span></span> <span data-ttu-id="c0f2f-209">(如需陣列的詳細資訊，請參閱 < [ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects)。)</span><span class="sxs-lookup"><span data-stu-id="c0f2f-209">(For information about arrays, see [Introduction to ASP.NET Web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)</span></span>
3. <span data-ttu-id="c0f2f-210">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-210">Run the page in a browser.</span></span> <span data-ttu-id="c0f2f-211">會顯示您在先前的範例輸入資料。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-211">The data you entered for the previous examples is displayed.</span></span> 

    ![[影像]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> <span data-ttu-id="c0f2f-213">**顯示從 Microsoft Excel 以逗號分隔檔案的資料**</span><span class="sxs-lookup"><span data-stu-id="c0f2f-213">**Displaying Data from a Microsoft Excel Comma-Delimited File**</span></span>
> 
> <span data-ttu-id="c0f2f-214">您可以使用 Microsoft Excel 來儲存為逗號分隔檔案的試算表中所包含的資料 (*.csv*檔案)。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-214">You can use Microsoft Excel to save the data contained in a spreadsheet as a comma-delimited file (*.csv* file).</span></span> <span data-ttu-id="c0f2f-215">這麼做之後，檔案會以純文字，不是以 Excel 格式儲存。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-215">When you do, the file is saved in plain text, not in Excel format.</span></span> <span data-ttu-id="c0f2f-216">在試算表中的每個資料列分隔文字檔中的分行符號，每個資料項目以逗號分隔。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-216">Each row in the spreadsheet is separated by a line break in the text file, and each data item is separated by a comma.</span></span> <span data-ttu-id="c0f2f-217">您可以使用上述範例所示的程式碼讀取 Excel 以逗號分隔的檔案，只要變更您的程式碼中的資料檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-217">You can use the code shown in the previous example to read an Excel comma-delimited file just by changing the name of the data file in your code.</span></span>


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a><span data-ttu-id="c0f2f-218">刪除檔案</span><span class="sxs-lookup"><span data-stu-id="c0f2f-218">Deleting Files</span></span>

<span data-ttu-id="c0f2f-219">若要刪除的檔案，從您的網站，您可以使用`File.Delete`方法。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-219">To delete files from your website, you can use the `File.Delete` method.</span></span> <span data-ttu-id="c0f2f-220">此程序示範如何讓使用者能夠刪除映像 (*.jpg*檔案) 從*映像*資料夾如果他們知道檔案的名稱。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-220">This procedure shows how to let users delete an image (*.jpg* file) from an *images* folder if they know the name of the file.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c0f2f-221">**重要**在生產網站中，您通常會限制誰可以對資料進行變更。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-221">**Important** In a production website, you typically restrict who's allowed to make changes to the data.</span></span> <span data-ttu-id="c0f2f-222">有關如何設定成員資格，以及有關如何授權使用者在站台上執行工作的相關資訊，請參閱[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-222">For information about how to set up membership and about ways to authorize users to perform tasks on the site, see [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).</span></span>


1. <span data-ttu-id="c0f2f-223">在網站中，建立名稱為的子*映像*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-223">In the website, create a subfolder named *images*.</span></span>
2. <span data-ttu-id="c0f2f-224">複製一或多個 *.jpg*將檔案*映像*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-224">Copy one or more *.jpg* files into the *images* folder.</span></span>
3. <span data-ttu-id="c0f2f-225">在網站根目錄中，建立新的檔案，名為*FileDelete.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-225">In the root of the website, create a new file named *FileDelete.cshtml*.</span></span>
4. <span data-ttu-id="c0f2f-226">以下列內容取代現有的內容：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-226">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    <span data-ttu-id="c0f2f-227">此頁面包含的表單，其中使用者可以在這裡輸入影像檔的名稱。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-227">This page contains a form where users can enter the name of an image file.</span></span> <span data-ttu-id="c0f2f-228">不進入 *.jpg*副檔名; 藉由限制這類的檔案名稱，協助您防止使用者刪除網站上的任意檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-228">They don't enter the *.jpg* file-name extension; by restricting the file name like this, you help prevents users from deleting arbitrary files on your site.</span></span>

    <span data-ttu-id="c0f2f-229">程式碼會讀取使用者輸入，並接著會建構的完整路徑的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-229">The code reads the file name that the user has entered and then constructs a complete path.</span></span> <span data-ttu-id="c0f2f-230">若要建立的路徑，此程式碼會使用目前的網站路徑 (傳回`Server.MapPath`方法)，則*映像*資料夾名稱、 使用者提供的名稱和".jpg"做為常值的字串。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-230">To create the path, the code uses the current website path (as returned by the `Server.MapPath` method), the *images* folder name, the name that the user has provided, and ".jpg" as a literal string.</span></span>

    <span data-ttu-id="c0f2f-231">若要刪除的檔案，程式碼會呼叫`File.Delete`方法，將您剛才建構的完整路徑傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-231">To delete the file, the code calls the `File.Delete` method, passing it the full path that you just constructed.</span></span> <span data-ttu-id="c0f2f-232">在結束標記時，程式碼會顯示確認訊息已刪除的檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-232">At the end of the markup, code displays a confirmation message that the file was deleted.</span></span>
5. <span data-ttu-id="c0f2f-233">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-233">Run the page in a browser.</span></span> 

    ![[影像]](working-with-files/_static/image5.jpg)
6. <span data-ttu-id="c0f2f-235">輸入要刪除，然後按一下 檔案名稱**送出**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-235">Enter the name of the file to delete and then click **Submit**.</span></span> <span data-ttu-id="c0f2f-236">如果刪除了檔案，檔案的名稱會顯示在頁面底部。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-236">If the file was deleted, the name of the file is displayed at the bottom of the page.</span></span>

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a><span data-ttu-id="c0f2f-237">讓使用者上傳檔案</span><span class="sxs-lookup"><span data-stu-id="c0f2f-237">Letting Users Upload a File</span></span>

<span data-ttu-id="c0f2f-238">`FileUpload`協助程式可讓使用者將檔案上傳至您的網站。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-238">The `FileUpload` helper lets users upload files to your website.</span></span> <span data-ttu-id="c0f2f-239">下列程序會示範如何讓使用者上傳一個檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-239">The procedure below shows you how to let users upload a single file.</span></span>

1. <span data-ttu-id="c0f2f-240">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您未在先前新增。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-240">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you didn't add it previously.</span></span>
2. <span data-ttu-id="c0f2f-241">在 *應用程式\_資料*資料夾中，建立新資料夾並將它命名*UploadedFiles*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-241">In the *App\_Data* folder, create a new a folder and name it *UploadedFiles*.</span></span>
3. <span data-ttu-id="c0f2f-242">在根目錄中，建立新的檔案，名為*FileUpload.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-242">In the root, create a new file named *FileUpload.cshtml*.</span></span>
4. <span data-ttu-id="c0f2f-243">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-243">Replace the existing content in the page with the following:</span></span> 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    <span data-ttu-id="c0f2f-244">頁面的內文部分使用`FileUpload`協助程式來建立上傳 方塊中，您應該已熟悉的按鈕：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-244">The body portion of the page uses the `FileUpload` helper to create the upload box and buttons that you're probably familiar with:</span></span>

    ![[影像]](working-with-files/_static/image6.jpg)

    <span data-ttu-id="c0f2f-246">您設定的屬性`FileUpload`協助程式會指定您想要上傳檔案的單一方塊，以及您想要讀取的 [提交] 按鈕**上傳**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-246">The properties that you set for the `FileUpload` helper specify that you want a single box for the file to upload and that you want the submit button to read **Upload**.</span></span> <span data-ttu-id="c0f2f-247">（您將在本文稍後新增更多的方塊）。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-247">(You'll add more boxes later in the article.)</span></span>

    <span data-ttu-id="c0f2f-248">當使用者按一下**上傳**，位於頁面頂端的程式碼取得的檔案，並將它儲存。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-248">When the user clicks **Upload**, the code at the top of the page gets the file and saves it.</span></span> <span data-ttu-id="c0f2f-249">`Request`平常用來取得從表單欄位的值的物件還擁有`Files`陣列，其中包含 （或多個檔案），已上傳。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-249">The `Request` object that you normally use to get values from form fields also has a `Files` array that contains the file (or files) that have been uploaded.</span></span> <span data-ttu-id="c0f2f-250">您可以取得個別的檔案，從陣列中的特定位置&#8212;，若要取得第一次上傳的檔案，您會收到`Request.Files[0]`，以取得第二個檔案中，會得到`Request.Files[1]`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-250">You can get individual files out of specific positions in the array &#8212; for example, to get the first uploaded file, you get `Request.Files[0]`, to get the second file, you get `Request.Files[1]`, and so on.</span></span> <span data-ttu-id="c0f2f-251">（請記住，在程式設計中，計算通常從零開始）。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-251">(Remember that in programming, counting usually starts at zero.)</span></span>

    <span data-ttu-id="c0f2f-252">當您擷取上傳的檔案時，您將它放在變數中 (在這裡， `uploadedFile`)，讓您可以操作。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-252">When you fetch an uploaded file, you put it in a variable (here, `uploadedFile`) so that you can manipulate it.</span></span> <span data-ttu-id="c0f2f-253">若要判斷上傳的檔案名稱，您只取得其`FileName`屬性。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-253">To determine the name of the uploaded file, you just get its `FileName` property.</span></span> <span data-ttu-id="c0f2f-254">不過，當使用者將檔案上傳，`FileName`包含使用者的原始名稱，其中包含的完整路徑。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-254">However, when the user uploads a file, `FileName` contains the user's original name, which includes the entire path.</span></span> <span data-ttu-id="c0f2f-255">它可能會看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-255">It might look like this:</span></span>

    <span data-ttu-id="c0f2f-256">*C:\Users\Public\Sample.txt*</span><span class="sxs-lookup"><span data-stu-id="c0f2f-256">*C:\Users\Public\Sample.txt*</span></span>

    <span data-ttu-id="c0f2f-257">您不想該路徑資訊，不過，因為這是使用者的電腦，不適用於您的伺服器上的路徑。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-257">You don't want all that path information, though, because that's the path on the user's computer, not for your server.</span></span> <span data-ttu-id="c0f2f-258">您只想實際檔案名稱 (*Sample.txt*)。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-258">You just want the actual file name (*Sample.txt*).</span></span> <span data-ttu-id="c0f2f-259">您可以使用去除只從路徑檔案`Path.GetFileName`方法，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-259">You can strip out just the file from a path by using the `Path.GetFileName` method, like this:</span></span>

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    <span data-ttu-id="c0f2f-260">`Path`物件是一個公用程式，有多種方法，就像這樣，您可以使用刪除路徑、 合併路徑，然後依此類推。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-260">The `Path` object is a utility that has a number of methods like this that you can use to strip paths, combine paths, and so on.</span></span>

    <span data-ttu-id="c0f2f-261">一旦上傳的檔案名稱後，您可以建立新的路徑，您要上傳的檔案儲存在您的網站。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-261">Once you've gotten the name of the uploaded file, you can build a new path for where you want to store the uploaded file in your website.</span></span> <span data-ttu-id="c0f2f-262">在此情況下，您將結合`Server.MapPath`，將資料夾的名稱 (*應用程式\_資料/UploadedFiles*)，與新已移除的檔案名稱，以建立新的路徑。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-262">In this case, you combine `Server.MapPath`, the folder names (*App\_Data/UploadedFiles*), and the newly stripped file name to create a new path.</span></span> <span data-ttu-id="c0f2f-263">接著，您就可以呼叫上傳的檔案`SaveAs`實際上儲存檔案的方法。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-263">You can then call the uploaded file's `SaveAs` method to actually save the file.</span></span>
5. <span data-ttu-id="c0f2f-264">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-264">Run the page in a browser.</span></span> 

    ![[影像]](working-with-files/_static/image7.jpg)
6. <span data-ttu-id="c0f2f-266">按一下 **瀏覽**，然後選取要上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-266">Click **Browse** and then select a file to upload.</span></span> 

    ![[影像]](working-with-files/_static/image8.jpg)

    <span data-ttu-id="c0f2f-268">文字方塊旁的**瀏覽**按鈕將包含路徑和檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-268">The text box next to the **Browse** button will contain the path and file location.</span></span>

    ![[影像]](working-with-files/_static/image9.jpg)
7. <span data-ttu-id="c0f2f-270">按一下 [上傳] 。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-270">Click **Upload**.</span></span>
8. <span data-ttu-id="c0f2f-271">在網站中，以滑鼠右鍵按一下專案資料夾，並再按**重新整理**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-271">In the website, right-click the project folder and then click **Refresh**.</span></span>
9. <span data-ttu-id="c0f2f-272">開啟*UploadedFiles*資料夾。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-272">Open the *UploadedFiles* folder.</span></span> <span data-ttu-id="c0f2f-273">您上傳的檔案是在資料夾中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-273">The file that you uploaded is in the folder.</span></span> 

    ![[影像]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a><span data-ttu-id="c0f2f-275">讓使用者上傳多個檔案</span><span class="sxs-lookup"><span data-stu-id="c0f2f-275">Letting Users Upload Multiple Files</span></span>

<span data-ttu-id="c0f2f-276">在上述範例中，您可以讓使用者上傳一個檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-276">In the previous example, you let users upload one file.</span></span> <span data-ttu-id="c0f2f-277">但您可以使用`FileUpload`一次上傳多個檔案的協助程式。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-277">But you can use the `FileUpload` helper to upload more than one file at a time.</span></span> <span data-ttu-id="c0f2f-278">這很方便的案例，例如上傳的相片，其中一個檔案上傳一次是單調乏味。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-278">This is handy for scenarios like uploading photos, where uploading one file at a time is tedious.</span></span> <span data-ttu-id="c0f2f-279">(您可以閱讀有關將在相片上傳[使用 ASP.NET Web Pages 網站中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)。)此範例示範如何讓使用者上傳兩個一次，雖然您可以使用相同的技巧上, 傳不止是這樣。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-279">(You can read about uploading photos in [Working with Images in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202897).) This example shows how to let users upload two at a time, although you can use the same technique to upload more than that.</span></span>

1. <span data-ttu-id="c0f2f-280">將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-280">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="c0f2f-281">建立名為的新頁面*FileUploadMultiple.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-281">Create a new page named *FileUploadMultiple.cshtml*.</span></span>
3. <span data-ttu-id="c0f2f-282">以下列內容取代現有的內容頁面中：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-282">Replace the existing content in the page with the following:</span></span>  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    <span data-ttu-id="c0f2f-283">在此範例中，`FileUpload`頁面的主體中的協助程式預設會設定讓使用者上傳兩個檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-283">In this example, the `FileUpload` helper in the body of the page is configured to let users upload two files by default.</span></span> <span data-ttu-id="c0f2f-284">因為`allowMoreFilesToBeAdded`設為`true`，helper 會呈現一個連結，可讓使用者將新增更多的上傳方塊：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-284">Because `allowMoreFilesToBeAdded` is set to `true`, the helper renders a link that lets user add more upload boxes:</span></span>

    ![[影像]](working-with-files/_static/image11.jpg)

    <span data-ttu-id="c0f2f-286">若要處理的使用者上傳的檔案，程式碼會使用相同的基本技巧，您在上一個範例中使用&#8212;取得檔案從`Request.Files`然後將它儲存。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-286">To process the files that the user uploads, the code uses the same basic technique that you used in the previous example &#8212; get a file from `Request.Files` and then save it.</span></span> <span data-ttu-id="c0f2f-287">（各種項目包括您需要如何取得正確的檔案名稱和路徑。）使用者可能上傳多個檔案，而您不知道有許多這次的創新。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-287">(Including the various things you need to do to get the right file name and path.) The innovation this time is that the user might be uploading multiple files and you don't know many.</span></span> <span data-ttu-id="c0f2f-288">若要了解，您可以取得`Request.Files.Count`。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-288">To find out, you can get `Request.Files.Count`.</span></span>

    <span data-ttu-id="c0f2f-289">使用此號碼在手中，您可以循環`Request.Files`，依次擷取每個檔案，並將它儲存。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-289">With this number in hand, you can loop through `Request.Files`, fetch each file in turn, and save it.</span></span> <span data-ttu-id="c0f2f-290">當您想要循環播放，已知的數次，透過集合時，您可以使用`for`迴圈中的，像這樣：</span><span class="sxs-lookup"><span data-stu-id="c0f2f-290">When you want to loop a known number of times through a collection, you can use a `for` loop, like this:</span></span>

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    <span data-ttu-id="c0f2f-291">變數`i`是只是暫時的計數器，就會從零到任何您所設定的時間上限。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-291">The variable `i` is just a temporary counter that will go from zero to whatever upper limit you set.</span></span> <span data-ttu-id="c0f2f-292">在此情況下，最高上限是檔案數目。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-292">In this case, the upper limit is the number of files.</span></span> <span data-ttu-id="c0f2f-293">但是，因為計數器便會啟動位於零，通常適用於計算在 ASP.NET 中的案例中，最高上限實際上是一個小於檔案計數。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-293">But because the counter starts at zero, as is typical for counting scenarios in ASP.NET, the upper limit is actually one less than the file count.</span></span> <span data-ttu-id="c0f2f-294">（如果這三個檔案上傳時的計數是 0 到 2。）</span><span class="sxs-lookup"><span data-stu-id="c0f2f-294">(If three files are uploaded, the count is zero to 2.)</span></span>

    <span data-ttu-id="c0f2f-295">`uploadedCount`變數加總，已成功上傳並儲存所有檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-295">The `uploadedCount` variable totals all the files that are successfully uploaded and saved.</span></span> <span data-ttu-id="c0f2f-296">此程式碼負責預期的檔案可能無法上傳的可能性。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-296">This code accounts for the possibility that an expected file may not be able to be uploaded.</span></span>
4. <span data-ttu-id="c0f2f-297">執行網頁瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-297">Run the page in a browser.</span></span> <span data-ttu-id="c0f2f-298">瀏覽器顯示的頁面和其上傳兩個方塊。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-298">The browser displays the page and its two upload boxes.</span></span>
5. <span data-ttu-id="c0f2f-299">選取要上傳的兩個檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-299">Select two files to upload.</span></span>
6. <span data-ttu-id="c0f2f-300">按一下 **加入另一個檔案**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-300">Click **Add another file**.</span></span> <span data-ttu-id="c0f2f-301">此頁面會顯示新的 [上傳] 方塊。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-301">The page displays a new upload box.</span></span> 

    ![[影像]](working-with-files/_static/image12.jpg)
7. <span data-ttu-id="c0f2f-303">按一下 [上傳] 。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-303">Click **Upload**.</span></span>
8. <span data-ttu-id="c0f2f-304">在網站中，以滑鼠右鍵按一下專案資料夾，並再按**重新整理**。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-304">In the website, right-click the project folder and then click **Refresh**.</span></span>
9. <span data-ttu-id="c0f2f-305">開啟*UploadedFiles*資料夾，以查看已成功上傳的檔案。</span><span class="sxs-lookup"><span data-stu-id="c0f2f-305">Open the *UploadedFiles* folder to see the successfully uploaded files.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="c0f2f-306">其他資源</span><span class="sxs-lookup"><span data-stu-id="c0f2f-306">Additional Resources</span></span>


[<span data-ttu-id="c0f2f-307">使用 ASP.NET Web Pages 網站中的映像</span><span class="sxs-lookup"><span data-stu-id="c0f2f-307">Working with Images in an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkId=202897)

[<span data-ttu-id="c0f2f-308">匯出至 CSV 檔案</span><span class="sxs-lookup"><span data-stu-id="c0f2f-308">Exporting to a CSV File</span></span>](https://msdn.microsoft.com/library/ms155919.aspx)
