<properties
   pageTitle="Virtuális azonosító elérése"
   description="Ismerteti megnyitása és használata az Azure virtuális egyedi azonosító"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Megnyitása és használata az Azure virtuális egyedi azonosító

Azure virtuális egyedi azonosító pedig egy 128 azonosító kódolt és tárolt összes Azure IaaS virtuális 's SMBIOS jelenleg is olvashatók platform BIOS parancsokat.

Azure virtuális egyedi azonosító egy írásvédett tulajdonságot. Azure egyedi virtuális azonosító nem módosítása után indítsa újra a rendszert Leállítás (vagy tervezett tervezett), kezdési és befejezési vonja kiosztani, javító szolgáltatás vagy helyen visszaállítása. Jó helyen jár Ha a virtuális pillanatfelvételek, és hozzon létre egy új példányt másolja, új Azure virtuális azonosító van beállítva.

> [AZURE.NOTE] Ha van régebbi VMs létrehozott, és fut, mivel ez a funkció használ közzétételének (szeptember 18, 2014-es), kérjük, indítsa újra a virtuális, hogy automatikusan megkapja az Azure egyedi azonosítót.


Azure egyedi virtuális azonosító a belül a virtuális elérése:


## <a name="create-a-vm"></a>Hozzon létre egy virtuális
 

További információ a [virtuális gép létrehozása](virtual-machines-linux-creation-choices.md) című témakör tartalmaz.


## <a name="connect-to-the-vm"></a>A virtuális csatlakoztatása
 

További tudnivalókért lásd: [a Linux SSH](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Lekérdezés virtuális egyedi azonosító

(A példában **Ubuntu**) parancsot:

    sudo dmidecode | grep UUID
    
Példa a várt eredményeket:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Nagy Endian bit rendezés, hogy a tényleges egyedi azonosító virtuális ebben az esetben lesz:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure virtuális egyedi azonosító használható különböző forgatókönyvekben, hogy a virtuális Azure fut, vagy a helyszíni és az licencelési, a jelentéskészítő vagy az általános nyomon követés igényeknek megfelelően alakíthatja a Azure IaaS telepítések előfordulhat segíthetnek. Sok független gyártók alkalmazások létrehozásába és igazoló őket a Azure előírhatja az életciklus során az Azure virtuális azonosítása és állapítható meg, hogy fut-e a virtuális Azure, a helyszíni be- és egyéb felhő szolgáltatók. A platform azonosító például is segítségével, észleli, ha a megfelelő szoftvert és virtuális adatokról, például az adatforrás összehangolására segítésére beállításával a megfelelő mérési módja miatt a megfelelő platform és nyomon követésére és összehangolására ezeket a mértékek, más célra egyik ablaktábláról a másikra.
