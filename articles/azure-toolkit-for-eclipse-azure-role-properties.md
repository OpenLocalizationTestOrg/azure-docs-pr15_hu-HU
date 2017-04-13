<properties
    pageTitle="Azure szerep tulajdonságai"
    description="Megtudhatja, hogy miként használja az Azure eszközkészlet Holdas az Azure szerepkör beállításainak konfigurálása."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->

# <a name="azure-role-properties"></a>Azure szerep tulajdonságai #

Azure szerepköre különböző beállításait beállítható, hogy az Azure eszközkészlete Holdas belül.

## <a name="configuring-azure-role-properties"></a>Azure szerepkör tulajdonságainak konfigurálása ##

Az Azure szerepkör tulajdonságainak beállítása a tulajdonság párbeszédpanelt a dolgozó szerepkör alkalmazásával. Nyissa meg a szerepkör a helyi menü Holdas a Project Explorer ablakban, és válassza az **Azure** almenük. (Ha nem látja a szerepkör a Project Explorer Holdas, bontsa ki az Azure projektet a Project Explorer.)

![][ic789599]

Ez a témakör megkötésekről különböző tulajdonságokat állíthat be a **Tulajdonságok** párbeszédpanelt. Figyelje meg, hogy hány tulajdonságok automatikusan kitölti Azure környezetben új projekt létrehozásakor.

A következő tulajdonságlapokat Azure szerepkörök érhetők el.

* [Virtuális gép tulajdonságai](#virtual_machine_properties)
* [Gyorsítótár-tulajdonságok](#caching_properties)
* [Tanúsítványok tulajdonságai](#certificates_properties)
* [Összetevők tulajdonságai](#components_properties)
* [Hibakeresési tulajdonságai](#debugging_properties)
* [Végpontok tulajdonságai](#endpoints_properties)
* [Környezet változók tulajdonságai](#environment_variables_properties)
* [Terheléselosztás / munkamenet affinitás (más néven "öntapadó munkamenetek") tulajdonságai](#session_affinity_properties)
* [Helyi tárolási tulajdonságai](#local_storage_properties)
* [Konfigurációs tulajdonságai](#server_configuration_properties)
* [SSL, mert tulajdonságai](#ssl_offloading_properties)
    
<a name="virtual_machine_properties"></a>
### <a name="virtual-machine-properties"></a>Virtuális gép tulajdonságai ###

Nyissa meg a helyi menü a szerepkör Holdas a Project Explorer ablakban kattintson a **Azure**, és kattintson a **Tulajdonságok parancsot**, és fog van, az azt jelenti, hogy virtuális gép méretének módosítása, és példányok száma is módosíthatja az alábbi képen látható módon.

![][ic719499]

>[AZURE.NOTE] Csak Windows: példányainak száma értéke 1-nél nagyobb és is beállíthatja az application server, az eszközkészlet lehetővé teszi a irányító ettől a beállítástól függetlenül futhat csak 1 szerepkör-példány. Az ütközések elkerülése érdekében port kötés az eltérő server-példányok között (Ha például minden kötést létrehozni a porttal 8080 próbál) futtatásakor ugyanazon a számítógépen. A kívánt példányok száma beállítás megmarad, de magától életbe csak akkor, amikor rendszerbe állítják a felhőben.

<a name="caching_properties"></a> 
### <a name="caching-properties"></a>Gyorsítótár-tulajdonságok ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és válassza a **gyorsítótár**. Ez a párbeszédpanel belül engedélyezheti elnevezett közös található memcache-kompatibilis gyorsítótárát lehetővé teszi a webalkalmazások gyorsíthatja.

![][ic719483]

A tulajdonságlap **gyorsítótárazás** belül a következő globális beállításokat is megadhat:

* dokumentumok közös található gyorsítótárazás van engedélyezve van-e.
* a memória százalékértékként használt gyorsítótár méretét.
* a mentési gyorsítótár állapotát, ha az alkalmazás egy felhőalapú szolgáltatásba, vagy nincs fut, ha nem szeretné menteni a gyorsítótár állapotának tároló fiók nevére. (A tárhely fiók neve nem használják a számítási irányító az alkalmazás futtatásakor.) Ha a tárterület-fiók nevét (Ez az alapértelmezett) **(automatikus)** , a gyorsítótár konfigurációs automatikusan használnak ugyanazzal a tárterület-fiókkal az adott, válassza a **Közzététel az Azure** párbeszédpanelen.

>[AZURE.NOTE] A **(automatikus)** beállítás a kívánt hatás csak akkor, ha teszi közzé a üzembe a Holdas eszközkészlet használata közzétételi varázsló lesz. Ha inkább tesz közzé a .cspkg fájlt kézzel egy külső mechanizmusa, például az [Azure Kezelőportálja][]lesz a telepítő nem működik megfelelően.

A következő párbeszédpanelen a gyorsítótár tulajdonságainak jeleníti meg.

![][ic719501]

* **Neve:** A közös található gyorsítótár neve.
* **Port száma:** A port száma a gyorsítótár használható.
* **Jelszólejárati házirendjének:** Az alábbi értékek közül, amely meghatározza, amikor egy kulcsot a gyorsítótárban lévő lejár.
    * **Abszolút:** A kulcs lejár, az idő **perc élő** által megadott elérésekor.
    * **NeverExpires:** A kulcs nincsenek elévülési időt.
    * **SlidingWindow:** A kulcs lejár, ha ez nem nyitottak **élő perc**; által meghatározott időtartamra minden alkalommal, amikor azt érhető el, a lejárat óra alaphelyzetbe állítása.
* **Élő perc:** Az aktív, az Elévülési házirend vonatkozik memcached kulcsnak percek maximális számát.
* **Magas elérhetősége a különböző szerepkör-példányok replikált biztonsági másolat:** Ha engedélyezve van, segít magas elérhetősége felhasználásával replikált másik szerepkör-példányok a biztonsági másolatok. Figyelje meg, hogy legalább két szerepkör-példányok gyakorlatilag kell lennie a telepítéshez ehhez a szolgáltatáshoz való használatra.

Új gyorsítótár hozzáadásához kattintson a **Hozzáadás** gombra, a **gyorsítótár** tulajdonságlapon, és megnyílik a **Nevű gyorsítótár beállítása** párbeszédpanel megjelenítéséhez. Adjon meg értéket a fent bemutatott tulajdonságok.

Ha módosítani szeretné egy névvel ellátott gyorsítótár, jelölje be a gyorsítótár, és kattintson a **Szerkesztés** gombra a **gyorsítótár** tulajdonságlapon. Egy párbeszédpanel nyílik meg, amely lehetővé teszi a gyorsítótár tulajdonságainak módosítása. Nyomja le **az OK** gombra a gyorsítótár értékek mentéséhez.

A gyorsítótár törléséhez jelölje ki a gyorsítótár és kattintson az **Eltávolítás** gombra, a **gyorsítótár** tulajdonságlapon, és kattintson az **Igen gombra** kattintva erősítse meg a törlést.

További információt a gyorsítótár használatát megtudhatja, [hogy miként Co-located gyorsítótárazás használata][].

<a name="certificates_properties"></a> 
### <a name="certificates-properties"></a>Tanúsítványok tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és kattintson a **tanúsítványok**.

![][ic710964]

Ez a párbeszédpanel belül felvehet, és távolítsa el a Holdas projekt által hivatkozott tanúsítványok. Megjegyzés: a tanúsítványok, az itt felsorolt nem automatikusan tárolt bármely Java keystore belül, és ezért nem érhetők el automatikusan bármely való használatra Java-alkalmazások belül. Ezek csak regisztrált az Azure, hogy azok is lehet előre betöltött használata tanúsítvány a virtuális gépeken futó a üzembe tárolására, és ezt követően használja a Windows rendszerbe más Windows szoftverrel. A csak az eszközkészlet a tanúsítványok, így a **tanúsítványok** párbeszédpanel hivatkozott használó funkciója jelenleg, [Mert az SSL][], miatt az Internet Information Services (IIS) és az alkalmazás kérése Routing (ARR), ily módon elérhetők legyenek a megfelelő hitelesítést igénylő támaszkodik.

Amikor rendszerbe állítják a projekt a közzétételi varázsló segítségével Azure, a rendszer kéri, mutasson a megfelelő annak érdekében, hogy automatikusan töltse fel őket a Azure szolgáltatás, de csak ha az azok nem lett feltöltött van korábban ezek a tanúsítványok jelszavukat, személyes információ Exchange (PFX) fájlok.

<a name="components_properties"></a> 
### <a name="components-properties"></a>Összetevők tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és kattintson az **összetevők**. Ez a párbeszédpanel belül, akkor az azt jelenti, hogy hozzáadása, módosítása, vagy távolítsa el a szerepkör-összetevők, valamint feldolgozási módjának sorrendjének módosítása.

![][ic719502]

A összetevők szolgáltatás lehetővé teszi, hogy függőségek az Azure környezetben projekthez hozzáadni, például Java alkalmazás projektek, a speciális fájlokat és a végrehajtható parancssori utasítások a telepítéshez szükséges.

Az egyes összetevők adhatja meg:

* Amennyiben a összetevő importálása az Azure telepítés projekthez, amikor elkészült, lépés.
* A lépés kell venni az Azure felhőben összetevő telepítésekor.

>[AZURE.NOTE] Összetevő-fájlok vagy parancssorokat megadásakor ne feledje, hogy a üzembe közzéteszi Windows virtuális géphez, így kell, hogy érvényes Windows-alapú operációs rendszer egyéni lépéseket. 

Összetevők van, a következő tulajdonságokat:

* **Importálása:** Ez a módszer azt jelzi, hogy hogyan az összetevő importált a projectbe, ha a projekt épül. A következő értékek egyike lehet:
    * **Másolás:** A összetevő program átmásolja a szerepkör **approot** könyvtárban **az** tulajdonságával megadott helyi elérési útját.
    * **EAR:** A összetevője egy Java vállalati archívum (EAR) importálni az alkalmazás **a** tulajdonságban meghatározott helyi elérési útját a vállalati projekt. (Ez észlelhető automatikusan által az adott helyen a projekt jellegét alapján eszközkészlet).
    * **JAR:** A összetevő egy Java archívum (üveg), és importálja a helyi elérési útját **a** tulajdonságban meghatározott Java projekt. (Ez észlelhető automatikusan által az adott helyen a projekt jellegét alapján eszközkészlet).
    * **Nincs:** Nem történik művelet a komponens importálásához. Ez a alkalmazható, amikor már megtalálható a szerepkör **approot** címtár tekinti a összetevő, vagy ha a összetevője pusztán egy végrehajtható parancssori utasítást, **mint** tulajdonságában **végrehajtási**esetén a **központi telepítés** módszerrel meghatározott.
    * **HÁBORÚ:** A összetevő Java webes alkalmazás archívum (HÁBORÚ), és importálja **a** tulajdonságban meghatározott helyi elérési útját a dinamikus webhely projekt. (Ez észlelhető automatikusan által az adott helyen a projekt jellegét alapján eszközkészlet).
    * **zip:** A összetevő zip-fájl, és importálja a címtár vagy **tulajdonságban a** megadott fájl tömörítéséről.
* **A:** Forrás elérési utat a helyi számítógépen, a mappa vagy fájl, amely az elemet vagy elemeket a példányban szeretné importálni. Ez a tulajdonság Windows környezeti változók használható. Minden importálható összetevő lesznek importálva a szerepkör **approot** címtár, amikor a projekt épül.
    
    Figyelje meg, hogy a letöltés összetevő telepítésének, a felhőbe (nem a számítási irányító) telepítésekor lehetősége van. Összetevő hozzáadásáról alatti kapcsolódó információk megtekintése:    
    
* **As:** A fájlnév a melyik a összetevő fog importálja a szerepkör **approot** címtár, és végül a Azure felhőben rendszerbe. A tulajdonság értékét hagyja üresen neve továbbra is a ugyanaz, mint a helyi számítógépen. (Végrehajtható összetevők, ez azt jelenti, hogy azokat, akiknek **Deploy** módszer beállítása **végrehajtási**, ez lehet egy tetszőleges Windows parancssori utasítást.)

    >[AZURE.IMPORTANT] Szóközöket használ ezt az értéket, ha azok máshogyan kell kezelni deploy módtól függően. Ha a központi telepítés módja **végrehajtási**, szóközöket értelmezi parancssori argumentum elválasztóként, és a fájlnevet részeként nem. Minden más módszerekkel üzembe, szóközök értelmezi részeként a fájl nevét.
    
* **Üzembe:** Módszer, amely jelzi a telepítési indításakor a összetevő alkalmazott művelet. A következő értékek egyike lehet:
    * **Másolás:** **A tulajdonság** által megadott rendeltetési hely elérési másolja az összetevőt.
    * **végrehajtási:** A összetevő-végrehajtható Windows parancssori utasítás végrehajtása a környezetben, amikor a telepítő elindul **a** tulajdonság által megadott elérési út nem.
    * **Nincs:** A telepítési indításakor nem tesz a összetevő érvényes.
    * **zip:** **A tulajdonság** által megadott rendeltetési hely elérési kicsomagolt összetevője. Ez a módszer csak akkor, amikor az **importálási** tulajdonság értéke **zip**érhető el.
* **To:** A virtuális gépen, ahol a összetevő rendszerbe állított cél elérési útja. Windows környezeti változók használható ez a tulajdonság, pedig fájl elérési utak **approot**viszonyított.
    
Új összetevő hozzáadásához kattintson az **összetevők** tulajdonság lapon a **Hozzáadás** gombra, és megnyílik egy **Azure szerepkör összetevő** párbeszédpanel. Adjon meg értéket a fent bemutatott tulajdonságok. 

A következő példa hozzáadásának új HÁBORÚ összetevőt.

![][ic719503]

Telepítésekor a felhőbe (nem a számítási irányító), ha szeretné telepíteni az összetevő letöltéssel, győződjön meg arról, hogy **a felhőben, helyett a csomagban, beleértve az üzembe** be van jelölve. Ha az Azure tárterület-fiókját, jelölje ki a tárhely fiók a **tárterület-fiók** legördülő listában (is a hivatkozásra kattintott **fiókok** módosítása, hogy mi az a lista), amelyek részben kitölti az **URL-cím** mezőbe, és az URL-cím fennmaradó részének majd töltse ki a letölteni kívánt. Ha nem szeretné, hogy Azure tároló használatára, a **tárhely fiók** legördülő listából válassza a **(nincs)** , és írja be az URL-cím és az **URL-címe** mezőben a összetevőt. Adja meg az alábbi lehetőségek közül:

* **Másolás:** A letöltés összetevő **Könyvtárának az** elérési útját által megadott rendeltetési hely elérési másolja a program.
* **azonos:** **A letöltés Deploy** mint **csomagból Deploy**használt azonos módszer.
* **zip:** A letöltés összetevője kicsomagolt **Könyvtárának az** elérési útját által megadott cél helyre.

Összetevő módosításához jelölje ki az összetevőt, és kattintson a **Szerkesztés** gombra a **összetevők** tulajdonságlapon. A párbeszédpanel lehetővé teszi, hogy az összetevő-tulajdonságok módosítása nyitja meg. Nyomja le **az OK** gombra az összetevő értékek mentéséhez.

Összetevő törléséhez jelölje ki az összetevőt, és kattintson az **összetevők** tulajdonságlap **eltávolítása** gombra, és kattintson az **Igen gombra** kattintva erősítse meg a törlést.

Összetevők feldolgozása a felsorolás sorrendjében. A **Fel** és **Le** gomb segítségével sorba.

>[AZURE.NOTE] A kiszolgáló konfigurációs funkció, valamint összetevők támaszkodik. Azokat a elemeket nem eltávolítja és nem szerkeszthetők a megfelelő beállítása törlése nélkül. Kéri arról, hogy amikor megpróbál ilyen összetevők szeretne változtatásokat végezni.

<a name="debugging_properties"></a> 
### <a name="debugging-properties"></a>Hibakeresési tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és válassza a **Hibakeresés**. Ez a párbeszédpanel belül, engedélyezése és letiltása a távoli hibakeresés lehetősége van, valamint hibakeresési konfigurációk, létrehozása, az alábbi képen látható módon.

![][ic719504]

Kapcsolódó információ hibakeresési [Azure alkalmazások hibakeresési Holdas][]témakörben talál.

<a name="endpoints_properties"></a> 
### <a name="endpoints-properties"></a>Végpontok tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és válassza a **Végpontok**. Ez a párbeszédpanel belül van az azt jelenti, hogy hozzon létre egy végpontot, valamint szerkesztése vagy eltávolítása gombra zárólap, az alábbi képen látható módon.

![][ic719505]

Zárólap hozzáadásához kattintson a **Hozzáadás** gombra, a **Végpontok** tulajdonságlapon, és megnyílik egy **Végpont hozzáadása** párbeszédpanel.

![][ic710897]

Írjon be egy nevet a végpontot, válassza ki a ( **beviteli**, **belső**vagy **InstanceInput**), és adja meg a nyilvános és titkos portot. Nyomja le **az OK gombra** az új végpont értékeket menti.

Végpont típusától függően előfordulhat, hogy használhatja port tartományok az alábbi képlettel történik:

* Beviteli példány zárólap a nyilvános port lehet egy porttartományt (például a **2000-2010**) és a személyes portja rögzített érték.
* Belső végpont a nyilvános port nem használják, a személyes port lehet tartomány, és üresen vagy a csillag azt jelzi, hogy beállítása Azure automatikusan állítja be.
* Beviteli végpontok a nyilvános port csak lehet egy rögzített értéket, és a személyes port is, ha a rögzített értékű, vagy a bal oldali üres vagy csillag meg, hogy automatikusan Azure úgy van beállítva.

Ha egyetlen portszámot tartomány helyett használandó, hagyja meg a szövegdobozt a végén a tartomány üres.

Az automatikus, ha beállított portokat meg kell határoznia, hogy melyik portot használja ténylegesen futtatókörnyezet során, az alkalmazás használható az Azure Service futtatókörnyezet API-hoz, az [összefoglaló com.microsoft.windowsazure.serviceruntime csomag][]ismertetését.

Hogyan példány beviteli végpontokat is használható több példány telepítés hibakeresési segítséget talál további [hibakeresési egy szerepkör-példányt, több példány környezetben][].

Zárólap módosításához válassza ki a végpontot, és kattintson a **Szerkesztés** gombra, a **Végpontok** tulajdonságok lapon. A párbeszédpanel lehetővé teszi a végpontjának neve, a típus és a nyilvános és titkos portokat nyitja meg. Nyomja le **az OK** gombra a módosított végpont értékek mentéséhez.

Zárólap törlése:, válassza ki a végpontot, és kattintson az **Eltávolítás** gombra, a **Végpontok** tulajdonságlapon, és kattintson az **Igen gombra** kattintva erősítse meg a törlést.

Annak érdekében, hogy a megfelelő módon adja meg (például mert gyorsítótárazás, távoli hibakeresés, munkamenet affinitás vagy SSL) a szerepkör a felhasználó által engedélyezett funkciók közül egyesek, az eszközkészlet automatikusan konfigurálja együtt a felhasználó által definiált végpontok szereplő speciális végpontok. Az eszközkészlet megakadályozza, hogy a felhasználó szerkesztése vagy törlése az ilyen automatikusan létrehozott végpontok mindaddig, amíg a társított engedélyezve van.

<a name="environment_variables_properties"></a> 
### <a name="environment-variables-properties"></a>Környezet változók tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és válassza a **Környezeti változók**. Ez a párbeszédpanel belül van az azt jelenti, hogy hozzon létre egy környezeti változóba, valamint módosítása vagy eltávolítása egy környezeti változóba a következő képen látható módon.

![][ic719506]

Környezet változók használhatók az indítási parancsfájl, a szerepkör indításakor.

>[AZURE.NOTE] Környezet változók megadásakor ne feledje, hogy a üzembe közzéteszi Windows virtuális géphez, így a környezeti változók érvényes Windows-alapú operációs rendszer kell lennie.

Éppen érhető el, a szerepkör indításakor környezeti változó példaként hozzon létre új környezeti változó kattintson a **Hozzáadás** gombra. Az alábbiakban látható **MyRoleVersion** nevű környezeti változó éppen létrehozott és az érték **1.0**kiosztani.

![][ic659268]

A jsp kód, megjelenítheti a érték használata a `System.getenv` módszer:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Az alkalmazás futtatásakor így a kimenet:

![][ic552233]

Egy környezeti változóba módosításához jelölje ki a környezet változót, és kattintson a **Szerkesztés** gombra a **Környezeti változók** tulajdonságlapon. Egy párbeszédpanel nyílik meg, amely lehetővé teszi a környezet változó tulajdonságainak módosítása. Nyomja le **az OK gombra** a környezet változóértékkel mentéséhez.

Környezeti változó törléséhez jelölje ki a környezet változót, és kattintson az **Eltávolítás** gombra, a **Környezeti változók** tulajdonságlapon, és kattintson az **Igen gombra** kattintva erősítse meg a törlést.

Annak érdekében, hogy megfelelően adja meg a szerepkör a felhasználó által engedélyezett (például a kiszolgáló konfigurálása távoli hibakeresés vagy helyi tárolóba) funkciók közül egyesek, az eszközkészlet automatikusan konfigurálja speciális környezeti változók, amely megjelenik a felhasználó által definiált környezeti változók együtt. Az eszközkészlet megakadályozza, hogy a felhasználó szerkesztése vagy törlése az ilyen automatikusan generált környezeti változók, mindaddig, amíg a társított engedélyezve van.

<a name="session_affinity_properties"></a> 
### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Terheléselosztás / munkamenet affinitás (más néven "öntapadó munkamenetek") tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és válassza a **Terheléselosztás**. Ez a párbeszédpanel belül van az azt jelenti, hogy engedélyezése, és tiltsa le azt a munkamenet, az alábbi képen látható módon.

![][ic719492]

Kapcsolódó információk olvassa el a [Munkamenetet affinitás][]című témakört. Megjegyzés: Ez a funkció működés a környezetében, mert az SSL, [Mert az SSL][]leírt módon.

<a name="local_storage_properties"></a> 
### <a name="local-storage-properties"></a>Helyi tárolási tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és válassza a **Helyi tároló**. Ez a párbeszédpanel belül, ha az azt jelenti, hogy létrehozása, módosítása vagy eltávolítása a virtuális gép az alkalmazást futtató ideiglenes helyi tárterület. Egyedi értékeket kell állíthat be a helyi tároló méretét, valamint hogy tartalmát megőrződnek esetén a szerepkör a rendszer, az alábbi képen látható módon.

![][ic719508]

Is megadhat egy környezeti változó, amely megfelel a helyi tárolóba.

Alapértelmezés szerint minden az Azure rendszerbe van elhelyezett (és kibontott) szerepkör-példány **approot** mappában. Legegyszerűbb telepítések fog van igazítva kicsomagolás, a **approot** könyvtár kiosztott terület után is is korlátozott és pontosan meghatározott (kisebb mint 1 GB egy lehetővé teszi a legjobb megoldás). Ezért annak érdekében, hogy Azure elegendő mennyiségű lemezterületet lefoglalja előfordulhat, hogy nem férnek el a **approot** mappában nagyobb telepítésekhez, állítson be egy helyi tároló erőforrás a **Helyi tároló** párbeszédpanelen. Ugyanígy Ehhez olvassa el a [Nagy telepítések üzembe helyezése][]című témakört.

A tároló erőforrás az indítási parancsfájlok (például a **startup.cmd**) a környezeti változó automatikusan a Holdas eszközkészlet által az erőforráshoz társított, ahogy azt a **Helyi tároló** párbeszédpanel használatával egyszerűen hivatkozhat. Környezet változó tartalmazni fogja az indítási parancsfájl végrehajtásakor beállította a helyi erőforrás teljes elérési útját. 

Helyi tároló erőforrás módosításához jelölje ki a helyi tároló erőforrást, és kattintson a **Helyi tároló** tulajdonságlap **szerkesztése** gombra. Egy párbeszédpanel nyílik meg, amely lehetővé teszi a helyi tároló erőforrás tulajdonságainak módosítása. Nyomja le **az OK** gombra a helyi tároló erőforrás értékeinek mentéséhez.

Helyi tároló erőforrás törléséhez jelölje ki a helyi tároló erőforrást, és kattintson az **Eltávolítás** gombra, a **Helyi tároló** tulajdonságlapon, és kattintson az **Igen gombra** kattintva erősítse meg a törlést.

<a name="server_configuration_properties"></a> 
### <a name="server-configuration-properties"></a>Konfigurációs tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és kattintson a **Kiszolgáló konfigurálása**. Ez a párbeszédpanel belül szeretne hozzáadása, eltávolítása és módosítása a példányban által használt JDK és Java alkalmazáskiszolgáló lehetősége van, valamint hozzáadni vagy eltávolítani az alkalmazások (például HÁBORÚ, üveg vagy EAR fájlokat), a telepítő által használt.

### <a name="jdk-configuration"></a>JDK konfigurálása ###

Ez a párbeszédpanel lehetővé teszi, hogy adja meg a JDK csomag a telepítéshez használni. Holdas Windows rendszer használata esetén megadhatja, hogy a JDK csomag helyileg használja, ha az Azure irányító működik, és a megjelenítéshez a telepítéshez használni a helyi telepítési Azure. Nem Windows operációs rendszereken a irányító JDK beállítás nem felel meg Önnek és a helyileg telepített JDK nem telepíthető, mert nem kompatibilis a Windows. Jó helyen jár függetlenül az operációs rendszer használni kívánt, mindig közül lehet választani a 3 fél JDK csomagok Azure telepítése, és mutasson a saját Windows-kompatibilis JDK csomag alternatív letöltési helyről.

Az alábbi képen hogyan adhatja meg a JDK Windows rendszeren:

![][ic780647]

Ha a Holdas Windows rendszer használata esetén is megadhat egy JDK használata a számítási irányító; Ehhez győződjön meg róla, **használja a JDK az Ez az útvonal helyileg teszteléshez** be van jelölve, a **irányító telepítési** szakaszban. Ezután adja meg a helyi elérési útját a JDK; akkor átválthat a különböző JDK, ha a használni kívánt egy automatikusan nincs bejelölve. Telepítse az Azure felhőszolgáltatásba; a JDK lehetősége is van Ehhez válassza ki a **Deploy a helyi JDK (automatikus feltöltés a felhőbeli tárhelyről)** beállítást a **felhőalapú környezetben** szakaszban.

Megjegyzés: A nem a Windows operációs rendszereken a **irányító telepítési** beállításokat, és a **Deploy a helyi JDK** beállítás nem érhetők el. Az alábbi példa bemutatja, hogy egy JDK megadása Macen vagy más-Windows támogatott operációs rendszer:

![][ic789643]

Az operációs rendszer dolgozik, függetlenül az alábbi két **felhő telepítési** beállítások a forrás- és a JDK csomag típusa van:

* **A 3 fél JDK csomag Azure elérhető terjesztése** 
* **Egyéni letöltéssel terjesztése** 

Ha a **központi telepítés egy 3rd fél JDK csomagban érhető el az Azure** beállítást használja:

1. **Egy 3rd fél JDK csomagban érhető el az Azure Deploy**nevű jelölőnégyzetet.
1. A legördülő listából válassza ki a 3 fél JDK-csomagot a Azure.
1. A **JDK** lap fog kinézni a Windows a következőhöz hasonló:  ![][ic780648] 
    és a Mac OS rendszerben a következőhöz hasonlóan fog kinézni, vagy más támogatottak, nem a Windows operációs rendszereken: ![][ic789643]
1. Kattintson az **OK gombra** a módosítások mentéséhez.
1. Amikor a rendszer kéri, fogadja el a licencszerződést, a 3 JDK csomag szolgáltatóról, olvassa el a licencfeltételeket. Feltételezve, hogy a feltételek elfogadása, kattintson az **Igen** az **Elfogadás licencszerződést** párbeszédpanel bezárásához.
    Figyelje meg, hogy a mögöttes logika, amelynek elemek jelennek meg a legördülő lista **egy 3rd fél JDK csomagban érhető el az Azure Deploy** beállítással szabható. Ha testre szeretné szabni a cikkek **JDK** párbeszédpanelen **testreszabása** hivatkozásra. Ezzel zárja be a **JDK** tulajdonság lapot, és nyissa meg a **componentsets.xml** fájlt Holdas, amely, majd módosítsa igény szerint. Dokumentációjában találhat **componentsets.xml** magát a **componentsets.xml** fájlt tartalmazza.

Ha a **Deploy egy egyéni letöltéssel JDK** beállítást használja:

1. Hozzon létre a JDK telepítési könyvtár, amely biztosítja, hogy a címtár-csomópont magát a gyermek a ZIP-struktúra és a tartalma nem ZIP. Jegyezze fel a név, könyvtárat fog később szükség lenne rá, és tartsa szem előtt az JDK telepítése a Windows virtuális gép telepítik.
1. Töltse fel az Azure tárterület-fiókjába a ZIP blob-ként. Megteheti azt is, ez eszközzel egy külső felekkel elérhető feltöltése BLOB-tárolóhoz Azure. Magánjellegű blob használata ajánlott. Ajánljuk figyelmébe a ZIP-tartalom blob URL-CÍMÉT.
1. Jelölje be a **Deploy egy egyéni letöltéssel JDK**nevű jelölőnégyzetet.
    Ha az Azure tárterület-fiókját, jelölje ki a tárhely fiók a **tárterület-fiók** legördülő listában (is a hivatkozásra kattintott **fiókok** módosítása, hogy mi az a lista), amelyek részben kitölti az **URL-cím** mezőbe, és az URL-cím fennmaradó részének majd töltse ki a letölteni kívánt. Ha nem szeretné, hogy Azure tároló használatára, a **tárhely fiók** legördülő listából válassza a **(nincs)** , és írja be az URL-CÍMÉT az **URL-címe** mezőben a JDK letöltése. Ha Azure tárterületet használ, blob nevét az URL-cím kisbetűre cseréli le kell lennie.
1. Győződjön meg arról, hogy a **JAVA_HOME** szövegdoboz hivatkozik-e a megfelelő könyvtár nevét. Alapértelmezés szerint JDK címtár neve megegyezik az érték, a helyi használatra kiválasztott hivatkoznak rá. De ha a címtárban szereplő az IRÁNYÍTÓSZÁM egy másik nevet (például, hogy egy másik verziót használ), a könyvtár nevét a **JAVA_HOME** rendelés_részletei.mennyiség megfelelő módon frissülnek, mivel ez a beállítás fogja használni a felhőbe (nem szerepel a számítási irányító).
1. Kattintson az **OK gombra** a módosítások mentéséhez.

Ez azt. Most ha az a felhő generál, megfigyelheti csomag méretének sokkal kisebb lesz, az összeállítás folyamat általában kell vennie kevesebb időt és a telepítést, magát a felhőbe történő közzétételekor kell vennie kevesebb időt. Figyelje meg, hogy a **Deploy a helyi JDK (automatikus feltöltés a felhőbeli tárhelyről)** vagy **egy egyéni letöltéssel JDK Deploy** beállítások érvényben csak ha az alkalmazás telepítve van a felhőben. Nincs hatással a számítási irányító felhasználói felület; rendelkeznek a helyi verzió összetevő továbbra is használhatók, amikor rendszerbe állítják a számítási irányító. 

### <a name="server-configuration"></a>Kiszolgáló konfigurálása ###

Az alábbi képen az, hogy miként adhatja meg az application server.

![][ic796926]

Győződjön meg arról, hogy a **Deploy az ilyen típusú kiszolgáló** jelölőnégyzet be van jelölve, és válassza ki a használni kívánt alkalmazáskiszolgáló típusát.

Adja meg a kiszolgáló felhő telepítéshez használni, a használatba veheti az alábbi lehetőségek közül:

1. **Egy 3rd fél server Azure a Deploy** – különösen akkor alkalmazható, ahol telepítési hatékonyság és egyszerűség prioritást és a kiszolgáló nem igényel egy egyéni konfigurációs fejlesztők/próba helyzetekben. Vagy ha kezdőpontjának használandó közül azokat a kiszolgálókat, de felvehet megfelelő kiszolgálón testreszabása a üzembe indítási logika lépéseit.
1. **Egyéni letöltéssel Deploy** – Ez akkor gyártási helyzetekben különösen akkor alkalmazható, ha rendelkezik egy speciálisan elkészített és konfigurált a felhőben használni kívánt.
1. A **központi telepítés helyi server telepítése** – különösen alkalmazható a Ha helyi server telepítésének már egyéni konfigurálva a használatra. Ha ezt a beállítást választja, a helyi kiszolgálói elérési út is meg kell adnia az alábbi **helyi kiszolgálói elérési út** mezőbe.

Ha a **Deploy egy 3rd fél server Azure a** beállítást használja:

1. **Egy 3rd gyártó server Azure a Deploy**nevű jelölőnégyzetet.
1. A legördülő menüből válassza ki a kívánt kiszolgáló szoftver használatához a üzembe a felhőben. Megjegyzés, ha megad egy korábbi verziójában használni kiszolgáló típusa fogja korlátozott csak a felhőben kiszolgálót, amely az adott kiszolgáló típusa ugyanazon család kiválasztása. De ha nem tudatosan választotta a kiszolgáló típusa, választhat a Azure jelenleg elérhető kiszolgálók és kiszolgálótípus automatikusan kijelöli azt az.
1. Kattintson az **OK gombra** a módosítások mentéséhez.

Ha használja az **egyéni letöltéssel Deploy** beállítás:

1. Győződjön meg arról, hogy már ki van választva a kiszolgáló típusa szerint az előző lépéseket. Ez ismerheti a beépülő modul a kiszolgálót az egyéni letöltése, telepítése, mint a a kijelölt kiszolgáló típusa ugyanazon család kell lennie.
1. **Egyéni letöltéssel Deploy**nevű jelölőnégyzetet.
    Ha töltse le a Azure tárterület-fiókjából szeretne, jelölje be a tár fiókot a **tárterület-fiók** legördülő listában (is a hivatkozásra kattintott **fiókok** módosítása, hogy mi az a lista), amelyek részben kitölti az **URL-cím** mezőbe, és az URL-cím fennmaradó részének majd töltse ki a kiszolgáló letöltése ZIP (ha az Azure tároló, blob nevét az URL-cím használatával kisbetűre cseréli le kell lennie) Ha nem szeretné, hogy Azure tároló használatára, a **tárhely fiók** legördülő listából válassza a **(nincs)** , és írja be az URL-címet a kiszolgáló letöltése az **URL-címe** mezőben ZIP. Az IRÁNYÍTÓSZÁM, amely az alkalmazás server telepítési könyvtár gyermek mappa tartalmaz. Például használja a zip-Apache Tomcat 7.0.35, a zip belül a gyermek mappát, amely a telepítési könyvtárban, például **apache-tomcat-7.0.35**lenne. 
1. Adja meg a kezdő könyvtár környezeti változó értékét. Ezt az értéket, a helyi alkalmazáskiszolgáló használható, ha vannak ilyenek alapértelmezés szerint, de megadhatja egy másik elemet, ha a felhőben alkalmazáskiszolgáló eltér a helyi alkalmazáskiszolgáló. Jó helyen jár, ellenőrizze, hogy van-e a felhőben alkalmazáskiszolgáló a kiszolgálójával azonos családtagok szükséges a korábban kijelölt típusát.
    Ha későbbi frissítése a felhőbeli alkalmazás kiszolgáló zip, manuálisan módosíthatja az otthoni címtár-beállítás, vagy újra a az helyi beállítást (Ha túl megváltozott a helyi alkalmazáskiszolgáló) egyeztetni kell.
1. Kattintson az **OK gombra** a módosítások mentéséhez.

Az alapul szolgáló logika, amelynek elemek jelennek meg a **kiszolgáló** lapon, a **Kiszolgáló konfigurálása** tulajdonságlap testre szabható. Ez a speciális szolgáltatás, ha az alapértelmezett értékeket terjedhet az igényeinek, vagy ha fel szeretné venni a többi kiszolgáló szükség lehet. Ha testre szeretné szabni a logika a **kiszolgáló** párbeszédpanel **testreszabása** hivatkozásra. Ezzel zárja be a **Kiszolgáló konfigurálása** tulajdonság lapot, és nyissa meg a **componentsets.xml** fájlt Holdas, amelyek ezután módosíthatja a kiszolgáló konfigurációs sablon meghosszabbítása szükség szerint. Dokumentációjában találhat **componentsets.xml** magát a **componentsets.xml** fájlt tartalmazza.

Ha a **Deploy a helyi kiszolgálója (automatikus feltöltés a felhőbeli tárhelyről)** beállítást használja:

1. **A helyi kiszolgálója (automatikus feltöltés a felhőbeli tárhelyről) Deploy**nevű jelölőnégyzetet.
1. A **tároló számla** legördülő lista használata esetén válassza a **(automatikus)**. Itt **(automatikus)** adja meg, ha a Holdas eszközkészlet ugyanazt a tárterület-fiókot a kiszolgáló az adott, válassza a **Közzététel az Azure** párbeszédpanelen a telepítéshez használja.
1. Kattintson az **OK gombra** a módosítások mentéséhez.

Válassza a **helyi kiszolgáló elérési útja** mezőben egy server telepítési helye a számítógépen, az alábbi feltételek teljesülése esetén:

* A telepítés tesztelése a irányító (ezt a funkciót a Windows csak) a kívánt.
* A helyileg telepített server telepítése a felhőbe szeretne.
* Szeretné használni a felhőben, amelyben egyéni eset egyéni kiszolgáló letöltés, is biztosítják a **Deploy a helyi kiszolgálója (automatikus feltöltés a felhőbeli tárhelyről)** beállítás fölé.

A fenti lehetőségek a egyike sem vonatkozik a helyzet, ha a helyi kiszolgáló beállítás nem kötelező.

### <a name="applications-configuration"></a>Alkalmazások konfigurálása ###

Az alábbi képen az, hogy miként adhatja meg az alkalmazás.

![][ic719512]

Kattintson a **Hozzáadás** egy másik alkalmazás hozzáadása vagy **eltávolítása** eltávolíthatja az alkalmazásokat. Hatékonyság célokra használandó letöltés a forrás az alkalmazás telepítésekor a felhőbe, ha segítségével [összetevők tulajdonságok](#components_properties) adja meg az URL-címet, tároló fiók stb. 

Áprilisi 2014-es kiadását kezdve az alkalmazások vannak automatikus feltöltése (csoportban a **eclipsedeploy** tároló) azonos tárterület-fiókba mint kiválasztott a telepítéshez. Az indítási logika a központi telepítés letöltődő először ezeket az alkalmazásokat a tárhely fiókból lépés tartalmazza. Ez azt jelenti, hogy, előfordulhat, hogy az alkalmazások frissítése a környezetben anélkül, hogy újraépítéséhez és telepítsen újra a teljes csomag közvetlenül figyelembe, hogy tárhelyek (például használatával az Azure-portálra) az alkalmazás újabb verziója manuálisan feltöltésével, az eszközkészlet által eredetileg feltöltött lecseréli az eredeti HÁBORÚ fájlokat van. Ezután csak indítsa az adott szerepkört példányait ismét vagy keresztül parancssori segédprogramok Azure tartozó adatkezelési portál használatával újrafelhasználás. (A Holdas eszközkészlet belül közvetlenül a újrafelhasználás szerepkör kiváltó jelenleg nem támogatott.)

### <a name="notes-about-server-configuration"></a>Megjegyzések a kiszolgáló konfigurálása ###

A **kiszolgáló konfigurálása** tulajdonságlap keresztül végrehajtott módosítások megjelenjenek a `<component>` package.xml fájl elemeket.

**Automatikus feltöltése a...** használja, vagy a JDK vagy alkalmazáskiszolgáló, és Ön **Deploy... le** lehetőségei vannak összeállítását, az a felhő (nem a számítási irányító), és a hálózathoz kapcsolódik, amikor azt tapasztalhatja, például a konzol eredményt ad, a következő üzenet összeállítása a telepítsenek szerkesztő ellenőrzi a letöltés elérhetőség szerint:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

A **Letöltés... a központi telepítés** lehetőséget választotta, ha az alábbi figyelmeztetés is megjeleníthető, de továbbra is a Szerkesztés:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Ez a figyelmeztetés csak megjelölése, hogy a letöltés elérhetősége nem ellenőrzött. Igen, ha a telepítés valamiért nem sikerül a felhőben, ellenőrizze, hogy kapott-e ez a figyelmeztetés.

Ha szeretné letiltani a letöltés ellenőrzését (például úgy érzi, akkor szükségtelenül lelassul összeállítása), és állítsa a `verifydownloads` attribútum `false` a a `<windowsazurepackage>` package.xml eleme: 

`<windowsazurepackage verifydownloads="false" ...>` 

Ha az **automatikus feltöltése a...** lehetőséget választotta, majd a konzol ablakban, látni fogja a feltöltés 5 másodpercenként, amikor egy feltöltés végrehajtásának jelentéskészítés build üzenetek szükség.

<a name="ssl_offloading_properties"></a> 
### <a name="ssl-offloading-properties"></a>SSL, mert tulajdonságai ###

Nyissa meg a helyi menü a szerep Holdas a Project Explorer ablakban kattintson a **Azure**, és válassza az **SSL, mert**. 

![][ic719481]

Ez a párbeszédpanel belül engedélyezheti SSL mert, amivel ahhoz, hogy könnyen Hypertext Transfer Protocol biztonságos (HTTPS) támogatja az Azure-Java telepítéssel anélkül, hogy a Java alkalmazáskiszolgáló SSL beállítása. További tudnivalókért lásd: a [SSL mert][] , és [hogyan használja az SSL mert][].

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[Az Azure eszközkészlet Holdas telepítése][]

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

[Azure projekt tulajdonságai][]

[Azure tároló számla lista][]

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Kezelőportálja segítségével]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure projekt tulajdonságai]: http://go.microsoft.com/fwlink/?LinkID=699524
[Azure tároló számla lista]: http://go.microsoft.com/fwlink/?LinkID=699528
[összefoglaló com.microsoft.windowsazure.serviceruntime csomag]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Egy adott szerepkör-példányt, több példány környezetben hibakeresése]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Azure alkalmazások Holdas hibakeresése]: http://go.microsoft.com/fwlink/?LinkID=699535
[Nagy telepítések üzembe helyezése]: http://go.microsoft.com/fwlink/?LinkID=699536
[Dokumentumok közös található gyorsítótárazás használata]: http://go.microsoft.com/fwlink/?LinkID=699542
[Használatát, mert SSL]: http://go.microsoft.com/fwlink/?LinkID=699545
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546
[Munkamenet affinitás]: http://go.microsoft.com/fwlink/?LinkID=699548
[Mert SSL]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png
