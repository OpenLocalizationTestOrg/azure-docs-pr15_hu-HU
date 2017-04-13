<properties 
   pageTitle="A mobileszköz-kezelés StorSimple PowerShell |} Microsoft Azure"
   description="Megtudhatja, hogy miként StorSimple eszköze kezelése a Windows PowerShell-StorSimple használatával."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli@microsoft.com" />

# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>Az eszköz felügyelete StorSimple a Windows PowerShell használatával

## <a name="overview"></a>– Áttekintés

A Windows PowerShell-StorSimple kezelése a Microsoft Azure StorSimple eszköz használható parancssori felületet biztosít. Javaslatokat tesz a nevét, mint a Windows PowerShell-alapú parancssor egy korlátozott runspace épített. A felhasználó a parancssorból szemszögéből egy korlátozott runspace egy korlátozott hozzáférésű verzióját a Windows PowerShell formában jelenik meg. A Windows PowerShell egyszerű funkcióinak részét megőrzésével a kapcsolatnak további dedikált parancsmagokról felé kezelése a Microsoft Azure StorSimple eszköz szolgálják. 

Ez a cikk ismerteti a Windows PowerShell StorSimple további funkcióiról, például, hogy miként csatlakozhat a felület- és lépésenkénti leírásaira kíváncsi, vagy munkafolyamatok hajthatja végre a kapcsolat használatával mutató hivatkozásokat tartalmaz. A munkafolyamatok hogyan regisztrálhatja az eszközön, a hálózati kapcsolat beállítása az eszközön, az eszköz karbantartási módban kell, módosítsa a eszköz állapotot és tapasztalható problémák megoldásához igénylő frissítések telepítése tartalmazzák.

Ez a cikk elolvasása, után lesz képes:

- Csatlakozás Windows PowerShell használatának StorSimple StorSimple eszközére.

- A Windows PowerShell használatának StorSimple StorSimple eszköz felügyelete.

- Segítség a Windows PowerShellben a StorSimple.

>[AZURE.NOTE]   

>- A Windows PowerShell-parancsmagok StorSimple StorSimple eszköze kezelését, a soros konzol vagy távolról a Windows PowerShell távelérési teszi lehetővé. További információt az egyes az egyes parancsmag kapcsolat használható megnyitásához [parancsmagjai – referencia a Windows PowerShell-StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

>- Az Azure PowerShell StorSimple parancsmag, amelyek lehetővé teszik StorSimple szolgáltatói automatizálhatók és a parancssorból áttelepítési feladatok parancsmagok különböző gyűjteménye. További információt a StorSimple Azure PowerShell-parancsmagok lépjen az [Azure StorSimple parancsmagjai – referencia](https://msdn.microsoft.com/library/azure/dn920427.aspx).

A Windows PowerShell érheti el az alábbi módszerek egyikével StorSimple:

- [Csatlakozás StorSimple eszköz soros konzolhoz](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Csatlakozás távoli StorSimple a Windows PowerShell használatával](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
    

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Csatlakozás Windows PowerShell StorSimple a via a soros eszköz-konzolon

[Töltse le a gitt](http://www.putty.org/) vagy hasonló Terminálszolgáltatások emulációs szoftver StorSimple Windows PowerShell csatlakozni. Állítsa be a Microsoft Azure StorSimple eszköz eléréséhez kifejezetten gitt szüksége. A következő témakörök tartalmaznak gitt konfigurálása és csatlakozás az eszköz kapcsolatban a lépések részletes leírását. A soros konzolban különböző menüjében elérhető lehetőségek is vannak magyarázata.

### <a name="putty-settings"></a>GITT beállításai

Győződjön meg arról, hogy a következő gitt beállításainak használatával a Windows PowerShell-felület csatlakoztatása a soros konzolról.

#### <a name="to-configure-putty"></a>GITT konfigurálása

1. GITT **Konfigurálás** párbeszédpanelen **kategória** mezőre jelölje be a **billentyűzetet**.

2. Győződjön meg arról, hogy vannak-e jelölve az alábbi lehetőségek (ezek az alapértelmezett beállítások új munkamenet indításakor). 

  	|Billentyűzet elem|Válassza a|
  	|---|---|
  	|BACKSPACE billentyűvel|Vezérlőelem-? (127.)|
  	|A kezdőlap és a befejező kulcsok|Normál|
  	|Funkcióbillentyűk és a billentyűzeten|ESC [n ~|
  	|A kurzor kulcsok Alapállapot|Normál|
  	|A numerikus billentyűzeten Alapállapot|Normál|
  	|Egyéb billentyűzet szolgáltatások engedélyezése|Vezérlőelem-Alt különbözik az AltGr|

    ![Támogatott gitt beállítások](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Kattintson a **alkalmazása**gombra.

4. A **kategória** mezőben jelölje ki a **fordítási**.

5. A **távoli karakterkészlet** legördülő listában jelölje ki a **UTF-8**.

6. A **vonalas rajz karakterek kezelését**válassza a **használati Unicode vonalas rajz kódjuk**lehetőséget. Az alábbi ábrán a megfelelő gitt beállításokat.

    ![UTF gitt beállításai](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Kattintson a **alkalmazása**gombra.


Az alábbi lépésekkel csatlakozhat a soros eszköz-konzol most már használhatja az gitt.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### <a name="about-the-serial-console"></a>A soros konzolról

A Windows PowerShell felületén keresztül a soros konzol transzparens üzenet StorSimple eszköze bemutatják elérésekor követő menü Beállítások. 

A szalagcím üzenet StorSimple eszköz alapvető információk, például a modell nevét, telepített szoftver verziószáma és érik el a vezérlő állapotának tartalmazza. Az alábbi képen az látható transzparens üzenet.

![Üzenet a soros transzparens](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] A szalagcím üzenet segítségével adja meg, hogy a vezérlő csatlakozik az aktív vagy passzív.

Az alábbi képen látható, a soros konzol menü a használható különböző runspace lehetőségeket.

![Regisztráljon az eszköz 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Az alábbi beállítások közül választhat:

1. **Jelentkezzen be a teljes hozzáférés** Ez a beállítás lehetővé teszi (az a megfelelő hitelesítő adatokkal) való csatlakozáskor a **SSAdminConsole** runspace a helyi vezérlőn. (A helyi vezérlő a vezérlőre, akkor jelenleg keresztül érik el a soros konzol a StorSimple eszköz a.) Ez a beállítás is használható lehetővé teszi a Microsoft Support korlátlan runspace (támogatás munkamenet) problémák megoldásához lehetőség eszköz eléréséhez. Után 1 beállítás használatával jelentkezzen be, engedélyezheti a Microsoft Support adatbázismodellbe egy adott parancsmag futtatásával korlátlan runspace eléréséhez. A részletekért olvassa el az [támogatási indítása](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Jelentkezzen be a teljes hozzáféréssel rendelkező peer vezérlő** Ez a beállítás akkor ugyanaz, mint 1, a beállítás azzal a különbséggel, hogy a **SSAdminConsole** runspace a peer vezérlőn (a megfelelő hitelesítő adatok) csatlakoztathatja. StorSimple egy nagy elérhetősége eszközről egy aktív-passzív konfigurációban két vezérlőkkel, mert peer utal, a többi vezérlő az eszköz a soros konzolon csatlakozó).
Hasonló kapcsoló 1, ezt a beállítást is használható lehetővé teszi a Microsoft Support peer vezérlőn korlátlan runspace eléréséhez.

3. **Korlátozott hozzáférésű csatlakozás** Ez a beállítás eléréséhez a Windows PowerShell-felület a korlátozott üzemmód használják. Nem kéri az access hitelesítő adatokat. Ez a beállítás egy 1 és 2 beállítások összehasonlítva szűkebb runspace csatlakozik.  1 beállítást az tevékenységek részét, amely **nem* lehet elvégezni a e runspace vannak:

    - A gyár beállításainak visszaállítása
    - A jelszó módosítása
    - Engedélyezése és letiltása az access támogatás
    - Frissítések telepítése
    - Gyorsjavítások telepítése 
                                                

    >[AZURE.NOTE] **Ha elfelejtette a eszközt rendszergazdai jelszót, és 1 vagy 2 beállítás keresztül nem tud kapcsolódni az a használni kívánt beállítást.**

4. **Nyelv megváltoztatása** Ezzel a beállítással a a Windows PowerShell-felület nyelvének megváltoztatása. A támogatott nyelvek angol, japán, orosz, francia, dél-koreai, spanyol, olasz, német, kínai és brazíliai portugál.


## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>A Windows PowerShell használatának StorSimple StorSimple távolról csatlakoztatása

A Windows PowerShell távelérési StorSimple eszköze csatlakozhat is használhatja. Amikor ilyen módon csatlakozni, menüt nem jelenik meg. (Megjelenik egy menü csak akkor, ha a soros konzol az eszközön való csatlakozáshoz használhatja. Csatlakozás távoli megnyitja közvetlenül az annak megfelelő "parancsa 1 – a teljes hozzáférés" a soros konzolt.) A Windows PowerShell távelérési kapcsolódni lehet egy adott runspace. Megadhatja, hogy a megjelenítési nyelvet. 

A megjelenítési nyelv az független a nyelvet, a soros konzol menü **Nyelv módosítása** parancsával beállíthatja. Távoli PowerShell fog automatikusan felveszi a területi beállításait az eszköz csatlakozik a Ha nincs megadva.

>[AZURE.NOTE] Ha a Microsoft Azure virtuális hosts és a StorSimple virtuális eszközök dolgozik, akkor is használhatja a Windows PowerShell távelérési és a virtuális állomás való csatlakozáshoz a virtuális eszközt. Ha van állítva az állomáson, amelyen a Windows PowerShell-munkamenetet adatainak mentése megosztáshoz tetszőleges pontjára, tartsa szem előtt, hogy a Mindenki fő tartalmazza a csak a hitelesített felhasználók kell lennie. Ha úgy állította be a megosztás mindenki hozzáférésének engedélyezése és a hitelesítő adatok megadása nélkülire kapcsolatot, ezért a névtelen hitelesítés egyszerű szolgálnak, és hiba jelenik. A problémát, a megosztott szolgáltató, akkor kell engedélyezze a vendégként való bekapcsolódáshoz fiókot, majd kattintson a vendégként való bekapcsolódáshoz fiók teljes hozzáférési engedély megosztása, vagy adjon meg érvényes hitelesítő adatokkal együtt a Windows PowerShell-parancsmag.

Http- vagy HTTPS segítségével csatlakoztatása a Windows PowerShell távelérési keresztül. Az alábbi oktatóanyagok használni, a képernyőn megjelenő utasításokat:

- [Csatlakozás távolról a HTTP használatával](storsimple-remote-connect.md#connect-through-http)
- [Csatlakozás távolról a HTTPS használatával](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Kapcsolat biztonsági kérdései

Csatlakozás a Windows PowerShell-StorSimple kiválasztásakor is vegye figyelembe a következőket:

- Csatlakozás közvetlenül a soros eszköz-konzol biztonságos, de a soros konzol csatlakozás hálózati kapcsolók keresztül nem. Lehet a óvatos a biztonsági kockázatot jelentenek, ha a Kapcsolódás a soros eszköz, hálózati kapcsolók keresztül.

- A HTTP-munkamenet keresztül csatlakozik nagyobb biztonság, mint a soros konzolon hálózaton keresztül csatlakoztatott is kínál lehetőséget. Bár ez nem a legbiztonságosabb módszer, is elfogadható megbízható hálózatokon.

- Keresztül HTTPS-KAPCSOLATON keresztül csatlakozik, a legbiztonságosabb, és a a javasolt lehetőséget.


## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>A Windows PowerShell használatának StorSimple StorSimple eszköz felügyelete
A következő táblázat mutatja a összefoglalását megtalálja a leggyakoribb felügyeleti feladatok és a végrehajtható összetett munkafolyamatok belül a Windows PowerShell felületén StorSimple eszközére. Az egyes munkafolyamat kapcsolatos további tudnivalókért kattintson a megfelelő a táblázatban.

#### <a name="windows-powershell-for-storsimple-workflows"></a>A Windows PowerShell-StorSimple munkafolyamatok

|Ha azt szeretné, hogy ehhez...|Az alábbi eljárással.|
|---|---|
|Regisztráljon az eszközön|[Állítsa be és regisztrálni az eszközt, a Windows PowerShell használatának StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Webes proxy beállítása</br>Nézet web proxy beállításai|[Webes proxy StorSimple mobileszközére beállítása](storsimple-configure-web-proxy.md)|
|Módosítsa az adatok 0 hálózati kapcsolat beállításai az eszközön|[ADATOK 0 hálózati kapcsolat StorSimple mobileszközére módosítása](storsimple-modify-data-0.md)|
|Egy vezérlő leállítása </br> Indítsa újra, vagy állítsa le a vezérlő </br> Egy eszközt leállítása</br>Az eszköz gyári alapértelmezett beállításainak visszaállítása|[Eszköz vezérlők kezelése](storsimple-manage-device-controller.md)|
|Karbantartási mód frissítések telepítése és gyorsjavítások|[Az eszköz frissítése](storsimple-update-device.md)|
|Írja be a karbantartási mód </br>Kilépés a karbantartás módból|[StorSimple eszköz módok](storsimple-device-modes.md)|
|Támogatás csomag létrehozása</br>Visszafejtése és a támogatási csomag szerkesztése|[Létrehozhatja és kezelheti a támogatási csomag](storsimple-create-manage-support-package.md)|
|Támogatás indítása</br>|[Támogatás munkamenet indítása a Windows PowerShell-StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## <a name="get-help-in-windows-powershell-for-storsimple"></a>A Windows PowerShell-súgójának StorSimple

A Windows PowerShell-StorSimple parancsmag súgó érhető el. A súgó online, a legfrissebb verziója érhető el, amelynek használatával frissítheti a súgójában, a rendszer.

A felületén a Súgó hasonlít, amely a Windows PowerShellben, és a Súgó kapcsolatos parancsmagok a legtöbb működni fog. Súgó a Windows PowerShell online találhat a TechNet könyvtár: [a Windows PowerShell parancsprogram-kezelés](http://go.microsoft.com/fwlink/?LinkID=108518).

Az alábbiakban segítséget típusú, a Windows PowerShell kapcsolathoz, például hogy miként frissítheti a súgó rövid leírását.

#### <a name="to-get-help-for-a-cmdlet"></a>Segítség a parancsmag

- Segítség kérése bármely parancsmag vagy függvény, használja az alábbi parancsot:`Get-Help <cmdlet-name>`

- Online súgó az bármely parancsmag, használja az előző parancsmagnak a `-Online` paraméter:`Get-Help <cmdlet-name> -Online`

- Teljes segítségre van szüksége, használhatja a `–Full` paraméter, és a példákban használata a `–Examples` paraméter.

#### <a name="to-update-help"></a>Súgó frissítése

A Súgó a Windows PowerShell-felületen egyszerűen hozzáigazítható. Hajtsa végre az alábbi lépéseket a rendszer a Súgó frissítéséhez.

#### <a name="to-update-cmdlet-help"></a>Súgó parancsmag frissítése

1. Indítsa el a Windows PowerShell a **Futtatás rendszergazdaként** lehetőséget.

1. A parancssorba írja be: `Update-Help`

1. A frissített Súgó fájlokat telepíti.

1. A Súgó fájlok telepítése után írja be: `Get-Help Get-Command`. Ez a művelet megjeleníti, amelynek súgója megtalálható parancsmagok listáját.


>[AZURE.NOTE] A rendelkezésre álló parancsmagok listájának eléréséhez egy runspace, jelentkezzen be a megfelelő menüpontot, és futtassa a `Get-Command` parancsmag.

## <a name="next-steps"></a>Következő lépések
Ha minden olyan problémák a StorSimple eszközzel egy a fenti munkafolyamatok végrehajtásakor, olvassa el az [StorSimple telepítések hibaelhárítási](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments)eszközök.

