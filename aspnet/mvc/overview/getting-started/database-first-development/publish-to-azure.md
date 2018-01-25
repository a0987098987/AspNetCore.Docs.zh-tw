---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: "發行至 Azure 的 MVC 資料庫第一個站台 |Microsoft 文件"
author: tfitzmac
description: "使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: eadc0f2b08df29f80fe53d03cf88cd3cdcecfb12
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="737f0-104">發行 MVC 資料庫第一個站台至 Azure</span><span class="sxs-lookup"><span data-stu-id="737f0-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="737f0-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="737f0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="737f0-106">使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="737f0-107">此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="737f0-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="737f0-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="737f0-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="737f0-109">數列的這個部分著重在發行至 Azure 的 web 應用程式和資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="737f0-110">您可以閱讀本主題來了解發行 web 應用程式和資料庫，但實際執行的步驟，您必須在本教學課程開頭啟動。</span><span class="sxs-lookup"><span data-stu-id="737f0-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="737f0-111">請參閱[入門](setting-up-database.md)。</span><span class="sxs-lookup"><span data-stu-id="737f0-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="737f0-112">部署 Azure 上的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="737f0-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="737f0-113">您需要 Azure 帳戶，以完成本教學課程：</span><span class="sxs-lookup"><span data-stu-id="737f0-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="737f0-114">您可以[開啟免費的 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="737f0-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="737f0-115">您可以[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。</span><span class="sxs-lookup"><span data-stu-id="737f0-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="737f0-116">若要發行 web 應用程式，以滑鼠右鍵按一下專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="737f0-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![發佈站台](publish-to-azure/_static/image1.png)

<span data-ttu-id="737f0-118">選取 Microsoft Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="737f0-118">Select Microsoft Azure Websites.</span></span>

![選取 Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="737f0-120">如果您未登入 Azure，提供您的 Azure 帳戶的認證。</span><span class="sxs-lookup"><span data-stu-id="737f0-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="737f0-121">然後，選取 [新增] 建立新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="737f0-121">Then, select New to create a new web app.</span></span>

![新的站台](publish-to-azure/_static/image3.png)

<span data-ttu-id="737f0-123">建立 web 應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="737f0-123">Create a unique name for your web app.</span></span> <span data-ttu-id="737f0-124">如果看到綠色的核取記號，右邊的 [名稱] 欄位，您就知道是唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="737f0-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="737f0-125">選取 web 應用程式的區域。</span><span class="sxs-lookup"><span data-stu-id="737f0-125">Select a region for your web app.</span></span> <span data-ttu-id="737f0-126">選取**建立新的伺服器**資料庫，並提供適用於這個新的資料庫伺服器的使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="737f0-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="737f0-127">完成後，請按一下**建立**。</span><span class="sxs-lookup"><span data-stu-id="737f0-127">When finished, click **Create**.</span></span>

![建立站台](publish-to-azure/_static/image4.png)

<span data-ttu-id="737f0-129">您的連線值現在所有設定。</span><span class="sxs-lookup"><span data-stu-id="737f0-129">Your connection values are now all set.</span></span> <span data-ttu-id="737f0-130">您可以將這些值不變。</span><span class="sxs-lookup"><span data-stu-id="737f0-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="737f0-132">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="737f0-132">Click **Next**.</span></span>

<span data-ttu-id="737f0-133">請注意兩個資料庫連接設定，會指定-ApplicationDBContext 和 ContosoUniversityDataEntities。</span><span class="sxs-lookup"><span data-stu-id="737f0-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="737f0-134">ApplicationDBContext 是使用者帳戶資料表的連接。</span><span class="sxs-lookup"><span data-stu-id="737f0-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="737f0-135">這些值只會顯示資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="737f0-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="737f0-136">它並不表示在發行您的網站時，將會發行這些資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="737f0-137">在您完成 web 應用程式發佈後，您將發行資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="737f0-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="737f0-138">資料庫連接旁邊的省略符號 （...） 會顯示連接字串的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="737f0-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="737f0-139">按一下 ContosoUniversityDataEntities 旁邊的省略符號。</span><span class="sxs-lookup"><span data-stu-id="737f0-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="737f0-140">請注意資料庫伺服器和資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="737f0-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="737f0-141">隨機產生的伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="737f0-141">The server name is randomly generated.</span></span> <span data-ttu-id="737f0-142">資料庫名稱是簡單的網站名稱 **\_db**附加。</span><span class="sxs-lookup"><span data-stu-id="737f0-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="737f0-143">稍後當您發行您的資料庫時，您會需要這兩個名稱。</span><span class="sxs-lookup"><span data-stu-id="737f0-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="737f0-144">按一下**確定**關閉資料庫連接字串 視窗。</span><span class="sxs-lookup"><span data-stu-id="737f0-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="737f0-145">在 [發行 Web] 視窗中，按一下**下一步**查看預覽。</span><span class="sxs-lookup"><span data-stu-id="737f0-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="737f0-146">您可以按一下 啟動預覽，請參閱要發佈之檔案的清單。</span><span class="sxs-lookup"><span data-stu-id="737f0-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="737f0-147">這是您已發佈此站台的第一次，因為清單是專案中每個相關的檔案。</span><span class="sxs-lookup"><span data-stu-id="737f0-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="737f0-148">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="737f0-148">Click **Publish**.</span></span>

<span data-ttu-id="737f0-149">[輸出] 窗格會顯示您的發行集的結果。</span><span class="sxs-lookup"><span data-stu-id="737f0-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="737f0-150">發行集，站台之後 immedialely 在網頁瀏覽器中啟動。</span><span class="sxs-lookup"><span data-stu-id="737f0-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="737f0-151">您的網站已部署，而且您可以註冊網站; 新的使用者不過，您 ContosoUniversityData 專案中的資料表有尚未發行。</span><span class="sxs-lookup"><span data-stu-id="737f0-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="737f0-152">如果您按一下的學生連結清單，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="737f0-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="737f0-153">發行到 SQL Azure 資料庫</span><span class="sxs-lookup"><span data-stu-id="737f0-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="737f0-154">發行前您的資料庫，您必須確定您的本機電腦可以連線到資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="737f0-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="737f0-155">您的資料庫伺服器的防火牆會限制哪些電腦可以連線至資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="737f0-156">您需要將您的電腦的 IP 位址新增至允許的 IP 位址的防火牆。</span><span class="sxs-lookup"><span data-stu-id="737f0-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="737f0-157">您透過 Azure 入口網站的 Azure 帳戶來登入。</span><span class="sxs-lookup"><span data-stu-id="737f0-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="737f0-158">選取新的資料庫，然後選取**管理**。</span><span class="sxs-lookup"><span data-stu-id="737f0-158">Select your new database and select **Manage**.</span></span>

![管理](publish-to-azure/_static/image10.png)

<span data-ttu-id="737f0-160">您必須設定您的資料庫伺服器，以允許從您的電腦連線。</span><span class="sxs-lookup"><span data-stu-id="737f0-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="737f0-161">選取 [管理]，系統會要求您加入目前的 IP 位址，為資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="737f0-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="737f0-162">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="737f0-162">Select Yes.</span></span>

![新增 ip 位址](publish-to-azure/_static/image11.png)

<span data-ttu-id="737f0-164">沒有您在上一個步驟中加入的 IP 位址不是唯一您需要設定連線的 IP 位址的機會。</span><span class="sxs-lookup"><span data-stu-id="737f0-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="737f0-165">您可以嘗試登入連線已正確設定資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="737f0-166">提供使用者名稱和您稍早建立的密碼。</span><span class="sxs-lookup"><span data-stu-id="737f0-166">Provide the user and password you created earlier.</span></span>

![登入](publish-to-azure/_static/image12.png)

<span data-ttu-id="737f0-168">如果您收到錯誤訊息，您需要加入其他 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="737f0-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="737f0-169">按一下要查看詳細錯誤的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="737f0-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="737f0-170">在詳細資料，您會看到您要新增的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="737f0-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="737f0-171">請注意此 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="737f0-171">Note this IP address.</span></span>

![不允許](publish-to-azure/_static/image13.png)

<span data-ttu-id="737f0-173">關閉此登入 視窗中，並返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="737f0-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="737f0-174">瀏覽至儀表板為您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="737f0-175">按一下**允許的 IP 位址管理**。</span><span class="sxs-lookup"><span data-stu-id="737f0-175">Click **Manage allowed IP addresses**.</span></span>

![管理 ip 位址](publish-to-azure/_static/image14.png)

<span data-ttu-id="737f0-177">您現在必須將 IP 位址從錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="737f0-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="737f0-178">變更要包含錯誤訊息中，從一個允許的 IP 位址範圍，或為個別項目加入該 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="737f0-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![加入新的位址](publish-to-azure/_static/image15.png)

<span data-ttu-id="737f0-180">將變更儲存至允許的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="737f0-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="737f0-181">按一下 [管理]，並再試一次登入資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="737f0-182">您可能需要等候幾分鐘的時間之前允許的 IP 位址已正確設定防火牆。</span><span class="sxs-lookup"><span data-stu-id="737f0-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="737f0-183">您可以順利登入資料庫，當您設定完成資料庫的連接。</span><span class="sxs-lookup"><span data-stu-id="737f0-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="737f0-184">因為您將會查看資料庫部署的結果，您可以讓此管理視窗保持開啟。</span><span class="sxs-lookup"><span data-stu-id="737f0-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="737f0-185">返回您的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="737f0-185">Return to your database project.</span></span> <span data-ttu-id="737f0-186">以滑鼠右鍵按一下專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="737f0-186">Right-click the project and select **Publish**.</span></span>

![發行資料庫](publish-to-azure/_static/image16.png)

<span data-ttu-id="737f0-188">在 [發行資料庫] 視窗中，選取**編輯**。</span><span class="sxs-lookup"><span data-stu-id="737f0-188">In the Publish Database window, select **Edit**.</span></span>

![編輯](publish-to-azure/_static/image17.png)

<span data-ttu-id="737f0-190">提供伺服器的資料庫伺服器，您的驗證認證的名稱。</span><span class="sxs-lookup"><span data-stu-id="737f0-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="737f0-191">提供認證之後，選取您從清單中可用的資料庫建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="737f0-192">根據預設，Visual Studio 設定資料庫欄位的名稱可能不是您所建立的資料庫與相同專案的名稱。</span><span class="sxs-lookup"><span data-stu-id="737f0-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="737f0-193">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="737f0-193">Click OK.</span></span>

<span data-ttu-id="737f0-194">您可能會想要儲存這個設定檔，因此您可以發佈在未來的更新，而不需重新輸入的所有連接資訊。</span><span class="sxs-lookup"><span data-stu-id="737f0-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="737f0-195">選取 [建立設定檔]。</span><span class="sxs-lookup"><span data-stu-id="737f0-195">Select **Create Profile**.</span></span>

![儲存設定檔](publish-to-azure/_static/image19.png)

<span data-ttu-id="737f0-197">它會建立名為專案中的檔案**ContosoUniversityData.publish.xml**。</span><span class="sxs-lookup"><span data-stu-id="737f0-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="737f0-198">下次您想要將資料庫發行至 Azure，只會載入該設定檔。</span><span class="sxs-lookup"><span data-stu-id="737f0-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="737f0-199">現在，按一下 **發行**Azure 上建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-199">Now, click **Publish** to create the database on Azure.</span></span>

![發行](publish-to-azure/_static/image20.png)

<span data-ttu-id="737f0-201">開始執行之後一段時間，會顯示發行結果。</span><span class="sxs-lookup"><span data-stu-id="737f0-201">After running for a while, the publishing results are displayed.</span></span>

![結果](publish-to-azure/_static/image21.png)

<span data-ttu-id="737f0-203">現在，您可以移回至管理入口網站為您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="737f0-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="737f0-204">重新整理 [設計] 檢視，並注意已部署 3 資料表的預先填入資料。</span><span class="sxs-lookup"><span data-stu-id="737f0-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![新的資料表](publish-to-azure/_static/image22.png)

<span data-ttu-id="737f0-206">現在您已準備好要測試 web 應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="737f0-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="737f0-207">瀏覽至 Azure 上 （例如 http://contosositeexample.azurewebsites.net/) 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="737f0-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="737f0-208">按一下 學員清單的連結，您應該會看到學生的索引檢視。</span><span class="sxs-lookup"><span data-stu-id="737f0-208">Click the link for List of students and you should see the index view for students.</span></span>

![view](publish-to-azure/_static/image23.png)

<span data-ttu-id="737f0-210">有時候，資料庫和連線需要一段時間才能正確地設定。</span><span class="sxs-lookup"><span data-stu-id="737f0-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="737f0-211">如果您收到錯誤，請稍候幾分鐘，然後再試。</span><span class="sxs-lookup"><span data-stu-id="737f0-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="737f0-212">結論</span><span class="sxs-lookup"><span data-stu-id="737f0-212">Conclusion</span></span>

<span data-ttu-id="737f0-213">這一系列提供如何從現有的資料庫，可讓使用者編輯、 更新、 建立和刪除的資料產生程式碼的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="737f0-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="737f0-214">它用於 ASP.NET MVC 5、 Entity Framework 和 ASP.NET Scaffolding 建立專案。</span><span class="sxs-lookup"><span data-stu-id="737f0-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="737f0-215">Code First 開發的簡介範例，請參閱[開始使用 ASP.NET MVC 5](../introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="737f0-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="737f0-216">如需更進階的範例，請參閱[建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="737f0-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="737f0-217">請注意，您用於第一個資料庫中的資料搭配使用 DbContext API 與您用於在第一個程式碼中使用資料的 API 相同。</span><span class="sxs-lookup"><span data-stu-id="737f0-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="737f0-218">即使您想要使用第一個資料庫，您可以了解如何處理複雜的案例，例如讀取及更新相關的資料處理並行衝突，從程式碼第一次的教學課程，依此類推。</span><span class="sxs-lookup"><span data-stu-id="737f0-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="737f0-219">唯一的差異是在建立資料庫、 內容類別和實體類別的方式。</span><span class="sxs-lookup"><span data-stu-id="737f0-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="737f0-220">上一步</span><span class="sxs-lookup"><span data-stu-id="737f0-220">Previous</span></span>](enhancing-data-validation.md)
