
1. A megoldás megtekintése (vagy **Solution Explorer** a Visual Studio alkalmazásban) kattintson a jobb gombbal a **összetevők** mappát, kattintson a **További összetevők az...**, a **Google Cloud üzenetküldés ügyfél** összetevő kereshet, és adja hozzá a projekthez.

2. Nyissa meg a ToDoActivity.cs projekt-fájlját, és adja hozzá a következő utasítással az osztály:

        using Gcm.Client;

3. A **ToDoActivity** osztály adja hozzá a következő új kódot: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Ez lehetővé teszi, hogy a leküldéses kezelő szolgáltatás folyamata a mobil ügyfélprogram-példány elérni.

4.  A **MobileServiceClient** létrehozását követően a **OnCreate** módszer hozzáadása a következő kódot:

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

A **ToDoActivity** most készíteni, a leküldéses értesítések hozzáadására.