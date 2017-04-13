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
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure kormányzati biztonsági és -Identitáskezelés

##  <a name="key-vault"></a>Fő tárolóból elemre
Ez a szolgáltatás és használatához a részletekért olvassa el a <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure kulcs tárolóra nyilvános dokumentációt.</a>

### <a name="data-considerations"></a>Adatok kapcsolatos szempontok
Az alábbi adatokat az Azure kulcs tárolóból elemre az Azure kormányzati oszlopazonosító sorolja fel:

| Megengedett szabályozott/ellenőrzött adatok | Szabályozni/ellenőrzött adatok nem engedélyezett. |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Az összes adat-Azure kulcs tárolóra kulccsal titkosított Regulated/szabályozott adatokat is tartalmazhatnak. | Azure kulcs tárolóra metaadatokat tartalmazó ellenőrzött exportálása nem engedélyezett. A metaadatok megadni, amikor létrehozása és fenntartása a kulcs tárolóból elemre az összes konfigurációs adatokat tartalmazza.  A következő mezőkben Regulated/ellenőrzött adatok nem kell megadni: erőforrások csoport nevét, a kulcs tárolóra nevét, az előfizetés neve |

Kulcs tárolóra érhető el általában az Azure kormányzati. Nyilvános, ahogy nem nincs kiterjesztése, így kulcs tárolóra csak akkor érhető el a PowerShell és CLI keresztül.

## <a name="next-steps"></a>Következő lépések

Feliratkozás a kiegészítő információk és frissítések, a <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure kormányzati Blog.</a>
