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
ms.openlocfilehash: dd1f2c8de58ea8d3a57e64ecc519184400cb52c8
ms.sourcegitcommit: ad01283f299d346cf757c4f4744c48634dc27e73
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2017
---
# <a name="aspnet-core-with-iis-on-nano-server"></a>Nano Server 上的 ASP.NET Core 與 IIS

由 [Sourabh Shirhatti](https://twitter.com/sshirhatti) 提供

在本教學課程中，您將取得現有的 ASP.NET Core 應用程式，並將它部署到執行 IIS 的 Nano Server 執行個體。

## <a name="introduction"></a>簡介

Nano Server 是 Windows Server 2016 中的安裝選項，提供比 Server Core 或完整伺服器更小的使用量、更佳的安全性，以及更好的服務。 如需 180 天評估版的詳細資訊和下載連結，請參閱官方的 [Nano Server 文件](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server)。 

有三種簡單的方法可讓您試用 Nano Server。 當您使用 MS 帳戶登入時：

1. 您可以下載 Windows Server 2016 ISO 檔案，並建置 Nano Server 映像。

2. 下載 Nano Server VHD。

3. 使用 Azure 資源庫中的 Nano Server 映像，在 Azure 中建立 VM。 如果您沒有 Azure 帳戶，則可取得免費的 30 天試用版。

在本教學課程中，我們將使用第 2 個選項：Windows Server 2016 中預先建置的 Nano Server VHD。

在繼續進行此教學課程之前，您需要現有 ASP.NET Core 應用程式的[已發行輸出](xref:hosting/directory-structure)。 請確認應用程式已建置為在 **64 位元**處理序中執行。

## <a name="setting-up-the-nano-server-instance"></a>設定 Nano Server 執行個體

利用先前下載的 VHD，在開發電腦上[使用 HYPER-V 建立新的虛擬機器](https://technet.microsoft.com/library/hh846766.aspx)。 電腦會要求您在登入之前，先設定系統管理員密碼。 在 VM 主控台中，按 F11 來設定密碼，才能進行第一次登入。 您還需要檢查新 VM 的 IP 位址，不論檢查的是佈建 VM 時或在 Nano Server 復原主控台的網路設定中，所提供的 DHCP 伺服器固定 IP。

> [!NOTE]
> 例如，假設新的 VM 是使用本機 V4 IP 位址 192.168.1.10 執行。

現在，您可以使用 PowerShell 遠端進行管理，這是完全管理 Nano Server 的唯一方法。

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a>使用 PowerShell 遠端連線到 Nano Server 執行個體

開啟已提升權限的 PowerShell 視窗，將遠端 Nano Server 執行個體新增至 `TrustedHosts` 清單。

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> 使用正確的 IP 位址取代 `$nanoServerIpAddress` 變數。

一旦您將 Nano Server 執行個體新增至 `TrustedHosts`，就可以使用 PowerShell 遠端與其連線。

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

成功的連線會產生一個提示，其格式如下所示：`[192.168.1.10]: PS C:\Users\Administrator\Documents>`

## <a name="creating-a-file-share"></a>建立檔案共用

在 Nano Server 上建立檔案共用，以便發行的應用程式可以複製到其中。 在遠端工作階段中執行下列命令：

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

執行上述命令之後，您應該能夠在主機電腦的 Windows 檔案總管中前往 `\\192.168.1.10\AspNetCoreSampleForNano`，來存取這個共用。

## <a name="open-port-in-the-firewall"></a>在防火牆中開啟連接埠

在遠端工作階段中執行下列命令，以在防火牆中開啟連接埠，可讓 IIS 在連接埠 TCP/8000 上接聽 TCP 流量。

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a>安裝 IIS

從 PowerShell 資源庫新增至 `NanoServerPackage` 提供者。 一旦安裝並匯入提供者，您就可以安裝 Windows 套件。

在稍早建立的 PowerShell 工作階段中執行下列命令：

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

若要快速確認 IIS 是否已正確設定，您可以前往 URL `http://192.168.1.10/` ，而且應該會看見歡迎頁面。 安裝 IIS 時，預設會建立在連接埠 80 上接聽的網站，稱為 `Default Web Site`。

## <a name="installing-the-aspnet-core-module-ancm"></a>安裝 ASP.NET Core Module (ANCM)

ASP.NET Core Module 是 IIS 7.5 + 模組，其負責 ASP.NET Core HTTP 接聽程式的處理序管理，以及對其所管理之處理序的 Proxy 要求。 目前，為 IIS 安裝 ASP.NET Core Module 的程序是手動作業。 您需要在一般 (不是 Nano) 電腦上安裝[.NET Core Windows Server 裝載套件組合](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe)。 在一般電腦上安裝套件組合之後，必須將下列檔案複製到我們稍早建立的檔案共用。

在含有 IIS 的一般 (不是 Nano) 伺服器上，執行下列複製命令：

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

在 Windows 10 電腦上使用 `C:\Program Files\IIS Express` 取代 `C:\windows\system32\inetsrv`。

在 Nano 端，您必須從我們稍早建立的檔案共用，將下列檔案複製到有效位置。 因此，執行下列複製命令：

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

在遠端工作階段中執行下列指令碼：

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
> 在上述步驟之後，從共用中刪除 *aspnetcore.dll* 和 *aspnetcore_schema.xml* 檔案。

## <a name="installing-net-core-framework"></a>安裝 .NET Core Framework

若應用程式以[與 Framework 相依的部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)發行，就必須在伺服器上安裝 .NET Core。 在遠端 PowerShell 工作階段中使用 [dotnet-install.ps1 PowerShell 指令碼](https://dot.net/v1/dotnet-install.ps1)，以在 Nano Server 上安裝 .NET Core。 使用 `-Version` 參數傳遞 CLI 版本：

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a>發行應用程式

將現有應用程式的已發行輸出複製到檔案共用的根目錄。

您可能需要變更 *web.config* 以指向 *dotnet.exe* 的解壓縮位置。 或者，您可以將 *dotnet.exe* 新增至 PATH。

*dotnet.exe* **不**在 PATH 上時，*web.config* 看起來如何的範例：

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

在遠端工作階段中執行下列命令，針對不同於預設網站的連接埠上所發行的應用程式，在 IIS 中建立新的網站。 您還需要開啟該連接埠，才能存取 Web。 為了簡單起見，此指令碼使用 `DefaultAppPool`。 如需在應用程式集區下執行的其他考量，請參閱[應用程式集區](xref:publishing/iis#application-pools)。

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a>在 Nano Server 上執行 .NET Core CLI 的已知問題和因應措施
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a>執行應用程式

透過瀏覽器前往 `http://192.168.1.10:8000`，即可存取已發行的 Web 應用程式。 如果您已遵循[記錄檔的建立和重新導向](xref:hosting/aspnet-core-module#log-creation-and-redirection)所述設定記錄，則可以檢視記錄檔，這些檔案位於 *C:\PublishedApps\AspNetCoreSampleForNano\logs*。
