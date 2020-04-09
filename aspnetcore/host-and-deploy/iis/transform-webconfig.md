---
title: 轉換 web.config
author: rick-anderson
description: 了解如何在發佈 ASP.NET Core 應用程式時轉換 web.config 檔案。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: 069b9bb516644a1a722235b33d4916460488ebf2
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78657931"
---
# <a name="transform-webconfig"></a>轉換 web.config

由[維傑·拉馬克里什南](https://github.com/vijayrkn)

對 *web.config* 檔案的轉換可在發佈應用程式時，根據下列條件進行套用：

* [建置組態](#build-configuration)
* [設定檔](#profile)
* [環境](#environment)
* [自訂](#custom)

這些轉換會針對下列任一 *web.config* 產生案例進行：

* 由 `Microsoft.NET.Sdk.Web` SDK 自動產生。
* 由應用[內容根](xref:fundamentals/index#content-root)中的開發人員提供。

## <a name="build-configuration"></a>建置組態

組建組態會第一個執行。

請為每個需要進行 *web.config* 轉換的 [組建組態 (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) 包含一個 *web.{CONFIGURATION}.config* 檔案。

在下列範例中，會在 *web.Release.config* 中設定一個組態特定的環境變數：

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Configuration_Specific" 
                               value="Configuration_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

當組態設定為 *Release* 時，就會套用轉換：

```dotnetcli
dotnet publish --configuration Release
```

組態的 MSBuild 屬性為 `$(Configuration)`。

## <a name="profile"></a>設定檔

設定檔轉換會第二個執行，亦即在[組建組態](#build-configuration)轉換之後執行。

請為每個需要進行 *web.config* 轉換的設定檔組態包含一個 *web.{PROFILE}.config* 檔案。

在下列範例中，會在資料夾發佈設定檔的 *web.FolderProfile.config* 中設定一個設定檔特定的環境變數：

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Profile_Specific" 
                               value="Profile_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

當設定檔為 *FolderProfile* 時，就會套用轉換：

```dotnetcli
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

設定檔名稱的 MSBuild 屬性為 `$(PublishProfile)`。

如果未傳遞任何設定檔，則預設的設定檔名稱為 **FileSystem**，而如果檔案存在於應用程式的內容根目錄中，就會套用 *web.FileSystem.config*。

## <a name="environment"></a>環境

環境轉換會第三個執行，亦即在 [組建組態](#build-configuration)與[設定檔](#profile)轉換之後執行。

請為每個需要進行 *web.config* 轉換的[環境](xref:fundamentals/environments)包含一個 *web.{ENVIRONMENT}.config*檔案。

在下列範例中，會在生產環境的 *web.Production.config* 中設定一個環境特定的環境變數：

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Environment_Specific" 
                               value="Environment_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

當環境為 *Production* 時，就會套用轉換：

```dotnetcli
dotnet publish --configuration Release /p:EnvironmentName=Production
```

環境的 MSBuild 屬性為 `$(EnvironmentName)`。

從 Visual Studio 發佈並使用發行設定檔時，請參閱 <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>。

已指定環境名稱時，`ASPNETCORE_ENVIRONMENT` 環境變數會自動新增至 *web.config* 檔案。

## <a name="custom"></a>Custom

自訂轉換會第四個執行，亦即在 [組建組態](#build-configuration)[設定檔](#profile)及[環境](#environment)轉換之後執行。

請為每個需要進行 *web.config* 轉換的自訂組態包含一個 *{CUSTOM_NAME}.transform* 檔案。

在下列範例中，會在 *custom.transform* 中設定一個自訂轉換環境變數：

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Custom_Specific" 
                               value="Custom_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

將 `CustomTransformFileName` 屬性傳遞給 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令時，就會套用轉換：

```dotnetcli
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

設定檔名稱的 MSBuild 屬性為 `$(CustomTransformFileName)`。

## <a name="prevent-webconfig-transformation"></a>防止 web.config 轉換

若要防止轉換 *web.config* 檔案，請設定 `$(IsWebConfigTransformDisabled)` MSBuild 屬性：

```dotnetcli
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a>其他資源

* [Web 應用程式專案部署的 Web.config 轉換語法](/previous-versions/dd465326(v=vs.100))
* [使用 Visual Studio 之 Web 專案部署的 Web.config 轉換語法](/previous-versions/aspnet/dd465326(v=vs.110)) \(英文\)
