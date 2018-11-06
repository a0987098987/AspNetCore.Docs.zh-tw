---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: 將 MVC 資料庫第一個站台發佈至 Azure |Microsoft Docs
author: Rick-Anderson
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 1d2c26c211c5c8d97076327d01fe59d5ba4dc9ac
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021627"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="c8699-104">將 MVC 資料庫第一個站台發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="c8699-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="c8699-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c8699-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c8699-106">您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="c8699-107">本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="c8699-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="c8699-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="c8699-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="c8699-109">系列的這個部分會著重於將 web 應用程式和資料庫發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="c8699-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="c8699-110">您可以閱讀本主題以了解發行的 web 應用程式和資料庫，但實際執行的步驟，您必須在本教學課程的開頭開始。</span><span class="sxs-lookup"><span data-stu-id="c8699-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="c8699-111">請參閱[開始使用](setting-up-database.md)。</span><span class="sxs-lookup"><span data-stu-id="c8699-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="c8699-112">Web 應用程式在 Azure 上部署</span><span class="sxs-lookup"><span data-stu-id="c8699-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="c8699-113">您需要有 Azure 帳戶才能完成本教學課程：</span><span class="sxs-lookup"><span data-stu-id="c8699-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="c8699-114">您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="c8699-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="c8699-115">您可以[啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。</span><span class="sxs-lookup"><span data-stu-id="c8699-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="c8699-116">若要發佈您的 web 應用程式，以滑鼠右鍵按一下專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="c8699-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![發佈站台](publish-to-azure/_static/image1.png)

<span data-ttu-id="c8699-118">選取 Microsoft Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="c8699-118">Select Microsoft Azure Websites.</span></span>

![選取 Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="c8699-120">如果您未登入 Azure，提供您的 Azure 帳戶認證。</span><span class="sxs-lookup"><span data-stu-id="c8699-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="c8699-121">然後，選取 [新增] 建立新的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8699-121">Then, select New to create a new web app.</span></span>

![新的站台](publish-to-azure/_static/image3.png)

<span data-ttu-id="c8699-123">建立您的 web 應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="c8699-123">Create a unique name for your web app.</span></span> <span data-ttu-id="c8699-124">如果您看到綠色勾號右邊的 [名稱] 欄位，您會知道是唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="c8699-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="c8699-125">選取您的 web 應用程式的區域。</span><span class="sxs-lookup"><span data-stu-id="c8699-125">Select a region for your web app.</span></span> <span data-ttu-id="c8699-126">選取 **建立新的伺服器**資料庫，並提供適用於這個新的資料庫伺服器的使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="c8699-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="c8699-127">完成後，按一下**建立**。</span><span class="sxs-lookup"><span data-stu-id="c8699-127">When finished, click **Create**.</span></span>

![建立站台](publish-to-azure/_static/image4.png)

<span data-ttu-id="c8699-129">您的連線值現在所有設定。</span><span class="sxs-lookup"><span data-stu-id="c8699-129">Your connection values are now all set.</span></span> <span data-ttu-id="c8699-130">您可以保留這些值維持不變。</span><span class="sxs-lookup"><span data-stu-id="c8699-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="c8699-132">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="c8699-132">Click **Next**.</span></span>

<span data-ttu-id="c8699-133">如需設定，指定兩個資料庫連線的通知-ApplicationDBContext 和 ContosoUniversityDataEntities。</span><span class="sxs-lookup"><span data-stu-id="c8699-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="c8699-134">ApplicationDBContext 是使用者帳戶資料表的連接。</span><span class="sxs-lookup"><span data-stu-id="c8699-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="c8699-135">這些值只會顯示資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="c8699-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="c8699-136">它並不表示在發行您的網站時，將會發行這些資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="c8699-137">在您完成發佈的 web 應用程式之後，您將發行資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="c8699-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="c8699-138">資料庫連接旁邊的省略符號 （...） 會顯示連接字串的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="c8699-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="c8699-139">按一下 ContosoUniversityDataEntities 旁邊的省略符號。</span><span class="sxs-lookup"><span data-stu-id="c8699-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="c8699-140">請注意資料庫伺服器和資料庫的名稱。</span><span class="sxs-lookup"><span data-stu-id="c8699-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="c8699-141">隨機產生的伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="c8699-141">The server name is randomly generated.</span></span> <span data-ttu-id="c8699-142">資料庫名稱是簡單的網站名稱 **\_db**附加。</span><span class="sxs-lookup"><span data-stu-id="c8699-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="c8699-143">稍後當您發行您的資料庫時，您將需要這兩個名稱。</span><span class="sxs-lookup"><span data-stu-id="c8699-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="c8699-144">按一下 [**確定**關閉資料庫連接字串] 視窗。</span><span class="sxs-lookup"><span data-stu-id="c8699-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="c8699-145">在 發佈 Web 視窗中，按一下**下一步**以查看預覽。</span><span class="sxs-lookup"><span data-stu-id="c8699-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="c8699-146">您可以按一下 [開始預覽] 以查看要發行之檔案的清單。</span><span class="sxs-lookup"><span data-stu-id="c8699-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="c8699-147">由於這是您已發佈此站台的第一次，清單會為每個相關專案檔中。</span><span class="sxs-lookup"><span data-stu-id="c8699-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="c8699-148">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="c8699-148">Click **Publish**.</span></span>

<span data-ttu-id="c8699-149">[輸出] 窗格會顯示您的發行集的結果。</span><span class="sxs-lookup"><span data-stu-id="c8699-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="c8699-150">在發行集之後, 的站台會是 immedialely 在網頁瀏覽器中啟動。</span><span class="sxs-lookup"><span data-stu-id="c8699-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="c8699-151">已部署您的網站，而且您可以註冊站台，新的使用者不過，您 ContosoUniversityData 專案中的資料表有尚未發行。</span><span class="sxs-lookup"><span data-stu-id="c8699-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="c8699-152">如果您按一下學生連結的清單，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="c8699-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="c8699-153">將資料庫發佈到 SQL Azure</span><span class="sxs-lookup"><span data-stu-id="c8699-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="c8699-154">之前發行您的資料庫，您必須確定您的本機電腦可以連線到資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="c8699-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="c8699-155">您的資料庫伺服器的防火牆會限制機器可以連接到資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="c8699-156">您需要將您的電腦的 IP 位址新增至允許的 IP 位址的防火牆。</span><span class="sxs-lookup"><span data-stu-id="c8699-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="c8699-157">登入您的 Azure 帳戶，透過 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="c8699-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="c8699-158">選取您新的資料庫，然後選取**管理**。</span><span class="sxs-lookup"><span data-stu-id="c8699-158">Select your new database and select **Manage**.</span></span>

![管理](publish-to-azure/_static/image10.png)

<span data-ttu-id="c8699-160">您必須設定您的資料庫伺服器，以允許從您的電腦的連線。</span><span class="sxs-lookup"><span data-stu-id="c8699-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="c8699-161">當您選取 [管理] 時，系統會要求您加入目前的 IP 位址所允許的資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="c8699-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="c8699-162">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="c8699-162">Select Yes.</span></span>

![新增 ip 位址](publish-to-azure/_static/image11.png)

<span data-ttu-id="c8699-164">沒有您在上一個步驟中新增的 IP 位址不是唯一的 IP 位址，您需要設定連線的機會。</span><span class="sxs-lookup"><span data-stu-id="c8699-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="c8699-165">您可以嘗試登入若要查看如果連線已正確設定資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="c8699-166">提供使用者名稱和您稍早建立的密碼。</span><span class="sxs-lookup"><span data-stu-id="c8699-166">Provide the user and password you created earlier.</span></span>

![登入](publish-to-azure/_static/image12.png)

<span data-ttu-id="c8699-168">如果您收到錯誤訊息，您需要新增其他 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="c8699-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="c8699-169">按一下要查看錯誤的更多詳細的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c8699-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="c8699-170">在詳細資料，您會看到您要新增的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="c8699-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="c8699-171">請注意此 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="c8699-171">Note this IP address.</span></span>

![不允許](publish-to-azure/_static/image13.png)

<span data-ttu-id="c8699-173">關閉此 [登入] 視窗中，並返回 Azure 入口網站。</span><span class="sxs-lookup"><span data-stu-id="c8699-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="c8699-174">瀏覽至您的資料庫的儀表板。</span><span class="sxs-lookup"><span data-stu-id="c8699-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="c8699-175">按一下 **允許的 IP 位址管理**。</span><span class="sxs-lookup"><span data-stu-id="c8699-175">Click **Manage allowed IP addresses**.</span></span>

![管理 ip 位址](publish-to-azure/_static/image14.png)

<span data-ttu-id="c8699-177">您現在必須新增錯誤訊息中的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="c8699-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="c8699-178">變更要包含的一個錯誤訊息中，從允許的 IP 位址的範圍，或將該 IP 位址新增為個別的項目。</span><span class="sxs-lookup"><span data-stu-id="c8699-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![新增新的地址](publish-to-azure/_static/image15.png)

<span data-ttu-id="c8699-180">將變更儲存到 允許的 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="c8699-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="c8699-181">按一下 [管理]，並再試一次登入的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="c8699-182">您可能需要等候幾分鐘的時間之前允許的 IP 位址已正確設定防火牆。</span><span class="sxs-lookup"><span data-stu-id="c8699-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="c8699-183">您可以成功登入資料庫，當您設定完成資料庫的連接。</span><span class="sxs-lookup"><span data-stu-id="c8699-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="c8699-184">您可以將此管理視窗保持在開啟，因為您很快就會檢查資料庫部署的結果。</span><span class="sxs-lookup"><span data-stu-id="c8699-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="c8699-185">返回您的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="c8699-185">Return to your database project.</span></span> <span data-ttu-id="c8699-186">以滑鼠右鍵按一下專案，然後選取**發佈**。</span><span class="sxs-lookup"><span data-stu-id="c8699-186">Right-click the project and select **Publish**.</span></span>

![發行資料庫](publish-to-azure/_static/image16.png)

<span data-ttu-id="c8699-188">在 [發行資料庫] 視窗中，選取**編輯**。</span><span class="sxs-lookup"><span data-stu-id="c8699-188">In the Publish Database window, select **Edit**.</span></span>

![編輯](publish-to-azure/_static/image17.png)

<span data-ttu-id="c8699-190">提供伺服器的資料庫伺服器和您的驗證認證的名稱。</span><span class="sxs-lookup"><span data-stu-id="c8699-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="c8699-191">提供認證之後，選取您建立從清單中的可用資料庫的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="c8699-192">根據預設，Visual Studio 會設定資料庫欄位的名稱，您可能不會為您建立的資料庫相同的專案的名稱。</span><span class="sxs-lookup"><span data-stu-id="c8699-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="c8699-193">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="c8699-193">Click OK.</span></span>

<span data-ttu-id="c8699-194">您可能會想要儲存這個設定檔，因此您可以發佈在未來的更新，而不必重新輸入所有的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="c8699-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="c8699-195">選取 [建立設定檔]。</span><span class="sxs-lookup"><span data-stu-id="c8699-195">Select **Create Profile**.</span></span>

![儲存設定檔](publish-to-azure/_static/image19.png)

<span data-ttu-id="c8699-197">它會建立名為專案中的檔案**ContosoUniversityData.publish.xml**。</span><span class="sxs-lookup"><span data-stu-id="c8699-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="c8699-198">下次您想要將資料庫發佈至 Azure，只會載入該設定檔。</span><span class="sxs-lookup"><span data-stu-id="c8699-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="c8699-199">現在，按一下 **發佈**在 Azure 上建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-199">Now, click **Publish** to create the database on Azure.</span></span>

![發行](publish-to-azure/_static/image20.png)

<span data-ttu-id="c8699-201">執行一段時間之後, 會顯示發行的結果。</span><span class="sxs-lookup"><span data-stu-id="c8699-201">After running for a while, the publishing results are displayed.</span></span>

![結果](publish-to-azure/_static/image21.png)

<span data-ttu-id="c8699-203">現在，您可以移回至管理入口網站為您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="c8699-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="c8699-204">重新整理 [設計] 檢視中，並注意到已部署 3 個資料表的預先填入資料。</span><span class="sxs-lookup"><span data-stu-id="c8699-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![新的資料表](publish-to-azure/_static/image22.png)

<span data-ttu-id="c8699-206">現在您已準備好測試部署至 Azure web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c8699-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="c8699-207">瀏覽至 Azure 上的 web 應用程式 (例如 http://contosositeexample.azurewebsites.net/)。</span><span class="sxs-lookup"><span data-stu-id="c8699-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="c8699-208">按一下 學生清單的連結，您應該會看到適用於學生的 索引 檢視。</span><span class="sxs-lookup"><span data-stu-id="c8699-208">Click the link for List of students and you should see the index view for students.</span></span>

![view](publish-to-azure/_static/image23.png)

<span data-ttu-id="c8699-210">有時候，資料庫和連線需要一段時間才能正確設定。</span><span class="sxs-lookup"><span data-stu-id="c8699-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="c8699-211">如果您收到錯誤，請等候幾分鐘的時間，並再試一次。</span><span class="sxs-lookup"><span data-stu-id="c8699-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c8699-212">結論</span><span class="sxs-lookup"><span data-stu-id="c8699-212">Conclusion</span></span>

<span data-ttu-id="c8699-213">這一系列將提供如何從現有的資料庫，可讓使用者編輯、 更新、 建立和刪除的資料產生程式碼的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="c8699-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="c8699-214">它使用 ASP.NET MVC 5、 Entity Framework 與 ASP.NET Scaffolding 來建立專案。</span><span class="sxs-lookup"><span data-stu-id="c8699-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="c8699-215">Code First 開發簡介範例，請參閱[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="c8699-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="c8699-216">如需更進階的範例，請參閱 <<c0> [ 建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="c8699-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="c8699-217">請注意，您使用第一個資料庫中的資料使用 DbContext API 與您用來處理在 Code First 資料的 API 相同。</span><span class="sxs-lookup"><span data-stu-id="c8699-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="c8699-218">即使您想要使用第一個資料庫，您可以了解如何處理更複雜的案例，例如讀取及更新的相關的資料，處理並行衝突，Code First 的教學課程中，依此類推。</span><span class="sxs-lookup"><span data-stu-id="c8699-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="c8699-219">唯一的差別是在建立資料庫、 內容類別和實體類別的方式。</span><span class="sxs-lookup"><span data-stu-id="c8699-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8699-220">上一步</span><span class="sxs-lookup"><span data-stu-id="c8699-220">Previous</span></span>](enhancing-data-validation.md)
