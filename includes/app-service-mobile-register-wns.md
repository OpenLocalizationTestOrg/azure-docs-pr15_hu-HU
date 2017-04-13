
1. A Visual Studio megoldás Intézőben kattintson a jobb gombbal a Windows Áruházbeli alkalmazás projektet, kattintson a **tár** > **... Áruházzal társítása alkalmazást**.

    ![Windows Áruházbeli alkalmazás társítása](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. A varázslóban kattintson a **Tovább**gombra, jelentkezzen be Microsoft-fiókjával, **Foglalás új alkalmazás nevét**írja be az alkalmazás nevét, majd kattintson a **Foglalás**parancsra.

3. Az alkalmazás regisztrációs sikeres létrehozását követően jelölje be az új alkalmazás nevére, kattintson a **Tovább**gombra, és válassza a **társítani**. Az alkalmazás jegyzék hozzáadása a Windows áruházból regisztráció szükséges információkat.

7. A Windows Phone áruház alkalmazás projekt ugyanazt a korábban létrehozott regisztrációt használata a Windows áruházból letölthető ismételje meg az 1 és 3.  

7. Nyissa meg a [Windows fejlesztői központ](https://dev.windows.com/en-us/overview), jelentkezzen be Microsoft-fiókja, kattintson a **saját alkalmazások**az új alkalmazás bejegyzésre, majd bontsa ki a **szolgáltatások** > **leküldéses értesítések**.

8. A **leküldéses értesítések** lapon kattintson a **szolgáltatások Live-webhelyre** a **Windows leküldéses értesítést szolgáltatások (WNS) és a Microsoft Azure Mobile-alkalmazások**, és jegyezze fel az értékeket az **Csomag biztonsági AZONOSÍTÓK** és *aktuális* értéke az **Alkalmazás titkos**. 

    ![A fejlesztői központ App beállítása](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Az alkalmazás titkos és biztonsági AZONOSÍTÓK csomagot olyan fontos hitelesítő adatokat. Ne bárkivel megoszthatja a ezeket az értékeket és terjesztése őket az alkalmazást.
