
# Startup Tasks

You can use startup tasks to perform operations before a role starts.

Startup tasks are defined in the ServiceDefinition.csdef file by using the Task element within the Startup element. Frequently startup tasks are batch files, but they can also be console applications, or batch files that start PowerShell scripts.

nvironment variables pass information into a startup task, and local storage can be used to pass information out of a startup task. For example, an environment variable can specify the path to a program you want to install, and files can be written to local storage that can then be read later by your roles.

Your startup task can log information and errors to the directory specified by the TEMP environment variable. During the startup task, the TEMP environment variable resolves to the C:\Resources\temp\<guid>.<rolename>\RoleTemp directory when running on the cloud.

Startup tasks can also be executed several times between reboots. For example, the startup task will be run each time the role recycles, and role recycles may not always include a reboot. Startup tasks should be written in a way that allows them to run several times without problems.

Startup tasks must end with an errorlevel (or exit code) of zero for the startup process to complete. If a startup task ends with a non-zero errorlevel, the role will not start.

## Startup Order

The following lists the role startup procedure in Windows Azure:

1. The instance is marked as Starting and does not receive traffic.
2. All startup tasks are executed according to their taskType attribute.
  * The simple tasks are executed synchronously, one at a time.
  * The background and foreground tasks are started asynchronously, parallel to the startup task.
3. The role host process is started and the site is created in IIS.
4. The ```OnStart``` method is called.
5. The instance is marked as Ready and traffic is routed to the instance.
6. The ```Run``` method is called.

IIS may not be fully configured during the startup task stage in the startup process, so role-specific data may not be available. Startup tasks that require role-specific data should use ```OnStart```.

## Example Startup Task

Startup tasks are defined in the ServiceDefinition.csdef file, in the Task element. The commandLine attribute specifies the name and parameters of the startup batch file or console command, the executionContext attribute specifies the privilege level of the startup task, and the taskType attribute specifies how the task will be executed.

In this example, an environment variable, MyVersionNumber, is created for the startup task and set to the value "1.0.0.0".

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
          <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
      </Task>
    </Startup>

In the following example, the Startup.cmd batch file writes the line "The current version is 1.0.0.0" to the StartupLog.txt file in the directory specified by the TEMP environment variable. The EXIT /B 0 line ensures that the startup task ends with an errorlevel of zero.

https://msdn.microsoft.com/en-us/library/hh180155.aspx
