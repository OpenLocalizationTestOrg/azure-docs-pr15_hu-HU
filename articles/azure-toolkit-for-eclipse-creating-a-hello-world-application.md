<properties
    pageTitle="Azure egy helló világ Felhőszolgáltatásba Holdas létrehozása"
    description="Megtudhatja, hogyan hozhat létre egy egyszerű Helló, világ alkalmazást az Azure eszközkészlet használata Holdas."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Azure egy helló világ Felhőszolgáltatásba Holdas létrehozása #

Az alábbi lépésekkel megtudhatja, hogyan hozhat létre és Azure egy egyszerű JSP alkalmazás telepítése az Azure eszközkészlet használata Holdas. Egy JSP példa az egyszerűség, de nagyon hasonló lépéseket a következő lesz egy Java servlet megfelelő Azure környezetben illeti.

Az alkalmazás az alábbihoz hasonlóan fog kinézni:

![][ic600360]

## <a name="prerequisites"></a>Előfeltételek ##

* Egy Java Developer Kit (JDK), v 1,7 vagy újabb verziója.
* IDE Holdas Java EE fejlesztőknek Indigo vagy újabb verziója. Ez a <http://www.eclipse.org/downloads/>lehet letölteni.
* Java-alapú webkiszolgálóra vagy alkalmazáskiszolgáló, például Apache Tomcat, GlassFish, JBoss Application Server, rakodóhely vagy IBM® WebSphere® alkalmazás Server szabadság Core megoszlása.
* Az Azure előfizetéssel, amely a <http://azure.microsoft.com/pricing/purchase-options/>szerezhetők be.
* A Holdas Azure eszközkészlet. További tudnivalókért olvassa el a [Holdas az Azure eszközkészlete telepítése][]című témakört.

## <a name="to-create-a-hello-world-application"></a>A Helló, világ-alkalmazás létrehozása ##

Először lássuk először Java projekt létrehozásával.

*  Holdas, indítsa el és a menüt, kattintson a **fájl**, kattintson az **Új**gombra, és válassza a **Dinamikus a Project Web**. (Ha nem látható a **fájl**és az **Új**parancsra kattintást követően a rendelkezésre álló projektként felsorolt **Dinamikus webes projekt** végezze el a következőket: kattintson a **fájl**, kattintson az **Új**, kattintson a **projekt...**, bontsa ki a **webes**, kattintson a **Dinamikus webhely projekt**, kattintson a **Tovább**gombra.)
*  Ebben az oktatóanyagban céljából adjon nevet a projekt **MyHelloWorld**. (Biztosítására használja ezt a nevet, további lépéseket, ebben az oktatóanyagban várt HÁBORÚ fájljait MyHelloWorld neve). A képernyő jelenik meg az alábbihoz hasonló: ![][ic589576]
* Kattintson a **Befejezés gombra**.
* Belül Holdas a Project Explorer nézetben bontsa ki a **MyHelloWorld**. Kattintson a jobb gombbal a **Web tartalma**, kattintson az **Új**gombra, és válassza a **Fájl JSP**.
* Az **Új JSP fájlt** párbeszédpanelen nevezze el a fájlt **index.jsp**. Tartsa **MyHelloWorld/Web tartalma**, mint a szülőmappát, ahogy a következő:  ![][ic659262]
* A **JSP sablon kiválasztása** párbeszédpanel ebben az oktatóanyagban alkalmazásában válassza az **Új JSP fájl (html)** , és kattintson a **Befejezés gombra**.
* Ekkor megnyílik a index.jsp fájl Holdas, adja hozzá a szöveg dinamikusan **Helló, világ!** a meglévő belül `<body>` elemet. A frissített `<body>` tartalom meg kell jelennie a következőket:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Mentse a index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Azure, az alkalmazás a gyors és egyszerű módszer terjesztése ##

Amint kötötte Java webalkalmazás van, a következő helyi próbálja ki az közvetlenül az Azure felhőbeli is használhatja.

1. Kattintson a Holdas a Project Explorer **MyHelloWorld**.
1. A Holdas eszköztáron kattintson a **Közzététel** legördülő gombra, és kattintson a **Közzététele, Azure felhőalapú szolgáltatás**
    ![][publishDropdownButton]
1. Ha webkiszolgálón teszi közzé a az első alkalommal Azure alkalmazás, és nem hozott létre Azure környezetben projektben előtt az alkalmazást, az Azure környezetben projekt hozható létre meg automatikusan. Meg kell jelennie a következő kérdés, amely felsorolja a JDK csomag is és kiszolgáló, amely automatikusan telepítik az alkalmazásnak a futtatására.
    ![][ic789598]

    Ez a helyi megközelítés lehetővé teszi, hogy gyorsan és egyszerűen módszer annak ellenőrzésére az alkalmazás Azure-ban, anélkül, hogy egy adott kiszolgáló vagy az alapértelmezettől eltérő JDK konfigurálása. Ha elégedett az alapértelmezett beállításokat, kattintson a hajtsa végre a következő lépéseket **az OK gombra** .
    Jó helyen jár Ha meg szeretné változtatni a JDK vagy az alkalmazás használatához alkalmazáskiszolgáló azt is megteheti később az Azure környezetben project meg automatikusan létrehozott szerkesztésével, vagy most a **Mégse** gombra, és olvassa el a ebben az oktatóanyagban **kapcsolatos Azure környezetben projektek szakaszban** .
1. A **Közzététel az Azure** párbeszédpanelen:
    1. Ha nincs még az **előfizetés** listában válassza az előfizetések, adatok importálása az előfizetés az alábbi lépésekkel:
        1. Kattintson a **Közzététel - fájl importálása**gombra.
        1. Az **Előfizetés-adatok importálása** párbeszédpanelen kattintson a **Közzététel-BEÁLLÍTÁSOKAT tartalmazó fájl letöltése**. Ha még nem jelentkezik az Azure-fiókjába, a rendszer kéri, jelentkezzen be a. Ezután kérni fogja az Azure mentés beállításfájl közzététele. Mentse a helyi számítógépre.
        1. Továbbra is az **Előfizetés-adatok importálása** párbeszédpanelen kattintson a **Tallózás** gombra, jelölje ki az előző lépésben helyi meghajtóra mentett a Közzététel beállításai fájlt, és kattintson a **Megnyitás**gombra. A képernyő a következőhöz hasonlóan kell kinéznie: ![][ic644267]
        1. Kattintson az **OK gombra**.
    1. **Előfizetés**válassza ki azt az előfizetést, amellyel meg szeretné a telepítéshez.
    1. **Tárterület-fiókot**jelölje ki a használni kívánt tárterület-fiókot, vagy kattintson az **Új** tároló új fiók létrehozása gombra.
    1. A **szolgáltatás neve**jelölje ki a használni kívánt felhőalapú szolgáltatást, vagy kattintson az új felhőalapú szolgáltatás hozzon létre **Új** gombra.
    1. **Cél-OS**jelölje ki a telepítéshez használni kívánt az operációs rendszer verziója.
    1. **Cél környezetben**ebben az oktatóanyagban alkalmazásában jelölje ki a **fejlesztői**. (Ha készen áll a termelési webhely telepítéshez használni, úgy kell megadnia a **termelési**.)
    1. Nem kötelező: Győződjön meg arról, hogy **írja felül a korábbi telepítési** van bejelölve, ha azt szeretné, hogy az új telepítési automatikusan felülírja az előző telepítési. Ha ezt a beállítást választja, lehetővé teszi a nem élmény "409 ütközés" problémák ugyanazon a helyen közzétételekor.
        Figyelje meg, hogy a **Közzététel az Azure** párbeszédpanel szakaszt tartalmaz, a **Távoli hozzáférés**. Alapértelmezés szerint nincs engedélyezve a távoli hozzáférés, és azt nem teszi lehetővé, hogy ebben a példában. Távoli hozzáférés engedélyezése a felhasználónevet és jelszót használja, ha távolról naplózás a írná be. Távoli hozzáférés kapcsolatos további tudnivalókért olvassa el a [Holdas az Azure-telepítések távoli hozzáférés engedélyezése][]című témakört.
        A **Közzététel az Azure** párbeszédpanel jelenik meg az alábbihoz hasonló: ![][ic719488]
1. Kattintson a **Közzététel** a fejlesztői környezet közzététele.
    Végezze el a teljes építés kéri, kattintson az **Igen**gombra. Ez eltarthat néhány percig, amíg az első build.
    A többlapos Holdas nézetek szakaszt az **Azure tevékenységnapló** jeleníti meg.
    ![][ic719489]
   Használhatja a napló, valamint az **konzol** nézetben, a telepítés állapotának megtekintéséhez. Egy alternatív megoldás, hogy jelentkezzen be az [Azure Kezelőportálja segítségével][], a **Cloud Services** szakasz használata a Lync-állapota.
1. A telepítési sikeresen telepíti, amikor az **Azure tevékenységnapló** jelennek meg a **közzétett**állapotát. Kattintson a **közzétett**, az alábbi képen látható módon, és a böngészőben megnyílik a üzembe egy példánya.
    ![][ic719490]

Mivel ez volt a fejlesztői környezet a telepítést, a DNS-név lesz az űrlap http://&lt;*globálisan egyedi azonosítója*&gt;. cloudapp.net, és az URL-címe lesz tartalmaz, a DNS-nevét, valamint az alkalmazás toldalékot. Ha például http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (A **MyHelloWorld** része a kis-és nagybetűk.) Ha a Azure Platform Kezelőportálja (belül az adatkezelési portál Cloud Services részét) a telepítési nevére kattint, a DNS-neve is megjelenik.

Bár ez segédlet volt a fejlesztői környezet telepítés, egy gyártási telepítés követi ezeket a lépéseket, kivéve a **Közzététel az Azure** párbeszédpanelen belül, és válassza a **gyártási** **előkészítési** helyett a **cél környezetben**. A telepítés termelési tetszőlegesen megválasztott átmeneti használt globálisan egyedi azonosítója helyett a DNS-név alapján URL-cím eredményez.

>[AZURE.WARNING] Ezen a ponton telepítette a az Azure-alkalmazásokat a felhőbe. Azonban a folytatás előtt látja, hogy a telepített alkalmazások, még akkor is, ha nem fut, továbbra is az előfizetéshez tartozó számlázható időt felmerülés. Ezért nagyon fontos az Azure előfizetéséből törli a felesleges telepítések.

## <a name="about-azure-deployment-projects"></a>Azure környezetben projektek ##

Azure egy vagy több Java alkalmazások telepítéséhez a projektben szereplő Azure környezetben van szükség. Ez a "csomag", az alkalmazások kell kell Azure tehető közzé a csomagolni szerepet.

Az alkalmazások információkért mellett az Azure környezetben projekt is tartalmaz információkat más fő összetevői a központi telepítés legfontosabb: az alkalmazás server tároló futtatásához a web app a és a Java futtatókörnyezet futtatásához. Azure Java szükséges és választhat Java alkalmazáskiszolgálók nagyobb kijelölt támogatja.

Bár a használja az alábbi példa szemléltetési célból nagyban megkönnyíti, Azure környezetben projektben is tartalmazhat, egyéb fontos konfigurációs adatok, amely lehetővé teszi, hogy hozzon létre szinte tetszőlegesen összetett, méretezhető, könnyen hozzáférhető, több szálon felhőszolgáltatások-alkalmazásokkal. **Munkamenet affinitás ("öntapadó munkamenetek")**, **gyors gyorsítótárazás**, **távoli hibakeresés**, **mert SSL**, **tűzfal/port útválasztás**, **távelérési**és számos más hatékony funkciók engedélyezése

Ha befejezte az előző szakaszban az ebben az oktatóanyagban ("a üzembe Azure, az alkalmazás a gyors és egyszerű módszer"), akkor Azure környezetben új projektet a Project Explorer létrehozza a automatikusan megjelenik, és "**MyHelloWorld_onAzure**" nevű.

Sikerült is elindítását követően ez az oktatóanyag első Azure környezetben üres projekt létrehozása saját magát, és töltse fel a rá az alkalmazásokat. Egy hosszabb folyamat, de, biztosítva további beállítási lehetőségekre a kiindulási konfiguráció az elejéről.

Azure környezetben új projekt létrehozása üres lapból, kattintson az **Új Azure környezetben projekt** gombra ![][ic710876].

Függetlenül attól, hogy Ön egy már meglévő Azure környezetben projekten dolgozik, vagy létre tudja hozni az alapoktól, Ön annak konfigurációs beállítások és -összetevők, például a JDK vagy az application server egyaránt módosíthatnak egyszerűen bármikor.

A JDK, vagy az application server vagy az alkalmazás lista Azure környezetben projekt módosítása:

1. Bontsa ki a Project Explorer (pl. **MyHelloWorld_onAzure**) projekt csomópontját
2. Kattintson a jobb gombbal a **WorkerRole1**
3. Bontsa ki az **Azure** almenü, kattintson a helyi menü
4. Kattintson a **kiszolgáló konfigurálása**

Függetlenül attól, hogy elkezdett kiszolgáló konfigurációs lépések Azure környezetben projekt szerkesztésével, mint a fenti vagy hoz létre egy újat az alapoktól, látni fogja az azonos típusú, lehetővé téve a JDK, a kiszolgáló és az alkalmazás összetevők konfigurálását párbeszédpanelek. A [kiszolgáló beállításainak][] cikke megtudhatja, hogy hogyan módosíthatja az adott párbeszédpanelek JDK, az application server módosítása, illetve hozzáadása és eltávolítása alkalmazások környezetben, például beállításai.

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Csak Windows: a számítási irányító az alkalmazás telepítése ##

>[AZURE.NOTE] Az Azure irányító csak érhető el Windows rendszeren. Átugorja ezt a szakaszt, ha nem a Windows operációs rendszert használ.

Ha hozott létre korábban, azaz implicit módon ismertetett lépéseket követve Azure környezetben projekt közzétéve Azure JDK és az alkalmazás az alkalmazás kiszolgálók van konfigurálva a felhőben, de nem helyi emulációs. A projekt előkészítése a helyi irányító tesztelésére, kövesse az alábbi lépéseket:

1. Kattintson a Holdas a Project Explorer **MyHelloWorld_onAzure**.
1. Kattintson a jobb gombbal a **WorkerRole1**.
1. Az **Azure** almenü, kattintson a helyi menü kibontása.
1. Kattintson a **kiszolgáló konfigurálása**.
1. A **JDK** lapon jelölje be, ha az eszközkészlet előre beállított egy alapértelmezett helyi JDK meg. Ha nem, vagy ha meg szeretné változtatni a felvett alapértelmezett beállításait, győződjön meg arról, hogy a **a JDK az Ez az útvonal helyileg teszteléshez használata** jelölőnégyzet be van jelölve, és ki a használni kívánt JDK telepítési helye van megadva. Ha szeretné megváltoztatni, kattintson a **Tallózás** gombra, és használja a Tallózás gombra a vezérlőt, jelölje ki a használandó JDK címtár helyét.
1. Kattintson a **kiszolgáló** fülre.
1. A párbeszédpanel alján **helyi kiszolgálói elérési út** mezőbe írja be az elérési útját a helyileg telepített kiszolgáló, amely megfelel a típus és főverzió a kijelölt tetején látható a párbeszédpanelen a **kiszolgáló az ilyen típusú Deploy** jelölőnégyzet a kiszolgáló. Szeretne más típusú vagy az application Server főverzió használata, ha először módosítása a kijelölés csoportban a jelölőnégyzetet.
1. Kattintson az **OK gombra**.
1. A Holdas eszköztáron kattintson az **Azure irányító a Futtatás** gombra, ![][ic710879]. Ha nincs engedélyezve a **Azure irányító a Futtatás** gombra, győződjön meg arról, hogy **MyHelloWorld_onAzure** Holdas a Project Explorer van kijelölve, és gondoskodjon arról, hogy Holdas a Projektböngésző a fókusz a jelenlegi ablak legyen. Ez először elindítja a projekt teljes építés, és indítsa újra a számítási irányító Java webes levelezőprogramból. (Megjegyzés, attól függően, hogy a számítógép teljesítményt nyújt, első összeállítása között néhány percet néhány másodpercbe telhet, de ezt követő épít fog kapni gyorsabb.) Az első build lépés befejezése után kéri a Windows felhasználói fiók vezérlő (fiókok) engedélyezése ezzel a paranccsal a számítógépen végezze el a módosításokat. Kattintson az **Igen**gombra.

>[AZURE.IMPORTANT] Ha nem látható a felhasználói fiókok felügyelete, jelölje be a Windows tálca a felhasználói fiókok felügyelete ikont, és kattintson rá. Előfordul, hogy a felhasználói fiókok felügyelete üzenet nem jelenik meg a legfelső ablak, de csak a tálcán ikonként látható.

1. Vizsgálja meg a számítási irányító határozza meg, hogy vannak-e a projekt problémák lépnek fel a felhasználói felület kimenetét. Attól függően, hogy a üzembe tartalmát az alkalmazás teljesen elindításának a számítási irányító néhány percig is eltarthat.
1. Indítsa el a böngésző és az URL-cím használata `http://localhost:8080/MyHelloWorld` címként (a `MyHelloWorld` az URL-cím része kis-és nagybetűket). Meg kell jelennie a MyHelloWorld alkalmazás (kimenetének index.jsp), az alábbi képhez hasonló: ![][ic589579]

Ha le szeretné állítani az alkalmazás, a számítási irányító Holdas eszköztár futását készen áll a gombra **Alaphelyzetbe Azure irányító** , ![][ic710880].

## <a name="to-delete-your-deployment"></a>A telepítési törlése ##

Győződjön meg arról, hogy Holdas a Project Explorer **MyHelloWorld_onAzure** be van jelölve a üzembe belül az Azure eszközkészlete Holdas törléséhez, Holdas Projektböngésző rendelkezzen a fókusz, és az **e verzió közzétételének megszüntetése** gombra, majd kattintson az aktuális ablak ![][ic710883], a Holdas eszköztáron. (Lehet hajtsa végre az azonos művelet **MyHelloWorld_onAzure** Holdas a Projektböngésző a jobb gombbal kattintva **Azure** majd **Azure felhőből Undeploy**.) Ekkor megjelenik az **Azure projekt közzétételének visszavonása** párbeszédpanel.

![][ic719491]

Jelölje ki a előfizetéssel, és felhőalapú szolgáltatást, amely tartalmazza a üzembe, jelölje ki a törölni kívánt példányban, és kattintson az **e verzió közzétételének megszüntetése**gombra.

(Ahelyett, hogy az eszközkészlet használata a telepítési törlése környezetbe a **Cloud Services** szakaszában az Azure adatkezelési portált: Nyissa meg azt a üzembe, jelölje ki azt, és kattintson a **Törlés** gombra. Ezzel leállítja, és a telepítés, majd törölje. Ha csak meg szeretné szüntetni a telepítést, és nem törli, kattintson a **Leállítás** gomb helyett a **Törlés** gombra, de említettük, ha a telepítő nem törli, mint számlázható díjak továbbra is szerepelni fognak felmerülés a telepítéshez, még akkor is, ha le van állítva).

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[Az Azure eszközkészlet Holdas telepítése][] 

[Az Azure eszközkészlete Holdas újdonságai][]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Kezelőportálja segítségével]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[Távoli hozzáférés engedélyezése a Holdas Azure telepítésekhez]: http://go.microsoft.com/fwlink/?LinkID=699538
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546
[Konfigurációs tulajdonságai]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Az Azure eszközkészlete Holdas újdonságai]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png
