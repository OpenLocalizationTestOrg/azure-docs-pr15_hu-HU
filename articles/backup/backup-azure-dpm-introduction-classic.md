<properties
    pageTitle="Azure DPM biztonsági másolat – bevezetés |} Microsoft Azure"
    description="Az Azure biztonsági másolat szolgáltatással DPM kiszolgálók mentésével bemutatása"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="System Center adatok védelme Manager, adatkezelő védelmét, dpm biztonsági mentése"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Biztonsági másolat készítése munkaterhelésekből DPM az Azure előkészítése

> [AZURE.SELECTOR]
- [Azure biztonsági kiszolgálón](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure biztonsági Server (klasszikus)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klasszikus)](backup-azure-dpm-introduction-classic.md)


Ez a cikk a Microsoft Azure mentéssel védelmét, a rendszer központ adatok védelme Manager (DPM) kiszolgálók és a feladatok áttekintése. Azt olvasásával fogja látnia:

- Hogyan működik a Azure DPM kiszolgáló biztonsági mentése
- Egy zökkenőmentes biztonsági élmény érdekében az Előfeltételek
- A szokásos hibák és a velük kezelése
- Támogatott felhasználási területei

System Center DPM biztonsági másolatot készít a fájl- és adatok. Biztonsági másolat DPM adatok lehetnek a lemezen, a szalag-on tárolt vagy biztonsági másolat a Microsoft Azure mentéssel Azure. DPM hogyan kommunikáljon a Azure biztonsági mentése az alábbi képlettel történik:

- **Fizikai kiszolgáló vagy a helyszíni virtuális gép DPM telepítése** – Ha DPM telepítik egy fizikai kiszolgálóhoz, vagy egy helyszíni a Hyper-V virtuális gép biztonsági másolatot készíthet adatok egy, a mellett a lemez és szalag Azure biztonsági tárolóból elemre kattintva biztonsági.
- **Az Azure virtuális gép szerint rendszerbe DPM** – a System Center 2012 R2 frissítés 3, DPM telepíthető az Azure virtuális gép szerint. Ha DPM telepíti, az Azure virtuális gép biztonsági másolatot készíthet adatok Azure lemezre a DPM Azure virtuális géphez csatlakoztatott, vagy meg is kiürítése a adattárolás által mögötte felfelé az Azure biztonsági másolat tárolóból elemre.

## <a name="why-backup-your-dpm-servers"></a>Miért biztonsági másolatot készíteni a DPM servers?

Azure biztonsági másolat használata DPM kiszolgálók mentésével üzleti előnyei többek között:

- A helyszíni DPM telepítéshez Azure biztonsági másolat szalag hosszú távú telepítés alternatívájaként is használhatja.
- Telepítésekhez DPM Azure-ban Azure biztonsági másolat lehetővé teszi az Azure lemezről tároló kiürítése lehetővé teszi a régebbi adatokat tárolja az Azure biztonsági mentése és az új adatok lemezen méretezni.

## <a name="how-does-dpm-server-backup-work"></a>Hogyan működik a DPM server biztonsági másolat?
Egy virtuális számítógépre mentéséről, először az adatai pillanatképét pont és az idő van szükség. A biztonsági másolat Azure szolgáltatás a biztonsági mentési feladat elindítja az ütemezett időpontban, és elindítja a biztonsági másolat bővítmény pillanatképet készíthet. A biztonsági másolat bővítmény koordináták konzisztencia eléréséhez a Vendég VSS szolgáltatással, és elindítja a blob pillanatkép API a tárhely Azure szolgáltatás, konzisztencia elérése után. Ez történik, kaphat a virtuális gép lemezt következetes pillanatképet anélkül, hogy le.

Után a pillanatkép jelent, az adatok átkerülnek az Azure biztonsági szolgáltatás által a biztonsági másolat tárolóból elemre. A szolgáltatás azonosítása és módosultak a legutóbbi biztonsági másolatot készít biztonsági másolatokat tárolása és hálózati hatékony blokkok átvitele gondoskodik. Az adatátvitel befejezése után a pillanatkép törlődik, és a helyreállítási pont jön létre. A helyreállítási pont az Azure klasszikus portálon is láthatja.

>[AZURE.NOTE] Linux virtuális gépekhez csak fájl egységes biztonsági másolat lehetőség.

## <a name="prerequisites"></a>Előfeltételek
Készítse elő az Azure biztonsági másolat adatok biztonsági mentéséhez DPM az alábbi képlettel történik:

1. **A biztonsági másolat tárolóra létrehozása** – az Azure biztonsági másolat konzolban hozzon létre egy tárolóból elemre.
2. **Letöltés tárolóra hitelesítő adatait** – az Azure biztonsági másolatot, töltse fel a létrehozott management tanúsítványt a tárolóból elemre.
3. **Telepítse az Azure biztonsági ügynök, és a kiszolgálón regisztrálását** – az Azure biztonsági másolat, minden egyes DPM kiszolgálón ügynököt és DPM kiszolgáló regisztrálása a biztonsági másolat tárolóból elemre.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Követelmények (és érvényes korlátozások)

- DPM fizikai kiszolgáló vagy a Hyper-V virtuális gép System Center 2012 SP1 vagy System Center 2012 R2 rendszeren futtatható. Is futhat-Azure virtuális gépen futó legalább System Center 2012 R2, mint DPM 2012 R2 frissítés összesítő 3-as vagy egy Windows virtuális gép VMWare legalább futó System Center 2012 R2 a frissítés összesítő 5.
- Ha a DPM System Center 2012 SP1 futtat, telepítenie kell System Center adatok védelme Manager SP1 rendszer 2. Szükség, mielőtt telepíthetné az Azure biztonsági másolat ügynök.
- A DPM kiszolgálón van, hogy a Windows PowerShell és a .net keretrendszer 4.5 telepítve.
- DPM biztonsági másolatot készíthet a legtöbb munkaterhelésekből Azure biztonsági másolatot készít. Lásd: Mi támogat teljes listáját a Azure biztonsági mentés támogatja az alábbi elemek.
- Azure biztonsági másolat tárolt adatok nem állíthatók "szalag Másolás" lehetőséggel.
- Az Azure biztonsági mentés funkciót engedélyezve van szüksége lesz az Azure-fiók. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. További információ: [Azure biztonsági másolat árak](https://azure.microsoft.com/pricing/details/backup/).
- Azure biztonsági másolat használatához az Azure biztonsági másolat ügynök készítsen biztonsági másolatot szeretne kiszolgálókon telepítve legyen. Minden kiszolgáló másolat készül, amelyekhez helyi ingyenes tárhelyet adatokat méretének legalább 10 %-át kell rendelkeznie. Például 100 GB-nyi adatok biztonsági másolatának szükséges legalább 10 GB szabad lemezterület az üres helyen. A minimális érték 10 %-os, miközben a szabad helyi tárhely a gyorsítótár helyét használandó 15 %-kal ajánlott.
- Adatok az Azure tárolóra tárolója szeretne tárolni. Nincs korlátozás akkor is vissza felfelé az Azure biztonsági másolat vault adatok mennyiségét, de egy adatforráshoz (például egy virtuális gép vagy egy adatbázis) mérete ne haladja meg a 54,400 GB.

A következő fájltípusok támogatott Azure felfelé vissza:

- Titkosított (teljes biztonsági másolatok csak)
- Tömörített (növekményes biztonsági támogatott)
- A ritka (növekményes biztonsági támogatott)
- Tömörített és ritka (Sparse kezelt)

És ezek nem támogatott:

- Nem támogatott a fájlrendszerben kis-és nagybetűket kiszolgálóján.
- Rögzített hivatkozások (kihagyott)
- Újraelemzési pontok (kihagyott)
- Titkosított és tömörített (kihagyott)
- Titkosított és ritka (kihagyja)
- Tömörített adatfolyam
- A ritka adatfolyam

>[AZURE.NOTE] A System Center 2012 DPM SP1 kezdve, az Ön is készítsen biztonsági másolatot fel a Microsoft Azure biztonsági másolat eszközzel Azure DPM védik munkaterhelésekből.
