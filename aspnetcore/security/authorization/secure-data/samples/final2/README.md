---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210102"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="72ca8-101">如何建置/執行安全的使用者資料範例</span><span class="sxs-lookup"><span data-stu-id="72ca8-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="72ca8-102">使用 Secret Manager 工具設定密碼：</span><span class="sxs-lookup"><span data-stu-id="72ca8-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="72ca8-103">更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="72ca8-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="72ca8-104">在專案中啟用 HTTPS</span><span class="sxs-lookup"><span data-stu-id="72ca8-104">Enable HTTPS in the project</span></span>
