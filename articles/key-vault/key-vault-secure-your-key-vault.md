<properties
    pageTitle="A fő tárolóra biztonságos |} Microsoft Azure"
    description="Kezelése a hozzáférési engedélyeit tárolókban és kulcsok és titkos kulcsok kezelésére szolgáló főbb tárolóból elemre. Hitelesítés és az engedélyezés modell fő tárolóból elemre, és hogyan biztonságos a fő tárolóból elemre"
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
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>A fő tárolóra biztonságos

Azure kulcs tárolóból elemre egy felhőalapú szolgáltatásba, védő a titkosítási kulcs és a felhő alkalmazások (például a tanúsítványok, a kapcsolati karakterláncot, a jelszavak) titkos kulcsok. Mivel ezeket az adatokat bizalmas és biztonságos módon elérhetők a fő tárolókban, úgy, hogy csak hitelesített kívánt business kritikus, alkalmazások és a felhasználók hozzáférhet kulcs tárolóból elemre. Ez a cikk áttekintést nyújt a fő tárolóból elemre az access modell, hitelesítési és engedélyezési ismerteti, és leírja, hogy miként biztonságos módon elérhetők kulcs tárolóból elemre az a felhő alkalmazások, például.

## <a name="overview"></a>– Áttekintés

A fő tárolóból elemre a hozzáférést két külön felületeken keresztül szabályozható: kezelő sík és adatok síkban. Mindkét sík a megfelelő hitelesítési és engedélyezési előtt szükség egy hívó (a felhasználó vagy alkalmazás) hozzáférhet kulcs tárolóból elemre. Hitelesítés hoz létre a hívó azonosítója, miközben engedélyezési határozza meg, hogy milyen műveletek elvégzéséhez a hívó engedélyezett.

Hitelesítés kezelő sík és a adatok sík Azure Active Directory használja. Engedélyezés kezelő sík szerepköralapú hozzáférés-szerepalapú használja, miközben adatok sík kulcs tárolóból elemre hozzáférési házirendet használja.

Az alábbiakban a témák rövid áttekintését:

[Hitelesítéssel Azure Active Directory](#authentication-using-azure-active-direcrory) – Ez a szakasz ismerteti, hogyan egy hívó hitelesíti Azure Active Directory eléréséhez egy, a kezelő sík és adatok sík kulcs tárolóból elemre. 

[Kezelő sík és adatok sík](#management-plane-and-data-plane) - kezelő sík és adatok sík is két access sík használja a eléréséhez a fő tárolóból elemre. Minden access sík támogatja a meghatározott műveletekhez. Ez a témakör ismerteti az access végpontjait, műveletek támogatott, minden sík beállítás módszerének eléréséhez. 

[Adatkezelési sík hozzáférés-vezérlés](#management-plane-access-control) – áttekintjük kezelése sík műveletekkel a szerepköralapú hozzáférés-vezérlés használata hozzáférés engedélyezése ebben a szakaszban.

[Adatok sík hozzáférés-vezérlés](#data-plane-access-control) – Ez a szakasz ismerteti főbb tárolóból elemre az access-házirend használata adatok sík a hozzáférés vezérléséhez.

[Példa](#example) – Ez a példa ismerteti a fontosabb tárolóból elemre (biztonsági csoport, a fejlesztők és műveleti jelek és ellenőrök) fejlesztése, kezelheti és Azure-alkalmazás figyelése Speciális feladatok végrehajtására három különböző csapatokat engedélyezni hozzáférés-vezérlés beállítása.


## <a name="authentication-using-azure-active-directory"></a>Azure Active Directory hitelesítéssel

A fő tárolóból elemre az Azure előfizetéssel hoz létre, esetén automatikusan társítva az előfizetéshez Azure Active Directory bérlői. Az összes hívók (a felhasználók és az alkalmazások) regisztrálni kell az aktuális bérlői webhely eléréséhez a fő tárolóból elemre. Az alkalmazások és a felhasználó kell hitelesítést végezni az Azure Active Directory eléréséhez kulcs tárolóból elemre. Ez a mindkét management sík és az adatok sík access vonatkozik. Mindkét esetben az alkalmazás fő tárolóra kétféleképpen érheti el:

-  **felhasználói + alkalmazást az access** - általában az alkalmazások, melyhez hozzáféréssel bejelentkezett felhasználó nevében kulcs tárolóból elemre. Azure PowerShell, Azure portál példák az ilyen típusú hozzáféréssel rendelkezzenek. Kétféleképpen elérhetővé tétele a felhasználóknak: egyik módja, ha a elérhetővé tétele a felhasználóknak, hogy bármely alkalmazásból elérhetik a fő tárolóból elemre, és a más módon való hozzáférést a felhasználónak kulcs tárolóból elemre csak akkor, ha egy adott alkalmazás (néven összetett azonosító) használata. 
-  **alkalmazás csak az access** - alkalmazások démon szolgáltatások futtatása, a háttérben feladatok stb. Az alkalmazás azonosítóját hozzáférést kap a fő tárolóból elemre.

Az alkalmazást mindkét típusát az alkalmazás használata [támogatott hitelesítési módszerek](../active-directory/active-directory-authentication-scenarios.md) az Azure Active Directory hitelesíti és jogkivonat szerez. Használt hitelesítési módszer az alkalmazás típusától függ. Kattintson az alkalmazás a token használ, és REST API-összehívást küld a fő tárolóból elemre. Adatkezelési sík access esetén a kérések továbbítása Azure erőforrás-kezelő végpont keresztül. Adatok sík elérésekor az alkalmazások megbeszélések közvetlenül egy kulcsot a tárolóból elemre végpontot. További részleteket lásd a [teljes hitelesítési folyamat](../active-directory/active-directory-protocols-oauth-code.md). 

Az erőforrás nevére, amelynek a alkalmazás kéri a jogkivonat eltér attól függően, hogy az alkalmazás kezelő sík vagy adatok sík elérése. Így az erőforrás neve vagy management sík vagy adatok sík végpont táblázatban későbbi szakaszában, attól függően, hogy az Azure környezetben.

Egy egyetlen mechanizmusa kezelési és adat sík hitelesítéshez kellene a saját előnyöket nyújtja:

- Szervezetek központilag is az összes kulcs tárolókban szervezetük való hozzáférés szabályozása
- Ha egy felhasználó elhagyja, azok azonnali elveszíti hozzáférését a szervezet összes kulcs tárolókban
- Szervezetek is testre szabhatja a beállításokat az Azure Active Directory (például engedélyezése többtényezős hitelesítés a nagyobb biztonság) útján hitelesítéshez

## <a name="management-plane-and-data-plane"></a>Kezelő sík és adatok sík

Azure kulcs tárolóból elemre egy erőforrás-kezelő Azure telepítési modell kapható Azure szolgáltatás. Amikor létrehoz egy fő tárolóból elemre, akkor tároló virtuális belül, amelyeket más objektumok, például kulcsok, a titkos kulcsok és a tanúsítványok hozhat létre. Kattintson a kulcs tárolóból elemre az adatkezelési sík és a adatok sík meghatározott műveleteket hajt végre érheti el. Sík kezelőfelület a fő tárolóból elemre, magát, például létrehozása, törlése, fő tárolóra attribútumok frissítése és az adatok sík access házirendek beállítása kezelésére szolgál. Adatok sík felület hozzáadása, törlése, módosítása és a billentyűk, titkos kulcsok és a fő tárolóra tárolt tanúsítványok szolgál.

Az adatkezelési sík és az adatok sík felületek (lásd a táblázatot) különböző végpontokon érhetők el. A második oszlopban a táblázat ismerteti a DNS-nevek ezeket a végpontokat különböző Azure környezetben. A harmadik oszlop minden access sík elvégezhető műveleteket ismerteti. Minden access sík is rendelkezik saját access mechanizmusa: az adatkezelési sík hozzáférés-vezérlés értéke Azure Resource Manager Role-Based Access vezérlő (RBAC) használ, az adatok sík hozzáférés-vezérlés van beállítva, fő tárolóból elemre az access-házirend használata közben.

| Az Access sík | Access-végpontok | Műveletek | Az Access mechanizmusa|
|--------------|------------------|--------------------|--------|
| Kezelő sík|**Globális:**<br> Management.Azure.com:443<br><br> **Azure Kína:**<br> Management.chinacloudapi.CN:443<br><br> **Azure USA-beli kormányzati:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Németország:**<br> Management.microsoftazure.de:443 | Fő tárolóra létrehozása és olvasási/frissítése és törlése <br> Fő tárolóból elemre az access-házirendek beállítása<br>Fő tárolóra címkék beállítása | Azure erőforrás Manager szerepköralapú hozzáférés-szerepalapú |
| Adatok sík | **Globális:**<br> &lt;tárolóra neve&gt;. vault.azure.net:443<br><br> **Azure Kína:**<br> &lt;tárolóra neve&gt;. vault.azure.cn:443<br><br> **Azure USA-beli kormányzati:**<br> &lt;tárolóra neve&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Németország:**<br> &lt;tárolóra neve&gt;. vault.microsoftazure.de:443 | A billentyűparancsok: Visszafejtése, titkosítása, UnwrapKey, WrapKey, ellenőrizze, jelentkezzen be, első, lista, frissítés, létrehozása, importálása, törlése, biztonsági mentése, visszaállítása<br><br> A titkos kulcsok: juthat, listájában, beállítása és törlése | Fő tárolóra hozzáférési házirend|

Az adatkezelési sík és az adatok sík vezérlőelemek elérése egymástól függetlenül működik. Szeretne egy alkalmazás hozzáférést billentyűk használatához az egy fő tárolóból elemre, ha például csak akkor van szükség kulcs tárolóból elemre az access házirendekkel adatok sík hozzáférési engedélyek megadása, és nincs management sík hozzáférés ehhez az alkalmazáshoz szükséges. És fordítva, ha azt szeretné, hogy egy felhasználó, olvassa el a tárolóból elemre tulajdonságok és a címkék, de nincs hozzáférése bármely kulcsok, titkos kulcsok vagy tanúsítványok láthatja, hogy engedélyezze kívánt felhasználónak, "olvassa el" elérésének RBAC és adatok sík nincs hozzáférése szükség.

## <a name="management-plane-access-control"></a>Adatkezelési sík hozzáférés-vezérlés

A kezelő sík, ahol a műveleteket, amelyek befolyásolják a fő tárolóra magát. Ha például létrehozása vagy törlése a fő tárolóból elemre. Az előfizetés elérheti tárolókban listáját. Beolvasni a fő tárolóra tulajdonságai (például Termékváltozat, címkéket), és a fő tárolóból elemre hozzáférési szabályok, amelyek a felhasználók és a billentyűk és a fő tárolóból elemre a titkos kulcsok elérésére jogosult alkalmazások megadása. Adatkezelési sík hozzáférés-vezérlés RBAC használja. Lásd: a teljes listáját az előző szakasz táblázat kezelő sík keresztül elvégezhető műveletek kulcs tárolóból elemre. 

### <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-szerepalapú
Minden egyes Azure előfizetés az Azure Active Directory tartalmaz. Felhasználók, csoportok és alkalmazások e könyvtárból is hozzáférési jogosultsággal erőforrások Azure-előfizetésben, amelyek az Azure erőforrás-kezelő telepítési modell. Az ilyen típusú hozzáférés-vezérlés szerepköralapú hozzáférés szerepalapú nevezik. A hozzáférés kezelése, az [Azure portál](https://portal.azure.com/), az [Azure CLI eszközök](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)vagy az [Azure erőforrás-kezelő REST API -khoz](https://msdn.microsoft.com/library/azure/dn906885.aspx)is használhatja.

Az erőforrás-kezelő Azure modell használatával hozhat létre a fő tárolóból elemre az erőforrás csoport és a vezérlő access és a fő tárolóból elemre azon kezelése Azure Active Directory használatával. Például biztosíthat a felhasználóknak és a csoport azt jelenti, hogy az adott erőforrás csoport fő tárolókban kezelése.

A felhasználók, csoportok és egy adott hatóköre alkalmazásokat az access által megfelelő RBAC szerepkörök kiosztása adhat meg. Például való hozzáférést a felhasználónak, ezzel kulcs tárolókban kezelése, volna előre definiált szerepkör hozzárendelése "kulcs tárolóra közreműködői" ennek a felhasználónak az egy adott hatóköre. A hatókör ebben az esetben lenne előfizetés, erőforráscsoport vagy csak egy bizonyos kulcs tárolóból elemre. Előfizetés szintjén szerepkört erőforrás-csoportok és a források belül az adott előfizetés vonatkozik. Erőforrás-csoport szintjén szerepkört összes erőforrásában található erőforrás csoport vonatkozik. Egy adott erőforrás egy szerepkört csak az adott erőforrás vonatkozik. Számos előre definiált szerepkör létezik (lásd: [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md)), és ha az előre definiált szerepkörök nem felel meg az igényeinek is meghatározhat a saját szerepkörök.

>[AZURE.IMPORTANT] Figyelje meg, hogy, ha egy felhasználó rendelkezik közreműködői engedélyekkel (RBAC) kulcs tárolóra kezelő sík, hivatkozásra lehet adni közte eléréséhez adatok sík, által beállítása kulcs tárolóból elemre hozzáférési házirendet, amely adatok sík elérését szabályozza. Ajánlott tehát, szorosan csak jogosult személyek érhetik el és kezelheti a fő tárolókban, a billentyűk, a titkos kulcsok és a tanúsítványok biztosítása érdekében a fő tárolókban "Közreműködői" hozzáféréssel rendelkező személyek vezérlő.

## <a name="data-plane-access-control"></a>Adatok sík hozzáférés-vezérlés

A fő tárolóra adatok sík befolyásoló főbb tárolóból elemre, például a billentyűk, a titkos kulcsok és a tanúsítványok objektumainak műveletek áll.  Ide tartoznak a műveletek, például: létrehozása kulcsot importálása, frissítése, lista, biztonsági mentése és visszaállítása billentyűk résznél cryptographic műveletek, például a bejelentkezési, ellenőrizze, titkosítása, visszafejtése, tördelése, és tördelésének és címkék és más attribútumait kulcsok beállítása. Hasonlóképpen a titkos kulcsok tartalmazza, az juthat, beállítása, lista, és törölje.

Adatok sík hozzáférés megadásával access házirendek egy fő tárolóból elemre. A felhasználó, csoport vagy az alkalmazás engedélyekkel kell rendelkezni közreműködői (RBAC)-kezelő sík a fő tárolóra, láthatja, hogy a kulcsfontosságú tárolóból elemre az access-házirendek beállítása. Egy felhasználó, csoport vagy alkalmazás is hozzáférési jogosultsággal hajtanak végre adott billentyűk vagy titkos kulcsok a egy fő tárolóból elemre. fő tárolóból elemre legfeljebb 16 hozzáférési házirend bejegyzéseinek támogatása a fő tárolóból elemre. Azure Active Directory-biztonsági csoport létrehozása és felhasználók hozzáadása, hogy a csoport adatok sík hozzáférést több felhasználókat a fő tárolóból elemre.

### <a name="key-vault-access-policies"></a>fő tárolóból elemre az Access-házirendek

fő tárolóból elemre az access-házirendek hozzáférési engedélyek megadása a billentyűk, a titkos kulcsok és a tanúsítványok külön-külön. Ha például úgy adhat egy felhasználó férhet hozzá csak kulcsok, de nincs engedélye titkos kulcsok. Jó helyen jár billentyűk vagy titkos kulcsok vagy tanúsítványok hozzáférési jogosultsággal a tárolóból elemre szinten vannak. Más szóval fő tárolóra hozzáférési házirend nem támogatja a szintű objektumengedélyek. [Azure portál](https://portal.azure.com/), az [Azure CLI eszközök](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)vagy a [fő tárolóból elemre az adatkezelési REST API -khoz](https://msdn.microsoft.com/library/azure/mt620024.aspx) segítségével a fő tárolóból elemre az access-házirendek beállítása.

>[AZURE.IMPORTANT] Figyelje meg, hogy a fő tárolóból elemre az access-házirendek szintre tárolóból elemre vonatkoznak. Például amikor a felhasználók létrehozása és törlése a billentyűk engedélyt, kikkel végezheti el ezeket a műveleteket, az összes kulcsokon adott fő tárolóból elemre.

## <a name="example"></a>Példa

Tegyük fel, ha olyan alkalmazás, amely a tanúsítvány használja az SSL, adatok tárolására szolgáló Azure tárhely, valamint egy RSA 2048 bites kulcs bejelentkezési műveletekhez fejlesztéséhez. Tegyük fel, hogy az alkalmazás fut a virtuális (vagy egy virtuális skála megadása). A fő tárolóra használva az alkalmazás titkos kulcsok, és a betöltő tanúsítványt az Azure Active Directory címtárral hitelesítést végezni az alkalmazás által használt kulcsfontosságú tárolóból elemre használatával.

Igen az alábbiakban összefoglaljuk az összes olyan kulcsok és titkos kulcsok tárolni a fő tárolóból elemre.

- **SSL-tanúsítvány** – az SSL
- **Tárterület kulcs** – tárterület-fiók elérésének parancssor
- **RSA 2048 bites billentyű** - használt bejelentkezési műveletek
- **Betöltő tanúsítvány** – hitelesíti Azure Active Directory érheti el a tárhely kulcs lehívása és a RSA billentyűvel aláíráshoz kulcs tárolóból elemre.

Most vegyük felel meg a személyek, akik kezelésére, üzembe helyezése és naplózás az alkalmazás. Ebben a példában a három szerepkörök használjuk.

- **Biztonsági csoport** – ezek általában a CSO (Chief biztonsági őr) office az informatikai szakemberek vagy egyenértékű, a megfelelő tegyen titkos kulcsok, például az SSL-tanúsítványok, RSA billentyűk aláíráshoz, a kapcsolat karakterláncok-adatbázisok, tároló fiók billentyűk használhatók a felelős.
- **A fejlesztők/operátorok** – ezek az emberek, akik kidolgozása ennek az alkalmazásnak és telepítheti Azure-ban. Általában ezek nem vesznek részt a biztonsági csoport, és így azok nem férhet hozzá minden olyan bizalmas adatokat, például az SSL-tanúsítványok, RSA kulcsok, de az alkalmazás terjesztése azokat a azok hozzáféréssel kell rendelkeznie.
- **Ellenőrök** – Ez általában a személyek, a fejlesztők és az általános informatikai szakemberek elszigetelt más-más szabálykészletet. Felelősségi, tekintse át a megfelelő használatának és a tanúsítványok, billentyűk, stb karbantartás, valamint adatok biztonsági szabványoknak. 

Van egy további szerepkör a alkalmazás, de vonatkozó alábbi feltüntetendő hatókörén kívüli, és hogy lenne, az előfizetést (vagy erőforráscsoport) rendszergazda. Előfizetés rendszergazda beállítja a biztonsági csoport kezdeti hozzáférési engedélyeit. Itt feltételezzük, hogy az előfizetés ügyvezető van a biztonsági csoport hozzáférési jogosultsággal mely az összes szükséges forrásokat a alkalmazás lakóhelye az erőforrás csoportban.

Most lássuk, milyen műveleteket minden szerepkör hajt végre, ez az alkalmazás környezetében.

- **Biztonsági csoport**
    - Fő tárolókban létrehozása
    - Kulcs bekapcsolja a tárolóból elemre a naplózás
    - Adja hozzá a billentyűk és titkos kulcsok
    - Billentyűparancsok vészhelyreállítás biztonsági másolat is készül
    - Ha engedélyeket szeretne adni a felhasználók és az alkalmazások meghatározott műveleteket hajt végre a fő tárolóból elemre az access házirend beállítása
    - Rendszeres időközönként Filmtekercs kulcsok és titkos kulcsok
- **A fejlesztők/operátorok**
    - Bootstrap mutató hivatkozásokat és SSL tanúsítványok (thumbprints), tároló kulcs (titkos URI) és a biztonsági csoport (kulcs URI) billentyű aláíráshoz
    - Kidolgozása és hozzáférő kulcsok és titkos kulcsok programozás útján alkalmazások telepítése
- **Ellenőrök**
    - Kattintva erősítse meg a megfelelő billentyűt vagy titkos használatát és az adatok biztonsági szabványoknak használatát naplók áttekintése

Most lássuk, milyen főbb tárolóból elemre a hozzáférési engedélyek szükségesek minden szerepkör (és az alkalmazás) a kijelölt feladataik elvégzéséhez. 

| Felhasználói szerepkör    | Kezelési sík engedélyek | Adatok sík engedélyek |
|--------------|------------------------------|------------------------|
|Biztonsági csoport|fő tárolóra közös munka|Kulcsok: készítsen biztonsági másolatot, létrehozása, törlése, juthat, importálása, lista, visszaállítása <br> Titkos kulcsok: minden|
|A fejlesztők/operátor| fő tárolóra üzembe jogosultsági, hogy a azok üzembe VMs titkos kulcsok beolvasni a a fő tárolóból elemre | Nincs lehetőség |
|Ellenőrök| Nincs lehetőség | Billentyűparancsok: listája<br>Titkos kulcsok: listában|
|Alkalmazás| Nincs lehetőség | Billentyűparancsok: bejelentkezési<br>Titkos kulcsok: get |

>[AZURE.NOTE] Ellenőrök szükséges lista engedéllyel kulcsok és titkos kulcsok, így vizsgálatára használható attribútumok kulcsok és titkos kulcsok, hogy a rendszer kihagyta a naplókat, például címkéket, aktiválási és a lejárati dátum.

Amellett engedéllyel kulcs tárolóból elemre az összes három szerepkörök is más erőforrások hozzáférésre van szükségük. Ha például engedélyezni szeretné üzembe VMs (vagy Web Apps alkalmazások stb.) A fejlesztők és operátorokat is e erőforrástípus "Közreműködői" hozzáférésre van szükségük. Ellenőrök a tárterület-fiók a fő tárolóra naplók tároló olvasható hozzáféréssel kell rendelkeznie.

Mivel ez a cikk a fókusz van hozzáférés biztosítása érdekében a fő tárolóból elemre kattintva, akkor csak bemutatják a fontos részeket, a témával kapcsolatos, és ugorja át a részleteinek üzembe helyezése a tanúsítványok, megnyitása kulcsok és titkos kulcsok programozás útján stb. Adott részletek máshol már tartoznak. Fő tárolóból elemre kattintva VMs tárolt tanúsítványok foglalt [blog közzé](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/), és [példakódot](https://www.microsoft.com/download/details.aspx?id=45343) érhető el, amely bemutatja, hogyan érheti el a fő tárolóra Azure Active Directory hitelesíteni betöltő tanúsítvány használatával.

A hozzáférési engedélyek többsége Azure portálon lehet adni, de részletes engedélyeket szeretne adni szüksége lehet Azure PowerShell (vagy az Azure CLI) a kívánt eredmény eléréséhez. 

Tegyük fel, az alábbi PowerShell kódrészletek:

- Az Azure Active Directory-rendszergazda hozott létre a három szerepkörök, azaz Contoso biztonsági csoport, Contoso alkalmazás Devops, Contoso-alkalmazás ellenőrök megjelenítő biztonsági csoportokat. A rendszergazda is magában hozzáadta a felhasználókat a csoportokba tartoznak.

- **ContosoAppRG** erőforráscsoport, ahol az források találhatók. **contosologstorage** , a naplókat tárolási helyére. 

- **ContosoKeyVault** és tárolási kulcs tárolóra naplók **contosologstorage** használt fiók kulcs tárolóból elemre Azure ugyanazon a helyen kell lennie.


Először az előfizetés rendszergazdája hozzárendeli "főbb tárolóból elemre a közös munka" és a felhasználói hozzáférés rendszergazdája szerepkörök a biztonsági csoport. Ezzel a biztonsági csoport más erőforrások: a hozzáférés kezelése, valamint az erőforráscsoport ContosoAppRG kulcs tárolókban kezelése.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

A következő parancsfájl azt szemlélteti, hogyan hozhat létre a biztonsági csoport fő tárolóból elemre, a telepítő naplózáshoz és a hozzáférési engedélyek más szerepkörök és az alkalmazás beállítása. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Az egyéni szerepkör definiálva, csak akkor rendelhető hozzá az előfizetés hol a ContosoAppRG erőforráscsoport jön létre. Az azonos egyéni szerepkörök más projektekben más előfizetések a használandó, ha annak hatókör lehet hozzáadott előfizetést.

Az egyéni szerepkör-hozzárendelés a "üzembe/művelet" engedélyt a fejlesztők/operátorok az erőforrás-csoportnak megfelelően változik. Csak az erőforráscsoport "ContosoAppRG" létrehozott VMs így jelenik meg a titkos kulcsok (SSL-tanúsítvány és betöltő tanúsítvány). Bármely VMs fejlesztők/ops csapat egyik tagjával más erőforráscsoport hoz létre, nem tudnak titkos adatok beszerzése, még akkor is, ha az azok tudomása volt, a titkos URL-címe.

Ez a Példa egyszerű példa szemlélteti. Lehet, hogy a valós életben esetek összetettebb, és akkor szükség szerint módosítsa a fő tárolóra szükségletek engedélyekkel. Például a példáinkban feltételezzük a biztonsági csoport nyújt a kulcsot, és a titkos hivatkozások (URL-címe és thumbprints), hogy a fejlesztők/operátorok csapat van szüksége a saját alkalmazások hivatkozni szeretne. Ezért ezeket nem kell hozzáférést a fejlesztők/operátorok bármely adatok sík. Emellett látható, hogy ez a példa koncentrál biztonságossá tétele a fő tárolóból elemre. Hasonló megfontolandó biztonságossá tétele [a VMs](https://azure.microsoft.com/services/virtual-machines/security/), [tároló fiókok](../storage/storage-security-guide.md) és más erőforrások: Azure is.

>[AZURE.NOTE] Megjegyzés: Ebben a példában látható, hogyan kulcs tárolóból elemre az access zárolva lesz lefelé gyártási. A fejlesztők rendelkeznie kell a saját előfizetés vagy resourcegroup hol teljes körű engedéllyel rendelkeznek tárolókban, a VMs és a tárterület-fiók kezelése ahol azok kialakítása az alkalmazás.


## <a name="resources"></a>Erőforrások

-   [Azure Active Directory-szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)

    Ez a cikk ismerteti, hogy az Azure Active Directory szerepköralapú hozzáférés-vezérlés és működéséről.

-   [RBAC: Szerepkörök beépített](../active-directory/role-based-access-built-in-roles.md)

    Ez a cikk a beépített RBAC használható szerepkörök részletezi.

-   [Erőforrás-kezelő-telepítés és üzembe klasszikus ismertetése](../resource-manager-deployment-model.md)

    Ez a cikk ismerteti, hogy az erőforrás-kezelő telepítési és klasszikus telepítési modellek, és bemutatja az erőforrás-kezelő és a erőforrás csoport használatának előnyei

-    [Szerepköralapú hozzáférés-vezérlő, a Azure PowerShell kezelése](../active-directory/role-based-access-control-manage-access-powershell.md)

     Ez a cikk bemutatja, hogyan kezelheti az Azure PowerShell szerepköralapú hozzáférés-vezérlő

-   [A REST API-val irányító szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-manage-access-rest.md)

    Ez a cikk bemutatja, hogyan a REST API-lel való kezelése RBAC.

-   [Microsoft Azure Ignite a szerepköralapú hozzáférés-vezérlés](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Ez a 2015-ös MS Ignite konferencia el egy csatorna 9 a hivatkozást. Ebben a munkamenetben azok beszélni feltárása ajánlott eljárások megalkotása Azure előfizetések Azure Active Directory használatával való hozzáférés biztonságossá tétele és a hozzáférés kezelése és jelentéskészítési lehetőségek az Azure-ban.

-   [Webalkalmazás az OAuth 2.0-s és Azure Active Directory számára a hozzáférés engedélyezése](../active-directory/active-directory-protocols-oauth-code.md)

    Ez a cikk ismerteti a teljes OAuth 2.0-s folyamata az Azure Active Directory hitelesítő.

-   [fő tárolóból elemre az adatkezelési REST API-hoz](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    A dokumentum hivatkozását a REST API-k programozás útján, kezelheti a fő tárolóból elemre, beleértve a fő tárolóra hozzáférési házirend beállítása.

-   [fő tárolóra REST API-hoz](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Hivatkozás kulcs a REST API-hivatkozás dokumentáció tárolóból elemre.

-   [Kulcs hozzáférés-vezérlés](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Kulcs access control hivatkozás dokumentáció hivatkozás.

-   [Titkos hozzáférés-vezérlés](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Kulcs access control hivatkozás dokumentáció hivatkozás.

-   [Beállítása](https://msdn.microsoft.com/library/mt603625.aspx) és [eltávolítása](https://msdn.microsoft.com/library/mt619427.aspx) kulcs tárolóra hozzáférési házirend PowerShell használatával

    Hivatkozások dokumentációjában találhat kulcs tárolóból elemre hozzáférési házirend kezelése a PowerShell-parancsmagok hivatkozni szeretne.

## <a name="next-steps"></a>Következő lépések

Rendszergazda számára beolvasása lépések oktatóanyagot lásd: [Első lépések az Azure kulcs tárolóból elemre](key-vault-get-started.md).

További információt a naplózás kulcs tárolóból elemre a használatát lásd: a [naplózás Azure kulcs tárolóból elemre](key-vault-logging.md).

Kulcsok és titkos kulcsok használata Azure kulcs tárolóra kapcsolatos további tudnivalókért olvassa el a [kapcsolatos kulcsok és titkos kulcsok](https://msdn.microsoft.com/library/azure/dn903623.aspx)című témakört.

Ha kérdései vannak kapcsolatban kulcs tárolóból elemre, keresse fel a [Azure kulcs tárolóra fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
