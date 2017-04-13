<properties
    pageTitle="A beépülő modul Azure kisegítő használata Jenkins folyamatos integrációval |} Microsoft Azure"
    description="A beépülő modul Azure kisegítő használatát Jenkins folyamatos integrációval ismerteti."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="how-to-use-the-azure-slave-plug-in-with-jenkins-continuous-integration"></a>A beépülő modul Azure kisegítő használatáról a folyamatos Jenkins-integrációval

Is használhatja a beépülő modul Azure kisegítő Jenkins az Azure rendelkezést kisegítő csomópontok amikor futó elosztott hoz létre.

## <a name="install-the-azure-slave-plug-in"></a>Az Azure kisegítő beépülő modul telepítése

1. Kattintson a Jenkins irányítópult **Jenkins kezelése**.

1. A **Jenkins kezelése** lapon kattintson a **Bővítmények kezelése**lehetőséget.

1. Kattintson a **rendelkezésre álló** fülre.

1. A szűrő mezőben a rendelkezésre álló bővítmények listája felett írja be az **Azure** korlátozhatja a megfelelő bővítmények listában.

    Ha úgy dönt, görgesse végig a rendelkezésre álló bővítmények találja az Azure kisegítő beépülő modul **kezelő és elosztott összeállítása** csoportjában.

1. Jelölje be az **Azure kisegítő beépülő modul** jelölőnégyzetet.

1. Kattintson a **telepítés újraindítása nélkül** , vagy **letöltése és telepítése után indítsa újra az**.

Most, hogy a beépülő modul telepítve van, a következő lépésekkel a beépülő modul állítható be az Azure előfizetés profilját, és létrehozásához használt sablon létrehozása a kisegítő csomópontot a virtuális gép.


## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a>A beépülő modul Azure kisegítő előfizetés profilját konfigurálása

Előfizetés profil is hivatkoznak, mivel közzétételi beállításokat, az XML-fájlban, amely tartalmazza a biztonságos hitelesítő adatait, és néhány további információt a fejlesztői környezet Azure munkához kell. A beépülő modul Azure kisegítő konfigurálásához van szükség:

* Az előfizetés azonosítója
* Előfizetéshez tartozó adatkezelési tanúsítvány

Ezek a [előfizetés profil]találhatók. Az alábbi képen egy előfizetés profil.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

        <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

            ServiceManagementUrl="https://management.core.windows.net"

            Id="<Subscription ID value>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Után előfizetés profilját, kövesse az alábbi lépéseket a beépülő modul Azure kisegítő konfigurálása:

1. Kattintson a Jenkins irányítópult **Jenkins kezelése**.

1. Kattintson a **rendszer konfigurálása**.

1. Görgessen lefelé a **felhőben** szakaszának lapra.

1. Kattintson a **Új felhő hozzáadása > a Microsoft Azure**.

    ![felhőalapú szakasz][cloud section]

    Ez jelennek meg a mezőket, ahol hozzá kell adni a az előfizetés részletei.

    ![előfizetés konfigurálása][subscription configuration]

1. Másolja a vágólapra az előfizetés azonosító és a kezelés tanúsítvány értékeket előfizetés profilját, és illessze be őket a megfelelő mezőket.

    Az előfizetés azonosító és a kezelés tanúsítvány másolásakor ne kerüljön bele az árajánlatok, hogy az érték.

1. Kattintson a **konfigurációjának ellenőrzése**gombra.

1. A konfiguráció helyes ellenőrzését követően kattintson a **Mentés**gombra.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Virtuális gép sablon beállítása az Azure kisegítő a beépülő modul

Virtuális gép sablon, amelyekkel a beépülő modul kisegítő csomópont létrehozása az Azure a paraméterek határozza meg. Az alábbi lépéseket a azt fogja sablont hozhat létre egy Ubuntu virtuális gépen.

1. Kattintson a Jenkins irányítópult **Jenkins kezelése**.

1. Kattintson a **rendszer konfigurálása**.

1. Görgessen lefelé a **felhőben** szakaszának lapra.

1. A **felhő** csoportban **Hozzáadása Azure virtuális gép sablon**keresése, és kattintson a **Hozzáadás**gombra.

    ![virtuális sablon hozzáadása][add vm template]

    Ez a mezők, amelyekben meg a sablon hoz létre részleteket jelennek meg.

    ![üres általános konfigurálása][blank general configuration]

1. A **név** mezőbe írja be az Azure felhőalapú szolgáltatás neve. Ha egy meglévő felhőalapú szolgáltatást a megadott név utal, a kiépítéstől a szolgáltatás a virtuális gépen kell. Egyéb esetben az Azure hoz létre egy újat.

1. A **Leírás** mezőben adja meg a sablon hoz létre leíró szöveget. Csak az a rekordot, és nem szerepel a kiépítési virtuális géphez.

1. **A lista** azonosítja a sablon hoz létre, és ezt követően használják a Jenkins feladat létrehozása a sablon hivatkozni. A célra írja be a **linux** ebbe a mezőbe.

1. A **terület** listában kattintson a régió, ahol a virtuális gép létrejön.

1. **Virtuális gép méret** listában kattintson a megfelelő méretet.

1. A **Tárterület-fiók neve** mezőbe adjon meg egy tárterület-fiókot, ahol hozható létre a virtuális gép. Győződjön meg arról, hogy az ugyanabban a régióban, mint a felhőbeli szolgáltatástól el. Ha azt szeretné, hogy új tároló létrehozni, akkor hagyja ebbe a mezőbe üres.

1. Adatmegőrzési időt adja meg, hogy hány perc, mielőtt Jenkins törli az üresjárati kisegítő. Hagyja üresen ezt a 60 az alapértelmezett értéket. Választhatja állítsa le a kisegítő törlés helyett inkább tétlen állapotában is. Ehhez jelölje be a **Leállítás csak (nem törli) után adatmegőrzési idő** jelölőnégyzetet.

1. **Használatát** listájában válassza a megfelelő feltétel, amikor a kisegítő csomópont szolgálnak. Most kattintson a **Utilize a lehető csomópontot**.

    Az űrlap ezen a ponton a némileg hasonlóan kell kinéznie:

    ![ellenőrzés általános sablon config][checkpoint general template config]

    A következő lépésként adja meg a részleteket az operációs rendszer képe, amelyet a kisegítő, létre kell hozni.

1. A **kép család vagy az azonosító** mezőbe meg kell adnia a rendszer képet telepíti a virtuális gépen. Jelölje ki a kép családok listájából, vagy adjon meg egy egyéni képe.

    Ha azt szeretné, jelölje be a kép családok listájából, írja be az első karakter (kis-és nagybetűket) a kép család nevét. Például **U** gépelés fog jelenítse meg Ubuntu kiszolgáló családok listáját. Miután kiválasztotta a listából, Jenkins kiépítésekor a virtuális gép az adott rendszer képet, hogy a család legújabb verzióját használja.

    ![Operációs rendszer kép lista minta][OS Image list sample]

    Ha egyéni használni kívánt képet, írja be a saját kép nevével. Egyéni kép nevek nem jelenik a listában, így Önnek nem kell ahhoz, hogy a nevének megfelelően van-e megadva.

    Ebben az oktatóanyagban írja be a **U** Ubuntu képek listájának megjelenítéséhez, és válassza a **Ubuntu kiszolgáló 14.04 LTS**.

1. Az **Indítási mód** listában kattintson a **SSH**.

1. Másolja az alábbi parancsfájl, és illessze be a **Init parancsfájl** mezőbe.

        # Install Java

        sudo apt-get -y update

        sudo apt-get install -y openjdk-7-jdk

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y openjdk-7-jdk

        # Install git

        sudo apt-get install -y git

        #Install ant

        sudo apt-get install -y ant

        sudo apt-get -y update --fix-missing

        sudo apt-get install -y ant

    A init parancsfájl végrehajtása után a virtuális gép jön létre. Ebben a példában a parancsfájl telepíti az Java, mely számjegy és telepítsenek.

1. A **felhasználónév** és **jelszó** mezőjébe írja be a használni kívánt értékeket a rendszergazdai fiók a virtuális gépen létrehozott.

1. Kattintson a **Sablon ellenőrzése** ellenőrzéséhez, ha a megadott paramétereket érvényesek.

1. Kattintson a **Mentés**gombra.


## <a name="create-a-jenkins-job-that-runs-on-a-slave-node-on-azure"></a>Azure kisegítő csomópontjait futó Jenkins feladat létrehozása

Ebben a részben létre Azure kisegítő csomópontjait fog futni Jenkins feladatot. A saját project GitHub követheti, hogy telepítve kell.

1. A Jenkins irányítópulton **Új elem**gombra.

1. Írja be egy nevet a tevékenység hoz létre.

1. Kattintson a projekt típusa **Freestyle projekt**.

1. Kattintson az **OK gombra**.

1. Tevékenység beállítása lapon válassza a **korlátozás, ahol a projekt futtatását is lehetővé teszi**.

1. A **Címke-kifejezést** mezőben adja meg a **Linux rendszerhez**. Az előző részben létrehozott, hogy nevű **linux**, amely olyan, mi megadjuk itt kisegítő sablon.

1. A **Szerkesztés** csoportban kattintson a **Hozzáadás összeállítása a lépést** , és válassza a **végrehajtás rendszerhéj**.

1. A következőt lecserélve **(a GitHub fióknév)**, **(a projekt neve)**és **(a project könyvtár)** megfelelő értékek, szerkesztése, és illessze be a szerkesztett parancsfájl a megjelenő területre.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e (your project directory) ]; then

            cd (your project directory)

            git pull origin master

        else

            git clone https://github.com/(your GitHub account name)/(your project name).git

        fi

        # change directory to project

        cd $currentDir/(your project directory)

        #Execute build task

        ant

1. Kattintson a **Mentés**gombra.

1. A Jenkins irányítópulton mutasson a az imént létrehozott feladatot, és kattintson a lefelé mutató nyílra a tevékenység lap beállításainak megjelenítéséhez.

1. Kattintson a **Létrehozás most**gombra.

Jenkins ezután kisegítő csomópont létrehozása a sablon az előző részben létrehozott használatával, és hajtsa végre a parancsfájlt, a feladat végrehajtásához az összeállítás lépésben megadott.

## <a name="next-steps"></a>Következő lépések

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[előfizetés-profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[cloud section]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-cloud-section.png
[subscription configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-account-configuration-fields.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-add-vm-template.png
[blank general configuration]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration-blank.png
[checkpoint general template config]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-slave-template-general-configuration.png
[OS Image list sample]: ./media/virtual-machines-azure-slave-plugin-for-jenkins/jenkins-os-family-list-sample.png