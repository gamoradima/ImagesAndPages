[![Logo](https://www.creatio.com/sites/default/files/2019-10/creatio-main-logo.svg)](https://github.com/sindresorhus/awesome#readme)
## Agenda (Day 1 - Jan 21, 2020)
- Convert Creatio to development in FileSystem Mode
- Configure Clio (restore NuGet packages)
- Configure NLog
- Set first breakpoint and make log entry

### Convert Creatio to development in **FileSystem Mode**.
 
| --- | --- |
|To enable development in the file system, edit Web.config file (located in the root folder with the installed application) and set the enabled attribute of the fileDesignMode element to true. (Update [AppPath]\web.config)|![Web.config](../Img/LocateWebConfig.png)|

```xml
<fileDesignMode enabled="true"/>
<add key="UseStaticFileContent" value="false" />
```


- **Download packages** to file system <br/>
![Download Packages To FileSystem](../Img/confguration_buttons.png)

- **Enable Debugging mode** for client side source code. Change SystemSetting Debug mode (code: IsDebug) to true<br/>
![EnableDebug](../Img/EnableDebug.png)
![IsDebug](../Img/IsDebug.png)
- [Academy Article](https://academy.creatio.com/documents/technic-sdk/7-15/introduction-9) - Enable File System Mode
- [IsDebug](https://academy.creatio.com/documents/technic-sdk/7-15/isdebug-mode) - Used to get additional debugging info.

### Configure Custom Logging with NLog
To Configure custom logging **update nlog.config** and **nlog.targets.config**. Both files are located in [AppPath]\Terrasoft.WebApp

Add the following to **nlog.config** file:
```xml
<logger name="GuidedLearningLogger" writeTo="GuidedLearningAppender" minlevel="Info" final="true" />
```

Add the following to the **nlog.target.config** file
```xml
<target name="GuidedLearningAppender" xsi:type="File"
	layout="${Date} [${ThreadIdOrName}] ${uppercase:${level}} ${UserName} ${MethodName} - ${Message}"
	fileName="${LogDir}/${LogDay}/GuidedLearning.log" />
```
- [Logging](https://academy.creatio.com/documents/technic-sdk/7-15/logging-creatio-nlog) - Logging

### Set First Break Point
- Add reference to Common.Logging, you can take necessary files from [AppPath]\bin directory.
- Create EntityNameEventListener Class and set a breakpoint anywhere inside onSaved method.
```C#
using global::Common.Logging;
using Terrasoft.Core;
using Terrasoft.Core.Entities;
using Terrasoft.Core.Entities.Events;

namespace GuidedLearningClio.Files.cs.el
{
    /// <summary>
    /// Listener for 'EntityName' entity events.
    /// </summary>
    /// <seealso cref="Terrasoft.Core.Entities.Events.BaseEntityEventListener" />
    [EntityEventListener(SchemaName = "Contact")]
    class EntityNameEventListener : BaseEntityEventListener
    {
        private static readonly ILog _log = LogManager.GetLogger("GuidedLearningLogger");
        public override void OnSaved(object sender, EntityAfterEventArgs e)
        {
            base.OnSaved(sender, e);
            Entity entity = (Entity)sender;
            UserConnection userConnection = entity.UserConnection;
            
            string message = $"Changing name for {entity.GetTypedColumnValue<string>("Name")}";
            _log.Info(message);
        }
    }
}
```
- Build your project / solution and use clio to restart the app
```text
clio restart -e NameOfYourEnvironment
```