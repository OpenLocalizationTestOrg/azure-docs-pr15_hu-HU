
<properties
    pageTitle="Azure RemoteApp kép követelmények |} Microsoft Azure"
    description="Ismerje meg, a vonatkozó követelményeket ismertető képek létrehozása az Azure RemoteApp használandó"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Azure RemoteApp képek vonatkozó követelmények

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp összes, hogy a felhasználók a megosztani kívánt program használ a Windows Server 2012 R2 képe. Egyéni kép készítése elindíthatja a meglévő képet, vagy [Hozzon létre egy újat](remoteapp-create-custom-image.md).

> [AZURE.TIP] Tudta, hogy az Azure RemoteApp előfizetés férhet hozzá az Azure virtuális gyűjteményben, amelyek segítségével saját sablon kép létrehozása a Windows Server 2012 R2 képként? [Jelölje ki](remoteapp-image-on-azurevm.md).  


A kép Azure RemoteApp való használatra feltölthető követelményei vannak:


- Egyéni alkalmazások nem adattárolásra helyileg a képet. Ezeket a képeket az állapot nélküli és alkalmazások csak tartalmazhat.
- A kép nem tartalmaz adatokat elveszhetnek.
- A kép mérete MB többszöröse kell lennie. Ha megpróbál, amely nem egy pontos több képet feltölteni, a feltöltés sikertelen lesz.
- A kép mérete kell 127 GB vagy kisebb.
- A virtuális fájl kell lennie (VHDX fájlok jelenleg nem támogatottak).
- A virtuális nem lehet generációs 2 virtuális géphez.
- A virtuális rögzített méretű vagy lehet dinamikusan növekvő. Egy dinamikusan növekvő virtuális használata javasolt, mert a kisebb, mint egy rögzített méretű virtuális fájl feltöltése a Azure időt vesz igénybe.
- A lemez inicializálni kell a fő rekord (Rendszertöltő) szétválasztás stílust használ. A globálisan egyedi azonosítója partíciót (GPT) partíciót táblázatstílus nem támogatott.
- A virtuális tartalmaznia kell a Windows Server 2012 R2 egyetlen telepítés. Több kötet, de csak egy Windows példányának tartalmazó tartalmazhat.
- A távoli asztali munkamenet Host (RDSH) szerepkört, és az asztali élmény szolgáltatást telepítve kell lennie.
- A távoli asztali kapcsolat ügynök szerepkör kell *nem* telepíthető.
- A titkosított (EFS) kell tiltható le.
- A kép kell lennie a paramétereket alkalmaz a Sysprep segédprogrammal előkészített **oobe / generalize/shutdown** (ne használja a **/mode:vm** paraméter).
- A virtuális a pillanatkép lánc feltöltése nem támogatott.

Lásd: az [Azure RemoteApp képet létrehozása](remoteapp-imageoptions.md) képek létrehozása az Azure RemoteApp további információt.
