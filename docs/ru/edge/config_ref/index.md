* * *

license: Licensed to the Apache Software Foundation (ASF) under one or more contributor license agreements. See the NOTICE file distributed with this work for additional information regarding copyright ownership. The ASF licenses this file to you under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

           http://www.apache.org/licenses/LICENSE-2.0
    
         Unless required by applicable law or agreed to in writing,
         software distributed under the License is distributed on an
         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
         KIND, either express or implied.  See the License for the
         specific language governing permissions and limitations
    

## under the License.

# Файл config.xml

Многие аспекты поведения приложения могут контролироваться с помощью глобального конфигурационного файла `config.xml`. Этот платформо-зависимый XML-файл основан на спецификации W3C [Упакованые веб-приложения (Packaged Web Apps)][1] и расширен для указаниях основных функций Cordova API, плагинов и настроек специфичных для платформы.

 [1]: http://www.w3.org/TR/widgets/

Для проектов, созданных с использованием Cordova CLI (описанном в разделе Интерфейс командной строки) этот файл можно найти в директории верхнего уровня:

        app/config.xml
    

Обратите внимание, что до версии 3.3.1-0.2.0, файл располагался в `app/www/config.xml`, и что размещение файла в этом месте по-прежнему поддерживается.

При использовании командной строки для построения проекта, версии этого файла копируются без изменений в различные подкаталоги `platforms/`, например:

        app/platforms/ios/AppName/config.xml
        app/platforms/blackberry10/www/config.xml
        app/platforms/android/res/xml/config.xml
    

Этот раздел описывает параметры глобальной и кросс платформенной конфигурации. В следующих разделах для платформо-зависимых параметров:

*   Настройка iOS
*   Настройка Android
*   Настройка BlackBerry 10

В дополнение к различным конфигурационным параметрам описанным ниже, можно также настроить основной набор изображений для приложения для каждой целевой платформы. Смотрите раздел Иконки и Заставки для дополнительной информации.

## Основные элементы конфигурации

Этот пример показывает значения по умолчанию в файле `config.xml` сформированном командой CLI `create`, описанной в разделе Интерфейс командной строки:

        <widget id="com.example.hello" version="0.0.1">
            <name>HelloWorld</name>
            <description>
                A sample Apache Cordova application that responds to the deviceready event.
            </description>
            <author email="dev@callback.apache.org" href="http://cordova.io">
                Apache Cordova Team
            </author>
            <content src="index.html" />
            <access origin="*" />
        </widget>
    

Следующие элементы конфигурации появляются в файле верхнего уровня `config.xml` и поддерживаются на всех существующих платформах Cordova:

*   Атрибут `id` элемента `<widget>` указывает идентификатор приложения в обратном формате доменных имен и атрибут `version` его полный номер версии в нотации майор/минор/патч.
    
    Тег виджета могут также иметь атрибуты, которые определяют альтернативные версии, а именно versionCode для Android и CFBundleVersion для iOS. В разделе дополнительных версий ниже для деталей.

*   Элемент `<name>` определяет официальное имя приложения, как он отображается на главном экране устройства и в интерфейсе магазина приложений.

*   Элементы `<description>` и `<author>` определяют метаданные и контактную информацию, которые могут отображаться в каталоге магазина приложений.

*   Необязательный элемент `<content>` определяет стартовую страницу приложения в каталоге верхнего уровня веб ресурсов. Значением по умолчанию является `index.html` , которая обычно находится в каталоге верхнего уровня `www` проекта.

*   Элементы `<access>` определяют набор внешних доменов, с которым приложение имеет право взаимодействовать. Значение по умолчанию, показанное выше позволяет осуществлять доступ к любому серверу. Смотрите раздел Руководство по разрешению доступа к доменам для подробностей.

*   Элемент `<preference>` задает различные параметры как пару атрибутов `name`/`value`. Имя каждого параметра, указанного в атрибуте `name` указывается без учета регистра. Многие параметры являются уникальными для конкретных платформ, как это указано в начале этой страницы. В следующих разделах подробно описаны настройки, которые применяются к более чем одной платформе.

### Дополнительное управление версиями

Так, Android и iOS поддерживает вторую строку версии (или номер) помимо видимой в app магазины, [versionCode][2] для Android и [CFBundleVersion][3] для iOS. Ниже приведен пример, который явно задает versionCode и CFBundleVersion

 [2]: http://developer.android.com/tools/publishing/versioning.html
 [3]: http://stackoverflow.com/questions/4933093/cfbundleversion-in-the-info-plist-upload-error

        <widget id="io.cordova.hellocordova"
          version="0.0.1"
          android-versionCode="7"
          ios-CFBundleVersion="3.3.3">
    

Если альтернативная версия не указана, будет использоваться следующие значения по умолчанию:

        // assuming version = MAJOR.MINOR.PATCH-whatever
        versionCode = PATCH + MINOR * 100 + MAJOR * 10000
        CFBundleVersion = "MAJOR.MINOR.PATCH"
    

## Глобальные настройки

Следующие глобальные настройки применяются для всех платформ:

*   `Fullscreen` позволяет скрыть строку состояния в верхней части экрана. Значение по умолчанию — `false` . Пример:
    
        <preference name="Fullscreen" value="true" />
        

*   `Orientation` позволяет заблокировать поворот приложение при изменении положения устройства. Возможные значения `default`, `landscape`, или `portrait` . Пример:
    
        <preference name="Orientation" value="landscape" />
        
    
    **Примечание**: `default` значение означает что разрешены *обе* ориентации, альбомная и портретная. Если вы хотите использовать настройки по умолчанию для каждой платформы (обычно только для портретная ориентация), удалите этот элемент из файла `config.xml`.

## Много-платформенный настройки

Следующие параметры применяются для более чем одной платформы, но не ко всем из них:

*   `DisallowOverscroll` (логическое значение, по умолчанию `false` ): Установите в `true` если вы не хотите чтобы интерфейс отображал каких-либо обратной связи, когда пользователи прокручивают за начало или конец содержимого.
    
        <preference name="DisallowOverscroll" value="true"/>
        
    
    Применяется к Android и iOS. На iOS, совершение перелистывание за границу контента (overscroll) плавно возвращает положение контента назад в исходное положение. На Android это действие производит более тонкий светящийся эффект вдоль верхнего или нижнего края контента.

*   `BackgroundColor`: Задайте цвет фона приложения. Поддерживает шестнадцатеричное значение размером 4 байта, где первый байт представляющий альфа-канал и стандартные значения RGB для следующих трех байтов. В этом примере указывается голубой цвет:
    
        <preference name="BackgroundColor" value="0xff0000ff"/>
        
    
    Применяется к Android и BlackBerry. Переопределяет CSS, который доступен для *всех* платформ, например:`body{background-color:blue}`.

*   `HideKeyboardFormAccessoryBar`(логическое значение, по умолчанию `false` ): Установите в `true` чтобы скрыть дополнительную панель инструментов, которая появляется над клавиатурой, помогая пользователям перемещаться из одной формы ввода на другой.
    
        <preference name="HideKeyboardFormAccessoryBar" value="true"/>
        
    
    Применяется к iOS и BlackBerry.

## Элемент *feature*

При использовании командной строки для построения приложений, использовать команду `plugin`, чтобы включить API устройства. Это не изменяет файл `config.xml` верхнего уровня, так что элемент `<feature>` не применяется к вашему рабочему процессу. Если вы работаете непосредственно в SDK и с помощью платформа специфического `config.xml` файл в качестве источника, используется `<feature>` тег, чтобы включить API функции устройства и внешних плагинов. Эти элементы обычно присутствуют с разными значениями в платформо-зависимых файлах `config.xml`. К примеру, таким образом можно указать Device API для проектов Android:

        <feature name="Device">
            <param name="android-package" value="org.apache.cordova.device.Device" />
        </feature>
    

Вот как появляется элемент в проектах iOS:

        <feature name="Device">
            <param name="ios-package" value="CDVDevice" />
        </feature>
    

Смотрите подробную информацию о том, как определить каждую функции в разделе Справочник API. Также смотрите Руководство по разработке плагинов для получения дополнительной информации по плагинам.

## Элемент *platform*

При использовании командной строки для построения приложений, иногда бывает необходимо указать параметры или другие элементы, специфичные для конкретной платформы. Используйте элемент `<platform>` для указания конфигурации которая должна применяться только в файле `config.xml` определенной платформы. Например, вот как можно указать, что только на платформе Android следует использовать параметр Fullscreen:

        <platform name="android">
            <preference name="Fullscreen" value="true" />
        </platform>