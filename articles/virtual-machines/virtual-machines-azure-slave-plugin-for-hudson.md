<properties
    pageTitle="A beépülő modul Azure kisegítő használata Hudson folyamatos integrációval |} Microsoft Azure"
    description="A beépülő modul Azure kisegítő használatát Hudson folyamatos integrációval ismerteti."
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

# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a>A beépülő modul Azure kisegítő használata Hudson folyamatos integrációval

Az Azure kisegítő Hudson beépülő modul lehetővé teszi, hogy kiépítése Azure kisegítő csomópontok, amikor futó elosztott hoz létre.

## <a name="install-the-azure-slave-plug-in"></a>Az Azure kisegítő beépülő modul telepítése

1. Kattintson a Hudson irányítópult **Hudson kezelése**.

1. A **Hudson kezelése** lapon kattintson a **Bővítmények kezelése**.

1. Kattintson a **rendelkezésre álló** fülre.

1. Kattintson a **Keresés** gombra, és írja be az **Azure** korlátozhatja a megfelelő bővítmények listára.

    Ha úgy dönt, görgesse végig a rendelkezésre álló bővítmények találja az Azure kisegítő beépülő modul **kezelő és elosztott Szerkesztés** csoportjában a **mások** lapon.

1. Jelölje be az **Azure kisegítő bővítményt**.

1. Kattintson a **telepítés**gombra.

1. Indítsa újra a Hudson.

Most, hogy a beépülő modul telepítve van, akkor a következő lépésekkel állítható be a beépülő modul az Azure előfizetés profilját, és a virtuális kisegítő csomópont létrehozásához használt sablon létrehozása.

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

            Id="<Subscription ID>"

            Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

        </PublishProfile>

    </PublishData>

Ha befejezte az előfizetés profilját, kövesse az alábbi lépéseket a beépülő modul Azure kisegítő konfigurálása.

1. Kattintson a Hudson irányítópult **Hudson kezelése**.

1. Kattintson a **rendszer konfigurálása**.

1. Görgessen lefelé a **felhőben** szakaszának lapra.

1. Kattintson a **Új felhő hozzáadása > a Microsoft Azure**.

    ![új felhő hozzáadása][add new cloud]

    Ez jelennek meg a mezőket, ahol hozzá kell adni a az előfizetés részletei.

    ![profil beállítása][configure profile]

1. Másolja az előfizetés azonosító és a kezelés tanúsítvány előfizetés profilját, és illessze be őket a megfelelő mezőket.

    Ha az előfizetés azonosító és a kezelés tanúsítvány másol, **nem** az idézőjelekre, hogy az érték.

1. Kattintson a **konfiguráció ellenőrzése**.

1. A konfiguráció sikeresen ellenőrzését követően kattintson a **Mentés**gombra.

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a>Virtuális gép sablon beállítása az Azure kisegítő a beépülő modul

Virtuális gép sablon a beépülő modul használatával hozzon létre egy kisegítő csomópont Azure paramétereit határozza meg. Az alábbi lépéseket a azt létre sablont egy Ubuntu virtuális.

1. Kattintson a Hudson irányítópult **Hudson kezelése**.

1. Kattintson a **rendszer konfigurálása**.

1. Görgessen lefelé a **felhőben** szakaszának lapra.

1. A **felhőben** szakaszok **Hozzáadása Azure virtuális gép sablon** keresése, és kattintson a **Hozzáadás** gombra.

    ![virtuális sablon hozzáadása][add vm template]

1. Adja meg a felhőalapú szolgáltatás nevét a **név** mezőbe. Ha egy meglévő felhőalapú szolgáltatást megadott név utal, a virtuális a szolgáltatásban kell építenie. Egyéb esetben az Azure hoz létre egy újat.

1. A **Leírás** mezőben adja meg a sablon hoz létre leíró szöveget. Ezt az információt csak az írásos célra, és nem használja egy virtuális kiépítési.

1. A **címkék** mezőben adja meg a **Linux rendszerhez**. Ez a címke azonosítja a sablon hoz létre, és ezt követően használják a Hudson feladat létrehozása a sablon hivatkozni.

1. Jelölje ki egy területet, ahol hozható létre a virtuális.

1. Válassza ki a megfelelő virtuális méretét.

1. Adjon meg egy hol hozható létre a virtuális tárterület-fiókot. Győződjön meg arról, hogy az ugyanabban a régióban, mint a felhőbeli szolgáltatástól el. Ha azt szeretné, hogy új tároló létrehozni, akkor is hagyja üresen a mezőt.

1. Adatmegőrzési idő perc, mielőtt Hudson törli az üresjárati kisegítő számát adja meg. Hagyja üresen ezt a 60 az alapértelmezett értéket.

1. A **használatát**jelölje ki a megfelelő feltételt, ha kisegítő csomópont szolgálnak. Most jelölje be **a lehető csomópont Utilize**.

    Ezen a ponton a űrlap nézne ki némileg hasonlóan, mint ez:

    ![sablon config][template config]

1. A **kép család vagy azonosító** meg kell adnia a rendszer képet a virtuális telepíthető. Jelölje ki a kép családok listájából, vagy adjon meg egy egyéni képe.

    Ha azt szeretné, jelölje be a kép családok listájából, írja be az első karakter (kis-és nagybetűket) a kép család nevét. Például **U** gépelés fog jelenítse meg Ubuntu kiszolgáló családok listáját. Miután kiválasztotta a listából, Jenkins kiépítésekor a virtuális az adott rendszer képet, hogy a család legújabb verzióját használja.

    ![Operációs rendszer családi lista][OS family list]

    Ha egyéni használni kívánt képet, írja be a saját kép nevével. Egyéni kép nevek nem jelennek meg lista, így Önnek nem kell ahhoz, hogy a nevének megfelelően van-e megadva.    

    Ebben az oktatóanyagban típusú **U** kattintva jelenítse meg a Ubuntu képek listáját, és válassza ki a **Ubuntu kiszolgáló 14.04 LTS**.

1. **Indítsa el a módszert**jelölje ki a **SSH**.

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

    A **Init parancsfájl** a virtuális létrehozását követően hajtani. Ebben a példában a parancsfájl telepíti az Java, mely számjegy és telepítsenek.

1. A **felhasználónév** és **jelszó** mezőbe írja be a használni kívánt értékeket a rendszergazdai fiók a virtuális létrehozott.

1. Kattintson a **Sablon ellenőrzése** ellenőrzéséhez, ha a megadott paramétereket érvényesek.

1. Kattintson a **Mentés**.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Azure kisegítő csomópontjait futó Hudson feladat létrehozása

Ebben a részben létre Azure kisegítő csomópontjait fog futni Hudson feladatot.

1. Kattintson a Hudson irányítópult **Új feladatot**.

1. Adja meg a létrehozandó feladat nevét.

1. A feladat típusa jelölje ki a **generál egy ingyenes stílusú szoftver feladatot**.

1. Kattintson az **OK gombra**.

1. A projekt beállítása lapon válassza a **korlátozás, ahol a projekt futtatását is lehetővé teszi**.

1. **Csomópontot, és a címke** menüt, és válassza ki a **linux** (azt megadott ezt a címkét a virtuális gép sablon létrehozása az előző szakaszban).

1. A **Szerkesztés** csoportban kattintson a **Hozzáadás összeállítása a lépést** , és válassza a **végrehajtás rendszerhéj**.

1. A következőt lecserélve **{a github fióknév}**, **{a projekt neve}**és **{a projekt címtárban}** megfelelő értékek, szerkesztése, és illessze be a szerkesztett parancsfájl a megjelenő területre.

        # Clone from git repo

        currentDir="$PWD"

        if [ -e {your project directory} ]; then

            cd {your project directory}

            git pull origin master

        else

            git clone https://github.com/{your github account name}/{your project name}.git

        fi

        # change directory to project

        cd $currentDir/{your project directory}

        #Execute build task

        ant

1. Kattintson a **Mentés**.

1. Hudson irányítópult lapon keresse meg a projekt csak létrehozott, és kattintson az **ütemezés a Szerkesztés** ikonra.

Hudson ezután az előző részben létrehozott sablonnal kisegítő csomópont létrehozása, és hajtsa végre a parancsfájlt, a feladat végrehajtásához az összeállítás lépésben megadott.

## <a name="next-steps"></a>Következő lépések

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[előfizetés-profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

