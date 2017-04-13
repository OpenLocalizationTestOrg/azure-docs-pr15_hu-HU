<properties
    pageTitle="Távoli hozzáférés engedélyezése a Holdas Azure telepítésekhez"
    description="Megtudhatja, hogyan engedélyezhető az Azure eszközkészlet használata Holdas Azure telepítésekhez távelérési."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Távoli hozzáférés engedélyezése a Holdas Azure telepítésekhez

Problémák megoldására a telepítéshez, előfordulhat, hogy engedélyezése és csatlakozhat távelérési szolgáltatója a üzembe virtuális gépen. A távelérési szolgáltatást a távoli asztali Protocol (RDP) a támaszkodik. Is beállíthatja távelérési a telepítés után az Azure közzétették vagy Holdas a Windows operációs rendszert használja, ha távelérési beállíthatja, mielőtt az Azure segítségével teszi közzé. Figyelje meg, hogy szüksége lesz egy távoli asztali ügyfélprogramban, amely az operációs rendszertől kompatibilis a üzembe virtuális gép Azure-ban való csatlakozáshoz.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Távoli hozzáférés engedélyezése, mielőtt beállítaná Azure

> [AZURE.NOTE] Távoli hozzáférés engedélyezése, mielőtt beállítaná az Azure-alkalmazásokat, Windows Holdas futnia kell.

Az alábbi képen látható párbeszédpanel szolgáló távoli hozzáférés engedélyezése a **Távoli hozzáférés** tulajdonságok.

![][ic719494]

Kétféleképpen a **Távelérési** tulajdonságai párbeszédpanel megjelenítéséhez:

* Kattintson a **Közzététel az Azure** párbeszédpanel a **Távoli hozzáférés** csoportban a **Speciális** hivatkozásra.
* Nyissa meg az a **tulajdonságokat** tartalmazó párbeszédpanel a Azure projekt.

Azure környezetben új projektet hoz létre, ha a projekt nem lesz távelérési alapértelmezés szerint engedélyezve van. A **Közzététel az Azure** párbeszédpanelen a felhasználónév és jelszó megadásával azonban távelérési egyszerűen engedélyezheti. A távelérési jelszót X.509 tanúsítványok titkosított. Ha nem adja meg a saját tanúsítványt, a titkosítást az Azure beépülő modul Holdas fellépő önaláírt tanúsítvány támaszkodik. E önaláírt tanúsítványt az Azure projektnézet egy nyilvános biztonságitanúsítvány-fájl (SampleRemoteAccessPublic.cer) mind a személyes információkat Exchange (PFX) biztonságitanúsítvány-fájl (SampleRemoteAccessPrivate.pfx) tárolt **tanúsítvány** mappában van. Az utóbbi tartalmaz a titkos kulcs a tanúsítvány, és rendelkezik egy alapértelmezett jelszavát, **Jelszo1**. Jó helyen jár mivel erre a jelszóra nyilvános Tudásbázis, az alapértelmezett tanúsítványt használandó csak tanulási a célból nem egy éles üzemi. Így más, mint a tanulási a célból, ha engedélyezve van a telepítésekhez távoli munkamenetet szeretne kell a hivatkozásra kattintott **Speciális** a **Közzététel az Azure** párbeszédpanelen megadhatja saját tanúsítványt. Figyelje meg, hogy kell a tanúsítvány PFX verzióját feltöltése belül az Azure portálon, a szolgáltatott szolgáltatásban, hogy az adott Azure fejtheti vissza a felhasználó jelszavát.

Az oktatóprogram maradéka mutatja be, amely az eredetileg letiltva távelérési készült Azure környezetben projekt távoli hozzáférés engedélyezése. Ebben az oktatóanyagban alkalmazásában azt hozunk létre egy új önaláírt tanúsítványt, és a .pfx fájl lesz egy tetszés szerinti jelszót. Akkor is, hogy egy hitelesítésszolgáltató által kibocsátott tanúsítványt használja.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Távoli hozzáférés engedélyezése, hogy Azure telepítése után

Távoli hozzáférés engedélyezése, miután telepítette a Azure, kövesse az alábbi lépéseket:

1. Jelentkezzen be az Azure kezelőportálja segítségével az Azure-fiók használatával
1. A **Cloud Services**listájában válassza a telepített felhőalapú szolgáltatás
1. Kattintson a felhőalapú szolgáltatás weblapon **konfigurálása** hivatkozásra
1. A konfiguráció lap alján kattintson a **távoli** hivatkozásra
1. Amikor megjelenik az előugró párbeszédpanel:
    * Adja meg a szerepkört, amelynek meg szeretné távoli hozzáférés engedélyezése
    * Jelölje be a **Távoli asztal engedélyezése** jelölőnégyzet
    * Adja meg a felhasználónevet és jelszót távoli eléréséhez használni kívánt
    * Jelölje ki a használni kívánt tanúsítványt
1. Kattintson az **OK gombra** 

Ekkor megjelenik egy üzenet arról, hogy a konfigurációs módosítást eltarthat néhány percig folyamatban van. A konfigurációs módosítás befejeződése után kövesse a jelen cikk **távolról történő bejelentkezéshez** szakaszban szereplő lépéseket.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Távoli hozzáférés engedélyezése a csomagban

1. Belül Holdas a Project Explorer ablakban kattintson a jobb gombbal a Azure projekt, és válassza a **Tulajdonságok parancsot**.

1. A **Tulajdonságok** párbeszédpanelen bontsa ki az **Azure** a bal oldali ablaktáblában és **Távoli hozzáférés**.

1. **Távoli hozzáférés** párbeszédpanelen győződjön meg róla, **fogadja el a távoli asztali kapcsolat ezeket a bejelentkezési adatok az összes szerepkörének engedélyezése** jelölőnégyzet be van jelölve.

1. Adja meg a távoli asztali kapcsolat felhasználó nevét.

1. Adja meg, és erősítse meg a jelszót a felhasználó számára. A felhasználó nevét és jelszavát értékek ezen a párbeszédpanelen, hogy a távoli asztali kapcsolaton történik. (Tájékoztatjuk, hogy ez legyen-e a PFX jelszavát a jelszó külön.)

1. Adja meg a felhasználói fiók lejárati dátummal.

1. Kattintson az **Új** új önaláírt tanúsítvány létrehozása gombra. (Azt is megteheti, akkor is jelöljön ki egy tanúsítványt a munkaterület vagy fájl rendszerből a **munkaterület** vagy **formázáshoz** gombok a kurzor, de ebben az oktatóanyagban alkalmazásában azt hozunk létre egy újat..)

    * Az **Új tanúsítvány** párbeszédpanelen adja meg, és erősítse meg a jelszót a PFX fájl kell megadnia.

    * Fogadja el a megadott **Név (CN)**érték, vagy egyéni nevet.

    * Adja meg az új tanúsítvány .cer formában mentési helyének elérési útját és nevét. Ezt a lépést, és a következő lépés használhatja a Azure projekt a **tanúsítvány** mappa is, de szabadon válasszon másik helyet. Ebben az oktatóanyagban alkalmazásában a **c:\mycert\mycert.cer**használjuk. (Hozza létre a **c:\mycert** mappát a folytatás előtt vagy egy meglévő mappába használata, ha szükségesnek látja.)

    * Adja meg, hogy az elérési utat és nevét, ahol az új tanúsítvány és a titkos kulcs, .pfx formában, a program menti. Ebben az oktatóanyagban alkalmazásában a **c:\mycert\mycert.pfx**használjuk. Az **Új a tanúsítvány** párbeszédpanel (frissítés a mappa elérési utak, ha nem használta **c:\mycert**) a következő hasonlóan kell kinéznie:

        ![][ic712275]

    * Kattintson az **OK gombra** az **Új a tanúsítvány** párbeszédpanel bezárásához.

1. A **Távoli hozzáférés** párbeszédpanel a következőhöz hasonlóan kell kinéznie:</p>

    ![][ic719495]

1. Kattintson az **OK gombra** a **Távelérési** párbeszédpanel bezárásához.
    
Az alkalmazás építenie cloud telepítéshez beállítása összeállítása.

## <a name="to-log-in-remotely"></a>Bejelentkezés távolról

Amikor elkészült a szerepkör-példányt, akkor is távolról jelentkezzen be a virtuális gépen, amelyen az alkalmazást.

* Ha használ Holdas, és a Windows Azure a telepítés során kijelölt a **távoli asztali kezdő telepítése** lehetőséget, és választhat egy távoli asztali kapcsolat bejelentkezési képernyő a üzembe indításakor. Ha megjelenik egy kérdés a felhasználónevet és jelszót, adja meg a távoli felhasználó megadott értékeket és tudnak bejelentkezni.

* Távolról bejelentkezhetne úgy, az <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure adatkezelési portál</a>használatával:

    * Belül az Azure kezelőportálja **Cloud Services** nézetben kattintson a felhőalapú szolgáltatás, kattintson a **példány**, kattintson egy adott példányhoz, és kattintson a **Csatlakozás** gombra. A **Csatlakozás** gombra a parancssáv a következőképpen jelenik meg:

        ![][ic659273]

    * Miután a **Csatlakozás** gombra kattintva, kéri RDP-fájl megnyitásához. Nyissa meg a fájlt, és kövesse a képernyőn megjelenő utasításokat. (Sikerült is mentheti a fájlt a helyi számítógépre, és futtassa a fájlban kattintson duplán a virtuális géphez a távoli naplóban anélkül, hogy először lépjen az adatkezelési portálon.)

    * Ha megjelenik egy kérdés a felhasználónevet és jelszót, adja meg a távoli felhasználó megadott értékeket és tudnak bejelentkezni.

> [AZURE.NOTE] Ha nem a Windows operációs rendszeren, kell használni egy távoli asztali ügyfél, hogy kompatibilis-e az operációs rendszer és beállításához hajtsa végre a lépéseket, hogy az ügyfél letöltött RDP-fájlban a beállításokkal.

## <a name="see-also"></a>Lásd még:

[Azure eszközkészlete Holdas][]

[A Holdas Azure egy helló világ alkalmazás létrehozása][]

[Az Azure eszközkészlet Holdas telepítése][] 

Java Azure használatával kapcsolatos további tudnivalókért olvassa el a az [Azure Java Developer Center][]című témakört.

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure eszközkészlete Holdas]: http://go.microsoft.com/fwlink/?LinkID=699529
[A Holdas Azure egy helló világ alkalmazás létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699533
[Az Azure eszközkészlet Holdas telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png
