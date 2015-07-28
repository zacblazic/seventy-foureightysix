
# Session State Modes

ASP.NET session state supports several different storage options for session data. Each option is identified by a value in the SessionStateMode enumeration.

* InProc mode, which stores session state in memory on the Web server. This is the default.
* StateServer mode, which stores session state in a separate process called the ASP.NET state service. This ensures that session state is preserved if the Web application is restarted and also makes session state available to multiple Web servers in a Web farm.
* SQLServer mode stores session state in a SQL Server database. This ensures that session state is preserved if the Web application is restarted and also makes session state available to multiple Web servers in a Web farm.
* Custom mode, which enables you to specify a custom storage provider.
* Off mode, which disables session state.

Session state mode can be set in the web.config file.

## In-Process Mode

* Default
* Stores session state values and variables in memory on the local Web server.
* Only mode that supports the Session_OnEnd event
* Web garden can cause data loss for this mode

## State Server Mode

* Stores session state in a process, referred to as the ASP.NET state service, that is separate from the ASP.NET worker process or IIS application pool.
* Ensures that session state is preserved if the Web application is restarted.
* Makes session state available to multiple Web servers in a Web farm.
* Installed with ASP.NET & .NET Framework.
* Objects stored in session state must be serializable.
* To use StateServer mode in a Web farm, you must have the same encryption keys specified in the machineKey element of your Web configuration for all applications that are part of the Web farm.


    <configuration>
      <system.web>
        <sessionState mode="StateServer"
          stateConnectionString="tcpip=SampleStateServer:42424"
          cookieless="false"
          timeout="20"/>
      </system.web>
    </configuration>

## SQL Server Mode

* Stores session state in a SQL Server database.
* Ensures that session state is preserved if the Web application is restarted and also makes session state available to multiple Web servers in a Web farm.
* Objects stored in session state must be serializable if the mode is SQL Server.
* To configure SQLServer mode for a Web farm, in the configuration file for each Web server, set the sessionState element's sqlConnectionString attribute to point to the same SQL Server database.
* In SQLServer mode, you can configure several computers running SQL Server to work as a failover cluster, which is two or more identical computers running SQL Server that store data for a single database. If one computer running SQL Server fails, another server in the cluster can take over and serve requests without session-data loss.


    <configuration>
      <system.web>
        <sessionState mode="SQLServer"
          sqlConnectionString="Integrated Security=SSPI;data
            source=SampleSqlServer;" />
      </system.web>
    </configuration>

## Custom Mode

* Custom mode specifies that you want to store session state data using a custom session state store provider.
* You must specify the type of the session state store provider using the providers sub-element of the sessionState configuration element.
You specify the provider type using an add sub-element and include both a type attribute that specifies the provider's type name and a name attribute that specifies the provider instance name.


    <configuration>
      <connectionStrings>
        <add name="OdbcSessionServices"
             connectionString="DSN=SessionState;" />
      </connectionStrings>

      <system.web>
        <sessionState
          mode="Custom"
          customProvider="OdbcSessionProvider">
          <providers>
            <add name="OdbcSessionProvider"
              type="Samples.AspNet.Session.OdbcSessionStateStore"
              connectionStringName="OdbcSessionServices"
              writeExceptionsToEventLog="false" />
          </providers>
        </sessionState>
      </system.web>
    </configuration>


### Resources

http://msdn.microsoft.com/library/ms178586.aspx

https://www.simple-talk.com/cloud/platform-as-a-service/managing-session-state-in-windows-azure-what-are-the-options/

http://www.c-sharpcorner.com/UploadFile/25c78a/load-balancing-session-state-configuration/
