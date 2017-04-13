<properties
    pageTitle="Mielőtt beállítaná Azure Papírhalom Ez |} Microsoft Azure"
    description="A környezet és a hardver követelmények Azure Papírhalom ez (szolgáltatás rendszergazdája) megtekintése."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Azure Papírhalom telepítési előfeltételek

Mielőtt beállítaná Azure Papírhalom ez ([Vásárlási, amely](azure-stack-poc.md)), győződjön meg arról, hogy a számítógép megfelel az alábbi követelményeket.
Az Ez a technikai előzetes verzió 2 telepítésének követelményei megegyeznek a technikai előzetes verzió 1 szükséges. Emiatt a azonos hardver az előző egy beépített Preview használt is használhatja.

## <a name="hardware"></a>Hardveres

| Összetevő | Minimális  | Ajánlott |
|---|---|---|
| Meghajtó: operációs rendszer | 1 OS lemezt, amely legalább 200 GB érhető el a rendszerkövetelmé partition (SSD vagy merevlemez) | 1 OS lemezt, amely legalább 200 GB érhető el a rendszerkövetelmé partition (SSD vagy merevlemez) |
| Meghajtó: Azure Papírhalom ez általános adatok | 4 lemezt. Minden egyes lemez legalább 140 GB kapacitás (SSD vagy merevlemez) tartalmaz. Az összes rendelkezésre álló lemez fogja használni. | 4 lemezt. Minden egyes lemez legalább 250 GB kapacitás (SSD vagy merevlemez) tartalmaz. Az összes rendelkezésre álló lemez fogja használni.|
| Számítja ki: Processzor | Kettős-szoftvercsatorna: 12 fizikai magmintákat (összesen)  | Kettős-szoftvercsatorna: 16 fizikai magmintákat (összesen) |
| Számítja ki: memória | 96 GB RAM  | 128 GB RAM |
| Számítja ki: BIOS | A Hyper-V engedélyezve van (SLAT támogatással)  | A Hyper-V engedélyezve van (SLAT támogatással) |
| Hálózati: hálózati kártya | A Windows Server 2012 R2 hitelesítő szükséges hálózati kártya; nincs szükség speciális funkciókra | A Windows Server 2012 R2 hitelesítő szükséges hálózati kártya; nincs szükség speciális funkciókra |
| Hardveres embléma hitelesítésszolgáltató | [A Windows Server 2012 R2 minősített](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[A Windows Server 2012 R2 minősített](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Adatok meghajtón konfigurációs:** Az összes adat meghajtók típusa (az összes Társítások vagy az összes SATA) és a kapacitás kell lennie. Társítások lemezmeghajtók használata esetén a lemezmeghajtók (nincs MPIO, több elérési út támogatási megadva) egyetlen elérési útján mellékelni kell.

**HBA beállítási lehetőségek**
 
- (Elsődleges) Egyszerű HBA
- RAID HBA – kártya kell beállítania, "haladnia" módban
- RAID HBA – lemezt kell beállítania, mint egyetlen-lemezre, RAID-0

**Támogatott bus és médiafájlok írja be az összes lehetséges kombinációinak**

-   SATA MEREVLEMEZ

-   TÁRSÍTÁSOK MEREVLEMEZ

-   RAID MEREVLEMEZ

-   RAID SSD (ha médiatípus meghatározatlan/ismeretlen\*)

-   SATA SSD + SATA MEREVLEMEZ

-   TÁRSÍTÁSOK SSD + TÁRSÍTÁSOK MEREVLEMEZ

\*Átadó lehetőséggel nem RAID vezérlők nem ismeri fel a médiatípus. Az ilyen vezérlők meghatározatlan fog megjelölés merevlemez és SSD is. Ebben az esetben a SSD helyett eszközök gyorsítótárazás állandó tároló lesz. Emiatt a Microsoft Azure Papírhalom Ez a e SSD telepítheti.

**Példa kiszolgálókról**: LSI 9207-8i, LSI-9300-8i vagy LSI-9265-8i átadó módban

Minta Számítógépgyártó konfigurációk érhetők el.

## <a name="operating-system"></a>Operációs rendszer

| | **Követelmények**  |
|---|---|
| **Operációs rendszer** | A Windows Server 2012 R2 vagy újabb verziójában. Az operációs rendszer verziója nem kritikus a telepítés megkezdése előtt, mint a számítógép hardvergyártók fogja az Azure Papírhalom telepítési zip a virtuális. Az operációs rendszer és az összes szükséges javítások már integrálni a képet. Nem minden billentyűk segítségével a ez használt Windows Server példányok aktiválása.|

## <a name="deployment-requirements-check-tool"></a>Telepítési követelmények eszköz ellenőrzése

Az operációs rendszer telepítése után a [Telepítési Azure Papírhalom technikai előzetes verzió 2-ellenőrzőt](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) kattintva erősítse meg, hogy a hardver megfelel az összes is használhatja.



## <a name="microsoft-azure-active-directory-accounts"></a>Microsoft Azure Active Directory-fiókok

A Microsoft Azure Papírhalom ez példányban Azure kell csatlakoznia. Ezért elő kell készítenie egy Microsoft Azure Active Directory-fiókját az üzembe PowerShell-parancsprogramot futtatása előtt. Ehhez a fiókhoz a globális rendszergazda lesz az Azure Active Directory-bérlői webhelyen. Építse és-alkalmazások és az Azure Active Directory és a grafikus API-val interaktív Azure Papírhalom szolgáltatások szolgáltatás rendszerbiztonsági delegálása lesz. Is lesz az alapértelmezett szolgáltató előfizetést (amelynek később módosíthatja) tulajdonosaként. Akkor is jelentkezzen be a Azure Papírhalom rendszerének felügyeleti portál ezen a fiókon keresztül.

1. Létrehozhat egy Azure AD-fiókot, amely legalább egy Azure Active Directory directory rendszergazdája. Ha már van egy, használhatja azt. Egyéb esetben létrehozhat egyet ingyen az [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (kínai, látogasson el a <http://go.microsoft.com/fwlink/?LinkID=717821> helyette.)

    Mentse ezeket a hitelesítő adatokat a 6 [futtassa a PowerShell telepítési parancsfájlt](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Ez a *szolgáltatás-rendszergazda* fiók konfigurálása és a erőforrás felhőket, felhasználói fiókokat, bérlői tervek, kvótáinak és árak kezelése. A portálon azok webhely felhőket ábrázoló virtuális gép magánjellegű felhőket létrehozása, tervek létrehozása, és felhasználói előfizetések kezelése.

2. [Létrehozás](azure-stack-add-new-user-aad.md) közül legalább egyet, hogy jelentkezzen az Azure Papírhalom ez bérlői webhelyre, figyelembe.

  	| **Azure Active Directory-fiókját**  | **Támogatott?** |
  	|---|---| 
  	| Munkahelyi vagy iskolai fiók érvényes nyilvános Azure-előfizetéssel  | igen |
  	| A Microsoft Account érvényes nyilvános Azure-előfizetéssel  | nem |
  	| Munkahelyi vagy iskolai fiók Kínában érvényes Azure-előfizetéssel  | igen |
  	| Munkahelyi vagy iskolai fiók érvényes US kormányzati Azure-előfizetéssel  | igen |


## <a name="network"></a>Hálózati

### <a name="switch"></a>Váltás

Egy elérhető portja a ez gépen kapcsolót.  

Az Azure Papírhalom ez gép támogatja-e a Váltás az access vagy közvetlen port kapcsolódás. A Váltás a nincs speciális funkciókra van szükség. Ha a törzs portot használja, vagy ha egy virtuális ID konfigurálnia kell, meg kell adnia a helyi hálózat Azonosítójával telepítési paraméterként. Példák a [telepítési paramétereinek listáját](azure-stack-run-powershell-script.md)megtekintheti.

### <a name="subnet"></a>Alhálózat

Ne csatlakozzon a Ez a gép a következő alhálózat:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Ezek a alhálózat a Microsoft Azure Papírhalom ez környezetén belül a belső hálózatok számára vannak fenntartva.

### <a name="ipv4ipv6"></a>IPv4 és az IPv6

Csak IPv4 használata támogatott. Nem hozhatók létre IPv6-hálózatokat.

### <a name="dhcp"></a>DHCP

Ellenőrizze, hogy nem érhető el egy útja a hálózaton, amely a hálózati kártya csatlakozik. DHCP nem érhető el, ha elő kell készítenie egy további statikus IPv4 hálózati host által használt mellett. Meg kell adnia az IP-címet és az átjáró telepítési paraméterként. Példák a [telepítési paramétereinek listáját](azure-stack-run-powershell-script.md)megtekintheti.

### <a name="internet-access"></a>Internetkapcsolat

Azure Papírhalom férnie az internethez, vagy közvetlenül egy áttetsző proxyn keresztül. Azure Papírhalom nem támogatja a webes proxy Internet-hozzáférés engedélyezése a beállításait. Az állomás IP-Címét és az új IP rendelt az m/m BGPNAT01 (DHCP vagy statikus IP-cím) is hozzáférhet az internethez kell lennie. 80-as és 443-as a graph.windows.net és login.windows.net tartományok szolgálnak.

### <a name="telemetry"></a>Telemetriai

Telemetriai adatfolyam támogatásához (HTTPS) 443-as port nyitva a hálózaton kell lennie. Az ügyfél végpontot https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Következő lépések

[Az Azure Papírhalom ez telepítőcsomag letöltési](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Azure Papírhalom ez terjesztése](azure-stack-run-powershell-script.md)
