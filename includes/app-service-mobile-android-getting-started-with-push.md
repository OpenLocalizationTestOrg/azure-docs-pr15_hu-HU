1. Nyissa meg a fájlt a **alkalmazás** projekt `AndroidManifest.xml`. A következő két lépést a kódban cseréje _`**my_app_package**`_ a projekt az alkalmazáscsomag nevét, amely az az érték a `package` jellemzője a `manifest` címke.

2. A következő új engedélyek hozzáadása után a meglévő `uses-permission` elem:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Adja hozzá a következő kód után a `application` címke megnyitása:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Nyissa meg a fájlt *ToDoActivity.java*, és adja hozzá a következő importálása utasítás:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. A következő személyes változó hozzáadása az osztály: cseréje _`<PROJECT_NUMBER>`_ az alkalmazás az előző eljárás szerint Google rendelt Project-gyel:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Módosítsa a *MobileServiceClient* meghatározása **saját** **nyilvános statikus**, ezért most formátumban jelennek meg:

        public static MobileServiceClient mClient;

7. Ezután szükség hozzáadása egy új osztály hogyan kezeli az értesítéseket. A Project Explorer nyissa meg a **src** => **fő** => **java** csomópontok parancsot, és kattintson a jobb gombbal a csomag nevére csomópontot: kattintson az **Új**gombra, majd kattintson a **Java osztály**.

8. A **név** mezőbe `MyHandler`, majd kattintson az **OK gombra**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. A MyHandler fájl cserélje ki az osztályjegyzetfüzet nyilatkozatot

        public class MyHandler extends NotificationsHandler {


10. Adja hozzá a következő importálása utasításait a a `MyHandler` osztály:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Ezután a a tag hozzáadása a `MyHandler` osztály:

        public static final int NOTIFICATION_ID = 1;


12. Az a `MyHandler` osztály, és adja meg a **onRegistered** módszer, amelyben az eszköz értesítési központi mobilszolgáltatás regisztrálja felülbírálása az alábbi kódot.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. Az a `MyHandler` osztály, és adja meg a következő kódot felülbírálása az **onReceive** módszer hatására az értesítés fogadott megjelenítéséhez.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Vissza a TodoActivity.java fájlban frissítse az értesítési kezelő osztály regisztrálhatja a *ToDoActivity* osztály **onCreate** metódusát. Ellenőrizze, hogy ez a kód hozzáadása a után a *MobileServiceClient* jön létre.


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Az alkalmazás letöltése frissül a leküldéses értesítések támogatása.
