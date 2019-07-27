---
title: Использование ресурсов Android
ms.prod: xamarin
ms.assetid: 70ECDDC9-FA40-03B4-BF04-E7CFFFE4260D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: f8a542b58fa891b63f43d1c87dea911b83e01949
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509321"
---
# <a name="using-android-assets"></a>Использование ресурсов Android

_Ресурсы_ предоставляют возможность включать в приложение произвольные файлы, такие как текст, XML, шрифты, музыка и видео. Если вы попытаетесь включить эти файлы как "Resources", Android обработает их в своей системе ресурсов и вы не сможете получить необработанные данные. Если вы хотите получить доступ к данным без вмешательства пользователя, ресурсы являются одним из способов сделать это.

Ресурсы, добавленные в проект, будут отображаться так же, как файловая система, способная выполнять чтение из приложения с помощью [ассетманажер](xref:Android.Content.Res.AssetManager).
В этой простой демонстрации мы добавим к нашему проекту ресурс текстового файла, прочесть его с помощью `AssetManager`и отобразить в TextView.


## <a name="add-asset-to-project"></a>Добавить ресурс в проект

Ресурсы находятся в `Assets` папке проекта. Добавьте в эту папку новый текстовый файл с именем `read_asset.txt`. Поместите в него некоторый текст, например "я пришел от ресурса!".

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio должно задать для этого файла **действие сборки** **AndroidAsset**:

![Установка для действия сборки значения AndroidAsset](android-assets-images/asset-properties-vs.png) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Visual Studio для Mac должен задать для этого файла **действие сборки** **AndroidAsset**:

[![Установка для действия сборки значения AndroidAsset](android-assets-images/asset-properties-xs-sml.png)](android-assets-images/asset-properties-xs.png#lightbox)

-----

Если выбрать правильное **действие** , это гарантирует, что файл будет УПАКОВАН в APK во время компиляции.


## <a name="reading-assets"></a>Чтение ресурсов

Ресурсы считываются с помощью [ассетманажер](xref:Android.Content.Res.AssetManager). Экземпляр `AssetManager` объекта доступен при доступе к свойству [Assets](xref:Android.Content.Context.Assets) в `Android.Content.Context`, такому как действие.
В следующем коде мы откроем ресурс **read_asset. txt** , прочтите его содержимое и отобразите с помощью TextView.

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Create a new TextView and set it as our view
    TextView tv = new TextView (this);
    
    // Read the contents of our asset
    string content;
    AssetManager assets = this.Assets;
    using (StreamReader sr = new StreamReader (assets.Open ("read_asset.txt")))
    {
        content = sr.ReadToEnd ();
    }

    // Set TextView.Text to our asset content
    tv.Text = content;
    SetContentView (tv);
}
```


## <a name="running-the-application"></a>Запуск приложения

Запустите приложение, и вы должны увидеть следующее:

![Пример снимка экрана](android-assets-images/screenshot.png)


## <a name="related-links"></a>Связанные ссылки

- [ассетманажер](xref:Android.Content.Res.AssetManager)
- [Локального](xref:Android.Content.Context)
