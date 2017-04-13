<properties 
    pageTitle="Azure tároló használata Jenkins folyamatos integrációs megoldást |} Microsoft Azure" 
    description="Ebben az oktatóanyagban bemutatják, hogyan használhatja az Azure blob-szolgáltatás, mint a összegyűjti Jenkins folyamatos integrációs megoldást által létrehozott eltérések összeállítása." 
    services="storage" 
    documentationCenter="java" 
    authors="dineshmurthy" 
    manager="jahogg" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Azure tárolási használata Jenkins folyamatos Integration megoldást

## <a name="overview"></a>– Áttekintés

Az alábbi információk segítségével Blob-tárolóhoz Jenkins folyamatos integrációs (ki) megoldást által létrehozott build eltérések a tár vagy a letölthető fájlok adatforrásként használni egy összeállítás folyamatban mutatja. Módon érheti el ez hasznos az esetek egyike, amikor Ön éppen kódolása a egy Agilis fejlesztői környezet (Java vagy az egyéb nyelvek használatával), és buildjeiben futó alapján folyamatos integrációs van szüksége a tárházba az összeállítás eltéréseket, hogy sikerült, például más szervezet tagjaival, az ügyfelekkel, megoszthatja őket vagy archívumot karbantartása. Egy másik példa esetén, a Szerkesztés feladatok magát más fájlok, például a beviteli Szerkesztés részeként letöltéséhez függőségek van szükség.

Ebben az oktatóanyagban fogja használni, akkor az Azure tárterület-bővítmények a Microsoft elérhetővé tett Jenkins ki.

## <a name="overview-of-jenkins"></a>Jenkins áttekintése ##

Jenkins lehetővé teszi, hogy a szoftver projekt folyamatos integrációs azáltal, hogy könnyen integrálása a kód módosításaikat fejlesztők és buildjeiben hoztak automatikusan és gyakran, ezáltal a fejlesztők termelékenység növelésének. A verziókat verzióval, és Szerkesztés eltérések is fel kell tölteni különböző tárházakban. Ez a témakör bemutatja, hogy hogyan Azure blob-tárolóhoz használni az összeállítás eltérések-tárházából. Azt is megjelennek az Azure blob-tárolóhoz függőségek letöltéséről.

[Értekezlet Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)Jenkins kapcsolatos további tudnivalók találhatók.

## <a name="benefits-of-using-the-blob-service"></a>A Blob-szolgáltatás használatának előnyei ##

Az Agilis fejlesztési build eltérések tárolni a Blob-szolgáltatás előnyei többek között:

- A Szerkesztés eltéréseket, illetve a letölthető függőségek magas elérhetősége.
- Ha a Jenkins ki megoldás feltöltések megjelenítése az összeállítás eltérések teljesítmény.
- Ügyfelei és partnerei letöltése az összeállítás eltérések teljesítményét.
- Szabályozási lehetőséget biztosít a felhasználói hozzáférés házirendek név nélküli hozzáférés, a lejárati-alapú átengedése aláírás access, a személyes közötti választási lehetőséget tartalmazó access stb.

## <a name="prerequisites"></a>Előfeltételek ##

Az alábbi módon a Blob-szolgáltatás használata a Jenkins ki megoldás lesz szüksége:

- Folytonos integrációs Jenkins megoldás.

    Ha jelenleg nem Jenkins ki megoldást, futtatását is lehetővé teszi a következő technikával Jenkins ki megoldást:

    1. Java-kompatibilis gép töltse le a jenkins.war <http://jenkins-ci.org>.
    2. Egy megnyitott azt a mappát, amely tartalmazza a jenkins.war parancssorba futtatása:

        `java -jar jenkins.war`

    3. A böngészőben nyissa meg a `http://localhost:8080/`. Ekkor megnyílik az Jenkins irányítópulton fog telepítse és állítsa be az Azure tároló beépülő modul használatával.

        Miközben tipikus Jenkins ki megoldást szeretne lett beállítva szolgáltatásként, Jenkins háború fut a parancssorból elegendő az ebben az oktatóanyagban lesz.

- Az Azure-fiók. Iratkozzon fel a az Azure-fiókjába a <http://www.azure.com>számára.

- Azure tároló fiókkal. Ha még nincs a tárterület-fiókot, létrehozhat egy a lépésekkel a [tárterület-fiók létrehozása](storage-create-storage-account.md#create-a-storage-account).

- A Jenkins ki megoldást ismerős ajánlott, de nem szükséges, az alábbi tartalom fogja használni egy egyszerű példa mutatja, hogy a lépéseket szükség, ha a Blob-szolgáltatás használata egy tárban tárolnak Jenkins ki az eltéréseket összeállítása.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Jenkins ki a Blob-szolgáltatás használata ##

Jenkins a Blob-szolgáltatás használatához kell telepítse az Azure tároló bővítményt, a beépülő modul tárterület-fiók konfigurálása és hozzon létre egy létrehozás után végrehajtandó műveletet, amely feltöltések megjelenítése az összeállítás eltérések a tárterület-fiókjába. Ezeket a lépéseket az alábbi szakaszok ismertetik.

## <a name="how-to-install-the-azure-storage-plugin"></a>Az Azure tároló beépülő modul telepítése ##

1. Jenkins irányítópult lapon kattintson az **Jenkins kezelése**gombra.
2. Az **Jenkins kezelése** lapon kattintson a **Bővítmények kezelése**lehetőséget.
3. Kattintson a **rendelkezésre álló** fülre.
4. **Eltérés Uploaders** csoportban jelölje be a **Microsoft Azure tárterület-bővítmények**.
5. Kattintson az **Újraindítás nélkül telepítése** vagy a **letöltése és telepítése után indítsa újra az**.
6. Indítsa újra a Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Azure tárhely a beépülő modul tárterület-fiók beállítása ##

1. Jenkins irányítópult lapon kattintson az **Jenkins kezelése**gombra.
2. Az **Jenkins kezelése** lapon kattintson a **Rendszer konfigurálása**.
3. A **Microsoft Azure tároló fiók konfigurálása** csoportban:
    1. Adja meg a fiók szerezhet be az [Azure-portálon](https://portal.azure.com)tárolási nevét.
    2. Írja be a tárhely fiókkulcs, az [Azure-portál](https://portal.azure.com)is elérhető.
    3. A nyilvános Azure felhő használatakor az alapértelmezett érték használata **Blob szolgáltatás végpontjának URL-CÍMÉT** . Különböző Azure felhő használja, ha használja az végpontot megadott az [Azure-portálra](https://portal.azure.com) a tárterület-fiókjához. 
    4. Kattintson a **érvényesítése tároló hitelesítő adatokat** a tárterület-fiók ellenőrzése lehetőségre. 
    5. [Választható] Rendelkezésére a Jenkins ki a kívánt további tárterület-fiókok esetén kattintson a **További tárterület-fiókot**.
    6. Kattintson a **Mentés** menteni szeretné a beállításait.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Hogyan hozhat létre egy létrehozás után végrehajtandó műveletet, amely feltöltések megjelenítése az összeállítás eltérések a tárterület-fiókjához ##

Utasítás célokra először azt kell hozzon létre egy projektet, amely több fájl létrehozása, majd ezt hozzáadhatja a utáni összeállítás műveletben a fájlok feltöltése a tárterület-fiókjába.

1. Jenkins irányítópult lapon kattintson az **Új elem**gombra.
2. Nevezze el a feladat **MyJob**kattintson a **free stílusú szoftver a projekt létrehozása**, és kattintson **az OK**gombra.
3. A feladat konfiguráció **Szerkesztés** csoportjában kattintson a **Hozzáadás összeállítása lépést** , és válassza a **végrehajtása Windows Köteg parancsot**.
4. A **parancs**a következő parancsokat használhatja:

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. A feladat konfiguráció **Utáni összeállítás műveletek** csoportjában kattintson a **Hozzáadás létrehozás után végrehajtandó művelet** , és válassza ki **a feltöltés Azure Blob-tárolóhoz eltérések**.
6. **Tárterület-fiók nevét**jelölje be a tárterület-fiók használata.
7. **Tároló nevét**adja meg a tároló nevét. (A tároló létrejönnek, ha azt még nem létezik, amikor az összeállítás eltérések van feltöltve.) Környezet változók használata, ezért ebben a példában adja meg ezt **a ${JOB_NAME}** tároló neveként.

    **Tipp:**
    
    A **parancs** az alábbi szakasz, ahol megadott parancsfájl **végrehajtása Windows Köteg parancs** képen a Jenkins által felismert környezet változók mutató hivatkozást. Kattintson a környezet változóinak nevében és a leírásokat megtudhatja, hogy hivatkozásra. Figyelje meg, hogy a környezeti változók, amely tartalmazhat speciális karaktereket, például a **BUILD_URL** környezeti változó nem engedélyezett tároló nevét vagy közös virtuális elérési út.

8. Kattintson az **Új tároló közzététel alapértelmezés szerint** az ebben a példában. (Ha privát tároló használni kívánt, kell hozzáférés engedélyezése átengedése aláírás létrehozása. Ez a témakör túlra. Többet is megtudhat a [Használatával megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md)átengedése aláírások.)
9. [Választható] Kattintson a **Karbantartás tároló feltöltése előtt** , ha azt szeretné, hogy a tárolóban a tartalomjegyzék törölni kell, mielőtt build eltérések van feltöltve (hagyja bejelölve, ha nem szeretné a tároló tartalmának törlése).
10. **Eltérések lista feltöltéséhez**, írja be a * *szöveg /*.txt**.
11. A **feltöltött eltérések közös virtuális útvonalon**, ebben az oktatóanyagban alkalmazásában adja meg az **${összeállítása\_azonosító} / ${összeállítása\_szám}**.
12. Kattintson a **Mentés** menteni szeretné a beállításait.
13. A Jenkins irányítópulton **Összeállítása most** **MyJob**futtatásához kattintson. Vizsgálja meg a konzol kimenetének státuszt. Azure tárolására állapotüzenetek szerepelni fog konzol kimeneti build eltérések töltse fel a létrehozás után végrehajtandó műveletet indításakor.
14. A feladat sikeres befejeztével vizsgálja meg a Szerkesztés eltérések úgy, hogy megnyitja a nyilvános blob.
    1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
    2. Kattintson a **tároló**elemre.
    3. Kattintson a tárolás Jenkins használt fiók nevére.
    4. Kattintson a **tárolók**parancsra.
    5. Kattintson a tároló nevű **myjob**, amely olyan, hogy a projekt nevét a Jenkins feladat létrehozásakor rendelt kisbetűs változatát. Tárolók és blob nevét, hogy kisbetűk (és kis-és nagybetűket) Azure-tárolóban lévő. BLOB-tároló **myjob** listájában meg kell jelennie **hello.txt** és **date.txt**. Másolja a vágólapra az URL-cím vagy ezeket az elemeket, és nyissa meg a böngészőben. Egy összeállítás eltérés, látni fogja a szövegfájlt, amely feltöltése.

Csak egy olyan létrehozás után végrehajtandó művelet, amelyet a feltöltések megjelenítése az eltéréseket az Azure blob-tárolóhoz Feladatonkénti hozhat létre. Figyelje meg, hogy a egyetlen utáni műveletet eltérések feltöltése Azure blob-tárolóhoz adhatja meg (beleértve a helyettesítő karakterek) más fájlokat és a fájlok belül **Eltérések lista feltöltéséhez** pontosvesszőt a használják elválasztó elérési út. Például ha a Jenkins build hoz létre, üveg és a munkaterület **összeállítása** mappában TXT fájlokat, és fel szeretné tölteni az Azure blob-tárolóhoz mindkét, használja a következő **Eltérések lista feltöltéséhez** értékét: **összeállítása /\*.jar; Szerkesztés /\*.txt**. Dupla kettőspont szintaxis belül blob neve használandó elérési útja is használhatja. Például ha azt szeretné, hogy a kancsó **bináris** blob elérési útvonalát és a TXT-fájlok használatával első feltöltött **hirdetmények** használata blob elérési első tölteni, használja a következő **Eltérések lista feltöltéséhez** értékét: **összeállítása /\*. jar::binaries; Szerkesztés /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Hogyan hozhat létre, amely az Azure blob-tárolóhoz letöltések build lépés ##

A következő lépések bemutatják, hogyan elemek letöltése Azure blob-tárolóhoz build lépés konfigurálása. A kívánt elemek szerepeljenek a fejlesztése, ha például kancsó adó az Azure blob-tároló segítségére lehetnek.

1. A feladat konfiguráció **Szerkesztés** csoportjában kattintson a **Hozzáadás összeállítása lépést** , és válassza a **Töltse le a Azure Blob-tárolóhoz**.
2. **Tárterület-fiók nevét**jelölje be a tárterület-fiók használata.
3. **Tároló nevét**adja meg a nevét, amelynek a letölteni kívánt BLOB-tároló. Környezet változót is használhatja.
4. **Blob nevét**adja meg a blob-nevét. Környezet változót is használhatja. Emellett a kezdeti betűjét vagy a blob nevének megadása után helyettesítő karakterként egy csillag, az is használhatja. Ha például **project\* ** módon adja meg a **Projekt**kezdődő összes BLOB.
5. [Választható] **Töltse le az elérési utat**adja meg az elérési utat a kívánt helyre kattintva töltse le a fájlok Azure blob-tárolóhoz Jenkins gépen. Környezet változót is használható. (Ha nem ad értéket az **elérési út letöltése**, a fájlokat az Azure blob-tárolóhoz letölt a feladat munkaterületre.)

Ha további elemekre töltse le a Azure blob-tárolóhoz, összeállítás további lépéseket is létrehozhat.

Építés futtatása után ellenőrizze az összeállítás előzmények konzol kimeneti, vagy nézze meg a letöltési helye kattintva megtekintheti, hogy a várt BLOB sikeresen letöltött.  

## <a name="components-used-by-the-blob-service"></a>A Blob-szolgáltatás által használt összetevők ##

Az alábbi áttekintést nyújt a Blob-szolgáltatás összetevőit.

- **Tárterület-fiók**: Azure tárolóhoz összes access befejeződött a tárterület-fiókon keresztül. Ez az a legmagasabb szintű a névtér BLOB eléréséhez. Fiók tárolók, tetszőleges számú is tartalmazhatnak, mindaddig, amíg a teljes méret csoportban 100 TB mennyiségben.
- **Tároló**: tároló tartalmaz egy sor olyan BLOB csoportja. Az összes BLOB tároló kell lennie. Fiók tárolók tetszőleges számú is tartalmazhat. A tároló BLOB tetszőleges számú tárolhat.
- **BLOB**: bármilyen típusú és méretű fájlok. Azure-tárolóban lévő tárolt BLOB két típusa van: tiltása és lap BLOB. A legtöbb fájlok továbbfejlesztett fájlblokkolás BLOB. Egyetlen blokk blob legfeljebb 200GB méretű lehet. Ebben az oktatóanyagban blokk BLOB használja. Lap BLOB, egy másik blob-típus legfeljebb 1TB méretét, és hatékonyabb, lehet, ha a fájlban bájt cellatartományok gyakran módosulnak. BLOB kapcsolatos további tudnivalókért [ismertetése blokk BLOB, a Hozzáfűzés BLOB és a lap BLOB](http://msdn.microsoft.com/library/azure/ee691964.aspx)témakörben talál.
- **URL-formátuma**: BLOB címmel rendelkező, az alábbi URL-cím formátumban:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    (A nyilvános Azure felhő vonatkozik a fenti formátumot. Különböző Azure felhő használatakor segítségével a végpont belül az [Azure-portálon](https://portal.azure.com) határozza meg az URL-cím végpontot.)

    A fenti formátumban `storageaccount` tárterület-fiókja nevét `container_name` a tárolót, nevét és `blob_name` a kurzor a blob, nevét jelenti. A tároló néven belül, akkor is több út perjellel, pontosvesszővel **/**. Ebben az oktatóanyagban példanév tároló lett **MyJob**, és **${összeállítása\_azonosító} / ${összeállítása\_szám}** használta a közös virtuális elérési utat, így a problémát a következőképp URL-cím blob:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Következő lépések

- [Értekezlet Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
- [Azure tárolási Java SDK](https://github.com/azure/azure-storage-java)
- [Azure tároló ügyfél SDK hivatkozás](http://dl.windowsazure.com/storage/javadoc/)
- [Azure tárolása REST API-val](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Azure tároló csapatának blogja](http://blogs.msdn.com/b/windowsazurestorage/)

További információt is megjelenik a [Java Developer Center](https://azure.microsoft.com/develop/java/).