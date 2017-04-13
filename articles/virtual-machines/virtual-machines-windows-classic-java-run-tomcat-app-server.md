<properties
    pageTitle="Tomcat virtuális gépre |} Microsoft Azure"
    description="Ebben az oktatóanyagban erőforrásokat a klasszikus telepítési modell készült használ, és bemutatja, hogyan hozhat létre egy Windows virtuális számítógépre, és konfigurálja úgy, hogy futtatni Apache Tomcat alkalmazáskiszolgáló."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Hogyan tehető függővé egy Java alkalmazáskiszolgáló a klasszikus telepítési modell készült virtuális gépen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Az Azure a virtuális gép segítségével adja meg a kiszolgáló funkciók. Példaként futó Azure virtuális gép beállítható úgy, hogy Java alkalmazást, például Apache Tomcat kiszolgáló állomás. Miután ez az útmutató, akkor futó Azure virtuális gép létrehozása és konfigurálja úgy, hogy futtassa a Java alkalmazáskiszolgáló megértéséhez.

Ismerkedhet meg:

* Hogyan lehet létrehozni egy virtuális számítógépre, amelyen egy Java fejlesztési Kit (JDK) már telepítve van.
* Hogyan lehet távolról jelentkezzen be a virtuális géphez.
* Hogyan lehet Java alkalmazás kiszolgáló telepítése a virtuális gépen.
* Hogyan lehet létrehozni a virtuális gép zárólap.
* Hogyan lehet a tűzfalban a alkalmazáskiszolgáló a port megnyitása.

Ebben az oktatóanyagban alkalmazásában Apache Tomcat alkalmazáskiszolgáló települ virtuális gépen. A kész telepítés a Tomcat példányát, például a következőket eredményezi.

![Virtuális gépen futó Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Virtuális gép létrehozása

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2. Kattintson az **Új**, kattintson a **számítja ki**, **virtuális gép**kattintson, és válassza **A gyűjtemény**.
3. **Virtuális gép kép kiválasztása** párbeszédpanelen jelölje be a **JDK 7 Windows Server 2012-ben**.
Ne feledje, hogy **JDK 6 Windows Server 2012-ben** érhető el, ha régebbi alkalmazások, amelyek nem JDK 7 futtathatja.
4. Kattintson a **Tovább**gombra.
5. A **virtuális gép konfigurációs** párbeszédpanel:
    1. Adja meg a virtuális számítógép nevét.
    2. Adja meg a virtuális gép használandó méretét.
    3. A **Felhasználónév** mezőbe írja be a rendszergazda nevét. Ne feledje, hogy ez a név és a jelszót, akkor ezután adja meg, akkor használja őket bejelentkezéskor távolról a virtuális géphez.
    4. Az **Új jelszó** mezőbe írjon be egy jelszót, és adja meg újra a **Megerősítés** mezőben. Ez az a rendszergazdai fiók jelszava.
    5. Kattintson a **Tovább**gombra.
6. A következő **virtuális gép konfigurációs** párbeszédpanelen:
    1. **Felhőbeli szolgáltatástól**használja az alapértelmezett **létrehozása egy új felhőszolgáltatásba**.
    2. **Felhőalapú szolgáltatás DNS-** név az értéknek egyedinek kell lennie a cloudapp.net keresztül. Szükség esetén módosítsa ezt az értéket, hogy Azure azt jelzi, hogy egyedi.
    2. Adja meg a régió, a affinitás csoport vagy a virtuális hálózat. Ebben az oktatóanyagban céljából adja meg, hogy egy például **Amerikai nyugati**régió.
    2. **Tárterület-fiókot**jelölje be **az automatikusan generált tárterület-fiók használata**.
    3. **Elérhetőség beállítása**jelölje be a **(nincs)**.
    4. Kattintson a **Tovább**gombra.
7. A végső **virtuális gép beállításai** párbeszédpanelen:
    1. Fogadja el az alapértelmezett végpont bejegyzéseket.
    2. Kattintson a **kész**gombra.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>A virtuális gép távolról bejelentkezni

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2. Kattintson a **virtuális gépeken futó**.
3. Kattintson a nevére a virtuális gépen jelentkezzen be a kívánt.
4. Miután a virtuális gép elindult, a lap alján az előugró menü engedélyezi a csatlakozást.
5. Kattintson a **Csatlakozás**gombra.
6. A virtuális gépen való kapcsolódáshoz szükség szerint megválaszolása a képernyőn megjelenő utasításokat. Ez célszerű jár mentése vagy megnyitása az .rdp fájlt, amely a kapcsolat adatait tartalmazza. Előfordulhat, hogy másolja a vágólapra az URL-címe: port .rdp fájl az első sor utolsó részeként, és illessze be egy távoli bejelentkezési alkalmazásra.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>A virtuális gépen Java alkalmazás kiszolgáló telepítése

A virtuális géphez egy Java alkalmazáskiszolgáló másolhatja, vagy egy Java application Server kiszolgálón keresztül egy telepítő telepítheti.

Ebben az oktatóanyagban alkalmazásában Tomcat telepíthető.

1. Ha be van jelentkezve a virtuális gép, nyissa meg a böngészési munkamenet [Apache Tomcat](http://tomcat.apache.org/download-70.cgi)szeretne.
2. Kattintson duplán a **32 bites és 64 bites Windows szolgáltatás Installer**hivatkozására. Ez az eljárás használatával Tomcat telepíthető Windows-szolgáltatásként.
3. Amikor a rendszer kéri, válassza a telepítő futtatását.
4. Belül a **Apache Tomcat beállítási** varázslót kövesse az útmutatást követve telepítse a Tomcat. Ebben az oktatóanyagban alkalmazásában az alapértelmezett beállítások elfogadása nem kell aggódnia. Amikor elér **a Apache Tomcat beállítása varázsló lépéseinek végrehajtása** párbeszédpanel, tetszés szerint érdemes **Apache Tomcat futtassa** az Indítás most Tomcat van. Kattintson a **Befejezés gombra** az Tomcat beállítási folyamat befejezéséhez.

## <a name="to-start-tomcat"></a>Tomcat indítása
Ha nem tudatosan Tomcat futtatásához **a Apache Tomcat beállítása varázsló lépéseinek végrehajtása** párbeszédpanel, indítsa el a virtuális gépen egy parancssort megnyitásával, és futtatása a **nettó Tomcat7 indítása**.

Ekkor megjelennek fut, ha a böngészőben a virtuális gép és nyissa meg a <8080>Tomcat.

A külső gépeken futó operációs rendszert futtató Tomcat megtekintéséhez szüksége végpont létrehozásához, és nyissa meg a olyan portot.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>A virtuális gép végpont létrehozása
1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2. Kattintson a **virtuális gépeken futó**.
3. Kattintson a nevére a virtuális gép a Java alkalmazás Servert futtató.
4. Kattintson a **Végpontok**.
5. Kattintson a **hozzáadása**gombra.
6. **Végpont hozzáadása** párbeszédpanelen győződjön meg arról, **Hozzáadás önálló végpont** választógombot, és kattintson a **Tovább gombra**.
7. Az **új végpont adatait** párbeszédpanelen:
    1. Adjon meg egy nevet a végpont; Ha például **HttpIn**.
    2. Adja meg a **TCP** a protokollhoz.
    3. Adja meg a nyilvános port **80** .
    4. Adja meg a **8080** magánjellegű portszámként.
    5. Kattintson a **kész** gombra a párbeszédpanel bezárásához. Az endpoint most hoz létre.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>A tűzfal a virtuális gép olyan portot megnyitása
1. Jelentkezzen be a virtuális gépet.
2. Kattintson a **Windows Start**.
3. Kattintson a **Vezérlőpult parancsra**.
4. Kattintson a **rendszer és biztonság**, a **Windows tűzfal**elemre, és válassza a **Speciális beállítások**elemre.
5. Kattintson a **Bejövő szabályok**elemre, és kattintson az **Új szabály**parancsra.
 ![Új bejövő szabály][NewIBRule]
6. A **Szabálytípus**jelölje be a **Port**, és kattintson a **Tovább gombra**.
 ![Új bejövő szabály port][NewRulePort]
7. A **Protocol (protokoll) és a portokra** képernyőn jelölje ki a **TCP**, adja meg az **adott helyi port** **8080** , és kattintson a **Tovább gombra**.
 ![Új bejövő szabály][NewRuleProtocol]
8. A **művelet** képernyőn válassza az **Engedélyezés lehetőséget a kapcsolatot**, és kattintson a **Tovább gombra**.
 ![Bejövő szabályt az új műveletet][NewRuleAction]
9. A **profil** képernyőn győződjön meg arról, hogy **tartomány**, **személyes**és **nyilvános** legyen kijelölve, és kattintson a **Tovább gombra**.
 ![Új bejövő szabály profil][NewRuleProfile]
10. A **név** képernyőn adjon meg egy nevet a szabálynak, például **HttpIn** (a szabály neve nem szükséges egyezzen végpontot, azonban), és kattintson a **Befejezés gombra**.  
 ![Új bejövő szabály neve][NewRuleName]

Ezen a ponton a Tomcat webhely kell lennie egy külső böngészőből megtekinthető az űrlap URL-cím használatával * *http://*a\_DNS\_neve*. cloudapp.net**ahol ** *a\_DNS\_neve*** a virtuális gép létrehozásakor megadott DNS-neve.

## <a name="application-lifecycle-considerations"></a>Alkalmazás életciklus kapcsolatos szempontok
* Hozzon létre saját webes alkalmazás archívum (HÁBORÚ) sikerült, és vegye fel a **webalkalmazás** mappát. Például egyszerű Java szolgáltatás, oldal (JSP) dinamikus webes projekt létrehozása és HÁBORÚ fájlba exportálhatja, HÁBORÚ másolja a Apache Tomcat **webalkalmazás** mappába a virtuális gépen, futtassa a böngészőben.
* Alapértelmezés szerint a Tomcat szolgáltatás telepítésekor beállítás manuális indításához. Annak indul el automatikusan a Services beépülő modul használatával válthat. A szolgáltatások beépülő modul elindítása **A Windows Start**, **Felügyeleti eszközök**és **szolgáltatások**gombra kattintva. Kattintson duplán a **Apache Tomcat** szolgáltatásra, és **az indítás típusának** beállítása **automatikusra**.

    ![Automatikus indításúként szolgáltatás beállítása][service_automatic_startup]

    Az automatikusan problémákat Tomcat kezdő előnye, hogy elindul, ha a virtuális számítógép újraindítása (például, hogy újra kell indítani szoftverfrissítések telepítése után).

## <a name="next-steps"></a>Következő lépések
Tudjon meg többet érdemes felvenni az Java-alkalmazásokkal a rendelkezésre álló információk [Java Developer Center](https://azure.microsoft.com/develop/java/)megtekintésével más szolgáltatások (például az Azure tároló szolgáltatás bus és SQL-adatbázis).

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
