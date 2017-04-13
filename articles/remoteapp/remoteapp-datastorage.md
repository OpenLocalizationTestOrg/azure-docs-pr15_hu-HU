
<properties
    pageTitle="Soha ne tároljon bizalmas adatokat egyéni képek az Azure RemoteApp |} Microsoft Azure"
    description="További tudnivalók: adatok tárolása az Azure RemoteApp egyéni képek szabályai"
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


# <a name="never-store-sensitive-data-on-custom-images"></a>Soha nem bizalmas adatokat tárolja egyéni képek

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ha Ön üzemelteti saját alkalmazás az Azure RemoteApp, első lépésként egyéni képét. A saját kép segítségével virtuális példányai, amelyek az alkalmazások szolgálják a felhasználók létrehozása. A saját kép csak alkalmazásokat és elveszhetnek, például az SQL-adatbázisait, személyzeti fájlok alapján vagy speciális adatfájlok például QuickBooks vállalat fájlok soha nem bizalmas adatokat tartalmazhat. Az összes bizalmas adatokat kell elhelyezkednie külső az Azure RemoteApp fájlkiszolgálón, egy másik Azure virtuális, vagy SQL Azure-ban. A kép csak kell tárolni az alkalmazást, az adatforráshoz kapcsolódik, és bemutatja az adatokat. Tekintse át a [Azure RemoteApp képek követelményeiről](remoteapp-imagereqs.md) további információt. 

Ha meg szeretné érteni, hogy miért nem kell tárolni bizalmas adatokat, kell Azure RemoteApp működésének megértése. Amikor egy webhelycsoport létrehozásakor vagy frissítésekor, a háttérben több klónok vagy a kép példányainak jönnek létre. A virtuális összes előfordulását másolatai pontosan a saját kép; Amikor a felhasználók alkalmazások csatlakoznak az ilyen virtuális esetek egyike. De nem biztos a példányt, és nem számít, mivel ezek nem állandó. A virtuális szolgáltatója, az alkalmazások elemei nem állandó és megsemmisült vagy törölt alapul, például a webhelycsoport-frissítés során. 

Miután a webhelycsoport már kiépítve, és a VMs kapcsolódás elindíthatók, állandó-e a felhasználói adatok és a védett, mert azt mentése külön tárolón belül egy virtuális, hogy a [felhasználói profil lemez (UPD)](remoteapp-upd.md), amely a felhasználói profil c:\users a hívása\<használata >. Az alkalmazás indításakor a UPD csatlakoztatott és helyi felhasználói profil hasonlóan kezeli az operációs rendszer. További információ a hogyan [Azure RemoteApp menti a felhasználó adatai és beállításai](remoteapp-upd.md).

Példa adatok, amelyek nem kell elhelyezni a képet:

- Megosztott adatok eléréséhez a felhasználók számára
- SQL DB- vagy QuickBooks-adatbázis
- A D:\ adatokról

Példa adatok, amelyek az alapértelmezett profil be minden felhasználói UPD másolandó is találhatók:

- Felhasználónként konfigurációs fájl
- Parancsfájlok felhasználók kellene megmaradnak a UPD

Összefoglalás:

- Soha nem bizalmas adatokat tárolja, amelyek a saját kép létrehozása a kép megszakadt.
- Bizalmas adatokat mindig kell elhelyezkednie külön Azure virtuális, a felhőben, és a virtuális példányaiban szolgáltatója, az alkalmazások belül Azure RemoteApp mindig külső külön fájlba-kiszolgálón. 
- Felhasználói adatok mentésekor, és továbbra is fennáll, az a felhasználói profil lemez (UPD)


