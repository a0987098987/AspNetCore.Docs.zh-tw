---
title: 使用 LibMan 讓 ASP.NET Core 取得用戶端程式庫
author: scottaddie
description: 了解如何使用程式庫 (LibMan) 在 ASP.NET Core 專案中安裝用戶端程式庫資產。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: 87987446b7f2c625da90951510e697e06569ba36
ms.sourcegitcommit: e7c56e8da5419bbc20b437c2dd531dedf9b0dc6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878325"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a>使用 LibMan 讓 ASP.NET Core 取得用戶端程式庫

作者：[Scott Addie](https://twitter.com/Scott_Addie)

程式庫管理員 (LibMan) 是輕量型的用戶端程式庫取得工具。 LibMan 會從檔案系統或[內容傳遞網路 (CDN)](https://wikipedia.org/wiki/Content_delivery_network)下載熱門的程式庫和架構。 支援的 Cdn 包括[CDNJS](https://cdnjs.com/)、 [jsDelivr](https://www.jsdelivr.com/)和[unpkg](https://unpkg.com/#/)。 所選程式庫檔會擷取並放置在 ASP.NET Core 專案中的適當位置。

## <a name="libman-use-cases"></a>LibMan 使用案例

LibMan 提供下列優點：

* 只會下載您所需的程式庫檔。
* 其他工具，例如 [Node.js](https://nodejs.org)、[npm](https://www.npmjs.com)和 [WebPack](https://webpack.js.org)，並不需取得程式庫中的部分檔案。
* 檔案可以放在特定位置，而不必依靠建置工作，或手動複製檔案。

如需 LibMan 優點的詳細資訊，請觀看 [Visual Studio 2017 中的新式前端網頁程式開發：LibMan 區段](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s) \(英文\)。

LibMan 不是套件管理系統。 如果您已使用套件管理員，如 npm 或 [yarn](https://yarnpkg.com)，請繼續使用。 LibMan 的開發目的並非在於取代這些工具。

## <a name="additional-resources"></a>其他資源

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [LibMan GitHub 存放庫](https://github.com/aspnet/LibraryManager)
