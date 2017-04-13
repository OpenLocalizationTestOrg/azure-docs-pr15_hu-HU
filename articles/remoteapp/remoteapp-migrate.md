
<properties
    pageTitle="Felhasználói adatok áttelepítése Azure RemoteApp |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti át a felhasználói adatok és Azure RemoteApp kijelentkezés."
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



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Adatok áttelepítése be- és kijelentkezés a Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Számos különböző eszközök és módszerek segítségével átviheti a [felhasználói adatok](remoteapp-upd.md) be- és kijelentkezés a Azure RemoteApp. Íme néhány:

- Másolás és beillesztés az vágólap megosztásának használata
- Fájlok másolása és fájlkiszolgálóra adatok
- Fájlok másolása a OneDrive vállalati verzió böngészőn keresztül
- Fájlok másolása az átirányítás segítségével

>[AZURE.NOTE] 
> A OneDrive engedélyezése nem vállalati vagy fogyasztói szinkronizálási ügynökök - azokat az Azure RemoteApp a [nem támogatott](remoteapp-onedrive.md) .

## <a name="use-copy-and-paste-in-file-explorer"></a>Másolással és beillesztéssel a Fájlkezelőben

Másolás és beillesztés a vágólap segítségével RemoteApp telepítések [alapértelmezés szerint](remoteapp-redirection.md)engedélyezve van. Ez lehetővé teszi a felhasználóknak, másolja a fájlok helyi PC-re és RemoteApp alkalmazás között. Gyakran keresztül a RemoteApp az alkalmazások használata a rendes felhasználók mentett fájlokat a UPDs –, hogy könnyen-e ki RemoteApp adatok áthelyezése:

1. [Fájlkezelő közzététele meg az app](remoteapp-publish.md) RemoteApp gyűjteménye. (Tájékoztatjuk, hogy ez a felügyeleti feladatot.)
2. A felhasználók, indítsa el az Ön által közzétett Fájlkezelőben alkalmazást, és használhatja a fájlok másolása és beillesztése a UPD be- és ki belőlük a közvetlen.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Töltse fel fájljait és adatait fájlkiszolgálóra szabványos hálózati fájlmásolás használatával

Gyakran szervezetek használva fájlkiszolgálókhoz általános adatok. Ha ismeri a kiszolgáló nevét vagy helyét, a felhasználók keresse meg a kiszolgáló a helyi hálózaton, és a fájlokat, majd másolja a sokkal megszokott módon felett. Újra szeretné RemoteApp Fájlkezelőben közzététele, és megoszthatja a felhasználókkal.

>[AZURE.NOTE] 
> A fájl kiszolgálói RemoteApp telepítették az átirányítható hálózaton kell lennie.

## <a name="copy-files-to-onedrive-for-business"></a>Fájlok másolása a OneDrive vállalati verzióban
Bár nem engedélyezi a OneDrive vállalati verzió szinkronizálási ügynök a RemoteApp, továbbra is másolhatja fájlokat a UPD a onedrive vállalati verzióba böngészőn keresztül. 

1. Fájlkezelő közzététele RemoteApp, és ezután tájékoztassa a felhasználókat a fájlok elérése, hogy az alkalmazás között. 
2. A legegyszerűbb fájlok átvitele Ha az azok tömörített, így a felhasználók hozzunk létre, amely tartalmazza az összes fájl áthelyezése a OneDrive vállalati verzió .zip fájlt.
3. Kérje meg a felhasználók az Office 365 portált, és nyissa meg a OneDrive és a .zip fájl feltöltése.

## <a name="copy-files-by-using-drive-redirection"></a>Fájlok másolása a meghajtó átirányítása használatával

Ha engedélyezte a [meghajtó átirányítása](remoteapp-redirection.md), a felhasználók már létrehozott egy csatlakoztatott meghajtóra. Ebben az esetben azok zip-azok az átirányított meghajtón lévő fájlok, és mentse őket a helyi számítógépre.