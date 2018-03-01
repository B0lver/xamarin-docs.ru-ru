---
title: "Отсутствующие пакеты ошибки после обновления пакетов Nuget"
ms.topic: article
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f330590132e4881484eeae3efafe570d991a509a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Отсутствующие пакеты ошибки после обновления пакетов Nuget

Эта проблема обнаружена в основном в Xamarin.Forms примеров приложений решений, но вероятность эта проблема может произойти на любой проект, который использует пакеты NuGet. 

Если после обновления пакетов Nuget в проекте или решении, появится сообщение об ошибке, которое ссылается на старый номера версий пакета, такие как:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

В этом примере *Xamarin.Forms.1.3.1.6296* старый номер версии, которое было удалено в результате обновления пакета Nuget.

Это может произойти, если XML-элементы в CSPROJ-файл, которые ссылаются на старый номер версии пакета добавленные вручную или редактирования, Nuget не удалить или обновить их, если бы они были вручную добавлено или изменено, поэтому проекта теперь доступны для пакетов, которые были удалены. 

Чтобы устранить эту проблему, вручную редактировать файлы CSPROJ-файл и удалите все элементы, которые ссылаются на старый номер версии. 

Пример элементов (если они имеют старый номер версии пакета):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

