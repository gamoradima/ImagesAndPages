[![Logo](https://www.creatio.com/sites/default/files/2019-10/creatio-main-logo.svg)](https://github.com/sindresorhus/awesome#readme)

# Agenda (Day 1)
- [Convert Creatio to development in FileSystem Mode](https://github.com/kirillkrylov/ImagesAndPages/wiki/Enable-development-in-FileSystem-Mode) 
- [Configure Clio](https://github.com/Advance-Technologies-Foundation/clio/blob/master/README.md)
- [Configure NLog](https://github.com/kirillkrylov/ImagesAndPages/wiki/Custom-Logging-with-NLog)
- [Create Package "GuidedLearning"](https://github.com/kirillkrylov/ImagesAndPages/wiki/Create-Package-GuidedLearning)
- [Convert Package to *CLIO* Project](https://github.com/kirillkrylov/ImagesAndPages/wiki/Convert-Package-To-Clio)
- Set first breakpoint and make log entry
- [Move GuidedLearning package to a cloud environment with Clio](https://github.com/Advance-Technologies-Foundation/clio#installing-package)

## Set First Break Point
- Add reference to Common.Logging, you can take necessary files from _**[AppPath]\bin**_ directory.
- <details>
    <summary>Create new ContactEventListener Class and set a breakpoint anywhere inside onSaved method.</summary>

    ```C#
        using global::Common.Logging;
        using Terrasoft.Core;
        using Terrasoft.Core.Entities;
        using Terrasoft.Core.Entities.Events;

        namespace GuidedLearningClio.Files.cs.el
        {
            /// <summary>
            /// Listener for 'Contact' entity events.
            /// </summary>
            /// <seealso cref="Terrasoft.Core.Entities.Events.BaseEntityEventListener" />
            [EntityEventListener(SchemaName = "Contact")]
            class ContactEventListener : BaseEntityEventListener
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
  </details>

- Attach your IDE to w3wp.exe process

- <details>
    <summary>Build your project / solution and use clio to restart the app</summary>

    ```text
    clio restart -e NameOfYourEnvironment
    ```
  </details>

# Recommended Academy Articles
- [ENTITY EVENT LAYER](https://academy.creatio.com/documents/technic-sdk/7-15/entity-event-layer)

# Recommended eLearning Modules 
- [Creatio Architecture](https://academy.creatio.com/online-courses/creatio-architecture)
- [Developer Environment Ssetup](https://academy.creatio.com/online-courses/developer-environment-setup)
- [Working With Packages](https://academy.creatio.com/online-courses/working-packages-creatio)