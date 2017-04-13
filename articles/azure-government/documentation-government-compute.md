<properties
    pageTitle="Azure kormányzati dokumentáció |} Microsoft Azure"
    description="Ez ez a témakör a szolgáltatást, és útmutatást összehasonlítás Azure kormányzati alkalmazások fejlesztéséhez"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="09/29/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-compute"></a>Azure kormányzati számítási

##  <a name="virtual-machines"></a>Virtuális gépeken futó

Ez a szolgáltatás és használatához a részletekért olvassa el [Azure virtuális gépeken futó méretét](../virtual-machines/virtual-machines-windows-sizes.md).

### <a name="variations"></a>Változatok

A következő virtuális termékváltozatok általában elérhető (kiadás) az Azure kormányzati:

VIRTUÁLIS TERMÉKVÁLTOZAT|Amerikai Egyesült Államok Gov VA|Amerikai Egyesült Államok Gov IA|Jegyzetek
---|---|---|---
A|KIADÁS|KIADÁS|Nincs lehetőség
D.v.1|KIADÁS|-|Nincs lehetőség
DSv1|KIADÁS|-|Nincs lehetőség
Dv2|KIADÁS|KIADÁS|15 hamarosan
F|KIADÁS|KIADÁS|Nincs lehetőség
G|Tervezett|-|Nincs lehetőség

###  <a name="data-considerations"></a>Adatok kapcsolatos szempontok

Az alábbi információk azonosítja az Azure kormányzati oszlopazonosító az Azure virtuális gépeken futó:

| Megengedett szabályozott/ellenőrzött adatok | Szabályozni/ellenőrzött adatok nem engedélyezett. |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adatok megadott tárolt és feldolgozása belül egy virtuális ellenőrzött exportálása is tartalmazhat. A bináris belül Azure virtuális gépeken futó futtatása. Statikus hitelesítő, például jelszavak és Azure platform-összetevők access intelligens kártya PIN-kódok. Titkos kulcs van tanúsítványok Azure platform összetevők kezelésére használható. SQL-kapcsolati karakterláncot.  Egyéb biztonsági információk és titkos kulcsok, például a tanúsítványok, a titkosítási kulcs, a fő kulcsok és a az Azure-szolgáltatásokban tárolt tároló kulcsok.  | Tartalmazzák, adatok exportálása a felügyelt metaadatok nem engedélyezett. A metaadatok létrehozása és fenntartása az Azure virtuális gép megadott konfigurációs adatait tartalmazza.  A következő mezőkben Regulated/ellenőrzött adatok nem kell megadni: bérlői szerepkör nevek, az erőforrás csoportok, telepítési nevek, erőforrásnevek, erőforrás-címkék  

## <a name="next-steps"></a>Következő lépések

Feliratkozás a kiegészítő információk és frissítések, a <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure kormányzati blogban.</a>
