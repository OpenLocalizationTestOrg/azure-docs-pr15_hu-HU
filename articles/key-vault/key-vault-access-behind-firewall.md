<properties
    pageTitle="Kulcs tárolóra elérése tűzfal mögött |} Microsoft Azure"
    description="Megtudhatja, hogy miként alkalmazásból kulcs tárolóból elemre egy tűzfal mögött"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Kulcs tárolóra elérése tűzfal mögött
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>Probléma: a fő tárolóra ügyfélalkalmazás kell lennie a tűzfal mögött, milyen portokat és hosts/IP-címek kell nyithatok meg hozzáférést biztosít a fő tárolóból elemre kattintva?

A fő tárolóból elemre a fő tárolóra ügyfélalkalmazás kell fér hozzá az egyes funkciók több előfizetéskor eléréséhez.

- Hitelesítés (keresztül Azure Active Directory)
- Kulcs tárolóból elemre (amely tartalmazza, létrehozása és olvasási/frissítése és törlése, és az access-házirendek beállítása) Azure erőforrás-kezelővel kezelése
- Eléréséhez, és a fő tárolóból elemre, magát tárolt objektumok (kulcsok és titkos kulcsok) kezeléséhez, Ugrás a fő tárolóra adott végpont (pl. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)) keresztül.  

Attól függően, hogy a rendszer konfigurációjától és környezetben létezik néhány változatok.   

## <a name="ports"></a>Portok

Fő tárolóból elemre az összes a 3 függvények (hitelesítés, kezelése és adatok sík hozzáférés) minden forgalom áttekintést HTTPS: Port 443-as. Azonban CRL, lesznek alkalmanként http (80-as port)-forgalmat. OCSP támogató ügyfelek CRL kerülni a Webhelyfiókok elérése, de időnként előfordulhat, hogy elérje a [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl).  

## <a name="authentication"></a>Hitelesítés

Kulcs tárolóra ügyfélalkalmazás kell Azure Active Directory authentication végpontokat eléréséhez. A használt végpont függ, hogy a AAD bérlő konfigurálása és a fő – a felhasználók egyszerű, szolgáltatás egyszerű és milyen típusú fiókot, például Microsoft-Account vagy szervezeti azonosítójával.  

| Egyszerű típusa | Végpont: port |
|----------------|---------------|
| Microsoft-Account felhasználói<br> (pl.user@hotmail.com) | **Globális:**<br> login.microsoftonline.com:443<br><br> **Azure Kína:**<br> login.chinacloudapi.CN:443<br><br>**Azure USA-beli kormányzati:**<br> bejelentkezés-us.microsoftonline.com:443<br><br>**Azure Németország:**<br> login.microsoftonline.de:443<br><br> és <br>login.live.com:443   |
| Szervezeti azonosító használata AAD (például felhasználói/szolgáltatás egyszerűuser@contoso.com) | **Globális:**<br> login.microsoftonline.com:443<br><br> **Azure Kína:**<br> login.chinacloudapi.CN:443<br><br>**Azure USA-beli kormányzati:**<br> bejelentkezés-us.microsoftonline.com:443<br><br>**Azure Németország:**<br> login.microsoftonline.de:443 |
| Szervezeti azonosító + ADFS vagy más szövetséges végpontot használ (például felhasználói/szolgáltatás egyszerűuser@contoso.com) | A fenti végpontok szervezeti azonosító plusz az ADFS- vagy más szövetséges végpontok |

Forgatókönyv más lehetséges összetett. Kérjük, olvassa el az [Azure Active Directory Authentication Flow](/documentation/articles/active-directory-authentication-scenarios/), [Alkalmazások integrálása az Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) és [Az Active Directory Authentication protokollok](https://msdn.microsoft.com/library/azure/dn151124.aspx) további információt.  

## <a name="key-vault-management"></a>Fő tárolóra kezelése

Tárolóra kulcskezelő (CRUD és hozzáférési házirend beállítása) a fő tárolóra ügyfélalkalmazás meg kell adni Azure erőforrás-kezelő végpont eléréséhez.  

| Művelet típusa | Végpont: port |
|----------------|---------------|
| Kulcs tárolóra vezérlő sík műveletek<br> VIA Azure erőforrás-kezelő | **Globális:**<br> Management.Azure.com:443<br><br> **Azure Kína:**<br> Management.chinacloudapi.CN:443<br><br> **Azure USA-beli kormányzati:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Németország:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory Graph API | **Globális:**<br> Graph.Windows.NET:443<br><br> **Azure Kína:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure USA-beli kormányzati:**<br> Graph.Windows.NET:443<br><br> **Azure Németország:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Fő tárolóra műveletek

Minden kulcs tárolóból elemre (kulcsok és titkos kulcsok) objektum kezelése és titkosítási műveletek kulcs tárolóra ügyfél meg kell adni a fő tárolóra végpont eléréséhez. Attól függően, hogy a kulcs tárolóra helyét a végpontok DNS-utótag különbözik. A formátum van a kulcs tárolóra végpont: < tárolóra neve >. < régió-specifikus-dns-utótag > az alábbi táblázatban ismertetett módon.  

| Művelet típusa | Végpont: port |
|----------------|---------------|
| Kulcs tárolóból elemre műveletek, például a titkosítási kulcs, létrehozás és olvasási/frissítés/törlése kulcsok és titkos kulcsok, műveletek megadása/get címkék és más jellemzők (kulcsok és titkos kulcsok) objektumokon kulcs tárolóból elemre     | **Globális:**<br> &lt;tárolóra neve&gt;. vault.azure.net:443<br><br> **Azure Kína:**<br> &lt;tárolóra neve&gt;. vault.azure.cn:443<br><br> **Azure USA-beli kormányzati:**<br> &lt;tárolóra neve&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Németország:**<br> &lt;tárolóra neve&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-címtartományok

Kulcs tárolóra szolgáltatás, viszont a többi Azure erőforrások, mint a PaaS infrastruktúra használja, így nem egy adott IP-címtartományokat kulcs tárolóra szolgáltatási végpontok fog rendelkezni az adott időpontban lehetővé. De ha a tűzfal csak támogatja az IP-cím tartományok majd olvassa el a [Microsoft Azure adatközpont IP-címtartományai](https://www.microsoft.com/download/details.aspx?id=41653) dokumentumot.   Hitelesítés és -Identitáskezelés (Azure Active Directory), az alkalmazás tud csatlakozni a végpontok [hitelesítési és identitás címek](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)ismertetett kell lennie.

## <a name="next-steps"></a>Következő lépések

- Ha kérdései vannak kapcsolatban kulcs tárolóból elemre, látogasson el a [Azure kulcs tárolóra fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
