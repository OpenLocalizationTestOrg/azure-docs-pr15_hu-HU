<properties
    pageTitle="Azure alkalmazások Holdas hibakeresése"
    description="Tudjon meg többet az Azure eszközkészlet használata Holdas hibakeresési Azure alkalmazásokat."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Azure alkalmazások Holdas hibakeresése #

Az Azure eszközkészlet Holdas használata esetén is hibakeresése az alkalmazások e futnak Azure, illetve a számítási irányító, ha a Windows operációs rendszert használ. Az alábbi képen látható párbeszédpanel használt engedélyezése a távoli hibakeresés **Hibakeresés** tulajdonságok:

![][ic719504]

Ebben az oktatóanyagban feltételezi, hogy már van egy alkalmazást, amely létrehozása sikeresen befejeződött, és a számítási irányító ismerős és a központi telepítés Azure.

Az alkalmazás a [használja az Azure Service futtatókörnyezet tárba JSP][] oktatóprogram kezdőpontjának Ez a témakör az be. A folytatás előtt hozzon létre az alkalmazást, ha, még nem tette.

## <a name="to-debug-your-application-while-running-in-azure"></a>Az alkalmazás az Azure futtatása közben hibakeresése ##

>[AZURE.WARNING] Az eszközkészlet aktuális támogatása Java, hibakeresése során elsődlegesen az Azure számítási irányító futó telepítések számára készült. Mivel a hibakeresési kapcsolat nem biztonságos, ne engedélyezze a távoli hibakereséshez a gyártási környezetekben. Ha van szüksége a Azure felhőben futó alkalmazás hibakeresése átmeneti tárolásra szolgáló telepítéshez használni, de figyelmen kívül hagyja, hogy ezzel az illetéktelen hozzáférést a hibakeresési munkamenet van lehetőség, ha valaki tudja, hogy a felhőben üzembe IP-címét, akkor is, ha egy átmeneti tárolásra szolgáló telepítés.

1. A projekt a irányító tesztelésére készítése: az Holdas Projektböngésző, kattintson a jobb gombbal a **MyAzureProject**, kattintson a **Tulajdonságok**parancsra, kattintson az **Azure**és beállításához a **felhőbe**példányhoz **tartozó vagy** .
1. A projekt újraépítéséhez: a Holdas menüben kattintson a **Projekt**, majd válassza az **Összes összeállítása**.
1. Telepítse az alkalmazást a *átmeneti tárolására* Azure-ban.
    >[AZURE.IMPORTANT] Amint már említettük, javasoljuk, hogy a legtöbb esetben a számítási irányító hibakeresési, majd a fejlesztői környezet hibakeresési csak akkor, ha további hibakeresési van szükség. Ajánlott nem hibakeresése az éles üzemi környezetben.
1. Amikor elkészült a üzembe Azure-ban, szerezze be a DNS-nevét a telepítéshez az [Azure Kezelőportálja segítségével][]. A fejlesztői telepítési van http:// formájában DNS neve*&lt;globálisan egyedi azonosítója&gt;*. cloudapp.net, ahol * &lt;globálisan egyedi azonosítója&gt; * Azure által megadott globálisan egyedi azonosítója érték.
1. Holdas a Project Explorer kattintson a jobb gombbal a **WorkerRole1** **Azure**kattintson, és válassza a **Hibakeresés**.
1. Az **WorkerRole1 hibakereséshez tulajdonságai** párbeszédpanelen:
    1. Jelölje be **engedélyezése a távoli hibakeresése során a szerepkör.**
    1. A **beviteli végpontot kell használni**használja a **Hibakeresés (nyilvános: 8090, személyes: 8090)**.
    1. Győződjön meg róla, **Felfüggesztett módban Várakozás a hibakereső kapcsolat indítása JVM** nincs bejelölve.
        >[AZURE.IMPORTANT] A **Felfüggesztett módban Várakozás a hibakereső kapcsolat indítása JVM** beállítás célja hibakeresési felhasználási helyzetei a számítási irányító csak a speciális (felhő telepítésekhez nem). A **Felfüggesztett módban Várakozás a hibakereső kapcsolat indítása JVM** lehetőség használata esetén mindaddig, amíg a Holdas debugger csatlakozik a JVM felfüggeszti a kiszolgáló indítási folyamat. Miközben használata a számítási irányító hibakeresési munkamenethez használhatja is ezt a beállítást, ne használja hibakeresési munkamenethez felhőalapú környezetben. A kiszolgáló inicializálni esedékes Azure indítási tevékenység, és az Azure felhő nem nyilvános végpontok elérhetővé tétele az indítási tevékenység befejeztéig. Ennélfogva indítási folyamat nem lesz sikeres Ha ez a beállítás engedélyezve van a felhőalapú környezetben, hiszen nem érkeznek a kapcsolatot egy külső Holdas ügyfélprogramból.
1. Kattintson a **Beállítások hibakeresési létrehozása**gombra.
1. **Azure hibakeresési konfigurációs** párbeszédpanelen:
    1. **Java projekt szeretné hibakeresése**jelölje be a **MyHelloWorld** projekt.
    1. A **Configure hibakeresése során**jelölje be a **Azure felhő (átmeneti)**.
    1. Gondoskodjon arról, hogy nincs bejelölve **Azure irányító számítja ki** .
    1. **Host**írja be a DNS-neve a szakaszos üzembe, de az előző **http://**nélkül. Ha például (használja a globálisan egyedi azonosítója, az itt ismertetett globálisan egyedi azonosítója helyett): **65-6973-526f62657274.cloudapp.net 4e616d65 6f6e-6 d**
1. Kattintson az **OK gombra** az **Azure hibakeresési konfigurációs** párbeszédpanel bezárásához.
1. Kattintson az **OK gombra** a **WorkerRole1 hibakereséshez tulajdonságai** párbeszédpanel bezárásához.
1. Ha nincs már be van állítva index.jsp töréspont, állítsa be:
    1. Holdas a Project Explorer belül bontsa ki a **MyHelloWorld**, bontsa ki a **Web tartalma**, és kattintson duplán a **index.jsp**.
    1. Belül index.jsp a bal oldalán a Java-kód kék sávon kattintson a jobb gombbal, és kattintson a **Töréspontok váltása**, ahogy az alábbi: ![][ic551537]
1. Belül Holdas a menüben kattintson a **Futtatás** parancsra, és válassza a **Konfigurációk hibakeresési**.
1. **Hibakeresési konfigurációk** párbeszédpanelen bontsa ki a bal oldali ablaktáblában a **Java-alkalmazások** , jelölje ki az **Azure felhő (WorkerRole1)**, majd kattintson a **hibakeresési**parancsra.
1. A böngészőben a szakaszos alkalmazás **http://**futtatásához*&lt;globálisan egyedi azonosítója&gt;***.cloudapp.net/MyHelloWorld**, helyettesítése a globálisan egyedi azonosítója, a DNS-nevét a * &lt;globálisan egyedi azonosítója&gt;*. Ha egy **Perspektíva kapcsoló megerősítése** párbeszédpanel kéri, kattintson az **Igen**gombra. A hibakeresési munkamenet most végre kell hajtani a vonalra, hol állította be a töréspont-kódot.

>[AZURE.NOTE] Ha egy távoli indítása próbál hibakeresése során a telepítéshez, amelynek több szerepkör példánya fut, nem jelenleg szabályozhatja, mely példány kapcsolat a debugger fog kezdetben csatlakoznia kell, szerint az Azure terheléselosztó fog válassza ki egy példány véletlenszerű. Miután az adott példányhoz hálózathoz kapcsolódik, azonban akkor is ugyanazon példányára hibakeresési. Megjegyzés: is, ha ponttal (például amikor, hogy le túl sokáig töréspont) több, mint a 4 perc inaktivitás, Azure előfordulhat, hogy zárja be a kapcsolatot.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Egy adott szerepkör-példányt, több példány környezetben hibakeresése ##

A telepítő futtatásakor a felhőben fogjuk valószínűleg futtatni, több számítási, vagy szerepkör, példányok. Ez lehetővé teszi Azure 99.95 % elérhetősége garancia előnyeit, és az alkalmazás méretezése.

Ilyen esetben szükség lehet a Java-alkalmazások szerepkör-példányban távolról hibakeresési. Jó helyen jár Ha engedélyezi a hibakereséshez csak normál beviteli végpontot, az Azure terheléselosztó fog lehetetlenné gyakorlatilag az, hogy csatlakozzon a debugger egy szerepkör-példányt. Ehelyett kapcsolódik, egy szerepkör-példányt, amely azt választja ki véletlenszerűen.

Ez a forgatókönyv, ahol példány beviteli végpontok nyújtotta előnyök kihasználása így egyszerűbben szeretné hibakeresése egy szerepkör-példány típusát.

Tegyük fel, hogy szeretné futtatni a üzembe legfeljebb 5 szerepkör példányait. Használja a **Végpontok** tulajdonságlap szerepkör tulajdonságai párbeszédpanelen, hozzon létre egy példány beviteli végpontot, és rendelje hozzá egy egyetlen portszámot helyett egy nyilvános porttartományt. Például a **Nyilvános port** beviteli mezőben adja meg a **81-85**.

Miután a példány végponttal az alkalmazás telepítéséhez Azure a egyedi portszámot ebben a tartományban az egyes példányok szerepkör hozzárendelése. Annak érdekében, hogy melyik példány gépe van hozzárendelve mely portszámot meg, akkor a *InstanceEndpointName***_PUBLICPORT** környezeti változó (ahol a *InstanceEndpointName* a nevét, az a példány végpont létrehozása után kapott) automatikusan konfigurált által az eszközkészlet (például úgy, hogy az érték visszaadása az weblapon, az élőláb böngészés közben rá olvasható, hogy) a környezetben.

Tudja, hogy milyen nyilvános portszámot példányhoz van beállítva, hivatkozhat azt Holdas, hibakeresési konfigurációjától által rögzítésének adja meg a szolgáltatás a host nevét. Ezzel engedélyezi az adott példányhoz, és nem pedig valamilyen más példányok csatlakozni Holdas debugger.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Csak Windows: a program irányítását, miközben a számítási irányító futó hibakeresése ##

>[AZURE.NOTE] Az Azure irányító csak érhető el Windows rendszeren. Átugorja ezt a szakaszt, ha nem a Windows operációs rendszert használ. 

1. A projekt a irányító tesztelésére összeállítása: A Holdas Projektböngésző, kattintson a jobb gombbal a **MyAzureProject**, kattintson a **Tulajdonságok**parancsra, kattintson az **Azure**és **építése** állítsuk **be irányító tesztelése**.
1. A projekt újraépítéséhez: a Holdas menüben kattintson a **Projekt**, majd válassza az **Összes összeállítása**.
1. Holdas a Project Explorer kattintson a jobb gombbal a **WorkerRole1** **Azure**kattintson, és válassza a **Hibakeresés**.
1. Az **WorkerRole1 hibakereséshez tulajdonságai** párbeszédpanelen:
    1. Jelölje be **engedélyezése a távoli hibakeresése során a szerepkör.**
    1. **Beviteli végpontot kell használni**használja az alapértelmezett végpont automatikusan hozza létre az eszközkészlet állapotúként **Hibakeresés (nyilvános: 8090, személyes: 8090)**.
    1. Gondoskodjon arról, hogy a **Felfüggesztett módban Várakozás a hibakereső kapcsolat indítása JVM** beállítás nincs bejelölve.
        >[AZURE.IMPORTANT] A **Felfüggesztett módban Várakozás a hibakereső kapcsolat indítása JVM** beállítás célja hibakeresési felhasználási helyzetei a számítási irányító csak a speciális (felhő telepítésekhez nem). A **Felfüggesztett módban Várakozás a hibakereső kapcsolat indítása JVM** lehetőség használata esetén mindaddig, amíg a Holdas debugger csatlakozik a JVM felfüggeszti a kiszolgáló indítási folyamat. Miközben használata a számítási irányító hibakeresési munkamenethez használhatja is ezt a beállítást, ne használja hibakeresési munkamenethez felhőalapú környezetben. A kiszolgáló inicializálni esedékes Azure indítási tevékenység, és az Azure felhő nem nyilvános végpontok elérhetővé tétele az indítási tevékenység befejeztéig. Ennélfogva indítási folyamat nem lesz sikeres Ha ez a beállítás engedélyezve van a felhőalapú környezetben, hiszen nem érkeznek a kapcsolatot egy külső Holdas ügyfélprogramból.
1. Kattintson a **Beállítások hibakeresési létrehozása**gombra.
1. **Azure hibakeresési konfigurációs** párbeszédpanelen:
    1. **Java projekt szeretné hibakeresése**jelölje be a **MyHelloWorld** projekt.
    1. A **Configure hibakeresése során**jelölje be a **Azure irányító számítja ki**.
1. Kattintson az **OK gombra** az **Azure hibakeresési konfigurációs** párbeszédpanel bezárásához.
1. Kattintson az **OK gombra** a **WorkerRole1 hibakereséshez tulajdonságai** párbeszédpanel bezárásához.
1. Töréspont index.jsp állíthatja be:
    1. Holdas a Project Explorer belül bontsa ki a **MyHelloWorld**, bontsa ki a **Web tartalma**, és kattintson duplán a **index.jsp**.
    1. Belül index.jsp a bal oldalán a Java-kód kék sávon kattintson a jobb gombbal, és kattintson a **Töréspontok váltása**, ahogy az alábbi: ![][ic551537]

       Töréspont van beállítva, ha bal oldalán a Java-kód egy töréspont ikon belül a kék sávon látható.
1. Indítsa el az alkalmazást a számítási irányító az Azure eszköztár **Azure irányító Futtatás** gombjára kattintva.
1. Belül Holdas a menüben kattintson a **Futtatás** parancsra, és válassza a **Konfigurációk hibakeresési**.
1. **Hibakeresési konfigurációk** párbeszédpanelen bontsa ki a bal oldali ablaktáblában a **Java-alkalmazások** , jelölje be az **Azure irányító (WorkerRole1)**, majd kattintson a **hibakeresési**parancsra.
1. Miután a számítási irányító azt jelzi, hogy az alkalmazás fut, a böngészőből, **8080/MyHelloWorld**futtatni.
    Ha egy **Perspektíva kapcsoló megerősítése** párbeszédpanel kéri, kattintson az **Igen**gombra.
    A hibakeresési munkamenet most végre kell hajtani a vonalra, hol állította be a töréspont-kódot.

Ez azt mutatja, akkor a számítási irányító; a hibakeresés a következő szakaszban bemutatja, hogyan szeretné hibakeresése Azure rendszerbe állított alkalmazás.

## <a name="debugging-notes"></a>Jegyzetek hibakeresése ##

* Után hibakeresése során, válthat a perspektívát **Java** **hibakeresési** a gombra kattintva Holdas a menüben, kattintson a **ablak**, **Nyissa meg a perspektívát**, és válassza ki a használni kívánt perspektíva keresztül.
* Ahhoz, hogy a távoli hibakereséshez GlassFish, nem használható az Azure eszközkészlet távoli hibakeresési konfiguráció szolgáltatás Holdas. GlassFish inkább manuális beállítását. Miatt GlassFish kezeli a környezeti változók előre definiált Java-beállítások, módon az eszközkészlet távoli hibakeresési konfigurációs funkciója nem működik megfelelően a GlassFish. Ha az eszközkészlet távoli hibakeresési konfigurációs engedélyezve van, előfordulhat, hogy GlassFish indítását.

## <a name="see-also"></a>Lásd még: ##

[Azure eszközkészlete Holdas][]

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

[Az Azure eszközkészlet Holdas telepítése][] 

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Kezelőportálja segítségével]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546
[Az Azure Service futtatókörnyezet tárba JSP használatával]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
