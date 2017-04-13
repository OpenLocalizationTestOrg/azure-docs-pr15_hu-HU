<properties 
   pageTitle="Közzétételi varázsló Azure alkalmazás |} Microsoft Azure"
   description="Azure alkalmazás varázsló közzététele"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publish-azure-application-wizard"></a>Azure alkalmazás varázsló közzététele

## <a name="overview"></a>– Áttekintés

Után a Visual Studióban webalkalmazás fejleszt, az alkalmazás-Azure felhőszolgáltatásba könnyebben közzéteheti az **Azure-alkalmazások közzététele** varázslóval. Az első szakasz lépéseit ismerteti, hogy előtt ki kell töltenie a varázslóval, és a hátralévő szakaszok ismertetik a varázsló funkcióját.

>[AZURE.NOTE] Ez a témakör az üzembe helyezése a felhőbeli szolgáltatásokhoz, a webhelyek nem. A webhelyekhez való telepítéséről megtudhatja, [hogy miként az Azure-webhely üzembe](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Előfeltételek

A webalkalmazás Azure közzététele, előtt kell lennie, Microsoft-fiók és egy Azure-előfizetést, és a webes alkalmazás társíthatja az Azure felhőszolgáltatásba van. Ha már végzett az alábbi műveleteket, kihagyhatja, az alábbi szakasszal.

1. Ismerkedés a Microsoft-fiók és egy Azure-előfizetést. Próbálja meg ingyenes, egy hónap ingyenes Azure előfizetés [Itt](https://azure.microsoft.com/pricing/free-trial/)

1. Azure egy felhőalapú szolgáltatásba, és a tárterület-fiók létrehozása. Ez a Visual Studióban, vagy az [Azure klasszikus portal](http://go.microsoft.com/fwlink/?LinkID=213885)Server Intézőből teheti meg.

1. Engedélyezze a webalkalmazás az Azure. Ha engedélyezni szeretné a Visual Studio Azure közzétételének webalkalmazás, kell társítása Azure felhőalapú szolgáltatás projektté a Visual Studio. A kapcsolódó felhőalapú szolgáltatás projektet létrehozni, nyissa meg a helyi menü a projekt a webes alkalmazáshoz, és válassza az átalakítás, **Azure felhőalapú szolgáltatás Project átalakítása**.

1. Miután a felhőbe szolgáltatás project bekerül a megoldás, nyissa meg ismét ugyanazt a helyi menü, és válassza a **Közzététel**. Arról, hogy miként engedélyezheti az alkalmazásokat az Azure további tudnivalókért lásd: [hogyan: áttelepítése és a Visual Studio Ha egy felhőalapú szolgáltatásba, Azure webalkalmazás közzététele](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Ügyeljen arra, hogy a Visual Studio Kiindulás rendszergazdai hitelesítő adatok (Futtatás csoportházirend-rendszergazdaként).

1. Ha készen áll az alkalmazás közzétenni, nyissa meg a helyi menü az Azure felhőalapú szolgáltatás projekt, és válassza a **Közzététel**. Az alábbi lépéseket az Azure-alkalmazások közzététele varázsló megjelenítése.

## <a name="choosing-your-subscription"></a>Az előfizetés kiválasztása

### <a name="to-choose-a-subscription"></a>Válassza ki az előfizetés

1. A varázsló első alkalommal használata, előtt be kell jelentkeznie. Válassza a **Bejelentkezés** hivatkozásra. Jelentkezzen be az Azure portálra, amikor a rendszer kéri, és adja meg az Azure felhasználónevét és jelszavát. 

    ![Ez az egyik a közzétételi varázsló képernyők](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    A választható előfizetések feltölti a előfizetéssel társított fiókja. Jelenhet meg előfizetéseket, amely az előzőleg importált előfizetés fájlokat is.

1. **Az előfizetés kiválasztása** listájában válassza az előfizetést, hogy a telepítéshez használni.

   Ha úgy dönt, hogy **<... kezelése >** **Előfizetések kezelése** párbeszédpanel, és megadhatja a használni kívánt előfizetést, és a felhasználói fiók. A **fiókok** lapon látható az összes a fiókok, és az **előfizetések** lapon látható az összes, az előfizetések egyikével társított a fiókok. Választhatja a régió az Azure források, valamint létrehozása és az előfizetéshez tartozó tanúsítványok importálása az Azure portálról is. Ha előfizetése importált előfizetés fájlból, a társított tanúsítványok jelenik meg a **tanúsítványok** lapon. Amikor elkészült, válassza a **Bezárás** gombra.

    ![Előfizetések kezelése](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Előfizetés fájl tartalmazhatnak egynél több előfizetés.

1. Válassza a **Tovább** gombra. 

    Ha nincsenek olyan felhőszolgáltatások az előfizetését, meg kell egy felhőalapú szolgáltatásba létrehozása az Azure tárolni a projektben. A **felhőalapú szolgáltatás hozzon létre és tároló fiók** párbeszédpanel jelenik meg.

    Adja meg az új nevet adhat a felhőalapú szolgáltatást. A név Azure egyedinek kell lennie. Ezután adja meg a régió vagy egy adatközpont, amely Ön vagy az ügyfelek a legtöbb affinitás csoportot. Ez a név is szolgál a felhőalapú szolgáltatás létrehoz Azure tároló új fiókba.

1. Módosítsa a minden kívánt beállításokat a telepítéshez majd közzétehető a **Közzététel** gombra (a következő szakaszban biztosít a különféle beállításokkal kapcsolatos részletekért) kiválasztásával. Tekintse át a beállításokat a közzététel előtt, válassza a **Tovább** gombra.

    >[AZURE.NOTE] Ha ezt a lépést a közzététel lehetőséget választja, a Visual Studio telepítési állapotának figyelheti meg.

Gyakori és a Speciális beállítások telepítés az **Azure-alkalmazások közzététele** varázsló használatával módosíthatók. Ha például megadhatja a felengedése előtt azt az alkalmazást egy tesztkörnyezetben telepítendő beállítás. Az alábbi ábrán az **Általános beállítások** lapon az Azure telepítéshez.

![Általános beállítások](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Konfigurálása a Közzététel beállításai

### <a name="to-configure-the-publish-settings"></a>A közzététel beállításainak konfigurálása

1. A **Felhőbeli szolgáltatástól** listában végezze el a következő lépésekkel adatsor egyikét:

   1. A legördülő listából válassza ki egy meglévő felhőalapú szolgáltatást. A szolgáltatás az adatok központ hely megjelenik. Megjegyzés: ezen a helyen kell, és győződjön meg arról, hogy az azonos adatközpont a fiók tárolóhelyen van.

    1. **Hozzon létre új** hozhat létre egy felhőalapú szolgáltatásba, hogy Azure hosts kiválasztása **Felhőalapú szolgáltatás hozzon létre** párbeszédpanelen nevezze el a szolgáltatás, és adja meg a régió vagy affinitás csoportban adja meg az Adatközpont felhőalapú szolgáltatást tárolni kívánt helyét. A név Azure egyedinek kell lennie.

1. A **környezet** listában válassza a **gyártási** vagy **átmeneti**. Válassza a fejlesztői környezet, ha azt szeretné, egy tesztkörnyezetben az alkalmazás telepítéséhez. Az alkalmazás az éles üzemi környezetben később léphet.

1. A **konfiguráció összeállítása** listában válassza a **hibakeresési** vagy **Release**.

1. A **szolgáltatás konfigurációja** listában válassza a **felhőben** vagy a **helyi**.

    Ha tudja a szolgáltatás távolról csatlakozni szeretne, jelölje be az **Összes szerepkörének távoli asztal engedélyezése** jelölőnégyzetet. Ez a beállítás elsősorban az alábbiakkal. Ha bejelöli ezt a jelölőnégyzetet, a **Távoli asztali beállítása** párbeszédpanel jelenik meg. Válassza a beállításainak módosításához a beállítások hivatkozásra.

    Jelölje be a **Webes üzembe engedélyezése minden webes szerepkörök** jelölőnégyzetet, ahhoz, hogy a szolgáltatás web telepítésének. Engedélyezni kell a szolgáltatás használatához a távoli asztal. További tudnivalókért lásd: a [[közzétételi egy felhőalapú szolgáltatásba, az Azure eszközeivel](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). További információt a webhely üzembe olvassa el a [[közzétételi egy felhőalapú szolgáltatásba, az Azure-eszközök használatával](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx)című témakört.

1. Kattintson a **Speciális beállítások** fülre. A **telepítési címke** mezőben elfogadhatja az alapértelmezett nevet, vagy írja be a nevét. A dátum hozzáfűzése a telepítési címkét, hagyja a jelölőnégyzet be van jelölve.

    ![A közzétételi varázsló harmadik képernyőjén](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. **Tárterület fiók** listájában válassza a tárterület-fiókot a telepítéshez használni. Összehasonlíthatja a felhőalapú szolgáltatásba, és a tárterület-fiók a adatközpontokban helye. Ideális esetben ezekre a helyekre azonosnak kell lennie.

    >[AZURE.NOTE] Az Azure tárterület-fiókot a csomagot, az alkalmazás telepítéshez tárolja. Az alkalmazás telepítve van, a tárterület-fiókból a csomag program törli.

1. Jelölje be a **telepítés frissítése** jelölőnégyzetet, ha azt szeretné, hogy csak a frissített összetevők telepítése. Lehet, hogy ilyen típusú telepítési gyorsabb, mint egy teljes telepítés. Válassza a **Beállítások** hivatkozásra kattintva nyissa meg a **telepítési beállítások frissítése** párbeszédpanelen az alábbi ábrán látható módon. 

    ![Telepítési beállítások](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    Frissítés telepítéshez, egyre növekvő tendenciát mutat vagy egyidejű két lehetőség közül választhat. Növekményes telepítés, hogy az alkalmazás online és a felhasználók számára elérhető marad frissíti egyszerre egy telepített példányt. Egyidejű telepítés egyszerre frissíti az összes telepített példányát. Egyidejű frissítése gyorsabb növekményes frissítést, de ha ezt a beállítást választja, az alkalmazás előfordulhat, hogy nem érhető el a frissítési folyamat során.

    Kell jelölje be a jelölőnégyzetet, az, ha a telepítő nem frissíthető, ha azt szeretné, hogy a teljes példányban kerüljön sor automatikusan, ha a frissítés telepítése sikertelen, végezze el a teljes telepítés. Egy teljes telepítési visszaállítja a felhőbeli szolgáltatástól virtuális IP (virtuális) címét. További tudnivalókért lásd: [Útmutató: a állandó virtuális IP-cím őrizni egy felhőalapú szolgáltatásba](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Szeretné hibakeresése a szolgáltatást, jelölje be a **IntelliTrace engedélyezése** jelölőnégyzetet, vagy ha a **hibakeresési** konfiguráció telepít, és szeretné hibakeresése az Azure felhőalapú szolgáltatásba, jelölje be az **Összes szerepkörének távoli Debugger engedélyezése** jelölőnégyzetet, a távoli hibakeresési szolgáltatásainak üzembe.

2. Az alkalmazás profil, jelölje be a **adatgyűjtés engedélyezése** jelölőnégyzetet, és válassza a **Beállítások** hivatkozásra a profilkészítési lap beállításainak megjelenítéséhez. 


    >[AZURE.NOTE] Kell használnia a Visual Studio Ultimate IntelliTrace vagy a réteg kapcsolati adatainak összegyűjtése (TIP), és nem lehet egyszerre mindkét engedélyezni.

    További tudnivalókért lásd: [hibakeresése egy közzétett Felhőszolgáltatásba IntelliTrace és a Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx) és [tesztelése a egy felhőalapú szolgáltatásba teljesítményét](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Válassza ki a **következő** gombot az alkalmazás az összesítő lap megjelenítéséhez.

## <a name="publishing-your-application"></a>Az alkalmazás közzététele

1. Megadhatja, hogy a választott beállítások közzétételi profil létrehozása. Létrehozhat például egy tesztkörnyezetben egy profilt, és egy másikat a termelési. A profil mentéséhez válassza a **Mentés** ikonra. A varázsló létrehozza a profil, és menti a Visual Studio projekt. Ha módosítani szeretné a profilnév, nyissa meg a **cél profil** listát, és válassza a **<... kezelése >**.

    ![A közzétételi varázsló összefoglaló képernyő](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] A közzétételi profil megjelenik a Visual Studio megoldás Intézőben, és a profilbeállítások .azurePubxml kiterjesztésű fájlba kerülnek. XML-címkéket attribútumainak menti a beállításai.

1. Válassza a **Közzététel** az alkalmazás közzététele lehetőséget. A folyamat állapota a **kimeneti** ablakban a Visual Studióban figyelheti meg.

## <a name="see-also"></a>Lásd még:

[Útmutató: áttelepítése és a Visual Studio-Azure Felhőszolgáltatásba webalkalmazás közzététele](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Közzététel egy felhőalapú szolgáltatásba, az Azure-eszközök használatával](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[A közzétett Felhőszolgáltatásba IntelliTrace és a Visual Studio hibakeresése](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[Egy felhőalapú szolgáltatásba teljesítményének tesztelése](https://msdn.microsoft.com/library/azure/hh369930.aspx)

