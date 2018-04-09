---
uid: whitepapers/ms03-32-issue
title: 修正 「 無法使用伺服器應用程式 」 錯誤之後套用安全性更新 IE |Microsoft 文件
author: rick-anderson
description: 本白皮書說明 MS03 32 安全性更新可以修正這個問題會影響 ASP.NET 1.0 Wi-fi 上執行的應用程式的 Internet explorer 的修補程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a><span data-ttu-id="a8bec-103">修正 「 無法使用伺服器應用程式 」 錯誤 IE 的安全性更新</span><span class="sxs-lookup"><span data-stu-id="a8bec-103">Fix for 'Server Application Unavailable' Error after Applying Security Update for IE</span></span>
====================
> <span data-ttu-id="a8bec-104">本白皮書說明 MS03 32 安全性更新可以修正這個問題會影響 ASP.NET 1.0 應用程式在 Windows XP Professional 上執行的 Internet explorer 的修補程式。</span><span class="sxs-lookup"><span data-stu-id="a8bec-104">This paper describes the patch that fixes an issue with the MS03-32 Security Update for Internet Explorer that affects ASP.NET 1.0 applications running on Windows XP Professional.</span></span>
> 
> <span data-ttu-id="a8bec-105">適用於 ASP.NET 1.0 和 Windows XP Professional。</span><span class="sxs-lookup"><span data-stu-id="a8bec-105">Applies to ASP.NET 1.0 and Windows XP Professional.</span></span>


<span data-ttu-id="a8bec-106">Microsoft Internet Explorer 的安全性修補程式 MS03 32 安全性更新與 Windows XP 上執行 ASP.NET 1.0 識別發生問題。</span><span class="sxs-lookup"><span data-stu-id="a8bec-106">Microsoft identified an issue with the MS03-32 Security Update for Internet Explorer security patch and ASP.NET 1.0 running on Windows XP.</span></span> <span data-ttu-id="a8bec-107">可以安裝這個修補程式，以手動方式或從 Windows Update 網站取得最新重大更新。</span><span class="sxs-lookup"><span data-stu-id="a8bec-107">This patch can be installed manually or by obtaining recent critical updates from the Windows Update site.</span></span>

<span data-ttu-id="a8bec-108">此問題的徵兆是，在 Windows XP 電腦上安裝修補程式之後, 本機 IIS 5.1 web 伺服器上執行的 ASP.NET 應用程式的所有要求會都導致錯誤訊息，指出 「 伺服器應用程式無法使用 」。</span><span class="sxs-lookup"><span data-stu-id="a8bec-108">The symptom of this issue is that after installing the patch on a Windows XP machine, all requests to ASP.NET applications running on the local IIS 5.1 web server result in an error message saying "Server Application Unavailable".</span></span> <span data-ttu-id="a8bec-109">遠端 web 伺服器的要求，則不會受到影響。</span><span class="sxs-lookup"><span data-stu-id="a8bec-109">Requests to remote web servers are unaffected.</span></span>

<span data-ttu-id="a8bec-110">這個問題只會影響 Windows XP 上執行 ASP.NET 1.0 安裝。</span><span class="sxs-lookup"><span data-stu-id="a8bec-110">This issue only impacts installations running ASP.NET 1.0 on Windows XP.</span></span> <span data-ttu-id="a8bec-111">它不會影響執行 Windows 2000 或 Windows Server 2003 的電腦。</span><span class="sxs-lookup"><span data-stu-id="a8bec-111">It does not impact machines running Windows 2000 or Windows Server 2003.</span></span> <span data-ttu-id="a8bec-112">它也不會影響安裝的 ASP.NET 1.1 與執行 Windows XP 的電腦。</span><span class="sxs-lookup"><span data-stu-id="a8bec-112">It also does not impact machines running Windows XP with ASP.NET 1.1 installed.</span></span>

<span data-ttu-id="a8bec-113">請注意，此問題**不**安全性與 ASP.NET 的錯誤。</span><span class="sxs-lookup"><span data-stu-id="a8bec-113">Please note that this issue **is not** a security bug with ASP.NET.</span></span> <span data-ttu-id="a8bec-114">它**不**開啟，或允許對 ASP.NET 應用程式或伺服器的任何惡意攻擊。</span><span class="sxs-lookup"><span data-stu-id="a8bec-114">It **does not** open up or allow any malicious attacks against an ASP.NET application or server.</span></span> <span data-ttu-id="a8bec-115">相反地，它是純功能性 bug 修補程式本身所造成。</span><span class="sxs-lookup"><span data-stu-id="a8bec-115">Instead, it is purely a functional bug caused by the patch itself.</span></span>

<span data-ttu-id="a8bec-116">我們正在硬永久此問題的解決方案。</span><span class="sxs-lookup"><span data-stu-id="a8bec-116">We are working hard on a permanent solution for this issue.</span></span> <span data-ttu-id="a8bec-117">在此同時，您可以執行下列批次檔因應措施的問題。</span><span class="sxs-lookup"><span data-stu-id="a8bec-117">In the meantime, you can execute the following batch file as a workaround for the issue.</span></span> <span data-ttu-id="a8bec-118">批次檔會執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="a8bec-118">The batch file does the following:</span></span>

1. <span data-ttu-id="a8bec-119">停止 IIS 和 ASP.NET 狀態服務</span><span class="sxs-lookup"><span data-stu-id="a8bec-119">Stops the IIS and ASP.NET state services</span></span>
2. <span data-ttu-id="a8bec-120">刪除並重新建立已知的暫時密碼 ASPNET 帳戶</span><span class="sxs-lookup"><span data-stu-id="a8bec-120">Deletes and recreates the ASPNET account with a known temporary password</span></span>
3. <span data-ttu-id="a8bec-121">使用 Windows`runas`命令，以啟動建立 ASPNET 使用者設定檔的可執行檔</span><span class="sxs-lookup"><span data-stu-id="a8bec-121">Uses the Windows `runas` command to launch an executable that creates an ASPNET user profile</span></span>
4. <span data-ttu-id="a8bec-122">重新登錄 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="a8bec-122">Re-registers ASP.NET.</span></span> <span data-ttu-id="a8bec-123">這會建立新的隨機密碼的帳戶，並套用預設 ASP.NET 存取控制設定，</span><span class="sxs-lookup"><span data-stu-id="a8bec-123">This creates a new random password for the account and applies default ASP.NET access control settings for it</span></span>
5. <span data-ttu-id="a8bec-124">重新啟動 IIS 服務</span><span class="sxs-lookup"><span data-stu-id="a8bec-124">Restarts the IIS service</span></span>

<span data-ttu-id="a8bec-125">批次檔會包含硬式編碼暫時密碼的 「<strong>1pass@word</strong>」 將會提示您輸入 runas 命令批次檔執行時。</span><span class="sxs-lookup"><span data-stu-id="a8bec-125">The batch file contains a hardcoded temporary password of "<strong>1pass@word</strong>" which you will be prompted to enter for the runas command when the batch file is run.</span></span> <span data-ttu-id="a8bec-126">Runas 命令完成之後，ASPNET 帳戶密碼會重新建立強式隨機值。</span><span class="sxs-lookup"><span data-stu-id="a8bec-126">After the runas command completes, the ASPNET account password is recreated with a strong random value.</span></span> <span data-ttu-id="a8bec-127">請注意，如果硬式編碼密碼不符合密碼複雜性需求，您的環境中，批次檔可能會失敗。</span><span class="sxs-lookup"><span data-stu-id="a8bec-127">Note that the batch file may fail if the hardcoded password does not meet the password complexity requirements in your environment.</span></span> <span data-ttu-id="a8bec-128">如果是這樣，您可以變更適用於您環境的另一個值。</span><span class="sxs-lookup"><span data-stu-id="a8bec-128">If that's the case, you can change it to another value that is appropriate for your environment.</span></span>

<span data-ttu-id="a8bec-129">*> [!IMPORTANT]* 如果您加入自訂存取控制設定或 ASPNET 帳戶的資料庫帳戶權限，他們必須完成這個批次檔後重新建立。</span><span class="sxs-lookup"><span data-stu-id="a8bec-129">*> [!IMPORTANT]* If you have added custom access control settings or database account permissions for the ASPNET account, they will need to be recreated after this batch file completes.</span></span> <span data-ttu-id="a8bec-130">這是因為當帳戶重新建立，它會取得新的安全性識別碼 (SID)。</span><span class="sxs-lookup"><span data-stu-id="a8bec-130">This is because when the account is recreated, it will get a new security identifier (SID).</span></span>

<span data-ttu-id="a8bec-131">*> [!IMPORTANT]* 如果您執行 ASP.NET 工作者處理序，以自訂帳戶，ASPNET 帳戶以外，那麼您不應該執行此批次檔。</span><span class="sxs-lookup"><span data-stu-id="a8bec-131">*> [!IMPORTANT]* If you are running the ASP.NET worker process with a custom account other than the ASPNET account, then you should not run this batch file.</span></span> <span data-ttu-id="a8bec-132">相反地，您應該以互動方式登入，或使用 runas 命令，與該帳戶將會建立該帳戶的使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="a8bec-132">Instead, you should log in interactively or use the runas command with that account which will create a user profile for that account.</span></span>

<span data-ttu-id="a8bec-133">批次檔隨附於下面的自我解壓縮的封存。</span><span class="sxs-lookup"><span data-stu-id="a8bec-133">The batch file is included in the self-extracting archive below.</span></span> <span data-ttu-id="a8bec-134">若要使用它：</span><span class="sxs-lookup"><span data-stu-id="a8bec-134">To use it:</span></span>

1. <span data-ttu-id="a8bec-135">您必須以系統管理權限執行身分帳戶</span><span class="sxs-lookup"><span data-stu-id="a8bec-135">You must be running as an account with Administrator privileges</span></span>
2. [<span data-ttu-id="a8bec-136">下載並開啟自動解壓縮可執行檔</span><span class="sxs-lookup"><span data-stu-id="a8bec-136">Download and open the self-extracting executable file</span></span>](ms03-32-issue/_static/fixup1.exe)
3. <span data-ttu-id="a8bec-137">擷取到 c:\ 的內容</span><span class="sxs-lookup"><span data-stu-id="a8bec-137">Extract the contents to c:\\</span></span>
4. <span data-ttu-id="a8bec-138">選取 [執行] 從 [開始] 功能表，然後輸入 `cmd.exe`</span><span class="sxs-lookup"><span data-stu-id="a8bec-138">Select Run... from the start menu, and enter `cmd.exe`</span></span>
5. <span data-ttu-id="a8bec-139">在 開啟 命令 視窗中，輸入`c:\fixup.cmd`。</span><span class="sxs-lookup"><span data-stu-id="a8bec-139">In the open command windows, type `c:\fixup.cmd`.</span></span>
6. <span data-ttu-id="a8bec-140">出現提示時，輸入<strong>1pass@word</strong>做為密碼。</span><span class="sxs-lookup"><span data-stu-id="a8bec-140">When prompted, enter <strong>1pass@word</strong> as the password.</span></span>
7. <span data-ttu-id="a8bec-141">如果您有先前自訂的存取控制設定或 ASPNET 帳戶的資料庫帳戶權限，您要立即重新套用這些設定。</span><span class="sxs-lookup"><span data-stu-id="a8bec-141">If you have previously custom access control settings or database account permissions for the ASPNET account, you'll need to re-apply these settings now.</span></span>

<span data-ttu-id="a8bec-142">在您的不便，這導致許多此。</span><span class="sxs-lookup"><span data-stu-id="a8bec-142">Many apologies for the inconvenience that this has caused.</span></span> <span data-ttu-id="a8bec-143">可用時，我們將會張貼的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="a8bec-143">We'll post additional information as it becomes available.</span></span>

<span data-ttu-id="a8bec-144">詳細資料平台和版本，此問題所致下方的矩陣。</span><span class="sxs-lookup"><span data-stu-id="a8bec-144">The matrix below details platforms and versions impacted by this issue.</span></span>

| <span data-ttu-id="a8bec-145">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="a8bec-145">.NET Framework</span></span> | <span data-ttu-id="a8bec-146">平台</span><span class="sxs-lookup"><span data-stu-id="a8bec-146">Platform</span></span> | <span data-ttu-id="a8bec-147">受影響</span><span class="sxs-lookup"><span data-stu-id="a8bec-147">Affected</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a8bec-148">1.0 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-148">Version 1.0</span></span> | <span data-ttu-id="a8bec-149">Windows 2000 Professional</span><span class="sxs-lookup"><span data-stu-id="a8bec-149">Windows 2000 Professional</span></span> | <span data-ttu-id="a8bec-150">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-150">No</span></span> |
| <span data-ttu-id="a8bec-151">1.0 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-151">Version 1.0</span></span> | <span data-ttu-id="a8bec-152">Windows 2000 Server</span><span class="sxs-lookup"><span data-stu-id="a8bec-152">Windows 2000 Server</span></span> | <span data-ttu-id="a8bec-153">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-153">No</span></span> |
| <span data-ttu-id="a8bec-154">1.0 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-154">Version 1.0</span></span> | <span data-ttu-id="a8bec-155">Windows XP Professional</span><span class="sxs-lookup"><span data-stu-id="a8bec-155">Windows XP Professional</span></span> | <span data-ttu-id="a8bec-156">[是]</span><span class="sxs-lookup"><span data-stu-id="a8bec-156">Yes</span></span> |
| <span data-ttu-id="a8bec-157">1.0 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-157">Version 1.0</span></span> | <span data-ttu-id="a8bec-158">Windows Server 2003</span><span class="sxs-lookup"><span data-stu-id="a8bec-158">Windows Server 2003</span></span> | <span data-ttu-id="a8bec-159">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-159">No</span></span> |
| <span data-ttu-id="a8bec-160">1.0 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-160">Version 1.0</span></span> | <span data-ttu-id="a8bec-161">Windows XP 家用版與 Cassini</span><span class="sxs-lookup"><span data-stu-id="a8bec-161">Windows XP Home with Cassini</span></span> | <span data-ttu-id="a8bec-162">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-162">No</span></span> |
| <span data-ttu-id="a8bec-163">1.1 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-163">Version 1.1</span></span> | <span data-ttu-id="a8bec-164">Windows 2000 Professional</span><span class="sxs-lookup"><span data-stu-id="a8bec-164">Windows 2000 Professional</span></span> | <span data-ttu-id="a8bec-165">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-165">No</span></span> |
| <span data-ttu-id="a8bec-166">1.1 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-166">Version 1.1</span></span> | <span data-ttu-id="a8bec-167">Windows 2000 Server</span><span class="sxs-lookup"><span data-stu-id="a8bec-167">Windows 2000 Server</span></span> | <span data-ttu-id="a8bec-168">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-168">No</span></span> |
| <span data-ttu-id="a8bec-169">1.1 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-169">Version 1.1</span></span> | <span data-ttu-id="a8bec-170">Windows XP Professional</span><span class="sxs-lookup"><span data-stu-id="a8bec-170">Windows XP Professional</span></span> | <span data-ttu-id="a8bec-171">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-171">No</span></span> |
| <span data-ttu-id="a8bec-172">1.1 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-172">Version 1.1</span></span> | <span data-ttu-id="a8bec-173">Windows Server 2003</span><span class="sxs-lookup"><span data-stu-id="a8bec-173">Windows Server 2003</span></span> | <span data-ttu-id="a8bec-174">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-174">No</span></span> |
| <span data-ttu-id="a8bec-175">1.1 版</span><span class="sxs-lookup"><span data-stu-id="a8bec-175">Version 1.1</span></span> | <span data-ttu-id="a8bec-176">Windows XP 家用版與 Cassini</span><span class="sxs-lookup"><span data-stu-id="a8bec-176">Windows XP Home with Cassini</span></span> | <span data-ttu-id="a8bec-177">否</span><span class="sxs-lookup"><span data-stu-id="a8bec-177">No</span></span> |

<span data-ttu-id="a8bec-178">感謝您，</span><span class="sxs-lookup"><span data-stu-id="a8bec-178">Thanks,</span></span>   
 <span data-ttu-id="a8bec-179">ASP.NET 團隊</span><span class="sxs-lookup"><span data-stu-id="a8bec-179">The ASP.NET Team</span></span>
