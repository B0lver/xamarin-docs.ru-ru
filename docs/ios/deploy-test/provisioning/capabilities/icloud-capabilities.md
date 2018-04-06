---
title: Возможности iCloud
description: Добавление возможностей в приложения часто требует дополнительной подготовки. Это руководство описывает процесс настройки, необходимый для добавления возможностей iCloud.
ms.prod: xamarin
ms.assetid: 3CBAC982-D8DE-48DD-97CD-32B551D9DB85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 7e18bff7f32303cb0c5016e4ade4b39723921534
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="icloud-capabilities"></a>Возможности iCloud

_Добавление возможностей в приложения часто требует дополнительной подготовки. Это руководство описывает процесс настройки, необходимый для добавления возможностей iCloud._

iCloud позволяет пользователям iOS легко и удобно хранить свои данные и обмениваться ими между устройствами. Существуют четыре способа, которыми разработчики могут использовать iCloud для предоставления пользователям средства хранения данных: хранилище ключ-значение, хранилище UIDocument, CoreData и прямое предоставление хранилища для отдельных файлов и каталогов при помощи CloudKit. Дополнительную информацию см. в руководстве [Введение в iCloud](~/ios/data-cloud/introduction-to-icloud.md).

Добавление возможности iCloud в приложение немного сложнее по сравнению с другими службами приложений из-за _контейнеров_. Контейнеры применяются в iCloud для хранения данных приложения и позволяют разделять данные, содержащиеся в одной учетной записи iCloud, как при использовании песочницы на устройстве iOS пользователя. Дополнительные сведения о контейнерах см. в руководстве [Введение в CloudKit](~/ios/data-cloud/intro-to-cloudkit.md).

<!--# Xcode

The steps in section [Adding App Services ](~/ios/deploy-test/provisioning/capabilities/index.md) above can be used to toggle the required iCloud capability. Depending on which means of storage is being used in your app, you may need to do additional setup. The image below shows the iCloud capabilities pane:

 ![iCloud Capability section](icloud-capabilities-images/image21.png)

First select the required service. If iCloud Documents or CloudKit have been selected, you must select a container. Selecting the default container will create a new container that is unique to the app. Alternatively, if you have created a container either via Xcode or the developer portal, select it here.
-->

<a name="icloud-developer-center" />

## <a name="developer-center"></a>Центр разработчика

При подготовке нового приложения в центре разработчика необходимо выполнить две операции:

1.  Создать контейнер.
2.  Создать ИД приложения с возможностью iCloud и добавить в него контейнер.
3. Создать профиль подготовки, содержащий этот идентификатор приложения.

Ниже приводятся инструкции по выполнению этих операций:

1.  Откройте [Центр разработчика Apple](https://developer.apple.com/account/) и перейдите к разделу "Сертификаты, идентификаторы и профили": 
    
     ![Главная страница Центра разработчиков Apple](icloud-capabilities-images/image22.png)

2.  В разделе **Identifiers** (Идентификаторы) выберите элемент **iCloud Containers** (Контейнеры iCloud), а затем выберите **+**, чтобы создать контейнер:  
    
    ![Экран контейнера iCloud](icloud-capabilities-images/image23.png)

3.  Введите **описание** и уникальный **идентификатор** для контейнера iCloud: 
    
    ![Экран регистрации контейнера iCloud](icloud-capabilities-images/image24.png)

4.  Нажмите кнопку **Continue** (Продолжить), проверьте правильность сведений и нажмите кнопку **Register** (Зарегистрировать), чтобы создать контейнер iCloud:  
    
    ![Экран регистрации контейнера iCloud](icloud-capabilities-images/image25.png)

Чтобы создать идентификатор приложения и добавить в него контейнер, выполните указанные ниже действия:

1.  В [Центре разработчика](https://developer.apple.com/account/) выберите элемент **App IDs** (ИД приложений) в разделе **Identifiers** (Идентификаторы): 
    
    ![Раздел Identifiers в Центре разработчика](icloud-capabilities-images/image26.png)

2.  Нажмите кнопку **+** для добавления ИД приложения: 
    
    ![Кнопка "Добавить ИД приложения"](icloud-capabilities-images/image27.png)

3.  Введите **имя** для ИД приложения и **понятный идентификатор приложения**:
    
    ![Ввод сведений об идентификаторе приложения](icloud-capabilities-images/image28.png)

4.  В разделе **App Services** (Службы приложений) выберите **iCloud**, а затем выберите параметр **Include CloudKit support** (Включить поддержку CloudKit):
    
    ![Выбор службы приложений iCloud](icloud-capabilities-images/image29.png)

5.  Нажмите кнопку **Continue** (Продолжить), а затем кнопку **Register** (Зарегистрировать). Обратите внимание, что в окне подтверждения параметр iCloud будет отмечен как настраиваемый с помощью желтого символа:   
    
    ![Окно подтверждения](icloud-capabilities-images/image30.png)

6.  Вернитесь к списку ИД приложений и выберите только что созданный идентификатор: 
    
    ![Экран выбора идентификатора приложения](icloud-capabilities-images/image31.png)

7.  Прокрутите вниз до конца этого развернутого раздела и нажмите **Edit** (Изменить).
    
    ![Изменение ИД приложения](icloud-capabilities-images/image32.png)

8.  Прокрутите список вниз до элемента iCloud и нажмите кнопку **Edit** (Изменить):  
    
    ![Изменение идентификатора приложения iCloud](icloud-capabilities-images/image33.png)

9.  Выберите контейнер, который следует использовать с этим идентификатором приложения:  
    
    ![Экран выбора контейнера](icloud-capabilities-images/image34.png)

10. Проверьте правильность выбора контейнера и нажмите кнопку **Assign** (Назначить).
 
Этот ИД приложения теперь можно использовать для создания или повторного создания профиля подготовки, как описано в руководстве [Работа с возможностями](~/ios/deploy-test/provisioning/capabilities/index.md). 

Дополнительные сведения об использовании iCloud см. в следующих руководствах:

*   [Введение в iCloud](~/ios/data-cloud/introduction-to-icloud.md)
*   [Введение в CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
*   [Введение в Document Picker](~/ios/platform/document-picker.md)

## <a name="next-steps"></a>Следующие шаги
 
Ниже перечислены дополнительные действия, которые необходимо выполнить:

* Используйте в приложении пространство имен платформы.
* Добавьте необходимые назначения к вашему приложению. Подробные сведения о необходимых назначениях и об их добавлении см. в руководстве [Работа с назначениями](~/ios/deploy-test/provisioning/entitlements.md).
* Убедитесь, что в **Подписывании пакета iOS** приложения параметр **Настраиваемые назначения** установлен в **Entitlements.plist**. Эта настройка _не устанавливается_ по умолчанию для сборок отладки и симулятора iOS.

Если вы столкнулись с проблемами при работе со службами приложений, обратитесь к разделу [Устранение неполадок](~/ios/deploy-test/provisioning/capabilities/index.md) основного руководства.