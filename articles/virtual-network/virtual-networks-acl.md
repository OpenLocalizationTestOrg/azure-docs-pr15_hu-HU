<properties
   pageTitle="Mi az, hogy egy hálózati hozzáférés vezérlő lista (vezérlés)?"
   description="Tudnivalók a hozzáférés-vezérlési listák"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="what-is-an-endpoint-access-control-list-acls"></a>Mi az hozzáférés-vezérlési lista (hozzáférés-vezérlési listák) zárólap?

Zárólap Access vezérlőelem-lista (vezérlés) egy biztonsági fejlesztés az Azure telepítéshez érhető el. Egy vezérlés lehetővé teszi a szelektív engedélyezése vagy tiltása a virtuális gép végpont irányítja. A csomag szűrési képesség egy további rétege biztonsági biztosít. Hálózati hozzáférés-vezérlési listák csak a végpontok is megadhat. Egy virtuális hálózat vagy egy virtuális hálózaton lévő adott alhálózat vezérlés nem adhat meg.

> [AZURE.IMPORTANT] Amikor csak lehetséges hozzáférés-vezérlési listák helyett hálózati biztonsági csoportok (NSGs) használata ajánlott. További tudnivalók a NSGs, [Mi az hálózati biztonsági csoport?](virtual-networks-nsg.md).

Hozzáférés-vezérlési listák PowerShell vagy az adatkezelési portál használatával beállíthatók. Vezérlés hálózat beállítása a PowerShell használatával, olvassa el a [Kezelése Access (-szabályozási listák) végpontok PowerShell használatával](virtual-networks-acl-powershell.md)című témakört. Az adatkezelési portál használatával egy hálózati vezérlés konfigurálásához megtudhatja, [hogy miként állítsa be végpontok virtuális géphez](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

Hálózati hozzáférés-vezérlési listák használ, akkor is tegye a következőket:

- Szelektív lehetővé teszik, vagy megtagadja a bejövő forgalom távoli alhálózat IPv4-címtartományokat egy virtuális gép beviteli végpontra alapján.

- Feketelistás IP-címek

- Egy virtuális gép végpont több szabályok létrehozása

- Adjon meg egy virtuális gép végpont legfeljebb 50 vezérlés szabályok

- Egy adott virtuális gép végpont (legalacsonyabb sorrendbe) használata a szabály rendezés ahhoz, hogy a megfelelő színkészletet szabályok érvényesek

- Adja meg, hogy egy adott IPv4-cím távoli alhálózat-vezérlés.

## <a name="how-acls-work"></a>Hogyan működnek a hozzáférés-vezérlési listák

Egy olyan objektum, amely a szabályok listáját tartalmazza. Ha létrehoz egy vezérlés alkalmazhatja azt egy virtuális gép végpontot, csomagok szűrése megy végbe, a virtuális host csomóponton. Ez azt jelenti, hogy a forgalmat, a távoli IP-címek a host csomópontot a virtuális a nem egyező vezérlés szabályok szerint szűrt. Ez a virtuális megakadályozza a értékes Processzor ciklus kiadások csomag szűrésével.

Virtuális gép létrehozásakor egy alapértelmezett vezérlés zárolhatja az összes bejövő forgalom helyen helyezni. Jó helyen jár Ha a végpontok jön létre (port 3389), majd az alapértelmezett vezérlés módosult használó összes bejövő forgalmának engedélyezésére. Bejövő forgalmat, a minden olyan távoli alhálózat majd engedélyezett végpontra, és nincs tűzfal kiépítési szükség. Minden más portokat blokkolt bejövő forgalom, kivéve, ha a végpontok azokat a portokat jönnek létre. Alapértelmezés szerint engedélyezve van a kimenő forgalmának engedélyezésére.

**Alapértelmezett vezérlés – példatáblázat**

| **Szabály #** | **Távoli alhálózat** | **Végpont** | **Engedély/elutasítása** |
|--------|---------------|----------|-------------|
| 100    | 0.0.0.0/0     | 3389     | Engedély      |

## <a name="permit-and-deny"></a>Engedélyezése és tiltása

Szelektív lehetővé teszik, vagy megtagadja a virtuális gép beviteli végpont hálózati forgalmának szabályokat adja meg a "lehetővé teszik" vagy "megtagadja" létrehozásával. Fontos tudni, hogy alapértelmezés szerint zárólap létrehozásakor minden forgalom számára engedélyezett az végpontot. Éppen ezért fontos megtudhatja, hogyan engedély/elutasítása szabályok létrehozása és helyezze el őket a megfelelő sorrendben elsőbbségi Ha azt szeretné, hogy a hálózati forgalmat, amelyet választva engedélyezi a virtuális gép végpont eléréséhez részletes szabályozható.

Megfontolandó szempontok:

1. **Nincs vezérlés –** Alapértelmezés szerint zárólap létrehozásakor azt teszi lehetővé az végpontot az összes.

1. **Lehetővé teszik-** Amikor felvesz egy vagy több "lehetővé teszik" tartományokat, a minden tartomány van megtagadja a alapértelmezés szerint. Csak az engedélyezett IP-tartomány csomagok tudnak kommunikálni az virtuális gép végpontot.

1. **-Elutasítása** Amikor felvesz egy vagy több "megtagadja" tartományokat, alapértelmezés szerint vannak lehetővé tevő a forgalom az összes többi cellatartományok.

1. **Engedélyezése és tiltása - kombinációja** "Lehetővé teszik a" és "megtagadja" Ha meg szeretné meg egy adott IP-tartomány engedélyezett, vagy megtagadja carve kombinációját is használhatja.

## <a name="rules-and-rule-precedence"></a>Szabályok és szabályok elsőbbségi sorrendjéről

Hálózati hozzáférés-vezérlési listák adott virtuális gép végponton állíthat be. Ha például a lefelé az access mely zárolások bizonyos IP-címek virtuális gépen létrehozott zárólap RDP hálózati vezérlés is megadhat. Az alábbi táblázatban látható nyilvános virtuális IP-címei (VIP) az egy bizonyos tartományon lehetővé teszi az access-RDP hozzáférés megadása lehetőséget. Az összes többi távoli IP-címei van megtagadja. A *legalacsonyabb elsőbbséget* szabály sorrendet kövesse azt.

### <a name="multiple-rules"></a>Több szabályok

Az alábbi példában ha azt szeretné, hogy a RDP végpont való hozzáférés engedélyezése csak a két nyilvános IPv4 címtartományok (65.0.0.0/8, és 159.0.0.0/8), érhet el a két *lehetővé teszik* szabályok megadásával. Ebben az esetben mivel RDP virtuális géphez alapértelmezés szerint jön létre, érdemes egy távoli alhálózat alapján RDP porthoz való hozzáférés zárolásához. Az alábbi példában látható nyilvános virtuális IP-címei (VIP) az egy bizonyos tartományon lehetővé teszi az access-RDP hozzáférés megadása lehetőséget. Az összes többi távoli IP-címei van megtagadja. Hálózati hozzáférés-vezérlési listák beállíthatja az egy adott virtuális gép végpontot, mert a hozzáférés megtagadva alapértelmezés szerint működik.

**Példa: a több szabállyal**

| **Szabály #** | **Távoli alhálózat** | **Végpont** | **Engedély/elutasítása** |
|--------|---------------|----------|-------------|
| 100    | 65.0.0.0/8    | 3389     | Engedély      |
| 200    | 159.0.0.0/8   | 3389     | Engedély      |

### <a name="rule-order"></a>Szabályok sorrendje

Több szabállyal zárólap adható meg, mert egy módját szabályok annak meghatározásához, hogy melyik szabály elsőbbséget kell. A szabály sorrendben adja meg az elsőbbségi. Hálózati hozzáférés-vezérlési listák hajtsa végre a *legalacsonyabb elsőbbséget* szabály sorrendet. Az alábbi példában a 80-as port végponton szelektív hozzáférést kap csak bizonyos IP-címtartományok. E beállítás azt, hogy a Megtagadás szabályt (szabály \# 100) címekhez a 175.1.0.1/24 helyen. A második szabály majd prioritású 200, amely lehetővé teszi a 175.0.0.0/8 más címekre való hozzáférés van megadva.

**Példa – szabályok elsőbbségi sorrendjéről**

| **Szabály #** | **Távoli alhálózat** | **Végpont** | **Engedély/elutasítása** |
|--------|---------------|----------|-------------|
| 100    | 175.1.0.1/24  | 80       | Elutasítása        |
| 200    | 175.0.0.0/8   | 80       | Engedély      |

## <a name="network-acls-and-load-balanced-sets"></a>Hálózati hozzáférés-vezérlési listák, és töltse be a kiegyensúlyozott beállítása

Hálózati hozzáférés-vezérlési listák egy terheléselosztása (LB beállítása) beállítása végpont adható meg. Ha egy LB beállítása egy vezérlés van megadva, a hálózati vezérlés adott LB található összes virtuális gépeken futó érvényes. Például ha egy LB beállítása "80-as Port" jön létre, és 3 VMs LB a Set tartalmaz, a hálózati vezérlés létrehozott végpont "80-as Port" egy virtuális automatikus alkalmazása a többi VMs.

![Hálózati hozzáférés-vezérlési listák, és töltse be a kiegyensúlyozott beállítása](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Következő lépések

[Kezelése Access (-szabályozási listák) végpontok PowerShell használatával](virtual-networks-acl-powershell.md)
