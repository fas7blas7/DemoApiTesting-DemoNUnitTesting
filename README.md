Demo API Testing 🚀

This project demonstrates how to work with APIs using different JSON libraries and make HTTP requests using the RestSharp library in C#. It shows various ways to handle JSON serialization/deserialization with both built-in (System.Text.Json) and popular external (Newtonsoft.Json) libraries. It also covers making GET and POST requests with RestSharp, including handling authentication. 🔐 Additionally, it includes NUnit-based unit tests to automate and validate API requests. 🧪
Features ✨

    JSON Serialization/Deserialization with System.Text.Json and Newtonsoft.Json. 📦
    Custom Naming Strategy for JSON properties. 📝
    Working with Anonymous Types for JSON deserialization. 🔍
    Using JObject for dynamic JSON processing. 🔧
    Making HTTP Requests with the RestSharp library (GET and POST). 🌐
    Handling URL Segments in HTTP requests. 🔗
    Basic Authentication in POST requests. 🔑
    NUnit Test Automation for validating API requests and responses. 🧪
    Testing ZipCode API using Zippopotamus API. 📍

Prerequisites ⚙️

    .NET SDK (5.0 or later). 📥
    Visual Studio or any C# editor of your choice. 💻
    Install NuGet packages for RestSharp, Newtonsoft.Json, and NUnit:

    dotnet add package RestSharp
    dotnet add package Newtonsoft.Json
    dotnet add package NUnit
    dotnet add package NUnit3TestAdapter
    dotnet add package Microsoft.NET.Test.Sdk

Project Setup 🛠️

    Clone the repository or download the source code. 📂
    Ensure you have the necessary NuGet packages installed (RestSharp, Newtonsoft.Json, NUnit). 📦
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

NUnit Unit Tests for GitHub API 🧪

The project also includes NUnit-based unit tests that validate various API calls to GitHub. These tests demonstrate how to use RestSharp to interact with the GitHub API and verify responses. Below is the class UnitTestsDemo, which contains multiple test cases to interact with GitHub issues.

using DemoNunitTest.Models;
using RestSharp;
using RestSharp.Authenticators;
using System.Net;
using System.Text.Json;

namespace DemoNunitTest
{
    public class UnitTestsDemo
    {
        RestClient client;

        [SetUp]
        public void Setup()
        {
            var options = new RestClientOptions("https://api.github.com")
            {
                MaxTimeout = 3000,
                Authenticator = new HttpBasicAuthenticator("username", "token")
            };

            this.client = new RestClient(options);
        }

        [TearDown]
        public void Teardown()
        {
            this.client.Dispose();
        }

        [Test]
        public void Test_GitHubAPIRequest()
        {
            var request = new RestRequest("/repos/testnakov/test-nakov-repo/issues", Method.Get);
            var response = client.Get(request);
            Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.OK));
        }

        [Test]
        public void Test_GetAllIssuesFromARepo()
        {
            var request = new RestRequest("repos/testnakov/test-nakov-repo/issues");
            var response = client.Execute(request);
            var issues = JsonSerializer.Deserialize<List<Issue>>(response.Content);

            Assert.That(issues.Count > 1);

            foreach (var issue in issues)
            {
                Assert.That(issue.id, Is.GreaterThan(0));
                Assert.That(issue.number, Is.GreaterThan(0));
                Assert.That(issue.title, Is.Not.Empty);
            }
        }

        private Issue CreateIssue(string title, string body)
        {
            var request = new RestRequest("repos/testnakov/test-nakov-repo/issues");
            request.AddBody(new { body, title });
            var response = client.Execute(request, Method.Post);
            var issue = JsonSerializer.Deserialize<Issue>(response.Content);
            return issue;
        }

        [Test]
        public void Test_CreateGitHubIssue()
        {
            string title = "This is a Demo Issue";
            string body = "QA Back-End Automation Course February 2024";
            var issue = CreateIssue(title, body);
            Assert.That(issue.id, Is.GreaterThan(0));
            Assert.That(issue.number, Is.GreaterThan(0));
            Assert.That(issue.title, Is.Not.Empty);
        }

        [Test]
        public void Test_EditIssue()
        {
            var request = new RestRequest("repos/testnakov/test-nakov-repo/issues/4946");
            request.AddJsonBody(new
            {
                title = "Changing the name of the issue that I created"
            });

            var response = client.Execute(request, Method.Patch);
            var issue = JsonSerializer.Deserialize<Issue>(response.Content);

            Assert.That(response.StatusCode, Is.EqualTo(HttpStatusCode.OK));
            Assert.That(issue.id, Is.GreaterThan(0), "Issue ID should be greater than 0.");
            Assert.That(response.Content, Is.Not.Empty, "The response content should not be empty.");
            Assert.That(issue.number, Is.GreaterThan(0), "Issue number should be greater than 0.");
            Assert.That(issue.title, Is.EqualTo("Changing the name of the issue that I created"), "The issue title should match the new title.");
        }
    }
}

Tests:

    Test_GitHubAPIRequest: Sends a GET request to the GitHub API and checks if the response status code is OK (200).
    Test_GetAllIssuesFromARepo: Fetches all issues from a GitHub repository and checks that there are more than one issue and validates key properties for each issue.
    Test_CreateGitHubIssue: Creates a new issue on GitHub and validates that the issue has a valid ID, number, and title.
    Test_EditIssue: Edits an existing issue and validates that the changes were successful by checking the status code and issue details.

NUnit Unit Tests for Zippopotamus API 🌍

We also include tests for a ZipCode API (Zippopotamus API), which retrieves location data based on a country code and postal code. The following test, ZippopotamusTest, validates that the expected location is returned for the given postal code.

using DemoNunitTest.Models;
using Newtonsoft.Json;
using RestSharp;

namespace DemoNunitTest
{
    public class ZippopotamusTest
    {
        [TestCase("BG", "1000", "Sofija")]
        [TestCase("BG", "5000", "Veliko Turnovo")]
        [TestCase("CA", "M5S", "Toronto")]
        [TestCase("GB", "B1", "Birmingham")]
        [TestCase("DE", "01067", "Dresden")]
        public void TestZippopotamus(
            string countryCode, string zipCode,
            string expectedPlace)
        {
            // Arrange
            var restClient = new RestClient("https://api.zippopotam.us");
            var httpRequest = new RestRequest(countryCode + "/" + zipCode, Method.Get);

            // Act
            RestResponse httpResponse = restClient.Execute(httpRequest);
            var location = JsonConvert.DeserializeObject<Location>(httpResponse.Content);

            // Assert
            StringAssert.Contains(expectedPlace, location.Places[0].PlaceName);
        }
    }
}

Tests:

    TestZippopotamus: Validates that the place name returned by the Zippopotamus API contains the expected place for a given country code and zip code.

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
