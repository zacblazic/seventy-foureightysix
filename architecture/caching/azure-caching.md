
# Azure Cache
Microsoft Azure Cache is a family of distributed, in-memory, scalable solutions that enable you to build highly scalable and responsive applications by providing super-fast access to your data.

## Redis Cache
Microsoft Azure Redis Cache is based on the popular open source Redis Cache. It gives you access to a secure, dedicated Redis cache, managed by Microsoft. A cache created using Azure Redis Cache is accessible from any application within Microsoft Azure.

## Managed Cache Service
Azure Managed Cache Service is based on the AppFabric Cache engine. It also gives you access to a secure, dedicated cache that is managed by Microsoft. A cache created using the Managed Cache Service is also accessible from applications within Azure running on Azure Web Sites, Web & Worker Roles and Virtual Machines.

### Features
* Pre-built ASP.NET providers for session state and page output caching, enabling acceleration of web applications without having to modify application code.
* Caches any serializable managed object - for example: CLR objects, rows, XML, binary data.
* Consistent development model across both Azure and Windows Server AppFabric.

### Using The Cache

* Library provided through "Windows Azure Caching" NuGet package.

      <configSections>
        <section name="dataCacheClients"
          type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection,
                Microsoft.ApplicationServer.Caching.Core"
          allowLocation="true"
          allowDefinition="Everywhere" />

      <section name="cacheDiagnostics"
          type="Microsoft.ApplicationServer.Caching.AzureCommon.DiagnosticsConfigurationSection,
                Microsoft.ApplicationServer.Caching.AzureCommon"
          allowLocation="true"
          allowDefinition="Everywhere" />
        </configSections>

      <dataCacheClients>
        <dataCacheClient name="default">
          <autoDiscover isEnabled="true" identifier="[Cache role name or Service Endpoint]" />
          <securityProperties mode="Message" sslEnabled="false">
            <messageSecurity authorizationInfo="[Authentication Key]" />
          </securityProperties>
        </dataCacheClient>
      </dataCacheClients>

### Session State

    <sessionState mode="Custom" customProvider="AFCacheSessionStateProvider">
        <providers>
          <add name="AFCacheSessionStateProvider"
            type="Microsoft.Web.DistributedCache.DistributedCacheSessionStateStoreProvider,
                  Microsoft.Web.DistributedCache"
            cacheName="default"
            dataCacheClientName="default"/>
        </providers>
      </sessionState>

### Output Caching
The Output Cache Provider for Azure Cache is an out-of-process storage mechanism for output cache data.

    <caching>
      <outputCache defaultProvider="AFCacheOutputCacheProvider">
        <providers>
          <add name="AFCacheOutputCacheProvider"
            type="Microsoft.Web.DistributedCache.DistributedCacheOutputCacheProvider,
                  Microsoft.Web.DistributedCache"
            cacheName="default"
            dataCacheClientName="default" />
        </providers>
      </outputCache>
    </caching>



## In-Role Cache
In-Role Cache is based on the AppFabric Cache engine. In-Role Cache allows you to perform caching by using a dedicated web or worker role instance in an application deployed to Microsoft Azure Cloud Services. This provides flexibility in terms of deployment options and size but you manage the cache yourself.
