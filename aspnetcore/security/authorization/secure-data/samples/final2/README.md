---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210102"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>如何建置/執行安全的使用者資料範例

* 使用 Secret Manager 工具設定密碼：

  `dotnet user-secrets set SeedUserPW <pw>`

* 更新資料庫：

  `dotnet ef database update`

* 在專案中啟用 HTTPS
