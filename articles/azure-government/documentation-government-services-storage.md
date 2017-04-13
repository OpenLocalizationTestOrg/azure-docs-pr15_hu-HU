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
    ms.date="10/13/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-storage"></a>Azure kormányzati tárhely

##  <a name="azure-storage"></a>Azure tárhely

Ez a szolgáltatás és használatához a részletekért olvassa el [Azure tároló nyilvános dokumentációt](https://azure.microsoft.com/documentation/services/storage/).

### <a name="variations"></a>Változatok

Az URL-címeit az Azure kormányzati a tárterület-fiókok eltérőek:

Szolgáltatás típusa|Azure nyilvános|Azure kormányzati
---|---|---
BLOB-tárolóhoz|*. blob.core.windows.net|*. blob.core.usgovcloudapi.net
A várakozási tárhely|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Táblatároló|*. table.core.windows.net| *. table.core.usgovcloudapi.net

>[AZURE.NOTE] Parancsfájlok és kódot kell a megfelelő végpontok számlája.  Lásd: [Azure tároló Csatlakozási_karakterlánc konfigurálása](../storage-configure-connection-string.md#creating-a-connection-string-to-the-explicit-storage-endpoint). 

API-khoz bővebben lásd: a <a href="https://msdn.microsoft.com/en-us/library/azure/mt616540.aspx">Felhőalapú tárolási fiók konstruktor</a>.

Az endpoint használata a következő túlterhelések képz_jel core.usgovcloudapi.net 

### <a name="considerations"></a>Megfontolandó szempontok

Az alábbi adatokat az Azure kormányzati oszlopazonosító azonosítja Azure tárhelyként használható:

| Megengedett szabályozott/ellenőrzött adatok | Szabályozni/ellenőrzött adatok nem engedélyezett. |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adatbeírás, tárolja, és Azure-tárolóhoz belül feldolgozott termék tartalmazhat ellenőrzött exportálása. Statikus hitelesítő, például jelszavak és Azure platform-összetevők access intelligens kártya PIN-kódok. Titkos kulcs van tanúsítványok Azure platform összetevők kezelésére használható. Egyéb biztonsági információk és titkos kulcsok, például a tanúsítványok, a titkosítási kulcs, a diaminta kulcsok és a az Azure-szolgáltatásokban tárolt tárolás kulcsok. | Azure tárterület-metaadatok nem engedélyezett ellenőrzött exportálás adatokat tartalmazza. A metaadatok létrehozása és fenntartása a tárterület-termék megadott konfigurációs adatait tartalmazza.  A következő mezőkben Regulated/ellenőrzött adatok nem kell megadni: az erőforrás csoportok, telepítési nevek, az erőforrásnevek, erőforrás címkék  

##  <a name="premium-storage"></a>Prémium tárhely

Ez a szolgáltatás és használatához a részletekért olvassa el [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md).

###  <a name="variations"></a>Változatok

Prémium tároló általában megtalálható a USGov Virginia. Ide tartoznak a DS-sorozat virtuális gépeken futó. 

### <a name="considerations"></a>Megfontolandó szempontok

A fent felsorolt tároló adatok ugyanezek prémium tároló fiókra alkalmazhatja. 

##  <a name="next-steps"></a>Következő lépések

A kiegészítő információk és frissítések fizessen elő a <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure kormányzati Blog.</a>
