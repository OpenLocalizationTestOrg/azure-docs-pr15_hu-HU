<properties
 pageTitle="Tervek és a számlázásra az Azure ütemező"
 description="Tervek és a számlázásra az Azure ütemező"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Tervek és a számlázásra az Azure ütemező

## <a name="job-collection-plans"></a>Feladat webhelycsoport csomagok

Feladat gyűjtemények az Azure ütemező számlázható entitás. Feladat gyűjtemények feladatok jellegű számértékeket tartalmaznak, és a három csomagon – ingyenes, normál és Premium – az alábbiakban azt.

|**Feladat webhelycsoport terv**|**Egy feladat webhelycsoport feladatok maximális száma**|**Max ismétlődés**|**Max feladat gyűjtemények előfizetésenként**|**Korlátai**|
|:---|:---|:---|:---|:---|
|**Ingyenes**|egy feladat webhelycsoport 5 feladatok|Óránként egyszer. Nem lehet végrehajtani, óránként egyszer gyakrabban feladatok|Előfizetés engedélyezett legfeljebb 1 ingyenes feladat gyűjtemény|[HTTP kimenő engedélyezési objektum](scheduler-outbound-authentication.md) nem használható.
|**Normál**|egy feladat webhelycsoport 50 feladatok|Percenkénti. Nem lehet végrehajtani, percben egyszer gyakrabban feladatok|Előfizetés engedélyezett legfeljebb 100 szabványos feladat gyűjtemények|Hozzáférés az ütemező teljes szolgáltatáskészlete|
|**P10 prémium verzió**|egy feladat webhelycsoport 50 feladatok|Percenkénti. Nem lehet végrehajtani, percben egyszer gyakrabban feladatok|Előfizetés legfeljebb 10 000 P10 prémium feladat gyűjtemények engedélyezett. <a href="mailto:wapteams@microsoft.com">Kapcsolatfelvétel</a> további.|Hozzáférés az ütemező teljes szolgáltatáskészlete|
|**P20 prémium verzió**|egy feladat webhelycsoport 1000-feladatok|Percenkénti. Nem lehet végrehajtani, percben egyszer gyakrabban feladatok|Előfizetés legfeljebb 10 000 P20 prémium feladat gyűjtemények engedélyezett. <a href="mailto:wapteams@microsoft.com">Kapcsolatfelvétel</a> további.|Hozzáférés az ütemező teljes szolgáltatáskészlete|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>A frissítések és Downgrades feladat a webhelycsoport csomagok

Előfordulhat, hogy frissítése vagy vissza léptetheti feladat webhelycsoport csomagra bármikor az ingyenes, normál és Premium csomagok között. Jó helyen jár Ha Visszalépés egy ingyenes feladat webhelycsoport-ról, a visszalépéssel kapcsolatos meghiúsulhat az alábbi okok valamelyike miatt:

- Ingyenes feladat gyűjtemény már létezik az előfizetés
- A projekt-gyűjteményben egy feladatot egy újabb ismétlődés ingyenes feladat gyűjtemények feladatok megengedettnél tartalmaz. A maximális ismétlődés egy ingyenes feladat a webhelycsoportban engedélyezve van, óránként egyszer
- Több mint 5 feladatok szerepelnek a feladat gyűjtemény
- A projekt-gyűjteményben egy feladat van [HTTP kimenő engedélyezési objektum](scheduler-outbound-authentication.md) használó HTTP vagy HTTPS művelet

## <a name="billing-and-azure-plans"></a>Számlázás és Azure csomagok

Az előfizetések nem terheli az ingyenes feladat gyűjtemények. Ha 100-nál több szabványos feladat gyűjtemények (10 szabványos számlázási egységgel), majd célszerű jobb üzletet, hogy az összes feladat gyűjtemények a premium csomagban.

Ha van egy normál feladat webhelycsoport és egy prémium feladat webhelycsoport, számlázott egy szabványos számlázási egység _és_ prémium számlázási egységnyi áll. Az ütemező szolgáltatás számlák aktív feladat gyűjtemények normál vagy a prémium; beállított száma alapján Ennek a következő két szakaszokban további magyarázatát.

## <a name="standard-billable-units"></a>Szabványos számlázható egységek

Egy szabványos számlázható egység legfeljebb 10 szabványos feladat gyűjtemények is tartalmazhat. Egy szabványos feladat webhelycsoport egy feladat webhelycsoport legfeljebb 50 feladatok is rendelkezik, mivel egy szabványos számlázási egység lehetővé teszi, hogy legfeljebb 500 feladatok – szinte 22 millió feladat végrehajtások havonta legfeljebb vannak-előfizetést.

Ha 1 és 10 szabványos feladat gyűjtemények között, akkor fognak számlát kapni 1 szabványos számlázási egységben. Ha 11 és 20 szabványos feladat gyűjtemények között, akkor fognak számlát kapni a 2 számlázási alapegység. Ha 21 és 30 szabványos feladat webhelycsoportok között, 3 szabványos számlázási egységek fognak számlát kapni, és így tovább.

## <a name="p10-premium-billable-units"></a>P10 Prémium számlázható erőforrás-mennyiség

P10 prémium számlázható időegység legfeljebb 10 000 P10 prémium feladat gyűjtemények is tartalmazhat. Mivel P10 prémium feladat gyűjtemény feladat webhelycsoport használati legfeljebb 50 feladatok is rendelkezik, egy prémium számlázási egységgel lehetővé teszi, hogy legfeljebb 500 000 feladatok – szinte 22 milliárd feladat végrehajtások havonta legfeljebb vannak-előfizetést.

Ha 1 és 10 000 prémium feladat gyűjtemények között, akkor fognak számlát kapni az 1 P10 prémium számlázási egység. Ha 10,001 és 20 000 prémium feladat webhelycsoportok között, a 2 P10 prémium számlázási egység fognak számlát kapni, és így tovább.

Így P10 prémium feladat gyűjtemények ugyanúgy működnek, mint a szokásos feladat gyűjtemények, de adja meg az ár töréspont abban az esetben, ha az alkalmazás feladat gyűjtemények sok van szükség.

## <a name="p20-premium-billable-units"></a>P20 Prémium számlázható erőforrás-mennyiség

P20 prémium számlázható időegység legfeljebb 5 000 P20 prémium feladat gyűjtemények is tartalmazhat. Feladat gyűjtemény P20 prémium feladat webhelycsoport használati legfeljebb 1000 feladatok is rendelkezik, mivel prémium számlázási egységnyi lehetővé teszi, hogy legfeljebb 5,000,000 feladatok – szinte 220 milliárd feladat végrehajtások havonta legfeljebb vannak-előfizetést.

P20 prémium feladat gyűjtemények mint P10 prémium feladat gyűjtemények ugyanolyan lehetőségeket kínál, de is támogatja a feladat webhelycsoport használati egy nagyobb szám feladatok és a nagyobb száma általános feladatok P10 prémium, amely lehetővé teszi, hogy több méretezhetőség mint.

## <a name="billing-and-active-status"></a>Számlázási és az aktív állapota

Feladat gyűjtemények mindig aktívak, kivéve ha az egész előfizetés számlázási problémák miatt egyes ideiglenes letiltott állapotba leállt. Győződjön meg arról, hogy egy feladat webhelycsoport nem számlát kapni csak úgy van szeretné vagy állítsa be a _szabad_ csomagot vagy törlése a projekt gyűjteményben.

Bár letilthatja az összes feladatot egy műveletben egy feladat gyűjteményben, nem változtatja meg a projekt gyűjtemény számlázási állapotának – a feladat gyűjtemény fog _továbbra is_ számlát kapni. Hasonlóképpen üres projekt gyűjtemények aktívnak tekinti, és fognak számlát kapni.

## <a name="pricing"></a>Árak

Részletek árak, olvassa el [A Feladatütemező árak](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Lásd még:


 [Mi az a Feladatütemező?](scheduler-intro.md)

 [Azure ütemező fogalmak, kifejezések és entitás hierarchia](scheduler-concepts-terms.md)

 [Az Azure-portálon a Feladatütemező használatának első lépései](scheduler-get-started-portal.md)

 [Azure ütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Azure ütemező PowerShell-parancsmagok hivatkozás](scheduler-powershell-reference.md)

 [Azure ütemező magas rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure ütemező korlátozások, alapértelmezett és hibakódok esetén](scheduler-limits-defaults-errors.md)

 [Azure ütemező kimenő hitelesítést](scheduler-outbound-authentication.md)
