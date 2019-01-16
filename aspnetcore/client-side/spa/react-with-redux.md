---
title: React-with-Redux 專案範本搭配 ASP.NET Core 使用
author: SteveSandersonMS
description: 了解如何開始使用 React with Redux 和 create-react-app 適用的 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341604"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>React-with-Redux 專案範本搭配 ASP.NET Core 使用

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> 這份文件並不屬於 React 與 Redux 的專案範本包含在 ASP.NET Core 2.0。 它適用於較新的 React 與 Redux 範本，您可以手動更新。 範本是適用於 ASP.NET Core 2.1 或更新版本。

::: moniker-end

更新的 React-with-Redux 專案範本很適合用來設計 ASP.NET Core 應用程式，而且可利用 React、Redux 和 [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 慣例來實作一個功能多樣的用戶端使用者介面 (UI)。

除了專案建立命令，所有與 React-with-Redux 範本有關的資訊，都與 React 範本相同。 若要建立這種專案類型，請執行 `dotnet new reactredux` 而不是 `dotnet new react`。 對於這兩個以 React 為基礎的範本，如需深入了解其通用功能，請參閱 [React 範本文件](xref:spa/react)。

如需在 IIS 中設定 React 與 Redux 的子應用程式的資訊，請參閱[ReactRedux 範本 2.1:無法在 IIS 上使用 SPA (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555)。
