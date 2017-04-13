
+ **.NET kódmentes (C#)**:    
    1. A Visual Studióban, kattintson a jobb gombbal a kiszolgáló projekt, és kattintson a **NuGet csomagok kezelése**, kereshet `Microsoft.Azure.NotificationHubs`, majd kattintson a **telepítés**gombra. Ez az értesítés hubok tárat a kódmentes értesítést küld a telepítést.

    2. A Visual Studio projekt a kódmentes, nyissa meg a **vezérlők** > **TodoItemController.cs**. Kattintson a fájl lapon adja hozzá a következő `using` utasítás:

            using Microsoft.Azure.Mobile.Server.Config;
            using Microsoft.Azure.NotificationHubs;


    3. Cserélje le a `PostTodoItem` módszert a következő kódot:  
      
            public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
            {
                TodoItem current = await InsertAsync(item);
                // Get the settings for the server project.
                HttpConfiguration config = this.Configuration;
    
                MobileAppSettingsDictionary settings = 
                    this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
    
                // Get the Notification Hubs credentials for the Mobile App.
                string notificationHubName = settings.NotificationHubName;
                string notificationHubConnection = settings
                    .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;
    
                // Create a new Notification Hub client.
                NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);
    
                // iOS payload
                var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";
    
                try
                {
                    // Send the push notification and log the results.
                    var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);
    
                    // Write the success result to the logs.
                    config.Services.GetTraceWriter().Info(result.State.ToString());
                }
                catch (System.Exception ex)
                {
                    // Write the failure result to the logs.
                    config.Services.GetTraceWriter()
                        .Error(ex.Message, null, "Push.SendAsync Error");
                }
                return CreatedAtRoute("Tables", new { id = current.Id }, current);
            }

    4. A kiszolgáló projekt közzé.

+ **Node.js kódmentes** : 
   
    1. Ha azt még nem tette meg, [Töltse le a quickstart útmutató projekt](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , különben [az Azure-portálon online szerkesztő](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor). 
    
    2. A todoitem.js táblázat parancsfájl cserélje ki a következő kódot:


            var azureMobileApps = require('azure-mobile-apps'),
                promises = require('azure-mobile-apps/src/utilities/promises'),
                logger = require('azure-mobile-apps/src/logger');
            
            var table = azureMobileApps.table();
            
            // When adding record, send a push notification via APNS
            table.insert(function (context) {
                // For details of the Notification Hubs JavaScript SDK, 
                // see http://aka.ms/nodejshubs
                logger.info('Running TodoItem.insert');
                
                // Create a payload that contains the new item Text.
                var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";
                
                // Execute the insert; Push as a post-execute action when results are returned as a Promise.
                return context.execute()
                    .then(function (results) {
                        // Only do the push if configured
                        if (context.push) {
                            context.push.apns.send(null, payload, function (error) {
                                if (error) {
                                    logger.error('Error while sending push notification: ', error);
                                } else {
                                    logger.info('Push notification sent successfully!');
                                }
                            });
                        }
                        return results;
                    })
                    .catch(function (error) {
                        logger.error('Error while running context.execute: ', error);
                    });
            });
            
            module.exports = table;

    2. Szerkeszti a fájlt a helyi számítógépen, amikor ismét közzéteheti a kiszolgáló-projektet.
