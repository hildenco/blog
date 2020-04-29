# Accessing configuration in .NET Core console applications

With the release of .NET Core 3.1 framework, Microsoft changed a few things in how we access configuration in our files. While with ASP.NET Core the setup, the dependencies and the configuration comes setup, the same does not happen with Console Applications. On this quick  tutorial let's see how we can setup the equivalent setup for our console apps.



## Packages
Once you create your .NET Core app, the first thing to do is to add the following packages:
"Microsoft.Extensions.Configuration"
"Microsoft.Extensions.Configuration.Binder"
"Microsoft.Extensions.Configuration.EnvironmentVariables"
"Microsoft.Extensions.Configuration.FileExtensions"
"Microsoft.Extensions.Configuration.Json"

Next, we add the necessary initialization code:
var env = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT");
var builder = new ConfigurationBuilder()
    .AddJsonFile($"appsettings.json", true, true)
    .AddJsonFile($"appsettings.{env}.json", true, true)
    .AddEnvironmentVariables();

var config = builder.Build();

Add the necessary namespaces:
using Microsoft.Extensions.Configuration;


## Adding a configuration file
Now add a configuration file. .NET Core projects usually work with appSettings.json file. So add one empty file with that name on the root of of your project with your configurations. Remember that this is a json file so the configurations should be added as a json document. For example, the config file for my aspnet-microservices project for the newsletter microservice is:

{
  "AppName": "HildenCo's ASP.NET Microservices",
  "RabbitMQ": {
    "Host": "rabbitmq://localhost",
    "Queue": "hildenco"
  },
  "Mail": {
    "Host": "smtp.gmail.com",
    "Port": "587",
    "Username": "<your-gmail-account>",
    "Password": "<your-gmail-app-password>",
    "FromName": "HildenCo Notification Service",
    "To": "<destination-email>",
    "Subject": "We got your request!",
    "Template": "Hello {Name},\nthanks for subscribing!\n\nWe'll add your email <{Email}> to our list\nso we can keep in touch!\n\nBest Regards,\nHildenCo."
  }
}

So let's see next how to access that configuration.


## Accessing the configurations
Accessing our configurations is now simple via the config instance. Let's see a few examples based on this configuration.

Accessing a root property:
var appName = config["AppName"];

Accessing a sub-property:
var rmqHost = config["RabbitMQ:Host"];


## Mapping the configuration
Mapping the configuration is also something that's usually recommended. You probably did that before when you mapped to/from your entities to DTOs for on you data-access layer. Interestingly, one of the packages we added will make that simple for us. We can break this in three steps: 
creating an options file to map to/from the settings
bind the configuration

### Creating our options file
So let's see a concrete example in how to do that for our configuration. Let's create an MailOptions file to hold the config for our smtp with:

    public class MailOptions
    {
        public string Host { get; set; }
        public int  Port { get; set; }
        public string Username { get; set; }
        public string Password { get; set; }
        public string FromName { get; set; }
        public string To { get; set; }
        public string Subject { get; set; }
        public string Template { get; set; }
    }

Then, binding it to our config should be done with:
var mailOptions = new MailOptions();
config.GetSection("Mail").Bind(mailOptions);


## Reviewing the solution
Here's our final program.cs file in case you want to review it:

static void Main(string[] args)
{
    var env = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT");
    var builder = new ConfigurationBuilder()
        .AddJsonFile($"appsettings.json", true, true)
        .AddJsonFile($"appsettings.{env}.json", true, true)
        .AddEnvironmentVariables();

    var config = builder.Build();

    var appName = config["AppName"];
    var rmqHost = config["RabbitMQ:Host"]; 

    // ...
}


# References



# See Also





