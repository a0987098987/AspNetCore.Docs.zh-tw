---
ms.openlocfilehash: 648ed8087c64eaf80fd055fa7dd08041ab9a8752
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320275"
---
執行身分識別框架：

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從**方案總管**，以滑鼠右鍵按一下專案 >**新增** > **新增 Scaffold 項目**。
* 從左窗格**新增 Scaffold**對話方塊中，選取**識別** > **新增**。
* 在 [ **ADD 身分識別**] 對話方塊中，選取您想要的選項。
  * 選取現有的版面配置頁面，或您的版面配置檔將會覆寫以不正確的標記。 比方說`~/Pages/Shared/_Layout.cshtml`Razor 頁面`~/Views/Shared/_Layout.cshtml`MVC 專案
  * 選取  **+** 來建立新的按鈕**資料內容類別**。
* 選取 **新增**。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

如果之前尚未安裝 ASP.NET Core scaffolder，請立即安裝：

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

新增的套件參考[Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/)至專案 (\*.csproj) 檔案。 在專案目錄中，執行下列命令：

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

執行下列命令來列出識別 scaffolder 選項：

```cli
dotnet aspnet-codegenerator identity -h
```

在 [專案] 資料夾中，執行身分識別 scaffolder 使用您想要的選項。 例如，若要設定的預設 UI 身分識別和檔案的最小數目，請執行下列命令：

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
