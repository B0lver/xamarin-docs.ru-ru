---
ms.openlocfilehash: 3b1603b6af5ebb5558c3cd764f41fdbe24351b9b
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "68669808"
---
Запросы REST выполняются по протоколу HTTP с помощью тех же HTTP-команд, которые веб-браузеры используют для извлечения страниц и отправки данных на серверы. В этом упражнении вы создадите класс, который использует команду GET для извлечения данных из веб-API [OpenWeatherMap](https://openweathermap.org/). Этот веб-API может использоваться для получения данных прогноза погоды для указанного расположения. Для использования этого веб-API нужно зарегистрироваться для получения ключа API.

> [!div class="nextstepaction"]
> [Регистрация для получения ключа API](https://home.openweathermap.org/users/sign_up)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `Constants`. Удалите в **Constants.cs** весь код шаблона и замените его приведенным ниже кодом:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public const string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    Этот код определяет две константы. Константа `OpenWeatherMapEndpoint` определяет конечную точку для отправки веб-запросов, а константа `OpenWeatherMapAPIKey` определяет ваш личный ключ API для службы [OpenWeatherMap](https://openweathermap.org/).

    > [!IMPORTANT]
    > Ваш личный ключ API OpenWeatherMap должен быть задан как значение константы `OpenWeatherMapAPIKey`.

1. В **обозревателе решений** в проекте **WebServicesTutorial** добавьте новый класс с именем `WeatherData`. Затем в **WeatherData.cs** удалите весь код шаблона и замените его приведенным ниже:

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    Этот код определяет четыре класса, которые используются для моделирования данных JSON, полученных от веб-службы. Каждое свойство снабжено атрибутом `JsonProperty`, который содержит имя поля JSON. Newtonsoft.Json будет использовать это сопоставление имен полей JSON со свойствами CLR при десериализации данных JSON в объекты модели.

    > [!NOTE]
    > Приведенные выше определения класса были упрощены и не полностью моделируют данные JSON, полученные от веб-службы. Полный пример модели данных см. в примере [Приложения с прогнозом погоды](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/weather/).

1. В **обозревателе решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `RestService`. Затем удалите в **RestService.cs** весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    Этот код определяет единственный метод, `GetWeatherDataAsync`, который извлекает данные о погоде для указанного расположения из веб-API [OpenWeatherMap](https://openweathermap.org/). Этот метод использует метод `HttpClient.GetAsync` для отправки запроса GET к веб-API, указанному аргументом `uri`. Веб-API отправляет ответ, который хранится в объекте `HttpResponseMessage`. Ответ включает код состояния HTTP, который указывает, успешно ли выполнен HTTP-запрос. Если запрос выполнен успешно, веб-API возвращает код состояния HTTP 200 (ОК) и ответ JSON, который находится в свойстве `HttpResponseMessage.Content`. Эти данные JSON считываются в `string` с помощью метода `HttpContent.ReadAsStringAsync`, а затем десериализируются в объект `WeatherData` с помощью метода `JsonConvert.DeserializeObject`. Этот метод использует сопоставления между именами полей JSON и свойствами CLR, которые определены в классе `WeatherData` для выполнения десериализации.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. На **панели решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `Constants`. Удалите в **Constants.cs** весь код шаблона и замените его приведенным ниже кодом:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public static string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public static string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    Этот код определяет две константы. Константа `OpenWeatherMapEndpoint` определяет конечную точку для отправки веб-запросов, а константа `OpenWeatherMapAPIKey` определяет ваш личный ключ API для службы [OpenWeatherMap](https://openweathermap.org/).

    > [!IMPORTANT]
    > Ваш личный ключ API OpenWeatherMap должен быть задан как значение константы `OpenWeatherMapAPIKey`.

1. На **панели решений** в проекте **WebServicesTutorial** добавьте новый класс с именем `WeatherData`. Затем в **WeatherData.cs** удалите весь код шаблона и замените его приведенным ниже:

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    Этот код определяет четыре класса, которые используются для моделирования данных JSON, полученных от веб-службы. Каждое свойство снабжено атрибутом `JsonProperty`, который содержит имя поля JSON. Newtonsoft.Json будет использовать это сопоставление имен полей JSON со свойствами CLR при десериализации данных JSON в объекты модели.

    > [!NOTE]
    > Приведенные выше определения класса были упрощены и не полностью моделируют данные JSON, полученные от веб-службы. Полный пример модели данных см. в примере [Приложения с прогнозом погоды](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/weather/).

1. На **панели решений** в проекте **WebServiceTutorial** добавьте новый класс с именем `RestService`. Затем удалите в **RestService.cs** весь код шаблона и замените его приведенным ниже:

    ```csharp
    using System;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    Этот код определяет единственный метод, `GetWeatherDataAsync`, который извлекает данные о погоде для указанного расположения из веб-API [OpenWeatherMap](https://openweathermap.org/). Этот метод использует метод `HttpClient.GetAsync` для отправки запроса GET к веб-API, указанному аргументом `uri`. Веб-API отправляет ответ, который хранится в объекте `HttpResponseMessage`. Ответ включает код состояния HTTP, который указывает, успешно ли выполнен HTTP-запрос. Если запрос выполнен успешно, веб-API возвращает код состояния HTTP 200 (ОК) и ответ JSON, который находится в свойстве `HttpResponseMessage.Content`. Эти данные JSON считываются в `string` с помощью метода `HttpContent.ReadAsStringAsync`, а затем десериализируются в объект `WeatherData` с помощью метода `JsonConvert.DeserializeObject`. Этот метод использует сопоставления между именами полей JSON и свойствами CLR, которые определены в классе `WeatherData` для выполнения десериализации.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.
