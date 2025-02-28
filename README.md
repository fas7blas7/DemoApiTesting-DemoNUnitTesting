Demo API Testing 🚀

This project demonstrates how to work with APIs using different JSON libraries and make HTTP requests using the RestSharp library in C#. It shows various ways to handle JSON serialization/deserialization with both built-in (System.Text.Json) and popular external (Newtonsoft.Json) libraries. It also covers making GET and POST requests with RestSharp, including handling authentication. 🔐
Features ✨

    JSON Serialization/Deserialization with System.Text.Json and Newtonsoft.Json. 📦
    Custom Naming Strategy for JSON properties. 📝
    Working with Anonymous Types for JSON deserialization. 🔍
    Using JObject for dynamic JSON processing. 🔧
    Making HTTP Requests with the RestSharp library (GET and POST). 🌐
    Handling URL Segments in HTTP requests. 🔗
    Basic Authentication in POST requests. 🔑

Prerequisites ⚙️

    .NET SDK (5.0 or later). 📥
    Visual Studio or any C# editor of your choice. 💻
    Install NuGet packages for RestSharp and Newtonsoft.Json:

    dotnet add package RestSharp
    dotnet add package Newtonsoft.Json

Project Setup 🛠️

    Clone the repository or download the source code. 📂
    Ensure you have the necessary NuGet packages installed (RestSharp, Newtonsoft.Json). 📦
    Create a demoData.json file in the root directory or specify the path to a valid JSON file for testing the deserialization. 📝

Code Walkthrough 📚
WeatherForecast Class 🌦️

The project includes a WeatherForecast class that serves as a model for weather data. It contains three properties: Date, TemperatureC, and Summary, which can be serialized and deserialized into/from JSON.

public class WeatherForecast
{
    public DateTime Date { get; set; } = DateTime.Now;
    
    //[JsonProperty("temperature_c")]
    public int TemperatureC { get; set; } = 30;

    public string Summary { get; set; } = "Hot summer days";
}

Properties:

    Date: A DateTime property that stores the date of the forecast. It's initialized to the current date and time by default (DateTime.Now).
    TemperatureC: An integer property representing the temperature in Celsius. By default, it’s set to 30 (hot day).
    Summary: A string property for a summary of the weather (e.g., "Hot summer days").

JsonProperty Attribute (Commented Out):

//[JsonProperty("temperature_c")]

    If uncommented, this attribute changes the serialized name of the TemperatureC property to temperature_c in the resulting JSON. For example, it would serialize like this:

    {
      "Date": "2025-02-28T14:45:00",
      "temperature_c": 30,
      "Summary": "Hot summer days"
    }

Usage:

    The WeatherForecast class can be serialized and deserialized using Newtonsoft.Json and System.Text.Json.
    When serialized, the object is converted into a JSON string that can be transmitted to a web API or saved locally.
    The object can also be created from a JSON string by deserializing it.

1. JSON Serialization and Deserialization

    System.Text.Json.JsonSerializer and Newtonsoft.Json.JsonConvert are used for serializing and deserializing JSON to/from WeatherForecast objects. 🌦️
    Example of serializing an object to JSON:

string weatherInfo = JsonSerializer.Serialize(forecast);

Example of deserializing JSON to an object:

    WeatherForecast forecastFromJson = JsonSerializer.Deserialize<WeatherForecast>(jsonString);

2. Anonymous Types

    The project demonstrates how to deserialize JSON into an anonymous object with JsonConvert.DeserializeAnonymousType. 🎭

    var person = JsonConvert.DeserializeAnonymousType(json, template);

3. Applying Custom Naming Strategy

    The code demonstrates how to apply a custom snake_case naming strategy for JSON properties using DefaultContractResolver and SnakeCaseNamingStrategy. 🐍

4. Working with JObject

    This part of the code uses JObject to dynamically parse and manipulate JSON content. 🔄

5. Making HTTP Requests 🌐

    GET Request: Fetching data from the GitHub API using RestSharp. 📡

var client = new RestClient("https://api.github.com");
var request = new RestRequest("/users/softuni/repos", Method.Get);
var response = client.Execute(request);

Using URL Segments: Demonstrates how to add URL segments to a request URL. 🔗

var requestURLSegments = new RestRequest("/repos/{user}/{repo}/issues/{id}", Method.Get);

POST Request with Authentication: Shows how to perform an authenticated POST request. 🔐

    var clientWithAuthentication = new RestClient("https://api.github.com")
    {
        Authenticator = new HttpBasicAuthenticator("userName", "api-Token")
    };

Example Output 🎉

The application will print out various JSON outputs and status codes from API requests, for example:

{
  "first_name": "Martin",
  "last_name": "Kirov",
  "job_title": "Trainee"
}

StatusCode: 200 OK
Response content: [ ... ]

License 📄

This project is open-source and available under the MIT License. 🥳
Contributing 🤝

Feel free to fork the repository and submit issues or pull requests. 🖊️
