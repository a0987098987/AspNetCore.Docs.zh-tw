---
title: React-with-Redux 專案範本搭配 ASP.NET Core 使用
author: SteveSandersonMS
description: 了解如何開始使用 React with Redux 和 create-react-app 適用的 ASP.NET Core 單頁應用程式 (SPA) 專案範本。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/13/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: spa/react-with-redux
ms.openlocfilehash: d44e1afabd909b7d46d3355a5d66848458416873
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85401736"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>React-with-Redux 專案範本搭配 ASP.NET Core 使用

更新的 React-with-Redux 專案範本很適合用來設計 ASP.NET Core 應用程式，而且可利用 React、Redux 和 [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) 慣例來實作一個功能多樣的用戶端使用者介面 (UI)。

除了專案建立命令，所有與 React-with-Redux 範本有關的資訊，都與 React 範本相同。 若要建立這種專案類型，請執行 `dotnet new reactredux` 而不是 `dotnet new react`。 對於這兩個以 React 為基礎的範本，如需深入了解其通用功能，請參閱 [React 範本文件](xref:spa/react)。

如需在 IIS 中設定 Redux 子應用程式的相關資訊，請參閱[ReactRedux Template 2.1：無法在 iis 上使用 SPA （aspnet/樣板化 &num; 555）](https://github.com/aspnet/Templating/issues/555)。
