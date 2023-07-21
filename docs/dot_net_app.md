---
title: Writing a .NET Application
layout: docs
---

The Migration SDK is written in and supports the .NET Framework. See [Requirements](requirements.md) for supported .NET versions.

## Required elements
The main elements required for a basic migration are listed in the following sections.

### Configuration

#### Source
* `ServerURL`
* `SiteContentUrl`
* `AccessTokenName`
* `AccessToken`

### Destination
* `SiteContentUrl`
* `AccessTokenName`
* `AccessToken`
* `ServerUrl`

### Migration plan

## Example code

Program.cs

```
Java

      
public static class Program
    {
        public static async Task Main(string[] args)
        {            
            using var host = Host.CreateDefaultBuilder(args)
                .ConfigureServices((ctx, services) =>
                {
                    services
                        .Configure<MyMigrationApplicationOptions>(ctx.Configuration)
                        .AddTableauMigrationSdk(ctx.Configuration.GetSection("tableau:migrationSdk"))
                        .AddHostedService<MyMigrationApplication>();
                })
                .Build();

            await host.RunAsync();
        }
    }

```

Startup code

```
Java

using System.Net.Mail;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Options;
using Tableau.Migration.TestApplication.Config;

namespace Tableau.Migration.TestApplication
{
    internal sealed class MyMigrationApplication : IHostedService
    {
        private readonly IHostApplicationLifetime _appLifetime;
        private IMigrationPlanBuilder _planBuilder;
        private readonly IMigrator _migrator;
        private readonly MyMigrationApplicationOptions _options;

        public MyMigrationApplication(IHostApplicationLifetime appLifetime, IMigrationPlanBuilder planBuilder, IMigrator migrator, IOptions<MyMigrationApplicationOptions> options)
        {
            _appLifetime = appLifetime;
            _planBuilder = planBuilder;
            _migrator = migrator;
            _options = options.Value;
        }

        public async Task StartAsync(CancellationToken cancel)
        {
            _planBuilder = _planBuilder
                .FromSourceTableauServer(
                _options.Source.ServerUrl,
                _options.Source.SiteContentUrl,
                _options.Source.AccessTokenName,
                _options.Source.AccessToken)
                .ToDestinationTableauCloud(
                _options.Destination.SiteContentUrl,
                _options.Destination.AccessTokenName,
                _options.Destination.AccessToken,
                _options.Destination.ServerUrl)
                .ForServerToCloud();

            _planBuilder.Mappings.Add(new UserTableauCloudEmailMapping(new MailAddress("myemail@example.com")));

            var plan = _planBuilder.Build();

            var result = await _migrator.ExecuteAsync(plan, cancel);

            //TODO: Handle results

            _appLifetime.StopApplication();
        }

        public Task StopAsync(CancellationToken cancel) => Task.CompletedTask;
    }
}
```
