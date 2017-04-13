

1. Azon a Macen indítsa el az **Access Kulcsláncában**. Nyissa meg a **Saját tanúsítványok** **kategóriában** a bal oldali navigationn sávon. Keresse meg az előző szakaszban letöltötte az SSL-tanúsítvány, és hozza nyilvánosságra a tartalmát. Jelölje be a csak a tanúsítvány (nem adja meg a titkos kulcs), majd [exportálja](https://support.apple.com/kb/PH20122?locale=en_US).

2. Az [Azure-portálon](https://portal.azure.com/)kattintson **Az összes böngészése** > **Alkalmazás szolgáltatások** > a mobilalkalmazás kódmentes. A **Beállítások**csoportban kattintson a **Alkalmazás szolgáltatás leküldéses**, majd a értesítés központi nevét. Nyissa meg **az Apple leküldéses értesítést szolgáltatások** > **tanúsítvány feltöltése**. Töltse fel a .p12 fájlt, jelölje ki a megfelelő **mód** (attól függően, hogy az ügyfelek SSL tanúsítvány-a korábban a gyártás vagy védőfalas). Mentse a módosításokat.

A szolgáltatás konfigurálva van az iOS a leküldéses értesítések használata!

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
