<properties
    pageTitle="Hogyan hozhat létre egy egyéni sablont képe az Azure RemoteApp |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre egy egyéni sablont képe az Azure RemoteApp. Ez a sablon egy hibrid, illetve a felhőbe webhelycsoport használhatja."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Hogyan hozhat létre egy egyéni sablont képe az Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp összes, hogy a felhasználók a megosztani kívánt program használ a Windows Server 2012 R2 sablonként képe. Hozzon létre egy egyéni RemoteApp sablonként képe, indítsa el a meglévő képet, vagy hozzon létre egy újat. 


> [AZURE.TIP] Tudta, létrehozhat egy Azure virtuális képet? A szövegegység igaz, és kivágása a idő mennyiségét azt veszi a kép importálásához. Nézze meg a lépéseket [az alábbi](remoteapp-image-on-azurevm.md).

A kép Azure RemoteApp való használatra feltölthető követelményei vannak:


- A kép mérete MB többszöröse kell lennie. Ha megpróbál, amely nem egy pontos több képet feltölteni, a feltöltés sikertelen lesz.
- A kép mérete kell 127 GB vagy kisebb.
- A virtuális fájl kell lennie ([a Hyper-V virtuális merevlemez-meghajtók] VHDX fájlok jelenleg nem támogatottak).
- A virtuális nem lehet generációs 2 virtuális géphez.
- A virtuális rögzített méretű vagy lehet dinamikusan növekvő. Egy dinamikusan növekvő virtuális használata javasolt, mert a kisebb, mint egy rögzített méretű virtuális fájl feltöltése a Azure időt vesz igénybe.
- A lemez inicializálni kell a fő rekord (Rendszertöltő) szétválasztás stílust használ. A globálisan egyedi azonosítója partíciót (GPT) partíciót táblázatstílus nem támogatott.
- A virtuális tartalmaznia kell a Windows Server 2012 R2 egyetlen telepítés. Több kötet, de csak egy Windows példányának tartalmazó tartalmazhat.
- A távoli asztali munkamenet Host (RDSH) szerepkört, és az asztali élmény szolgáltatást telepítve kell lennie.
- A távoli asztali kapcsolat ügynök szerepkör kell *nem* telepíthető.
- A titkosított (EFS) kell tiltható le.
- A kép kell lennie a paramétereket alkalmaz a Sysprep segédprogrammal előkészített **oobe / generalize/shutdown** (ne használja a **/mode:vm** paraméter).
- A virtuális a pillanatkép lánc feltöltése nem támogatott.


**Első lépések**

Kell a szolgáltatás létrehozása előtt tegye a következőket:

- [Regisztráció](https://azure.microsoft.com/services/remoteapp/) RemoteApp.
- Felhasználói fiók létrehozása az Active Directory RemoteApp szolgáltatás fiókként használni. Ehhez a fiókhoz az engedélyek korlátozása, úgy, hogy a gép csak csatlakozhatnak a tartomány. [Konfigurálása Azure Active Directory RemoteApp](remoteapp-ad.md) talál további információt.
- A helyszíni hálózaton információkat gyűjthet: IP-cím VPN adatai és az információk.
- Telepítse az [Azure PowerShell](../powershell-install-configure.md) -modult.
- A felhasználók, ha szeretne engedélyt adni kívánt információkat gyűjthet. Ez lehet a Microsoft-fiók adatainak vagy a fiók adatainak az Active Directory-felhasználóknak.



## <a name="create-a-template-image"></a>Hozzon létre egy sablonként képe ##

Hozzon létre egy új sablon képe az alapoktól a magas szintű lépései a következők:

1.  Keresse meg a Windows Server 2012 R2 frissítés DVD-n vagy az ISO képe.
2.  Hozzon létre egy virtuális fájlt.
4.  Telepítse a Windows Server 2012 R2.
5.  Telepítse a távoli asztali munkamenet Host (RDSH) szerepkört, és az asztali élmény szolgáltatást.
6.  Telepítse az alkalmazások által igényelt további funkciókkal.
7.  Telepítse és állítsa be az alkalmazások. Olvashatóbbá teheti a megosztási alkalmazások, a **Start** menü a kép kifejezetten **%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs parancsot a megosztani kívánt programokat vagy alkalmazások hozzáadni.
8.  Végezze el az alkalmazások által igényelt további Windows beállításaitól.
9.  Tiltsa le a titkosított fájlok fájlrendszer.
10. **REQUIRED:** Nyissa meg a Windows Update, és a fontos frissítések telepítése.
9.  A kép SYSPREP.

A részletes lépéseket hozhat létre egy új képet a következők:

1.  Keresse meg a Windows Server 2012 R2 frissítés DVD-n vagy az ISO képe.
2.  Virtuális fájl létrehozása a lemez beépülő modul segítségével.
    1.  Indítsa el a merevlemez-kezelés (diskmgmt.msc).
    2.  Hozzon létre egy dinamikusan növekvő virtuális 40 GB vagy több méretű. (Becsüljük meg a távolságot a Windows, az alkalmazások és a testreszabások szükséges. A Windows Server RDSH szerepkör és az asztali élmény szolgáltatást igényel, 10 GB szabad terület)
        1.  Kattintson a **művelet > hozzon létre virtuális**.
        2.  Adja meg a helyet, a méret és a virtuális formátum. Jelölje ki a **dinamikusan növekvő**, és kattintson **az OK**gombra.

            Ez a néhány másodpercig fog futni. Amikor befejeződött a virtuális létrehozását, meg kell jelennie egy új lemez nélkül bármely betűjele és "Nem inicializált" állapotú a lemez Management console.

        - Kattintson a jobb gombbal a lemez (nem a szabad terület), és válassza a **Lemez inicializálni**. Jelölje ki a **MBR** (fő indítási rekord) partíciót stílust, és kattintson **az OK**gombra.
        - Hozzon létre egy új mennyiségi: kattintson a jobb gombbal a szabad terület, és kattintson az **Új egyszerű kötet**. Az alapértelmezett beállításokat a varázslóban fogadja el, de ellenőrizze, hogy az esetleges problémák elkerülése érdekében a sablon képe feltöltésekor meghajtó betűvel rendelhet.
        - Kattintson a jobb gombbal a lemez, és kattintson a **Leválasztás virtuális**.





1. Telepítse a Windows Server 2012 R2:
    1. Hozzon létre egy új virtuális számítógépre. Az új virtuális gép varázsló használata a Hyper-V vezető vagy egy ügyfél a Hyper-V.
        1. A generációs megadása lapon válassza ki a **generációs 1**.
        2. Kapcsolódás virtuális merevlemez lapon jelölje be **a meglévő virtuális merevlemez használata**, és tallózással keresse meg a virtuális az előző lépésben létrehozott.
        2. A telepítési változatok lapon jelölje be az **operációs rendszer pedig egy indítástól CD-re/DVD_ROM telepítése**, és válassza a hol található a Windows Server 2012 R2 telepítési adathordozóját.
        3. A Windows és az alkalmazások telepítéséhez szükséges varázslóban válassza ki az egyéb beállításokat. A varázsló.
    2.  A varázsló befejezése után a virtuális gép beállításainak szerkesztése és más szükséges változtatásokat telepítése a Windows és az alkalmazások, például a virtuális processzorok, a, és kattintson **az OK**gombra.
    4.  A virtuális gép csatlakozzon, és telepítse a Windows Server 2012 R2.
1. Telepítse a távoli asztali munkamenet Host (RDSH) szerepkört, és az asztali élmény szolgáltatást:
    1. Indítsa el a Kiszolgálókezelő.
    2. Az üdvözlőképernyő be- és a **kezelés** menüből, kattintson a **Szerepkörök hozzáadása és szolgáltatások** .
    3. Mielőtt elkezdené lapon kattintson a **Tovább gombra** .
    4. Jelölje ki a **szerepköralapú vagy a szolgáltatás-alapú telepítés**, és kattintson a **Tovább gombra**.
    5. Válassza a helyi számítógép zónában a listából, és kattintson a **Tovább gombra**.
    6. Jelölje be a **Távoli asztali szolgáltatásokat**, és kattintson a **Tovább**gombra.
    7. Bontsa ki a **felhasználói felülete és infrastruktúra** , és válassza ki **Az asztali élmény**.
    8. Kattintson a **Szolgáltatás hozzáadása**elemre, és kattintson a **Tovább gombra**.
    9. A távoli asztali szolgáltatásokat lapon kattintson a **Tovább**gombra.
    10. Kattintson a **távoli asztali munkamenet gazdájához**.
    11. Kattintson a **Szolgáltatás hozzáadása**elemre, és kattintson a **Tovább gombra**.
    12. A megerősítés telepítési beállítások lapon jelölje ki az **automatikusan, ha szükséges, indítsa újra a célként megadott kiszolgáló**, és majd a restart-figyelmeztetés a kattintson az **Igen** gombra.
    13. Kattintson a **telepítés**gombra. A számítógép újraindul.
1.  Telepítse a követel meg az alkalmazások, például a .NET-keretrendszer 3.5-ös további funkciókkal. A szolgáltatások telepítéséhez futtassa a szerepkörök hozzáadása és szolgáltatások varázsló.
7.  Telepítse és állítsa be a programok és az alkalmazások keresztül RemoteApp közzé szeretné tenni.

>[AZURE.IMPORTANT]
>
>Telepítse a RDSH szerepkör annak érdekében, hogy a kép feltöltött RemoteApp előtt megjelenése kompatibilitási problémák lépnek fel az alkalmazások telepítése előtt.
>
>Ellenőrizze, hogy az alkalmazás (**.lnk** fájl) parancsikon jelenik meg a **Start** menü minden felhasználó számára (%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs parancsot). Is győződjön meg arról, hogy a **Start** menüben látható ikonra kívánt felhasználók számára megjeleníteni. Ha nem módosítható. (Nem *rendelkezik* hozzáadása az alkalmazás a Start menü, de sokkal egyszerűbbé teszi RemoteApp közzétenni az alkalmazás. Egyéb esetben kell adnia a telepítési hely elérési útja az alkalmazás az alkalmazás közzétételekor.)


8.  Végezze el az alkalmazások által igényelt további Windows beállításaitól.
9.  Tiltsa le a titkosított fájlok fájlrendszer. A következő parancsot, emelt szintű parancsablakban:

        Fsutil behavior set disableencryption 1

    Másik lehetőségként állíthat be, és adja hozzá a következő duplaszó (DWORD) értéket a beállításjegyzékben:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  A kép belül az Azure virtuális gép készítésekor, nevezze át a ** \%windir%\Panther\Unattend.xml** fájl, mint ez blokkolja a feltöltés parancsfájl használt később a működését. Ez a fájl nevének módosítása Unattend.old, hogy a program továbbra is a fájl abban az esetben, ha a üzembe vissza kell.
10. Nyissa meg a Windows Update, és a fontos frissítések telepítése. Szükség lehet a többször a összes frissítések a Windows Update futtatása. (Előfordul, hogy telepíteni egy frissítést, és maga frissítést frissítésére van szüksége.)
10. A kép SYSPREP. A rendszergazda jogú parancssort futtassa a következő parancsot:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize oobe/shutdown**

    **Megjegyzés:** Ne használja a **/mode:vm** Váltás a SYSPREP parancs annak ellenére, hogy ez egy virtuális gép.


## <a name="next-steps"></a>Következő lépések ##
Most, hogy az egyéni sablon képe, kell, hogy a kép feltöltése a RemoteApp webhelycsoport. A következő cikkekben szereplő információk segítségével a webhelycsoport létrehozása:


- [Hogyan hozhat létre RemoteApp hibrid gyűjteménye](remoteapp-create-hybrid-deployment.md)
- [Hogyan hozhat létre RemoteApp felhő gyűjteménye](remoteapp-create-cloud-deployment.md)
 