---
title: Сообщения об ошибках Xamarin.Mac (mmp)
description: Справочное руководство ошибок для mmp.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: 0a4bab44cfa260060c990949b45d3f5e6b103d0c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinmac-error-messages-mmp"></a>Сообщения об ошибках Xamarin.Mac (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: сообщения об ошибках mmp

Например, параметры среды, средств отсутствует.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM0000: Непредвиденная ошибка - файл ошибки сообщите в http://bugzilla.xamarin.com

Произошла непредвиденная ошибка. Проверьте [отчет об ошибке](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) как можно больше информации, включая:

* Полные журналы, построений с максимальная степень подробности (например `-v -v -v -v` в **mmp дополнительные аргументы**);
* Минимальный тестовый случай, воспроизведите ошибку. и
* Все версии сведения

Чтобы получить сведения о точной версии проще всего использовать **Xamarin Studio** меню, **о Xamarin Studio** элемента, **Показать подробности** кнопку и скопируйте его версия сведения (можно использовать **сведения о копировании** кнопка).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: В этой версии Xamarin.Mac требуется моно {0} (текущая версия моно: {1}). Обновите Mono.framework из http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: Имя «{0} .exe» конфликтов приложений с именем сборки (DLL) пакета SDK или продукта.

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: Корневой сборки «{0}» не существует

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: Необходимо указать один корневые сборки только, найдено {0} сборки: «{1}»

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: Не удалось выполнить синтаксический анализ аргументов командной строки: {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: Параметр «{0}» является устаревшим.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: Необходимо указать корневой сборки

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: Неизвестный параметр командной строки: «{0}»

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: Допустимые параметры для «{0}», «{1}».

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: Приложения имя «{0} .exe», конфликтует с другой пользователь сборки.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: Не удалось выполнить синтаксический анализ аргументов командной строки «{0}»: {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Сборщик мусора Боэм не поддерживается. Вместо этого было выбрано SGen сборщик мусора.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: Не может предоставить корневой сборки, если--передается нет корневой сборки.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: Выходной каталог (--вывод) является обязательным, если--передается нет корневой сборки.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Не удается отключить новых счетчика ссылок с помощью единого интерфейса API.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Не удается найти Xcode в любом из наших расположения по умолчанию. Установите Xcode или передать пользовательский путь, используя--sdkroot =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: Не удалось найти выбранный Xcode в системе: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: Не удалось найти выбранный Xcode в системе. «xcode выберите--путь печати» вернул «{0}», но этот каталог не существует.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Недопустимое значение для требуемой версии .NET framework: {0}.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Неизвестная платформа: *. Это обычно указывает на ошибку в Xamarin.Mac; Отправьте отчет об ошибках в https://bugzilla.xamarin.com с тестовым случаем.

Это обычно указывает на ошибку в Xamarin.Mac; Отправьте отчет об ошибках в [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) с тестовым случаем.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: Внутренняя ошибка - не исполняемый файл был скопирован в комплект приложения. Обратитесь в службу "support@xamarin.com"

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: Отключение NewRefCount,--new-refcount:false, является устаревшим.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Эта версия Xamarin.Mac требует * пакет SDK (поставляется с Xcode *). Либо обновите Xcode обязательные файлы заголовков или использовать динамические регистратора или задаваемого поведения управляемых компоновщика связи платформу или только ссылку Framework SDK (попытаться избежать новых API).

Xamarin.Mac требует файлы заголовков, из пакета SDK версии, указанной в сообщении об ошибке для построения приложения статических регистратора... Для устранения этой ошибки рекомендуется обновить Xcode, чтобы получить необходимые SDK, это будет включать все необходимые файлы заголовков. Если установлено несколько версий Xcode установлен, или хотите использовать Xcode в нестандартное расположение, не забудьте установить правильное расположение Xcode в настройках вашей интегрированной среды разработки.

Один потенциальных альтернативных решений, является включение управляемых компоновщика. Это приведет к удалению неиспользуемых API включая, в большинстве случаев новый API, где находятся файлы заголовка, отсутствует (или является неполным). Однако это не будет работать, если проект использует API, который был введен в новой SDK, отличной от вашей Xcode предоставляет.

Во-вторых, потенциальные альтернативные решения, вместо этого используйте динамические регистратора. Это будет наложить стоимость запуска, динамическую регистрацию типов, но удалить требование файла заголовка. 

Последние straw решением было бы использовать более старую версию Xamarin.Mac, версии, поддерживающей SDK проекта требуется.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: файл machine.config «{0}» не найден.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: Компиляции AOT доступна только в единой

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: Внутренняя ошибка {0}. Отправьте отчет об ошибках с тестовым случаем (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Гибридные AOT компиляции требуется для компиляции AOT всех сборок.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: копирование файла / символических ссылок (связанные проект)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: Не удается создать символьную ссылку «{файл}» -> «{target}»: ошибка {число}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Сборки продукта

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: Сборки требуется «{0}» отсутствует в ссылки

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: Сборка «{0}» не совместима с помощью этого инструмента

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: не удается найти {0} «{1}». Требуемая версия .NET framework «{0}» не может использоваться для упаковки приложения.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: Недопустимый целевой платформы «{0}».

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework всегда должны предназначаться для framework .NET 4.5, не «{0}» является недопустимой

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: Требуемая версия .NET framework «{0}» является недопустимым при framwork Xamarin.Mac 4.5 .NET для различных версий.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Несоответствие между Xamarin.Mac framework ссылка «{0}» и целевой выбран «{1}».

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Ошибки сбора данных (не требуя компоновщика) сборки

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: Не удается разрешить ссылку: {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: Не соответствие-O динамическую библиотеку (Неизвестный заголовок "0 x {0}"): {1}.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: Не статической библиотеки (Неизвестный заголовок) «{0}»: {1}.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: Не соответствие-O динамическую библиотеку (Неизвестный заголовок "0 x {0}"): {1}.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: Неизвестный формат fat запись находится в позиции {0} в {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: Файл типа {0} не является файлом MachO ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: компоновщика

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Ошибки компоновщика (Общие)

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: Не удалось выполнить привязку сборок

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: Не удается разрешить ссылку: {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: Параметр «{0}» будет игнорироваться, так как компоновка отключена

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Лишний компоновщика определения «{0}» не удалось найти файл.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Не удалось разобрать определений из «{0}».

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: Собственной библиотеки «{0}» указан, но не удалось найти.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Xamarin.Mac единой API от полного профиля .NET не поддерживает связывание. Передайте флаг - nolink.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: Ссылается {0}. \ {1\\} ** это сообщение относится к MM2006 **

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Неизвестный HttpMessageHandler `{0}`. Допустимые значения: HttpClientHandler (по умолчанию), CFNetworkHandler или NSUrlSessionHandler

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Неизвестный TLSProvider "{0}.  Допустимые значения: по умолчанию или appletls

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: Только первый {0} для {1} «ссылается на» предупреждений, отображаемых. ** Это сообщение относится к 2009 г. **

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: Не удалось разрешить ссылку на «{0}», на который ссылается «{1}». Приложение не будет содержать ссылочной сборки и может завершиться ошибкой во время выполнения.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac модули не поддерживают связывание. Запрос компоновки будет игнорироваться. ** Это сообщение является устаревшим в XM 3.6 + **

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Недопустимый TlsProvider `{0}` параметр. Единственным допустимым значением `{1}` будет использоваться.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: Не удалось обработать XML-описание: {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Оптимизатор привязки сбой обработки `...`.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac классический API не поддерживает связывание платформы.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Ошибка при обработке сборки "\*": *

Произошла непредвиденная ошибка при обработке сборки.

Сборка, причиной проблемы является имя в сообщении об ошибке. Чтобы устранить эту проблему сборки необходимо будет предоставляться в [отчет ошибок](https://bugzilla.xamarin.com) вместе с полная сборка журнала с помощью детализации включен (т. е. `-v -v -v -v` в **mtouch дополнительные аргументы**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Не удалось связать сборки «{0}», как и смешанном режиме.

Сборки смешанного режима не могут обрабатываться компоновщиком.

В разделе https://msdn.microsoft.com/en-us/library/x0w2664k.aspx Дополнительные сведения о смешанных сборках.

## <a name="mm3xxx-aot"></a>MM3xxx: AOT

### <a name="mm30xx-aot-general-errors"></a>MM30xx: Ошибки AOT (Общие)

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: Не удалось AOT сборки «{0}»

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: AOT «{0}» был запрошен, однако не найден

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: Исключение AOT «{0}» был запрошен, однако не найден

## <a name="mm4xxx-code-generation"></a>MM4xxx: создание кода

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: Основного шаблона не может быть развернут для `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx: регистратора

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: Приложение использует «{0}» платформу, которая не содержится в пакете SDK MacOS используется для построения приложения (эта структура впервые появился в OSX {2} при построении с MacOS {1} SDK.) Эта конфигурация не поддерживается статических регистратора (pass--регистратора: динамические дополнительные mmp в качестве аргумента параметр Mac построение проекта, чтобы выбрать). Также можно выберите новую SDK в параметры Mac сборки приложения.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC и инструментов

### <a name="mm51xx-compilation"></a>MM51xx: компиляция

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: Отсутствует «{0}» компилятора. Установите компонент «Средства командной строки» Xcode.

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: Ошибка при компиляции. Код ошибки - {0}. Отправьте отчет об ошибках в http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: связывание

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.framework MDK отсутствует. Установите MDK для вашей версии Mono.framework из http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: Не удается найти libxammac.a, вероятно, из-за неправильной установки Xamarin.Mac. Переустановите Xamarin.Mac.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: Недопустимый архитектуры. x86_64 поддерживается только для профилей не классическом.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: Недопустимая архитектура «{0}». Допустимые архитектуры: i386 и x86_64 (когда--профиль = мобильных устройств).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: Нельзя не учитывать динамический символ {символ} (--игнорировать динамической символом = {символ}), так как он не был обнаружен в качестве символа динамического.

В разделе [предупреждение эквивалентные mtouch](~/ios/troubleshooting/mtouch-errors.md#MT5218).

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: другие средства

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: не удается найти pkg-config. Установите Mono.framework из http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Отсутствует средство «otool». Установите компонент «Средства командной строки» Xcode

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: Отсутствуют зависимости. Установите компонент «Средства командной строки» Xcode

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode лицензионное соглашение может не приняты.  Запустите Xcode.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: Собственный связывания сбой с кодом ошибки 1.  Проверьте подробности в журнале сборки.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool сбой с кодом ошибки «{0}». Проверьте подробности в журнале сборки.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: среды выполнения

### <a name="mm800x-misc"></a>MM800x: Прочее

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Сборщик мусора Боэм не поддерживается. Вместо этого используйте SGen.

