---
title: 支援的 .NET 實作 - EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: 8fc25f4a35794162c92fd292990c24e977d1bf1b
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022258"
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core 支援的 .NET 實作

我們想要 EF Core 在您可撰寫 .NET 程式碼的任何位置都可以使用，因此仍然朝該目標努力中。 當 EF Core 對 .NET Core 及 .NET Framework 的支援皆已涵蓋在自動化測試內，且已知有許多應用程式能夠正常加以使用時，Mono、Xamarin 及 UWP 仍存在問題。

## <a name="overview"></a>總覽

下表提供每個 .NET 實作的指導：

| .NET 實作                                                                                                  | 狀態                                                             | EF Core 1.x 需求                                                                                | EF Core 2.x 需求 <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md)、[主控台](../get-started/netcore/index.md) 等等) | 完整支援與建議。                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework** (WinForms、WPF、ASP.NET、[主控台](../get-started/full-dotnet/index.md) 等等)                    | 完整支援與建議。 也可以使用 EF6 <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono 和 Xamarin**                                                                                                   | 進行中 <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**通用 Windows 平台**](../get-started/uwp/index.md)                                                        | 建議 EF Core 2.0.1 <sup>(4)</sup>                           | [.NET Core UWP 5.x 套件](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET Core UWP 6.x 套件](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup> EF Core 2.0 將其設為目標，因此需要支援 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) 的 .NET 實作。

<sup>(2)</sup> 請參閱[比對 EF Core 與 EF6](../../efcore-and-ef6/index.md) 來選擇正確的技術。

<sup>(3)</sup> Xamarin 有一些問題與已知限制，可能會使某些使用 EF Core 2.0 開發的應用程式無法正常運作。 請查看[待處理的問題](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin)清單，以了解因應措施。

<sup>(4)</sup> 請參閱此文章的[通用 Windows 平台](#universal-windows-platform)一節。

## <a name="universal-windows-platform"></a>通用 Windows 平台

舊版 EF Core 與 .NET UWP 有許多相容性問題，其中以 .NET Native 工具鏈編譯的應用程式問題最多。 新版 UWP 新增了 .NET Standard 2.0 的支援且包含 .NET Native 2.0，這能夠修正上述提到的大多數相容性問題。 EF Core 2.0.1 已採用 UWP 進行更全面的測試，但測試並非自動進行。

在 UWP 上使用 EF Core 時：

* 若要最佳化查詢效能，請避免在 LINQ 查詢中使用匿名型別。 若要將 UWP 應用程式部署至應用程式市集，應用程式需要使用 .NET Native 來編譯。 使用匿名型別的查詢在 .NET Native 上會有較差的效能。

* 若要最佳化 `SaveChanges()` 效能，請使用 [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy) 並在您的實體類型中實作 [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx)、[INotifyPropertyChanging](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanging.aspx) 與 [INotifyCollectionChanged](https://msdn.microsoft.com/library/system.collections.specialized.inotifycollectionchanged.aspx)。

## <a name="report-issues"></a>回報問題

如有任何組合無法如預期運作，我們鼓勵您在 [EF Core 問題追蹤器](https://github.com/aspnet/entityframeworkcore/issues/new)建立新的問題。 若為 Xamarin 相關的問題，請使用 [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) 或 [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new)的問題追蹤器。
