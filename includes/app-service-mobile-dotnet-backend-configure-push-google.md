Az eljárást, amely megfelel a kódmentes projekttípus&mdash; [.NET kódmentes](#dotnet) vagy a [Node.js kódmentes](#nodejs).

### <a name="dotnet"></a>.NET kódmentes projekt

1. A Visual Studióban, kattintson a jobb gombbal a kiszolgáló projekt, és kattintson a **NuGet csomagok kezelése**, kereshet `Microsoft.Azure.NotificationHubs`, majd kattintson a **telepítés**gombra. Ez az értesítés hubok ügyfél tár a telepítést.

2. A vezérlők mappát, nyissa meg a TodoItemController.cs, és adja hozzá a következő `using` kimutatások:

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

            // Android payload
            var androidNotificationPayload = "{ \"data\" : {\"message\":\"" + item.Text + "\"}}";

            try
            {
                // Send the push notification and log the results.
                var result = await hub.SendGcmNativeNotificationAsync(androidNotificationPayload);

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

### <a name="nodejs"></a>NODE.js kódmentes projekt

1. Ha azt még nem tette meg, [Töltse le a quickstart útmutató projekt](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) , különben [az Azure-portálon online szerkesztő](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
 
1. A meglévő kódot a todoitem.js fájlban lecserélése a következőre:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
        
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the GCM payload.
        var payload = {
            "data": {
                "message": context.item.text
            }
        };   
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a GCM native notification.
                    context.push.gcm.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });
        
        module.exports = table;  

    Új teendő elem beszúrásakor a item.text tartalmazó GCM értesítést küld ez. 

2. Szerkeszti a fájlt a helyi számítógépen, amikor ismét közzéteheti a kiszolgáló-projektet. 
