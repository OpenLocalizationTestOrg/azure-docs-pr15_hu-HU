
<properties
    pageTitle="Az Azure RemoteApp alkalmazás – követelmények |} Microsoft Azure"
    description="További tudnivalók: Azure RemoteApp használni kívánt alkalmazások vonatkozó követelmények"
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



# <a name="app-requirements"></a>Alkalmazás – követelmények

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp adatfolyam 32 bites vagy 64 bites Windows-alapú alkalmazásokat támogatja a Windows Server 2012 R2 képből. A legtöbb meglévő 32 bites vagy 64 bites Windows-alapú alkalmazások futtatásához "," az Azure RemoteApp (távoli asztali szolgáltatásokat vagy a korábbi néven Terminálszolgáltatások) környezetben. Azonban fut, és működik közötti különbség van – egyes alkalmazások működik helyesen, és hajtsa végre, míg mások nem. A következő információkat nyújt útmutatást távoli asztali környezetben alkalmazások fejlesztése, és tesztelje: a kompatibilitás érdekében.

Tipp: Dolgozunk a probléma a munka példák az alkalmazások létrehozása meg. Új megvitatása a Microsoft Access, a QuickBooks és az alkalmazás-V használata RemoteApp témakörökben talál.

## <a name="requirements"></a>Követelmények
Három ezeknek a követelményeknek, ha követik, Súgó az alkalmazás jól RemoteApp fut:

1.  Felel meg az összes [hitelesítő követelményeket az asztali Windows-alkalmazások](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) és a [távoli asztali szolgáltatásokat programozási irányelvek](https://msdn.microsoft.com/library/aa383490.aspx) igazodjon-alkalmazások teljes kompatibilitásának RemoteApp lesz.
2.  Alkalmazások kell soha nem adattárolásra helyileg a kép vagy RemoteApp példányai, amelyek elveszhetnek.  Miután létrehozott egy RemoteApp webhelycsoport, a példányok vannak klónozva állapot nélküli és alkalmazások csak tartalmazhat. Külső forrásból vagy a felhasználói profil tárolja az adatokat.
3.  Egyéni képek soha nem kell elveszhetnek adatokat tartalmaznak.  

## <a name="testing-your-apps"></a>Az alkalmazások tesztelése
Használja ezeket a lépéseket követve alkalmazások tesztelése:

1.  A Windows Server 2012 R2 és az alkalmazás telepítése
2.  Távoli asztal engedélyezése
3.  Két felhasználói fiókok, ' a ' felhasználó és biztosít, mind a felhasználói fiókok felvétele a távoli asztali biztonsági csoport létrehozása
4.  Több elem munkamenet kompatibilitás ellenőrzése a két egyidejű távoli asztali munkamenetek arról, hogy a PC-n az alkalmazás indítása közben létrehozásával.
5.  Alkalmazás működése ellenőrzése

## <a name="application-development-guidelines"></a>Alkalmazás fejlesztési útmutató
Kövesse az alábbi útmutatásokat RemoteApp-alkalmazások fejlesztésével.

### <a name="multiple-users"></a>Több felhasználó

- Egy- [egy felhasználó az alkalmazás ](https://msdn.microsoft.com/library/aa380661.aspx)telepítése hozhat létre problémák többfelhasználós környezetben.
- Alkalmazások kell a [felhasználói adatok tárolására](https://msdn.microsoft.com/library/aa383452.aspx) a felhasználó-specifikus helyek elkülönítve globális információt, amely az összes felhasználóra vonatkozik.
- Távoli alkalmazás formájában használva használja több [névtér kernel objektumok](https://msdn.microsoft.com/library/aa382954.aspx); globális névteret elsősorban a szolgáltatások ügyfél-kiszolgáló alkalmazások által használt.
- Még nem biztonságos feltételezik, hogy a számítógép és a számítógép hozzárendelt [IP-cím](https://msdn.microsoft.com/library/aa382942.aspx) társítva egyetlen felhasználó mivel több felhasználó be kell bejelentkeznie egyidejű távoli asztali munkamenetgazda (munkamenetgazda) kiszolgálón.

### <a name="performance"></a>Teljesítmény
- Tiltsa le a [grafikus effektusai](https://msdn.microsoft.com/library/aa380822.aspx) , mielőtt RemoteApp hozzáadni az alkalmazást.
- Teljes méretűre állíthatja a Processzor elérhetőség az összes felhasználó számára, tiltsa le a [háttérben futó feladatokat](https://msdn.microsoft.com/library/aa380665.aspx) , vagy, amelyek nem erőforrás-igényes hatékony háttér-feladatok létrehozása.
- Érdemes finomhangolása és egyenleg alkalmazás [szál használatát](https://msdn.microsoft.com/library/aa383520.aspx) többfelhasználós, többprocesszoros környezetben.
- A teljesítmény optimalizálása tanácsos alkalmazások [észleli](https://msdn.microsoft.com/library/aa380798.aspx) , hogy az ügyfél futnak.
