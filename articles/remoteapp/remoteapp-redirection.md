<properties
    pageTitle="Az Azure RemoteApp átirányítással |} Microsoft Azure"
    description="Megtudhatja, hogy miként beállítása és használata az átirányítást az RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-redirection-in-azure-remoteapp"></a>Az Azure RemoteApp átirányítással

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Eszközök átirányítása lehetővé teszi, hogy a felhasználók távoli alkalmazások használata a helyi számítógépen, telefon vagy táblagép beállíttatása csatlakozó eszközök kezelése. Például ha meg van adva Skype Azure RemoteApp keresztül, a felhasználónak kell a kamera telepítve van a saját PC-re készült Skype. Ez akkor is nyomtatók, hangszórók, monitorok és egy tartományt az USB-kompatibilis perifériák igaz.

Távoli alkalmazás formájában használva a távoli asztali Protocol (RDP) és a RemoteFX átirányításra használja.

## <a name="what-redirection-is-enabled-by-default"></a>Alapértelmezés szerint engedélyezve van a milyen átirányítást?
Ha RemoteApp használja, az alábbi átirányításának alapértelmezés szerint engedélyezve vannak. Zárójelek között lévő információk megjelenítése a RDP-beállítás.

- Hangok lejátszása a helyi számítógép (az**ezen a számítógépen a lejátszás**). (audiomode:i:0)
- A helyi számítógép hangja rögzíthet, és küldje el a távoli számítógépet (**a jelen számítógépen rekord**). (audiocapturemode:i:1)
- A helyi nyomtatókra (redirectprinters:i:1) nyomtatása
- COM-portok (redirectcomports:i:1)
- Intelligens kártya eszköz (redirectsmartcards:i:1)
- A vágólap (azt jelenti, hogy másolja és illessze be) (redirectclipboard:i:1)
- Törölje a típus betűtípus simítás (betűtípus simítás engedélyezése: i:1)
- Átirányíthatja az összes támogatott beépülő and Play eszközöket. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Milyen más átirányítás-e?
Két átirányítási beállítások alapértelmezés szerint le vannak tiltva:

- Meghajtó átirányítása (megfeleltetés meghajtót): helyi számítógépen meghajtók a távoli munkamenetet megfeleltetett meghajtók válnak. Ezzel a lehetőséggel, a Mentés vagy a helyi meghajtón származó fájlok megnyitása a távoli munkamenetet használata közben.
- USB-átirányítás: az USB-eszközök a helyi számítógéphez csatlakoztatott a távoli munkamenetet belül is használhatja.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Távoli alkalmazás formájában használva az átirányítási beállítások módosítása
Az eszköz átirányítási beállításainak gyűjtemény SDK a Microsoft Azure PowerShell használatával módosíthatja. Telepítése után az új PowerShell és SDK, először állítsa be, hogy az előfizetés kezeléséhez, [telepítése, és állítsa be a Azure PowerShell](../powershell-install-configure.md)leírt módon.

A következőhöz hasonló parancs segítségével RDP egyéni tulajdonságainak beállítása:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Tájékoztatjuk, hogy *Szélesség* használt elválasztó egyéni tulajdonságok között.)

Lista beszerzése az milyen egyéni RDP tulajdonság be van állítva, az alábbi parancsmag futtatásával. Tartsa szem előtt, hogy csak az egyéni tulajdonságok kimeneti eredmények és nem az alapértelmezett tulajdonságok látható:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Egyéni tulajdonságok beállításakor meg kell adnia az összes egyéni tulajdonságok minden alkalommal, amikor; a beállítás hiányában letiltva visszatér.   

### <a name="common-examples"></a>Hétköznapi példát
Ahhoz, hogy meghajtó átirányítása használja a következő parancsmagot:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Ezzel a parancsmaggal használható USB- és a meghajtó engedélyezése átirányítása:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Ezzel a parancsmaggal használható vágólap megosztásának letiltása:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Ne felejtse el teljesen jelentkezzen ki a webhelycsoport összes felhasználója (és nem csak kapcsolat bontása őket), tesztelje a módosítás előtt. Annak érdekében, hogy felhasználók teljesen kijelentkezett, nyissa meg a gyűjteményben az Azure-portálon **munkamenetek** lapon, és megszakad a kapcsolata, vagy aláírt rendelkező összes felhasználó kijelentkezik. Előfordul, hogy is eltarthat néhány másodpercig az a helyi meghajtón, szeretné megjeleníteni az Explorer belül a munkamenetet.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>A Windows-ügyfél USB átirányítási beállítások módosítása

Ha szeretne RemoteApp kapcsolódó rendszerű számítógépen használt USB-átirányítást, 2 tevékenységek vannak, fordulhat elő kell. 1 – a rendszergazdának engedélyeznie USB-átirányítása a webhelycsoport szintjén Azure PowerShell használatával. 2 – egyenként az összes eszközön, amelyhez használni USB-átirányítást hogy engedélyeznie kell a csoportházirendet alkalmaz, amely lehetővé teszi, hogy. Ezt a lépést kell végrehajtania minden olyan felhasználóhoz, USB-átirányítást használni szeretné.

> [AZURE.NOTE] Azure RemoteApp USB-átirányítást csak a Windows rendszerű számítógépeken támogatott.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>A RemoteApp vonatkozóan USB-átirányítás engedélyezése
Ahhoz, hogy a webhelycsoport szintjén USB-átirányítást használja a következő parancsmagot:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Az ügyfélszámítógép USB-átirányítás engedélyezése

USB-átirányítási beállítások konfigurálása a számítógépen:

1. Nyissa meg a helyi csoportházirend-szerkesztőben (GPEDIT. MSC). (Gpedit.msc futtassa a parancssorból.)
2. Nyissa meg a **Számítógép konfigurációja\Házirendek\Felügyeleti sablonok\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali kapcsolat Client\RemoteFX USB eszköz átirányítása**.
3. Kattintson duplán **a jelen számítógépen más támogatott RemoteFX USB-eszközök engedélyezése RDP átirányítása**.
4. Jelölje ki a **engedélyezve**, és válassza a **Rendszergazdák és a felhasználók a RemoteFX USB átirányítás engedélyeket**.
5. Nyisson meg egy parancssorablakot rendszergazdai engedélyekkel rendelkező, és futtassa az alábbi parancsot:

        gpupdate /force
6. Indítsa újra a számítógépet.

A csoportházirend-kezelő eszköz segítségével létrehozása és alkalmazása az USB-átirányítási házirend az összes számítógépre történő az Ön tartományában lévő:

1. Jelentkezzen be a tartományvezérlőnek a tartomány rendszergazdájaként.
2. Nyissa meg a Csoportházirend kezelése konzolban. (Kattintson a **Start > Felügyeleti eszközök > csoportházirend-kezelés**.)
3. Nyissa meg a tartomány vagy a szervezeti egységre, amelyhez hozzá szeretne létrehozni a házirend.
4. Kattintson a jobb gombbal az **Alapértelmezett tartomány házirend**, és kattintson a **Szerkesztés**gombra.
5. Nyissa meg a **Számítógép konfigurációja\Házirendek\Felügyeleti sablonok\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali kapcsolat Client\RemoteFX USB eszköz átirányítása**.
6. Kattintson duplán **a jelen számítógépen más támogatott RemoteFX USB-eszközök engedélyezése RDP átirányítása**.
7. Jelölje ki a **engedélyezve**, és válassza a **Rendszergazdák és a felhasználók a RemoteFX USB átirányítás hozzáférési jogait**.
8. Kattintson az **OK gombra**.  
