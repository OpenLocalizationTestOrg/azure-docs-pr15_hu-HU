<properties
   pageTitle="A tevékenységek kezelése rendszerben (MOBILE) megoldások |} Microsoft Azure"
   description="Megoldások bővítése műveletek Management csomagja (MOBILE) megadásával ügyfelek vehet fel a MOBILE munkaterület csomagolt management forgatókönyvet.  Ez a cikk részletesen ügyfelei és partnerei által létrehozott hogyan egyéni megoldások."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Adatkezelési megoldások műveletek Management csomagja (MOBILE) (előzetes verzió)

>[AZURE.NOTE]Az előzetes dokumentációjában management megoldások a MOBILE, amely jelenleg előzetes verzióban.    

Megoldások kezelése bővítése műveletek Management csomagja (MOBILE) megadásával ügyfelek vehet fel a környezetet csomagolt management forgatókönyvet.  A [Microsoft által nyújtott megoldások](../log-analytics/log-analytics-add-solutions.md)partnerek és ügyfelek hozhat létre saját környezetben használt management megoldások vagy a Közösség – ügyfelek számára hozzáférhetővé.

## <a name="finding-and-installing-management-solutions"></a>Keresése és kezelési megoldás telepítése
Több módon megkeresése és megoldások kezelése telepíti, az alábbi szakaszokban ismertetett módon.

### <a name="azure-marketplace"></a>Azure piactérről
A Microsoft által nyújtott megoldások kezelése és megbízható partnerek előfordulhat, hogy telepítve van az Azure-portálon Azure piactérről.

1. Jelentkezzen be az Azure-portálra.
2. A bal oldali ablaktáblában válassza a **további szolgáltatásokat**.
3. Görgessen le a **megoldások** vagy *megoldások* írja be a **Szűrés** párbeszédpanel.
4. Kattintson a **+ Hozzáadás** gombra.
5. Keresse meg a megoldások számára, amely érdekli böngészési, kattintson a **Szűrés** gombra, vagy a **Keresési Everthing** mezőbe írja be.
6. Kattintson a részletes adatok megjelenítése egy piactéren elérhető elemre.
4. Kattintson a **Létrehozás** a **Megoldás hozzáadása** ablak megnyitásához.
5. A szükséges információkat, például a [MOBILE munkaterület és automatizálási fiók](#oms-workspace-and-automation-account) nemcsak a paramétereket a megoldás értékei kéri.
6. Kattintson a **Create** a megoldás telepítése gombra.

### <a name="oms-portal"></a>MOBILE portál
A Microsoft által nyújtott Management megoldások előfordulhat, hogy telepítve van a megoldástárból a MOBILE portálon.

1. Jelentkezzen be a MOBILE portálon.
2. Kattintson a **Megoldástárból** csempére.
2. A megoldástár MOBILE lapon című témakörben olvashat minden elérhető megoldást. Kattintson a nevére a MOBILE hozzáadni kívánt megoldás.
3. A lapon az Ön által választott megoldás jelenik meg a megoldást részletes információt. Kattintson a **hozzáadása**gombra.
4. A megoldást, mely részén jelenjen meg az Áttekintés lapon a MOBILE, és használatba vehetik azt a MOBILE szolgáltatás folyamatok az adatok után felvett új csempéje.

### <a name="azure-quickstart-templates"></a>Azure quickstart útmutató sablonok
A Közösség tagjainak kezelése megoldások Azure quickstart útmutató sablonok küldhet.  Ezek a későbbi telepítéshez sablonok letöltése vagy vizsgálja meg őket, megtudhatja, hogy miként hozhat [létre egyéni megoldások](#creating-a-solution).

1. Ismertetett [MOBILE munkaterület és automatizálási fiók](#oms-workspace-and-automation-account) egy munkaterületi és a fiók, amelyre a hivatkozás folyamatot követik.
2. Nyissa meg az [Azure quickstart útmutató sablonokat](https://azure.microsoft.com/documentation/templates/).  
3. Keresse meg a megoldást, amely érdekli.
4. Válassza ki a megoldást az eredmények a részletek megtekintéséhez.
5. A **központi telepítés az Azure** gombra.
6. A rendszer kéri a megoldás paramétereket a szükséges információkat, például a erőforráscsoport és a hely értékek mellett.
7. Kattintson a **vásárlás** telepítheti a megoldást.

### <a name="deploy-azure-resource-manager-template"></a>Erőforrás-kezelő Azure-sablon terjesztése
A Közösség el vagy erőforrás-kezelő sablonként végrehajtását, hogy [saját maga hozza létre](#creating-a-solution) , és, amellyel a normál módszerek bármelyikével [üzembe helyezése a sablon](../resource-group-template-deploy-portal.md)megoldásokat.  Figyelje meg, hogy a megoldás a telepítés előtt kell létrehozni és a [MOBILE munkaterület és automatizálási fiók](#oms-workspace-and-automation-account)hivatkozás.

## <a name="oms-workspace-and-automation-account"></a>MOBILE munkaterület és automatizálási fiók
A legtöbb kezelési megoldások szükség egy [MOBILE munkaterület](../log-analytics/log-analytics-manage-access.md) tartalmazzák, nézetek és [automatizálási fiókot](../automation/automation-security-overview.md#automation-account-overview) , amely tartalmazhat runbooks és a kapcsolódó források. A munkaterület és a fiók kell az alábbi követelményeknek.

- Megoldás csak egy MOBILE munkaterület és automatizálási fiókok használhatók.  
- A MOBILE munkaterület és automatizálási fiók megoldást által használt kell csatolhatók egymáshoz. Egy MOBILE munkaterület csak függhet egyetlen automatizálási fiókkal, és automatizálási fiók csak függhet egyetlen MOBILE munkaterületet.
- Csatolt, a MOBILE munkaterület és automatizálási figyelembe kell lennie erőforráscsoport és a régió.  A kivétel egy kelet-USA-beli tartományban lévő MOBILE-munkaterülethez és automatizálási fiókja kelet-amerikai 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Hivatkozás egy MOBILE munkaterület és automatizálási-fiók létrehozása
Hogyan a MOBILE munkaterület ad meg, és automatizálási fiók függ, hogy a megoldás a telepítéssel.

- Ha telepíti a MOBILE portál Microsoft megoldást, az aktuális MOBILE munkaterület telepítve van, és a szolgáltatás nincs automatizálási fiókra szükség.

- Ha telepíti a Microsoft Azure piactéren megoldást, megjelenik egy kérdés egy MOBILE munkaterület és automatizálási fiókot, és a hivatkozást, közöttük létrejön.  

- A Microsoft Azure piactéren kívüli megoldásainak hozzá kell rendelnie a MOBILE munkaterület és automatizálási fiókot a megoldás telepítése előtt.  Ehhez minden megoldást a Microsoft Azure piactéren található, a MOBILE munkaterület és automatizálási fiók gomb kiválasztásával.  Nem kell ténylegesen telepíteni az a megoldást, mert a hivatkozás jön létre, amint az MOBILE munkaterület és automatizálási fiók van jelölve.  Amikor létrejött a hivatkozást, majd használhatja a MOBILE munkaterület és automatizálási fiók bármely megoldás. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>A hivatkozás egy MOBILE munkaterület és automatizálási-fiók ellenőrzése
Ellenőrizheti, hogy egy MOBILE munkaterület és az alábbi eljárást követve automatizálási fiók közötti kapcsolat.

1. Jelölje be az automatizálási fiókot az Azure-portálon.
2. Görgessen a **Beállítások** ablaktábla alján.
3. Ha egy szakasz **MOBILE források** nevű a **beállításai** ablakban, ehhez a fiókhoz csatolva van egy MOBILE munkaterülethez.
4. Jelölje ki a **munkaterület** az ehhez automatizálási fiókhoz MOBILE munkaterület részleteinek megtekintéséhez.


## <a name="listing-management-solutions"></a>Listaelem-megoldások kezelése
A következő eljárással az adatkezelési megoldásokkal tekintheti meg a munkaterületek csatolt Azure-előfizetéséhez.

1. Jelentkezzen be az Azure-portálra.
2. A bal oldali ablaktáblában válassza a **további szolgáltatásokat**.
3. Görgessen le a **megoldások** vagy *megoldások* írja be a **Szűrés** párbeszédpanel.
4. Telepítve van az összes a munkaterületek megoldások lesz látható.

Figyelje meg, hogy csak az aktuális munkaterület a MOBILE portálon telepített Microsoft megoldásokkal tekinthet meg.

## <a name="removing-a-management-solution"></a>A kezelési megoldás eltávolítása
Kezelési megoldás eltávolításakor összes erőforrásában található a megoldást is törlődnek.  

1. Keresse meg azt a megoldást az Azure-portálon a eljárás a [bejegyzésére a megoldások](#listing-solutions).
2. Jelölje ki az eltávolítani kívánt megoldást.
3. Kattintson a **Törlés** gombra.

## <a name="creating-a-management-solution"></a>A kezelési megoldás létrehozása
Adatkezelési megoldások teljes útmutatást a [tevékenységek kezelése csomagja (MOBILE) létrehozása solutions](operations-management-suite-solutions-creating.md)érhetők el. 


## <a name="next-steps"></a>Következő lépések

- Keresés [Azure quickstart útmutató sablonok](https://azure.microsoft.com/documentation/templates) minták Megnyitás más erőforrás-kezelő sablonokat.
- A saját [management megoldások](operations-management-suite-solutions-creating.md)létrehozása.
 