---
title: "Nano Server 上的 ASP.NET Core"
author: shirhatti
description: "了解如何取得現有的 ASP.NET Core 應用程式，並將它部署到執行 IIS 的 Nano Server 執行個體。"
keywords: ASP.NET Core, Nano Server
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.assetid: 50922cf1-ca58-4006-9236-99b7ff2dd0cf
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: 337cc69ef522452c17cdd6ea4a5e71cd122035dc
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="40418-104">Nano Server 上的 ASP.NET Core 與 IIS</span><span class="sxs-lookup"><span data-stu-id="40418-104">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="40418-105">由 [Sourabh Shirhatti](https://twitter.com/sshirhatti) 提供</span><span class="sxs-lookup"><span data-stu-id="40418-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="40418-106">在本教學課程中，您將取得現有的 ASP.NET Core 應用程式，並將它部署到執行 IIS 的 Nano Server 執行個體。</span><span class="sxs-lookup"><span data-stu-id="40418-106">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="40418-107">簡介</span><span class="sxs-lookup"><span data-stu-id="40418-107">Introduction</span></span>

<span data-ttu-id="40418-108">Nano Server 是 Windows Server 2016 中的安裝選項，提供比 Server Core 或完整伺服器更小的使用量、更佳的安全性，以及更好的服務。</span><span class="sxs-lookup"><span data-stu-id="40418-108">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="40418-109">如需 180 天評估版的詳細資訊和下載連結，請參閱官方的 [Nano Server 文件](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server)。</span><span class="sxs-lookup"><span data-stu-id="40418-109">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="40418-110">有三種簡單的方法可讓您試用 Nano Server。</span><span class="sxs-lookup"><span data-stu-id="40418-110">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="40418-111">當您使用 MS 帳戶登入時：</span><span class="sxs-lookup"><span data-stu-id="40418-111">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="40418-112">您可以下載 Windows Server 2016 ISO 檔案，並建置 Nano Server 映像。</span><span class="sxs-lookup"><span data-stu-id="40418-112">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="40418-113">下載 Nano Server VHD。</span><span class="sxs-lookup"><span data-stu-id="40418-113">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="40418-114">使用 Azure 資源庫中的 Nano Server 映像，在 Azure 中建立 VM。</span><span class="sxs-lookup"><span data-stu-id="40418-114">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="40418-115">如果您沒有 Azure 帳戶，則可取得免費的 30 天試用版。</span><span class="sxs-lookup"><span data-stu-id="40418-115">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="40418-116">在本教學課程中，我們將使用第 2 個選項：Windows Server 2016 中預先建置的 Nano Server VHD。</span><span class="sxs-lookup"><span data-stu-id="40418-116">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="40418-117">在繼續進行此教學課程之前，您需要現有 ASP.NET Core 應用程式的[已發行輸出](xref:hosting/directory-structure)。</span><span class="sxs-lookup"><span data-stu-id="40418-117">Before proceeding with this tutorial, you will need the [published output](xref:hosting/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="40418-118">請確認應用程式已建置為在 **64 位元**處理序中執行。</span><span class="sxs-lookup"><span data-stu-id="40418-118">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="40418-119">設定 Nano Server 執行個體</span><span class="sxs-lookup"><span data-stu-id="40418-119">Setting up the Nano Server instance</span></span>

<span data-ttu-id="40418-120">利用先前下載的 VHD，在開發電腦上[使用 HYPER-V 建立新的虛擬機器](https://technet.microsoft.com/library/hh846766.aspx)。</span><span class="sxs-lookup"><span data-stu-id="40418-120">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="40418-121">電腦會要求您在登入之前，先設定系統管理員密碼。</span><span class="sxs-lookup"><span data-stu-id="40418-121">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="40418-122">在 VM 主控台中，按 F11 來設定密碼，才能進行第一次登入。</span><span class="sxs-lookup"><span data-stu-id="40418-122">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="40418-123">您還需要檢查新 VM 的 IP 位址，不論檢查的是佈建 VM 時或在 Nano Server 復原主控台的網路設定中，所提供的 DHCP 伺服器固定 IP。</span><span class="sxs-lookup"><span data-stu-id="40418-123">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="40418-124">例如，假設新的 VM 是使用本機 V4 IP 位址 192.168.1.10 執行。</span><span class="sxs-lookup"><span data-stu-id="40418-124">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="40418-125">現在，您可以使用 PowerShell 遠端進行管理，這是完全管理 Nano Server 的唯一方法。</span><span class="sxs-lookup"><span data-stu-id="40418-125">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="40418-126">使用 PowerShell 遠端連線到 Nano Server 執行個體</span><span class="sxs-lookup"><span data-stu-id="40418-126">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="40418-127">開啟已提升權限的 PowerShell 視窗，將遠端 Nano Server 執行個體新增至 `TrustedHosts` 清單。</span><span class="sxs-lookup"><span data-stu-id="40418-127">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="40418-128">使用正確的 IP 位址取代 `$nanoServerIpAddress` 變數。</span><span class="sxs-lookup"><span data-stu-id="40418-128">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="40418-129">一旦您將 Nano Server 執行個體新增至 `TrustedHosts`，就可以使用 PowerShell 遠端與其連線。</span><span class="sxs-lookup"><span data-stu-id="40418-129">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="40418-130">成功的連線會產生一個提示，其格式如下所示：`[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="40418-130">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="40418-131">建立檔案共用</span><span class="sxs-lookup"><span data-stu-id="40418-131">Creating a file share</span></span>

<span data-ttu-id="40418-132">在 Nano Server 上建立檔案共用，以便發行的應用程式可以複製到其中。</span><span class="sxs-lookup"><span data-stu-id="40418-132">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="40418-133">在遠端工作階段中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="40418-133">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="40418-134">執行上述命令之後，您應該能夠在主機電腦的 Windows 檔案總管中前往 `\\192.168.1.10\AspNetCoreSampleForNano`，來存取這個共用。</span><span class="sxs-lookup"><span data-stu-id="40418-134">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="40418-135">在防火牆中開啟連接埠</span><span class="sxs-lookup"><span data-stu-id="40418-135">Open port in the firewall</span></span>

<span data-ttu-id="40418-136">在遠端工作階段中執行下列命令，以在防火牆中開啟連接埠，可讓 IIS 在連接埠 TCP/8000 上接聽 TCP 流量。</span><span class="sxs-lookup"><span data-stu-id="40418-136">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="40418-137">安裝 IIS</span><span class="sxs-lookup"><span data-stu-id="40418-137">Installing IIS</span></span>

<span data-ttu-id="40418-138">從 PowerShell 資源庫新增至 `NanoServerPackage` 提供者。</span><span class="sxs-lookup"><span data-stu-id="40418-138">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="40418-139">一旦安裝並匯入提供者，您就可以安裝 Windows 套件。</span><span class="sxs-lookup"><span data-stu-id="40418-139">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="40418-140">在稍早建立的 PowerShell 工作階段中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="40418-140">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="40418-141">若要快速確認 IIS 是否已正確設定，您可以前往 URL `http://192.168.1.10/` ，而且應該會看見歡迎頁面。</span><span class="sxs-lookup"><span data-stu-id="40418-141">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="40418-142">安裝 IIS 時，預設會建立在連接埠 80 上接聽的網站，稱為 `Default Web Site`。</span><span class="sxs-lookup"><span data-stu-id="40418-142">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="40418-143">安裝 ASP.NET Core Module (ANCM)</span><span class="sxs-lookup"><span data-stu-id="40418-143">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="40418-144">ASP.NET Core Module 是 IIS 7.5 + 模組，其負責 ASP.NET Core HTTP 接聽程式的處理序管理，以及對其所管理之處理序的 Proxy 要求。</span><span class="sxs-lookup"><span data-stu-id="40418-144">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="40418-145">目前，為 IIS 安裝 ASP.NET Core Module 的程序是手動作業。</span><span class="sxs-lookup"><span data-stu-id="40418-145">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="40418-146">您需要在一般 (不是 Nano) 電腦上安裝[.NET Core Windows Server 裝載套件組合](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe)。</span><span class="sxs-lookup"><span data-stu-id="40418-146">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="40418-147">在一般電腦上安裝套件組合之後，必須將下列檔案複製到我們稍早建立的檔案共用。</span><span class="sxs-lookup"><span data-stu-id="40418-147">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="40418-148">在含有 IIS 的一般 (不是 Nano) 伺服器上，執行下列複製命令：</span><span class="sxs-lookup"><span data-stu-id="40418-148">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="40418-149">在 Windows 10 電腦上使用 `C:\Program Files\IIS Express` 取代 `C:\windows\system32\inetsrv`。</span><span class="sxs-lookup"><span data-stu-id="40418-149">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="40418-150">在 Nano 端，您必須從我們稍早建立的檔案共用，將下列檔案複製到有效位置。</span><span class="sxs-lookup"><span data-stu-id="40418-150">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="40418-151">因此，執行下列複製命令：</span><span class="sxs-lookup"><span data-stu-id="40418-151">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="40418-152">在遠端工作階段中執行下列指令碼：</span><span class="sxs-lookup"><span data-stu-id="40418-152">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="40418-153">在上述步驟之後，從共用中刪除 *aspnetcore.dll* 和 *aspnetcore_schema.xml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="40418-153">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="40418-154">安裝 .NET Core Framework</span><span class="sxs-lookup"><span data-stu-id="40418-154">Installing .NET Core Framework</span></span>

<span data-ttu-id="40418-155">若應用程式以[與 Framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)發行，就必須在伺服器上安裝 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="40418-155">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="40418-156">在遠端 PowerShell 工作階段中使用 [dotnet-install.ps1 PowerShell 指令碼](https://dot.net/v1/dotnet-install.ps1)，以在 Nano Server 上安裝 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="40418-156">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="40418-157">使用 `-Version` 參數傳遞 CLI 版本：</span><span class="sxs-lookup"><span data-stu-id="40418-157">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="40418-158">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="40418-158">Publishing the application</span></span>

<span data-ttu-id="40418-159">將現有應用程式的已發行輸出複製到檔案共用的根目錄。</span><span class="sxs-lookup"><span data-stu-id="40418-159">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="40418-160">您可能需要變更 *web.config* 以指向 *dotnet.exe* 的解壓縮位置。</span><span class="sxs-lookup"><span data-stu-id="40418-160">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="40418-161">或者，您可以將 *dotnet.exe* 新增至 PATH。</span><span class="sxs-lookup"><span data-stu-id="40418-161">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="40418-162">*dotnet.exe* **不**在 PATH 上時，*web.config* 看起來如何的範例：</span><span class="sxs-lookup"><span data-stu-id="40418-162">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="40418-163">在遠端工作階段中執行下列命令，針對不同於預設網站的連接埠上所發行的應用程式，在 IIS 中建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="40418-163">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="40418-164">您還需要開啟該連接埠，才能存取 Web。</span><span class="sxs-lookup"><span data-stu-id="40418-164">You also need to open that port to access the web.</span></span> <span data-ttu-id="40418-165">為了簡單起見，此指令碼使用 `DefaultAppPool`。</span><span class="sxs-lookup"><span data-stu-id="40418-165">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="40418-166">如需在應用程式集區下執行的其他考量，請參閱[應用程式集區](xref:publishing/iis#application-pools)。</span><span class="sxs-lookup"><span data-stu-id="40418-166">For more considerations on running under an application pool, see [Application Pools](xref:publishing/iis#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="40418-167">在 Nano Server 上執行 .NET Core CLI 的已知問題和因應措施</span><span class="sxs-lookup"><span data-stu-id="40418-167">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="40418-168">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="40418-168">Running the application</span></span>

<span data-ttu-id="40418-169">透過瀏覽器前往 `http://192.168.1.10:8000`，即可存取已發行的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="40418-169">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="40418-170">如果您已遵循[記錄檔的建立和重新導向](xref:hosting/aspnet-core-module#log-creation-and-redirection)所述設定記錄，則可以檢視記錄檔，這些檔案位於 *C:\PublishedApps\AspNetCoreSampleForNano\logs*。</span><span class="sxs-lookup"><span data-stu-id="40418-170">If you've set up logging as described in [Log creation and redirection](xref:hosting/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>
