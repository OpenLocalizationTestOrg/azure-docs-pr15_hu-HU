<properties
    pageTitle="Replikáció helyszíni VMware virtuális gépeken futó vagy a fizikai kiszolgálók másodlagos webhelyre |} Microsoft Azure"
    description="Ez a cikk segítségével VMware VMs vagy a Windows vagy Linux fizikai kiszolgálók bizonyos az Azure-webhely helyreállításhoz másodlagos webhelyre."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>A helyszíni VMware virtuális gépeken futó vagy a fizikai kiszolgálók bizonyos másodlagos webhelyre


## <a name="overview"></a>– Áttekintés

Az Azure webhely helyreállítási InMage Scout biztosít a helyszíni VMware webhelyek közötti valós idejű replikáció. Azure webhely helyreállítási szolgáltatási előfizetések InMage Scout szerepel.


## <a name="prerequisites"></a>Előfeltételek

**Azure-fiók**: szüksége van a [Microsoft Azure](https://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. [Tudjon meg többet](https://azure.microsoft.com/pricing/details/site-recovery/) is megtudhat a webhely helyreállítási árak.


## <a name="step-1-create-a-vault"></a>Lépés: 1: Hozzon létre egy tárolóból elemre
1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2. Kattintson az új elemre > kezelés > biztonsági mentése és a webhely helyreállítási (MOBILE). Másik lehetőségként is kattintson a Tallózás gombra > helyreállítási szolgáltatások tárolóból elemre > Hozzáadás gombra.
3. **Neve** mezőjében adja meg egy rövid nevet, amely azonosítja a tárolóból elemre. Ha egynél több előfizetése van, válasszon közülük.
4. **Erőforráscsoport** hozzon létre egy új erőforráscsoport, vagy jelöljön ki egy meglévőt. Adjon meg egy Azure régiót befejezéséhez szükséges mezőket. 
5.  **Helyét**jelölje ki a földrajzi régióban esetében a tárolóból elemre. Jelölje be a támogatott régiók, olvassa el az [Azure webhely helyreállítási árak](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Ha szeretne gyorsan hozzáférést a tárolóból elemre az irányítópult kattintson a rögzítés irányítópult, és kattintson a Létrehozás gombra.
6. Az új tárolóból elemre az irányítópulton fog megjelenni > összes erőforrás, és a fő helyreállítási szolgáltatások tárolókban lap.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Lépés: 2: Állítsa be a tárolóból elemre, és töltse le a InMage Scout összetevők
7. A tárolókban helyreállítási szolgáltatások lap jelölje ki a tárolóból elemre, és kattintson a beállítások gombra.
8. A **Beállítások** > **Első lépések** kattintson a **Webhely helyreállítási** > lépés 1: **Felkészülés infrastruktúra** > **védelem cél**.
9. A **védelem cél** válassza a helyreállítás webhelyre, és jelölje be az Igen, a VMware vSphere hipervizor. Kattintson az OK gombra.
10. A **telepítő Scout**letöltési való letöltés InMage Scout 8.0.1 Georgia szoftverek és a regisztrációs kulcs gombjára. A letöltött .zip fájl szerepelnek, a telepítőfájlokat minden szükséges összetevőt.


## <a name="step-3-install-component-updates"></a>Lépés 3: Összetevő-frissítések telepítése

További információ: a legújabb [frissítéseket](#updates). Kell telepítenie kiszolgálókon frissítés fájlokat a következő sorrendben:

1. Ha van ilyen RX kiszolgáló
2. Konfigurációs kiszolgálóihoz
3. Folyamatábra-kiszolgálókon
3. Fő cél kiszolgálók
4. vContinuum kiszolgálók
5. Forráskiszolgáló (Windows és Linux Server)

A frissítések telepítése a következőképpen:

1. Töltse le a [frissítés](https://aka.ms/asr-scout-update4) .zip fájlt. A .zip fájlt tartalmazza a következő fájlokat:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.GZ
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UA update4 bittel RHEL5 OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Bontsa ki a .zip fájlokat.<br>
3. **Az RX kiszolgáló**: másolás **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** RX kiszolgálóra és bontsa ki azt. A kinyert mappában futtassa a **telepítése**elemre.<br>
4. **A konfiguráció kiszolgálói/folyamat kiszolgáló**: másolás **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** a fiókkonfigurációs kiszolgáló és a folyamat kiszolgálóhoz. Dupla kattintással indítsa el.<br>
5. **A Windows fő tároló kiszolgáló**: az egységesített ügynök frissítéséhez másolja a fő cél kiszolgálóhoz **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** . Kattintson duplán a futtatni. Ne feledje, hogy az egyesített ügynök is alkalmazható a forráskiszolgálóval. Telepítenie kell azt a forrás kiszolgáló, valamint, mint az itt felsorolt később.<br>
7. **A vContinuum kiszolgáló**: **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** másolja a vContinuum kiszolgálóra.  Győződjön meg arról, hogy a vContinuum varázsló bezárta. Kattintson duplán a kattintással indítsa el a fájlt.<br>
8. **A Linux fő tároló kiszolgáló**: az egységesített ügynök frissítéséhez **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** másolása a fő cél kiszolgálóhoz, és bontsa ki. A kinyert mappában futtassa a **telepítése**elemre.<br>
9. **A Windows server adatforrás**: az egységesített ügynök frissítéséhez másolja a forráskiszolgálóval **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** . Kattintson duplán a futtatni.<br>
10. **A Linux forráskiszolgáló**: az egységesített ügynök frissítéséhez UA fájl verziójának megfelelő másolja a Linux kiszolgálóra, és bontsa ki. A kinyert mappában futtassa a **telepítése**elemre.  Példa: A RHEL 6,7 64 bites kiszolgálója mezőben **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** másolja a kiszolgálóra, és bontsa ki. A kinyert mappában futtassa a **telepítése**elemre.

## <a name="step-4-set-up-replication"></a>Lépés: 4: A replikáció beállítása
1. Állítsa be a forrás közötti replikáció, és VMware webhelyek Megcélozhat.
2. A termék letöltődik InMage Scout dokumentációját használata útmutatást. Másik lehetőségként érheti el a dokumentáció az alábbi képlettel történik:

    - [Kibocsátási megjegyzések](https://aka.ms/asr-scout-release-notes)
    - [Kompatibilitási mátrix](https://aka.ms/asr-scout-cm)
    - [Útmutatója](https://aka.ms/asr-scout-user-guide)
    - [RX útmutatója](https://aka.ms/asr-scout-rx-user-guide)
    - [Rövid telepítési útmutatója](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Frissítések

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure webhely helyreállítási Scout 8.0.1 frissítés 4
4 Scout frissítés az összesítő frissítés. Az eddig update3 és a következő új hibajavítások és a továbbfejlesztett update1 a javítások rendelkezik.

**Új platform támogatása** 

- Támogatás a vCenter/vSphere 6.0, 6.1 és 6.2 hozzá lett adva
- Támogatás hozzá lett adva a következő Linux operációs rendszerek
    - Piros kalap vállalati Linux (RHEL) 7.0-s, 7.1 és 7.2. 
    - CentOS 7.0-s, 7.1 és 7.2.
    - Piros kalap vállalati Linux (RHEL) 6.8
    - CentOS 6.8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64 bites **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** alap Scout Georgia csomaggal **InMage_Scout_Standard_8.0.1 GA.zip**csomagolt. Scout Georgia csomag [1. lépés](site-recovery-vmware-to-vmware.md#Step 1: Create a vault)említett portálról letöltése

**Hibajavítás és kapcsolatos fejlesztések** 

- Továbbfejlesztett leállítás kezelésére szolgáló következő Linux bejegyzéseit és klónok nem kívánt szinkronizálja újra észlelések kiküszöböléséhez.
    - Piros kalap vállalati Linux (RHEL) 6.x
    - Az Oracle Linux (OL) 6.x
- Linux rendszerhez a mappa hozzáférési engedélyek egységesített ügynök telepítési könyvtárban most korlátozódik csak a helyi felhasználó végrehajtása
- A Windows közös elosztott konzisztencia könyvjelző erősen a kiállító közben hibát időtúllépés betöltött elosztott alkalmazásokban, például SQL- és megosztási pont fürt.
- Hozzáadott napló kapcsolódó fix CX alap telepítőt.
- Windows fő cél alap installer VMware vCLI 6.0 letöltési hivatkozás kerül.
- Megjelenik a további vizsgálat és hálózati konfigurációk módosítások naplók feladatátvevő és DR gyakorlatokat során.
- Bizonyos adatmegőrzési adatokat nem a CX van jelenteni.  
- Fizikai fürt amikor a forrás mennyiségi Lekicsinyítve történt mennyiségi művelet ismételt méretezés vContinuum varázsló segítségével nem működnek.
- Fürt védelem nem sikerült "Nem sikerült keresse meg a lemez aláírását" Ha a fürt lemez PRDM lemez.
- cxps átviteli a kiszolgáló összeomlik tartományon kívüli kivétel miatt. 
- Kiszolgáló neve és IP-oszlopok, most már a átméretezhető az leküldéses telepítés lap vContinuum varázsló.
- RX API-val kapcsolatos fejlesztések
    - Itt öt legújabb elérhető gyakori konzisztencia pontok (csak a garantált címkék).
    - A védett eszközök beosztását, és a szabad terület részletek biztosít.
    - Ez a témakör Scout illesztőprogram állam forráskiszolgáló. 
    
>[AZURE.NOTE] 
>
>- **InMage_Scout_Standard_8.0.1_GA.zip** alap csomag letöltése frissítette CX alap installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** és a diaminta a Windows-céltartományban alap installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**. Az összes új telepítés használata új CX és a Windows mesteralakzat-céltartományban Georgia bittel.
>- Frissítés 4 is közvetlenül alkalmazható 8.0.1 GA
>- A fiókkonfigurációs kiszolgáló és a RX frissítések vissza után azok is a rendszer nem állítható.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure webhely helyreállítási Scout 8.0.1 frissítés 3
Frissítés 3 tartalmazza, az alábbi hibajavítások és kapcsolatos fejlesztések:

- A konfigurációs kiszolgálója és RX sikertelen regisztrálhatja a a webhely helyreállítási tárolóból elemre, ha azok a proxy mögött.
- A helyreállítási pont cél (Készletben) nem teljesül órák számát nem első frissül a állapotjelentés.
- Ha a ESX hardver részletek vagy a hálózati adatok tartalmazhat UTF-8 karaktert a fiókkonfigurációs kiszolgáló nem szinkronizálódnak RX.
- A Windows Server 2008 R2 tartomány vezérlők sikertelen helyreállítás indításáról.
- Kapcsolat nélküli szinkronizálás van nem a várt módon működnek.
- Virtuális gép (virtuális) feladatátvételt hosszú időn keresztül replikációs-pár törlés beragad CX felhasználói felület és felhasználók nem fejezze be a visszaállás vagy folytatása.
- Általános a konzisztencia feladat által végzett műveleteket alkalmazás csökkentésére optimalizálva pillanatkép leválasztja az például SQL-ügyfelek számára.
- A konzisztencia eszköz (VACP.exe) teljesítményét továbbfejlesztettük a memóriahasználat pillanatképek létrehozása a Windows működéséhez szükséges csökkentésével.
- A leküldéses lefagy a szolgáltatás telepítésekor a jelszó nem nagyobb, mint 16 karaktert.
- vContinuum nem ellenőrzése és új vCenter hitelesítő adatokat kér, ha a hitelesítő adatok módosulnak.
- Linux a fő cél gyorsítótár-kezelő (cachemgr) van nem fájlok letöltése a folyamat kiszolgálóról, ez replikációs pár szabályozásának eredményez.
- Ha a fizikai feladatátvevő fürt (MSCS) lemez sorrend nem azonos a csomóponton replikációs egyes fürt kötet nincs beállítva.
<br/>Figyelje meg, hogy a fürt kell kell reprotected javítás előnyeit.  
- SMTP funkció nem működik a RX Scout 7.1 Scout 8.0.1 frissítése után is.
- További stat felvétele a visszaállítási művelet nyomon az időt tett fejezze be a naplóban
- Támogatás hozzá lett adva a forráskiszolgálóval a Linux operációs rendszerhez:
    - Piros kalap vállalati Linux (RHEL) 6 frissítés 7
    - CentOS 6 7 frissítése
- A CX és RX felhasználói felület most megjelenítheti az értesítésre, a két között, amely bitkép módba.
- A következő biztonsági javításokat a RX felvétele:

**Probléma leírása**|**Végrehajtási eljárások**
---|---
Engedély a via paraméter illetéktelen hozzáférés figyelmen kívül hagyása|Nem alkalmazható felhasználók korlátozott hozzáférést.
A webhelyközi kérelem hamisítása|A lap-jogkivonat fogalom, amelyet a véletlenszerűen hoz létre, minden oldalán végrehajtani. <br/>Az adatokkal jelennek meg: <li> Nincs csak egyetlen bejelentkezési példány ugyanarra a felhasználóra.</li><li>Nem működik a lapot frissíteni – átirányítja az irányítópult.</li>
Rosszindulatú fájl feltöltése|Korlátozott fájlok egyes bővítményeket. Kiterjesztés engedélyezett: 7z, aiff, ASP avi, bmp, csv, dokumentum, docx, fla, flv, gif, gz, gzip, jpeg, jpg, napló, a közép, a mov, mp3, mp4, Microsoft termékkód, mpeg, mpg, ods, odt, PDF-fájlt, png, ppt, pptx, pxd, qt, ram, rar, erőforrás-kezelő, rmi, rmvb, rtf, sdc, sitd, swf, sxc, sxw, tar, tgz, tif, tiff, txt, vsd, wav, wma, wmv, xls, xlsx, xml, és zip.
Állandó webhelyközi parancsfájlok | Megjelenik a beviteli ellenőrzések.


>[AZURE.NOTE]
>
>-  Az összes webhely helyreállítási frissítés mindig összegzők. Frissítés 3 frissítés 1 és frissítés 2 a javításokat tartalmaz. Frissítés 3 is közvetlenül alkalmazható 8.0.1 GA
>-  A fiókkonfigurációs kiszolgáló és a RX frissítések vissza után azok is a rendszer nem állítható.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure webhely helyreállítási Scout 8.0.1 frissítés 2 (a 03 frissítés Dec 15)

A frissítés 2 javításokat tartalmazza:

- **Konfigurációs kiszolgálója**: megakadályozó a 31 napig ingyenes mérési funkció a működését is, ha a webhely helyreállítási regisztrálva volt a fiókkonfigurációs kiszolgáló probléma megoldása.
- **Egységesített agent**: Update 1, a frissítés nincs telepítve a fő cél-kiszolgálón Ha 8.0-ban való 8.0.1 verzióról frissített eredményező a probléma megoldása.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure webhely helyreállítási Scout 8.0.1 frissítés 1

Frissítés 1 a következő hibajavítások és új szolgáltatásokat tartalmazza:

- kiszolgálói példány egy ingyenes védelem 31 napot. Lehetővé teszi, hogy tesztelje a használható funkciók körét, vagy állítsa be a vásárlási-a-fogalom.
    - A kiszolgálón, feladatátvétel és visszaállás, beleértve az összes művelet szabadon az első 31 nap kezdve a kiszolgáló először látták webhely helyreállítási Scout időpontját.
    - A naptól 32-től minden védett server Azure webhely helyreállítási védelem ügyfél által birtokolt webhelyre a szabványos példány rátáját kell fizetnie.
    - Bármikor jelenleg alapdíjával védett kiszolgálók számának érhető el a webhely-helyreállítás Azure tárolóra irányítópult oldalán.
- Támogatás hozzá vSphere parancssori felület (vCLI) 2 5,5 frissítés.
- A támogatás Linux operációs rendszerek a forráskiszolgálóval a hozzáadott:
    - RHEL 6 6 frissítése
    - RHEL 5 11 frissítése
    - CentOS 6 frissítés 6
    - CentOS 5 frissítés 11
- Hibaadatbázis javítja a következő kapcsolatos problémák megoldása:
    - Regisztráció tárolóból elemre a fiókkonfigurációs kiszolgáló vagy RX kiszolgáló sikertelen lesz.
    - Fürt kötet nem jelennek meg, ha csoportosított virtuális gépeken futó reprotected vannak, ha azokat az önéletrajz vártnak.
    - A fő tároló kiszolgáló helyezkedik el a különböző ESXi kiszolgálón fut a helyszíni gyártási virtuális gépeken futó visszaállás meghiúsul.
    - Konfigurációs fájl engedélyek módosul frissítésekor 8.0.1, amely hatással van a védelem és műveleteket.
    - A újraszinkronizálás küszöbértékét nem vannak érvényben megfelelően működjön, amely nem következetes replikáció működését vezet.
    - A Készletben beállítások nem jelennek meg helyesen konfigurációs kiszolgáló felületén. A nem tömörített adatértéke helytelenül a tömörített értéket jeleníti meg.
    -  Az eltávolítási művelet nem törli a vContinuum varázsló a várt módon működik, és a replikáció nem törlődnek a konfigurációs kiszolgáló felületről.
    -  A varázslóban vContinuum lemez automatikusan kijelöletlen során MSCS virtuális gépeken futó védelme a lemez nézetben a **Részletek** gombra..
    - A tényleges és – a virtuális (P2V) eset során szükséges HP szolgáltatások, például CIMnotify és CqMgHost, a virtuális gép helyreállítási kézi nem került. Ennek eredményeképpen további betöltés idejét.
    - Linux virtuális gép védelem vannak 26-nál több lemezt a fő tároló kiszolgáló nem sikerül.

## <a name="next-steps"></a>Következő lépések

Tartalmakat tehet közzé a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)rendelkező kérdéseit.
