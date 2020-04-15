---
title: 'Xamarin.Essentials: Подключение'
description: Класс Connectivity в Xamarin.Essentials позволяет отслеживать изменения в условиях подключения устройства к сети, проверять текущее состояние и используемый метод доступа.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 01/08/2019
ms.custom: video
ms.openlocfilehash: c70510f7b47f93c6119532b6a1c06f6c2e9e56ea
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "67855758"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: Подключение

Класс **Connectivity** позволяет отслеживать изменения в условиях подключения устройства к сети, проверять текущее состояние и используемый метод доступа.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

Чтобы проверить функциональность класса **Connectivity**, нужно создать описанную ниже конфигурацию для конкретной платформы.

# <a name="android"></a>[Android](#tab/android)

Требуется разрешение `AccessNetworkState`, которое следует настроить в проекте Android. Для этого можно применить любой из следующих методов:

Откройте файл **AssemblyInfo.cs** в папке **Свойства** и добавьте в него:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

ИЛИ обновите манифест Android:

Откройте файл **AndroidManifest.xml** в папке **Properties** и добавьте приведенный ниже код в узел **manifest**.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

ИЛИ щелкните правой кнопкой мыши проект Android и откройте свойства проекта. В разделе **Манифест Android** найдите область **Требуемые разрешения:** и установите флажок для разрешения **Access Network State** (Доступ к состоянию сети). Это действие автоматически обновляет файл **AndroidManifest.xml**.

# <a name="ios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-connectivity"></a>Использование подключения

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Проверьте текущий сетевой доступ:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Сетевой доступ](xref:Xamarin.Essentials.NetworkAccess) разделяется на следующие категории:

* **Internet** — локальная сеть и доступ к Интернет;у
* **ConstrainedInternet** — ограниченный доступ к Интернету. Обозначает режим условного доступа через портал, при котором предоставляется локальный доступ к веб-порталу, а доступ к Интернету осуществляется через этот портал со специальными учетными данными;
* **Local** — только доступ к локальной сети;
* **None** — подключение недоступно;
* **Unknown** — не удается определить режим подключения.

Вы можете проверить, какой тип [профиля подключения](xref:Xamarin.Essentials.ConnectionProfile) устройство активно использует:

```csharp
var profiles = Connectivity.ConnectionProfiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Вы можете получать события с информацией о любых изменениях профиля подключения и (или) режима сетевого доступа:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(object sender, ConnectivityChangedEventArgs e)
    {
        var access = e.NetworkAccess;
        var profiles = e.ConnectionProfiles;
    }
}
```

## <a name="limitations"></a>Ограничения

Обратите внимание, что при состоянии `Internet`, полученном от `NetworkAccess`, доступ к Интернету может быть не полным. Так как на разных платформах используются различные методы подключения, можно гарантировать только факт наличия подключения. Например, устройство может быть подключено к сети Wi-Fi, маршрутизатор которой не имеет подключения к Интернету. В этом случае будет зарегистрировано подключение к Интернету, но не будет возможности его активно использовать.

## <a name="api"></a>API

* [Исходный код Connectivity](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Документация по API-интерфейсу Connectivity](xref:Xamarin.Essentials.Connectivity)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Connectivity-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
