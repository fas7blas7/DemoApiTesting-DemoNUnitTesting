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
