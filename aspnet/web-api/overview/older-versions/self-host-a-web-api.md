---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "自我裝載 ASP.NET Web API 1 (C#) |Microsoft 文件"
author: MikeWasson
description: "ASP.NET Web API 並不需要 IIS。 自我，您就可以在您自己的主控件程序裝載的 web API。 本教學課程會示範如何裝載 applic 主控台內的 web API..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a>自我裝載 ASP.NET Web API 1 (C#)
====================
由[Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API 並不需要 IIS。 自我，您就可以在您自己的主控件程序裝載的 web API。 本教學課程會示範如何裝載於主控台應用程式的 web API。
> 
> **新的應用程式應該使用 OWIN 自我裝載的 Web API。** 請參閱[使用 OWIN 自我裝載 ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>建立主控台應用程式專案

啟動 Visual Studio，然後選取**新專案**從**啟動**頁面。 或從**檔案**功能表上，選取**新增**然後**專案**。

在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。 在下**Visual C#**，選取**Windows**。 在專案範本清單中選取**主控台應用程式**。 將專案命名&quot;SelfHost&quot;按一下**確定**。

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>設定目標架構 (Visual Studio 2010)

如果您使用 Visual Studio 2010，將目標 framework 變更為.NET Framework 4.0。 (根據預設，專案範本的目標[.Net Framework 用戶端設定檔](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)。)

在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。 在**目標 framework**下拉式清單中，將目標 framework 變更為.NET Framework 4.0。 當出現提示，以套用變更，請按一下 **是**。

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>安裝 NuGet 封裝管理員

NuGet 封裝管理員是 Web 應用程式開發介面的組件加入至非 ASP.NET 專案最簡單的方式。

若要檢查是否已安裝的 NuGet 封裝管理員，請按一下**工具**Visual Studio 中的功能表。 如果您看到的功能表項目稱為**程式庫套件管理員**，就會有 NuGet 套件管理員。

若要安裝 NuGet 封裝管理員：

1. 啟動 Visual Studio。
2. 從**工具**功能表上，選取**擴充功能和更新**。
3. 在**擴充功能和更新**對話方塊中，選取**線上**。
4. 如果看不到 」 NuGet 封裝管理員 」，請在搜尋方塊中輸入"nuget 封裝管理員 」。
5. 選取 NuGet 封裝管理員，然後按一下**下載**。
6. 下載完成後，系統會提示您安裝。
7. 在安裝完成之後，您可能會提示您重新啟動 Visual Studio。

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>加入 Web API NuGet 套件

安裝 NuGet 套件管理員之後，將 Web 應用程式開發介面 Self-Host 封裝加入您的專案。

1. 從**工具**功能表上，選取**程式庫套件管理員**。 *請注意*： 如果您未看見這個功能表項目，請確定已正確安裝的 NuGet 套件管理員。
2. 選取**管理方案的 NuGet 封裝...**
3. 在**管理 NugGet 封裝**對話方塊中，選取**線上**。
4. 在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.SelfHost&quot;。
5. 選取 ASP.NET Web API Self Host 封裝，然後按一下**安裝**。
6. 此封裝會安裝之後，請按一下**關閉**以關閉對話方塊。

> [!NOTE]
> 請務必安裝名為 Microsoft.AspNet.WebApi.SelfHost，不 AspNetWebApi.SelfHost 的套件。


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>建立模型和控制器

本教學課程使用相同的模型和控制器類別做為[入門](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。

加入名為公用類別`Product`。

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

加入名為公用類別`ProductsController`。 衍生此類別從**System.Web.Http.ApiController**。

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

如需此控制器中的程式碼的詳細資訊，請參閱[入門](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。 此控制器會定義三個 GET 動作：

| URI | 描述 |
| --- | --- |
| / api/產品 | 取得所有產品的清單。 |
| /api/products/*id* | 取得產品的識別碼。 |
| /api/products/?category=*category* | 取得類別目錄的產品清單。 |

## <a name="host-the-web-api"></a>裝載 Web 應用程式開發介面

開啟 Program.cs 檔案並加入下列 using 陳述式：

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

將下列程式碼加入**程式**類別。

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>（選擇性）新增 HTTP URL 命名空間保留區

此應用程式接聽`http://localhost:8080/`。 根據預設，在特定的 HTTP 位址的接聽要求系統管理員權限。 當您執行本教學課程時，因此，您可能會收到這個錯誤: 「 HTTP 無法登錄 URL http://+:8080/"有兩種方式以避免此錯誤：

- 以提升的系統管理員權限，執行 Visual Studio 或
- 使用 Netsh.exe 給予您的帳戶權限，以保留 URL。

若要使用 Netsh.exe，以系統管理員權限開啟命令提示字元並輸入下列命令： 下列命令：

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

其中*machine\username*是您的使用者帳戶。

當您完成的自我裝載的時請務必刪除保留區：

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>呼叫 Web API，從用戶端應用程式 (C#)

我們要撰寫簡單的主控台應用程式呼叫 web API。

將新的主控台應用程式專案加入方案：

- 在 [方案總管] 中，以滑鼠右鍵按一下方案，然後選取**加入新的專案**。
- 建立新的主控台應用程式，名為&quot;ClientApp&quot;。

![](self-host-a-web-api/_static/image5.png)

使用 NuGet 套件管理員來新增 ASP.NET Web API Core Libraries 封裝：

- 從 [工具] 功能表中，選取**程式庫套件管理員**。
- 選取**管理方案的 NuGet 封裝...**
- 在**管理 NuGet 封裝**對話方塊中，選取**線上**。
- 在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.Client&quot;。
- 選取 Microsoft ASP.NET Web API 用戶端程式庫封裝，然後按一下**安裝**。

ClientApp 中加入 SelfHost 專案的參考：

- 在 [方案總管] 中，以滑鼠右鍵按一下 ClientApp 專案。
- 選取**將參考加入**。
- 在**參考管理員**對話方塊下方**方案**，選取**專案**。
- 選取 SelfHost 專案。
- 按一下 [確定 **Deploying Office Solutions**]。

![](self-host-a-web-api/_static/image6.png)

開啟 Client/Program.cs 檔案。 加入下列**使用**陳述式：

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

新增靜態**HttpClient**執行個體：

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

加入下列方法，以依類別列出所有產品清單中依識別碼、 產品和產品清單。

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

每一種方法遵循相同的模式：

1. 呼叫**HttpClient.GetAsync**將 GET 要求傳送到適當的 URI。
2. 呼叫**HttpResponseMessage.EnsureSuccessStatusCode**。 如果 HTTP 回應狀態錯誤程式碼，則這個方法會擲回例外狀況。
3. 呼叫**ReadAsAsync&lt;T&gt;** 還原序列化之 HTTP 回應中的 CLR 型別。 這個方法是擴充方法，定義於**System.Net.Http.HttpContentExtensions**。

**GetAsync**和**ReadAsAsync**方法都是非同步。 它們會傳回**工作**代表非同步作業的物件。 取得**結果**屬性會封鎖執行緒，直到作業完成為止。

如需有關使用 HttpClient，包括如何讓非封鎖的呼叫，請參閱[呼叫 Web API 的.NET 用戶端](../advanced/calling-a-web-api-from-a-net-client.md)。

然後再呼叫這些方法，設定 BaseAddress 屬性 HttpClient 執行個體上 「`http://localhost:8080`"。 例如: 

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

這應該輸出。 （請記住第一次執行 SelfHost 應用程式）。

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
