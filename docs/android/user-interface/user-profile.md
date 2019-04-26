---
title: Профиль пользователя
ms.prod: xamarin
ms.assetid: 6BB01F75-5E98-49A1-BBA0-C2680905C59D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/22/2018
ms.openlocfilehash: 0613411e5436a0ea8ed08bf4af52dae84a9a701c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61307964"
---
# <a name="user-profile"></a>Профиль пользователя

Android поддерживается перечисление контакты с [ContactsContract](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/) поставщика, так как API уровня 5. Например, список контактов сложнее, чем с помощью [ContactContracts.Contacts](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Contacts/) класса, как показано в следующем примере кода:

```csharp
// Get the URI for the user's contacts:
var uri = ContactsContract.Contacts.ContentUri;

// Setup the "projection" (columns we want) for only the ID and display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id, 
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the user's contacts data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();

// Print the contact data to the console if reading back succeeds:
if (cursor != null)
{
    if (cursor.MoveToFirst())
    {
        do
        {
            Console.WriteLine("Contact ID: {0}, Contact Name: {1}",
                               cursor.GetString(cursor.GetColumnIndex(projection[0])),
                               cursor.GetString(cursor.GetColumnIndex(projection[1])));
        } while (cursor.MoveToNext());
    }
}
```

Начиная с Android 4 (API уровня 14), [ContactsContact.Profile](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract+Profile/) класс доступен через `ContactsContract` поставщика. `ContactsContact.Profile` Предоставляет доступ к личного профиля для владельца устройства, включая контактные данные, такие как владелец устройства имя и номер телефона.


## <a name="required-permissions"></a>Необходимые разрешения

Для чтения и записи контактные данные, приложения должны запрашивать `READ_CONTACTS` и `WRITE_CONTACTS` разрешения, соответственно.
Кроме того, чтобы прочитать и изменить профиль пользователя, приложения должны запрашивать `READ_PROFILE` и `WRITE_PROFILE` разрешения.


## <a name="updating-profile-data"></a>Обновление данных профиля

После задания этих разрешений приложение может использовать обычный Android методы для взаимодействия с данными профиля пользователя. Например, чтобы обновить отображаемое имя профиля, вызовите [ContentResolver.Update](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.Update) с `Uri` извлечь с помощью [ContactsContract.Profile.ContentRawContactsUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentRawContactsUri/) свойства, как показано ниже:

```csharp
var values = new ContentValues ();
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName, "John Doe");

// Update the user profile with the name "John Doe":
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri, values, null, null);
```

## <a name="reading-profile-data"></a>Чтение данных профиля

Выполнив запрос к [ContactsContact.Profile.ContentUri](https://developer.xamarin.com/api/property/Android.Provider.ContactsContract+Profile.ContentUri/) операций чтения обратно данные профиля. Например следующий код будет считывать отображаемое имя профиля пользователя:

```csharp
// Read the profile
var uri = ContactsContract.Profile.ContentUri;

// Setup the "projection" (column we want) for only the display name:
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };

// Use a CursorLoader to retrieve the data:
CursorLoader loader = new CursorLoader(this, uri, projection, null, null, null);
ICursor cursor = (ICursor)loader.LoadInBackground();
if (cursor != null)
{
    if (cursor.MoveToFirst ())
    {
        Console.WriteLine(cursor.GetString (cursor.GetColumnIndex (projection [0])));
    }
}
```

## <a name="navigating-to-the-user-profile"></a>Перейдите к профилю пользователя

Наконец, чтобы перейти к профилю пользователя, создайте объект Intent с `ActionView` действия и `ContactsContract.Profile.ContentUri` затем передать его в `StartActivity` метод следующим образом:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

При запуске приведенный выше код, профиль пользователя отображается в том случае, как показано на следующем снимке экрана:

[![Снимок экрана: Отображение профиля пользователя John Doe профиля](user-profile-images/01-profile-screen-sml.png)](user-profile-images/01-profile-screen.png#lightbox)

Работа с профилем пользователя аналогична взаимодействия с другими данными в Android, и он обеспечивает дополнительный уровень персонализации устройства.



## <a name="related-links"></a>Связанные ссылки

- [ContactsProviderDemo (пример)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Знакомство с Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4.0](https://developer.android.com/sdk/android-4.0.html)
