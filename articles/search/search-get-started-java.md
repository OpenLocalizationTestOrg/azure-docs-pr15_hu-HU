<properties
    pageTitle="Első lépések az Azure keresési Java-ban |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Java a használják a programnyelv Azure a felhőben tárolt keresőalkalmazás létrehozásának módját."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-java"></a>Első lépések az Azure keresési Java-ban
> [AZURE.SELECTOR]
- [Portál](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Megtudhatja, hogyan hozhat létre egy egyéni Java keresőalkalmazás, amely a keresési élményt Azure keresése használja. Ebben az oktatóanyagban objektumot és a következő gyakorlatban használt műveletek létrehozása az [Azure keresési szolgáltatás REST API -t](https://msdn.microsoft.com/library/dn798935.aspx) használja.

Ez a példa futtatásához kell rendelkeznie az Azure keresési szolgáltatása, amelynek a jelentkezzen be az be az [Azure-portálon](https://portal.azure.com). Lásd: a [létrehozása az Azure keresési szolgáltatás a portálon](search-create-service-portal.md) részletes útmutatást.

Az alábbi szoftvereknek készítése és tesztelje a következő példában használt:

- [Holdas IDE Java EE fejlesztők számára](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Feltétlenül töltse le a EE verziót. A tartomány ellenőrzéséhez lépések egyikét szükséges szolgáltatás, amely csak a edition szerepel.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0-ban](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Az adatokkal

Minta alkalmazás az [Amerikai Egyesült Államok geológiai szolgáltatások (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), kattintson a állapota a Rhode sziget adatkészlet méretének csökkentése érdekében szűri adatait használja. Az adatok használjuk, amely visszaadja a környezet épületeket, például a kórházat és iskolai, valamint geológiai szolgáltatásokat, például a adatfolyamok tavakat és summits keresőalkalmazás össze.

Ennek az alkalmazásnak a **SearchServlet.java** program hozza létre, és betölti az index az [Indexelő](https://msdn.microsoft.com/library/azure/dn798918.aspx) szerkezetet, a szűrt USGS adatkészlet lekérése nyilvános Azure SQL-adatbázis használata. Előre megadott hitelesítő adatok és a kapcsolat adatait az online adatforrás szerepelnek a program kódot. Adathozzáférés, értelmez további beállítás szükség.

> [AZURE.NOTE] A adatkészlet csoportban a szabad árak réteg 10 000 dokumentum határértékén kapcsolatának azt alkalmazott szűrő. Ha használja a szabványos réteg, ezt a korlátot nem vonatkozik, és módosíthatja a méretét adatkészletet használandó kód. Az egyes árak réteg kapacitás kapcsolatos részletekért lásd: [korlátai és korlátozásokkal](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>A program fájlokról

Az alábbi lista bemutatja a fájlokat, amelyek megfelelnek a következő példában.

- Search.jsp: A felhasználói felületet biztosít az
- SearchServlet.java: Bemutat (hasonlóan egy MVC-vezérlő)
- SearchServiceClient.java: Fogópontok HTTP kérések
- SearchServiceHelper.java: Segítő osztály bemutat statikus
- Document.Java: Az adatmodellt tartalmaz
- Config.Properties: Beállítja a keresési szolgáltatás URL-CÍMÉT és az api-kulcs
- Pom.XML: Maven tesztelése függőség

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Keresse meg a szolgáltatás neve és az api-kulcs az Azure keresési szolgáltatás

Azure keresés minden REST API-hívások kell, hogy Ön megadja a szolgáltatás URL-CÍMÉT, és az API-ja kulcs. 

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Ugrás sávon kattintson a lista minden vásárlásáról az előfizetéshez tartozó kiépítéstől Azure keresési szolgáltatás **keresési szolgáltatás** parancsra.
3. Jelölje ki a használni kívánt szolgáltatás.
4. A szolgáltatás irányítópulton látni fogja a lényeges adatokkal csempéit és elérni a felügyeleti billentyűk kulcs ikonra.

    ![][3]

5. A szolgáltatás URL-CÍMÉT, és egy rendszergazda kulcs másolja. Szüksége lesz őket később, amikor ad hozzá őket a **config.properties** fájlt.

## <a name="download-the-sample-files"></a>A minta fájlok letöltése

1. Nyissa meg a [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) a Github.

2. Kattintson a **Letöltés ZIP-**, a .zip fájl lemezre mentése és kibonthatja a benne található fájlokat. Fontolja meg a fájlok kibontása, így azok könnyebben a projekt megtalálása a Java-munkaterületre.

3. A minta fájlok csak olvasható. Kattintson a jobb gombbal a mappa tulajdonságai, és törölje a jelet az írásvédett attribútum.

További fájl módosítását és a Futtatás kimutatások szemben a mappában található fájlok történik.  

## <a name="import-project"></a>Projekt importálása

1. Az Holdas, válassza a **fájl** > **importálása** > **Általános** > **Meglévő projektek munkaterületre**.

    ![][4]

2. **Jelölje ki a legfelső szintű címtár**keresse meg a minta fájlokat tartalmazó mappát. Jelölje ki a mappát, amely tartalmazza a .project mappát. A kijelölt elem, a projekt megjelennek a **projektek** listában.

    ![][12]

3. Kattintson a **Befejezés gombra**.

4. **Project Explorer** használatával megtekintheti és szerkesztheti a fájlt. Ha azt még nincs megnyitva, kattintson az **ablak** > **Nézet megjelenítése** > **Projektböngésző** vagy használata a helyi a megnyitáshoz.

## <a name="configure-the-service-url-and-api-key"></a>Állítsa be a szolgáltatás URL-CÍMÉT, és az api-kulcs

1. Kattintson duplán a **Project Explorer** **config.properties** a kiszolgáló nevét és az api-kulcsot tartalmazó konfigurációs beállításainak szerkesztése.

2. A cikkben leírt lépésekkel korábbi, ha Ön megtalálható a szolgáltatás URL-címe és az api-kulcs az [Azure-portálra](https://portal.azure.com), az értékek **config.properties**most fog kezdenek vonatkoznak.

3. Cserélje ki "API-ja kulcs" **config.properties**, a szolgáltatásbeli api-billentyűt. Ezután a szolgáltatás neve (a az URL-cím http://servicename.search.windows.net első része) helyettesíti ugyanannak a fájlnak "szolgáltatás neve".

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>A projekt, Szerkesztés és runtime környezetben konfigurálása

1. A Holdas a Projektböngésző, kattintson a jobb gombbal a Projekt > **Tulajdonságok** > **Project metszettel**.

2. Jelölje be a **dinamikus webhely modulban**, a **Java**és a **JavaScript**.

    ![][6]

3. Kattintson a **alkalmazása**gombra.

4. Válassza az **ablak** > **Beállítások** > **Server** > **futtatókörnyezet környezetekben** > **Hozzáadás.**.

5. Bontsa ki a Apache, és jelölje ki a korábban telepített Apache Tomcat kiszolgáló verzióját. A rendszer hogy 8-as verziójú telepítve van.

    ![][7]

6. A következő lapon adja meg a Tomcat telepítési könyvtár. Windows rendszerű számítógépen, a nagy valószínűséggel C:\Program Files\Apache szoftver Foundation\Tomcat lesz *verzió*.

6. Kattintson a **Befejezés gombra**.

7. Válassza az **ablak** > **Beállítások** > **Java** > **JREs telepített** > **hozzáadása**.

8. **Hozzáadása JRE**válassza a **Normál virtuális**.

10. Kattintson a **Tovább**gombra.

11. JRE meghatározása a JRE kezdőlap kattintson a **címtárban**elemre.

12. Nyissa meg azt a **Program Files** > **Java** , és válassza ki a JDK korábban már telepítette. Fontos, hogy a JDK jelöli ki a JRE.

13. Válassza ki a **JDK**JREs telepítve. A beállításokat az alábbi képernyőképen hasonlóan kell kinéznie.

    ![][9]

14. Tetszés szerint jelölje be az **ablakot** > **Webböngészőben** > **Az Internet Explorer** külső böngészőablakban az alkalmazás megnyitásához. Egy külső böngésző használatával lehetővé teszi a Web alkalmazás kényelmesebb.

    ![][8]

Most már befejezése a feladatokat. Ezután fogja összeállítása és a projekt futtatásához.

## <a name="build-the-project"></a>A projekt létrehozása

1. A Projektböngésző, kattintson a jobb gombbal a projekt nevét, és válassza a **Futtatás mint** > **maven tesztelése összeállítása...** beállítása a projekthez.

    ![][10]

8. Szerkesztheti a konfigurációban a kitűzött célok írja be a "tiszta telepítése", és válassza a **Futtatás**gombra.

Állapotüzenetek a konzol ablak kimeneti. Meg kell jelennie a projekt hibátlan beépített jelző összeállítása sikeres.

## <a name="run-the-app"></a>Futtassa az alkalmazást

Utolsó lépésként az alkalmazás egy helyi kiszolgáló futtatókörnyezet fog futni.

Ha egy kiszolgáló futtatókörnyezet megadott Holdas egyelőre még nem, kell ehhez először.

1. Bontsa ki a Project Explorer **Web tartalma**.

5. Kattintson a jobb gombbal a **Search.jsp** > **futtatása** > **futtassa a kiszolgálón**. Jelölje ki a Apache Tomcat kiszolgálót, és kattintson a **Futtatás**gombra.

> [AZURE.TIP] Ha az alapértelmezettől munkaterület tárolni a projektben használt, kell **Futtatni konfigurációs** indítási kiszolgálóhiba elkerülése érdekében a projekt helyre mutassanak módosítása. A Projektböngésző, kattintson a jobb gombbal **Search.jsp** > **Futtatás mint** > **Konfigurációk futtatni**. Jelölje ki a Apache Tomcat kiszolgálót. Kattintson az **argumentumokat**. Kattintson a **munkaterület** vagy a **Fájlrendszerben** beállítása a projekthez tartalmazó mappát.

Az alkalmazás futtatásakor meg kell jelennie egy böngészőablakban feltételek megadása kezeléséről a keresőmezőbe.

Várjon körülbelül egy percig a **keresési** szolgáltatás ideje hozhat létre, és töltse be az index gombra kattintás előtt. Ha egy HTTP 404-es hibaüzenet jelenik meg, csak kicsit már elteltével próbálja újra kell.

## <a name="search-on-usgs-data"></a>USGS adatok keresése

A USGS adatkészlet tartalmazó szempontjából fontos Rhode sziget állam rekordokat tartalmazza. Ha egy üres keresőmezőbe **Keresés** gombra kattint, kapja a felső 50 bejegyzéseket, az alapértelmezett.

Keresési kifejezés beírásával képet ad a keresőprogram parancsával egy üzenetet. Adjon meg egy területi nevet. "Roger Williams" Rhode sziget első elsősorban volt. Számos parkok, épületeket és iskolai azonos nevű vele.

![][11]

Is megpróbálhatnak e kifejezések egyikét:

- Pawtucket
- Pembroke
- lúd + modern

## <a name="next-steps"></a>Következő lépések

Ez a az első Azure keresési oktatóprogram Java és az USGS adatkészlet alapján. Az idő Ez az oktatóanyag további keresési funkciókat, érdemes lehet használni a egyéni megoldások az igazolni fogja kibővíthetjük.

Már tájékoztatatást Azure keresés, is használhatja az alábbi példa egy springboard további kísérletezés, az esetleg a [Keresés lap](search-pagination-page-layout.md)augmenting vagy [síklapos navigációs](search-faceted-navigation.md)végrehajtása. Is javíthatja a keresési eredmények lapja alapján számolja hozzáadásával, és kötegelés dokumentumok, így a felhasználók is lapozhat a az eredmények.

Új Azure keresés? Azt javasoljuk, hogy próbálja más oktatóanyagok fejlesztését megértéséhez mi hozhat létre. Keresse fel a [dokumentáció lap](https://azure.microsoft.com/documentation/services/search/) további információforrásokért. A hivatkozások megjelenítése a [Video- és oktatóanyag lista](search-video-demo-tutorial-list.md) eléréséhez további információt.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
