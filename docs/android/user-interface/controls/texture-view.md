---
title: TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: 1cd332894deac1e1fb076f647bb0120bda2306da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="textureview"></a>TextureView

`TextureView` Класс является представление, которое использует аппаратное ускорение двухмерной отрисовки для видео или OpenGL поток содержимого для отображения. Например, на следующем рисунке показан `TextureView` отображения динамического канала с камеры в устройстве:

[![Снимок экрана примера динамической изображения с камеры в устройстве](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

В отличие от `SurfaceView` TextureView класс, который может также использоваться для отображения OpenGL или видео, не отображается в отдельном окне.
Таким образом `TextureView` способен поддерживать преобразований представления, как любое другое представление. Например, смена `TextureView` можно выполнить, просто задав его `Rotation` свойством, его прозрачность, установив его `Alpha` свойства и т. д.

Поэтому при использовании `TextureView` теперь можно выполнять следующие действия отображения обновляющегося потока с камеры и преобразовать его, как показано в следующем коде:

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;
       
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;
           
        SetContentView (_textureView);
    }
       
    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }
           
        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

Приведенный выше код создает `TextureView` экземпляра в действии `OnCreate` метод и задает действие как `TextureView` `SurfaceTextureListener`. Чтобы быть `SurfaceTextureListener`, реализует действие `TextureView.ISurfaceTextureListener` интерфейса. Система будет вызывать `OnSurfaceTextAvailable` метод при `SurfaceTexture` готов к использованию. В этом методе, мы используем `SurfaceTexture` , передается в и задайте для него значение камеры Предварительная версия текстуры. Затем мы для выполнения обычных операций на основе представления, такие как параметр `Rotation` и `Alpha`, как показано в приведенном выше примере. Ниже приводится результирующий приложение, работающее на устройстве.

[![Пример приложения, запущенного на устройстве, отображения изображения](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

Чтобы использовать `TextureView`, аппаратное ускорение должна быть включена, которой он будет по умолчанию на уровне API с 14. Также, поскольку в этом примере используется камеры, как `android.permission.CAMERA` разрешений и `android.hardware.camera` функция должна быть задана в **AndroidManifest.xml**.



## <a name="related-links"></a>Связанные ссылки

- [TextureViewDemo (пример)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Знакомство с приложением Сандвичевы мороженого](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 платформы](http://developer.android.com/sdk/android-4.0.html)
