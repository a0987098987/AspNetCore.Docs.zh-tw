---
uid: web-api/overview/older-versions/self-host-a-web-api
title: 自我裝載 ASP.NET Web API 1 (C#) |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API 不需要 IIS。 您可以在您自己的主控件程序中，自我裝載的 web API。 本教學課程會示範如何裝載 web API 應用程式的主控台內...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 63d192a6fa2aafef3770d5b0b97ec32e001b69db
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912694"
---
<a name="self-host-aspnet-web-api-1-c"></a>自我裝載 ASP.NET Web API 1 (C#)
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API 不需要 IIS。 您可以在您自己的主控件程序中，自我裝載的 web API。 本教學課程會示範如何裝載於主控台應用程式的 web API。
> 
> **新的應用程式應該使用 OWIN 自我裝載 Web API。** 請參閱[使用 OWIN 自我裝載 ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>建立主控台應用程式專案

啟動 Visual Studio，然後選取**新的專案**從**開始**頁面。 或從**檔案**功能表上，選取**新增**，然後**專案**。

在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。 底下**Visual C#**，選取**Windows**。 在專案範本清單中，選取**主控台應用程式**。 將專案命名為&quot;SelfHost&quot;然後按一下**確定**。

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>設定目標架構 (Visual Studio 2010)

如果您使用 Visual Studio 2010，將.NET Framework 4.0 目標 framework。 (根據預設，專案範本的目標[.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)。)

在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。 在 **目標 framework**下拉式清單中，將目標 framework 變更為.NET Framework 4.0。 當出現提示，以套用變更，請按一下**是**。

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>安裝 NuGet 套件管理員

NuGet 套件管理員是最簡單的方式，將 Web API 組件新增至非 ASP.NET 專案。

若要檢查是否已安裝 NuGet 套件管理員，請按一下**工具**Visual Studio 中的功能表。 如果您看到的功能表項目呼叫**NuGet 套件管理員**，則您可以 NuGet 套件管理員。

若要安裝 NuGet 套件管理員：

1. 啟動 Visual Studio。
2. 從**工具**功能表上，選取**擴充功能和更新**。
3. 在 **擴充功能和更新**對話方塊中，選取**線上**。
4. 如果您沒有看到 [NuGet 套件管理員]，請在搜尋方塊中輸入 「 nuget 封裝管理員 」。
5. 選取 NuGet 套件管理員，然後按一下**下載**。
6. 下載完成之後，系統會提示您安裝。
7. 安裝完成之後，您可能會提示重新啟動 Visual Studio。

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>新增 Web API NuGet 套件

NuGet 套件管理員安裝之後，將 Web API 的自我裝載封裝加入專案。

1. 從**工具**功能表上，選取**NuGet 套件管理員**。 *請注意*： 如果您不會看到此功能表項目，請確定已正確安裝該 NuGet 套件管理員。
2. 選取**管理方案的 NuGet 套件**
3. 在 **管理 Nuget 封裝**對話方塊中，選取**線上**。
4. 在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.SelfHost&quot;。
5. 選取 ASP.NET Web API 自助主應用程式封裝，然後按一下**安裝**。
6. 套件會安裝之後，請按一下**關閉**以關閉對話方塊。

> [!NOTE]
> 請務必安裝名為 Microsoft.AspNet.WebApi.SelfHost，不 AspNetWebApi.SelfHost 的套件。

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>建立模型和控制器

本教學課程使用相同的模型和控制器類別作為[開始使用](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。

新增名為公用類別`Product`。

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

新增名為公用類別`ProductsController`。 從這個類別的衍生**System.Web.Http.ApiController**。

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

如需有關此控制器中的程式碼的詳細資訊，請參閱[開始使用](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。 此控制器會定義三個 GET 動作：

| URI | 描述 |
| --- | --- |
| / api/產品 | 取得所有產品的清單。 |
| /api/產品/*識別碼* | 取得產品的識別碼。 |
| /api/products/?category=*category* | 依類別取得產品的清單。 |

## <a name="host-the-web-api"></a>裝載 Web API

開啟 Program.cs 檔案並新增下列 using 陳述式：

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

將下列程式碼加入**程式**類別。

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>（選擇性）新增 HTTP URL 命名空間保留區

此應用程式接聽`http://localhost:8080/`。 根據預設，在特定的 HTTP 位址上進行接聽要求系統管理員權限。 當您執行本教學課程時，因此，您可能會收到此錯誤: 「 HTTP 無法登錄 URL http://+:8080/」 有兩種方式可避免這個錯誤：

- 提高權限的系統管理員權限，以執行 Visual Studio 或
- 您可以使用 Netsh.exe，讓您的帳戶權限，來保留 URL。

若要使用 Netsh.exe，以系統管理員權限開啟命令提示字元並輸入下列命令： 下列命令：

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

何處*machine\username*是您的使用者帳戶。

當您完成自我裝載時，請務必刪除保留區：

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>呼叫 Web API，從用戶端應用程式 (C#)

讓我們撰寫會呼叫 web API 的簡單主控台應用程式。

將新的主控台應用程式專案加入方案：

- 在 [方案總管] 中，以滑鼠右鍵按一下方案，然後選取**加入新的專案**。
- 建立新的主控台應用程式，名為&quot;ClientApp&quot;。

![](self-host-a-web-api/_static/image5.png)

使用 NuGet 套件管理員來新增 ASP.NET Web API 核心程式庫套件：

- 從 [工具] 功能表中，選取**NuGet 套件管理員**。
- 選取**管理方案的 NuGet 套件**
- 在 **管理 NuGet 套件**對話方塊中，選取**線上**。
- 在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.Client&quot;。
- 選取 Microsoft ASP.NET Web API 用戶端程式庫套件，然後按一下**安裝**。

在 ClientApp 加入 SelfHost 專案的參考：

- 在 [方案總管] 中，以滑鼠右鍵按一下 ClientApp 專案。
- 選取 [新增參考]。
- 在 [**參考管理員**] 對話方塊底下**解決方案**，選取**專案**。
- 選取 SelfHost 專案。
- 按一下 [確定 **Deploying Office Solutions**]。

![](self-host-a-web-api/_static/image6.png)

開啟 Client/Program.cs 檔案。 新增下列**使用**陳述式：

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

新增靜態**HttpClient**執行個體：

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

新增下列方法來依類別列出所有產品清單的識別碼、 產品和產品清單。

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

每一種方法都遵循相同的模式：

1. 呼叫**HttpClient.GetAsync**將 GET 要求傳送至適當的 URI。
2. 呼叫**HttpResponseMessage.EnsureSuccessStatusCode**。 如果 HTTP 回應狀態錯誤碼，則這個方法會擲回例外狀況。
3. 呼叫**ReadAsAsync&lt;T&gt;** 還原序列化的 HTTP 回應中的 CLR 型別。 這個方法是擴充方法，定義於**System.Net.Http.HttpContentExtensions**。

**GetAsync**並**ReadAsAsync**方法為非同步。 它們會傳回**任務**代表非同步作業的物件。 取得**結果**屬性會封鎖執行緒直到作業完成為止。

如需有關使用 HttpClient，包括如何建立非封鎖式呼叫，請參閱 <<c0> [ 呼叫 Web API 從.NET 用戶端](../advanced/calling-a-web-api-from-a-net-client.md)。

之前呼叫這些方法，設定 HttpClient 執行個體上的 BaseAddress 屬性 「`http://localhost:8080`"。 例如: 

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

這應輸出下列項目。 （請記得先執行 SelfHost 應用程式。）

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
