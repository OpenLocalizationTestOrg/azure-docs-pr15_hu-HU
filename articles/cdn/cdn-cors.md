<properties
    pageTitle="Azure CDN használata CORS |} Microsoft Azure"
    description="Megtudhatja, hogy miként használható az Azure tartalom kézbesítési hálózati (CDN) való az idegen származási erőforrás megosztása (CORS)."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Azure CDN használata CORS     

## <a name="what-is-cors"></a>Mi az CORS?

(Cross Origin erőforrás-megosztás) CORS fiókfunkció HTTP, amely lehetővé teszi, hogy a webalkalmazás futtató egyik tartományt egy másik tartományban erőforrások eléréséhez. Csökkentheti a webhelyközi parancsfájl támadások lehetőségét, érdekében modern böngészők [azonos származási házirend](http://www.w3.org/Security/wiki/Same_Origin_Policy)neve biztonsági korlátozások hajtja végre.  Megakadályozza, hogy a hívó API-másik tartományt az weblapon.  Egy tartomány (az origin) fel szeretne hívni egy másik tartomány API-k engedélyezése biztonságos CORS segítségével.
 
## <a name="how-it-works"></a>Működése
1.  A böngésző küld a beállítások **Origin** HTTP fejlécet. Ezt a fejlécet értéke a tartományban, amely a szülő lap felszolgált. Amikor megkísérel https://www.contoso.com az weblapon elérni egy felhasználó adatait a fabrikam.com tartományt, az alábbi kérelem fejléc fabrikam.com küldi el: 
    
    `Origin: https://www.contoso.com`
 
2.  A kiszolgáló jelenhetnek meg a következő:
    - Egy **Access-vezérlőelem-engedélyezése – Origin** fejléc, a válasz, jelezve, hogy melyik origin webhelyek engedélyezettek. Példa:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Ha a kiszolgáló nem teszi lehetővé az idegen származási kérelem hibalap
    - Egy **Access-vezérlőelem-engedélyezése – Origin** élőfej egy helyettesítő karaktert, amely lehetővé teszi, hogy minden tartomány:
        
        `Access-Control-Allow-Origin: *`
 
Összetett HTTP-kérelmeket nincs első kész "ellenőrzés" kérelem határozza meg, hogy rendelkezik engedéllyel a teljes kérelem elküldése előtt.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Helyettesítő karakter vagy egyetlen origin felhasználási területei

A Azure CDN CORS működni fog automatikusan, nincs további beállítási az **Access-vezérlőelem-engedélyezése – Origin** élőfej helyettesítő karakter (*) vagy egy egyetlen origin beállítása esetén.  A CDN fog gyorsítótár-az első válasz, és ezután valaki fogja használni a azonos fejléc.
 
Ha már történtek kérelmek a CDN CORS a beállítása előtt a az origin, meg kell a végpontot tartalom újbóli betöltéséhez a tartalmat, az **Access-vezérlőelem-engedélyezése – Origin** fejlécekkel tartalmának törlése.
 
## <a name="multiple-origin-scenarios"></a>Több origin felhasználási területei

Ha a forrásokból CORS engedélyezett az adott listájának engedélyeznie kell a dolog, amit kicsit összetettebb vissza. A probléma fordul elő, ha az a CDN gyorsítótárát az első CORS érvényességé **Access-vezérlőelem-engedélyezése – Origin** fejlécét.  Ha egy másik CORS érvényességé későbbi kérést, az a CDN lesz, nem egyeznek gyorsítótárazott **Access-vezérlőelem-engedélyezése – Origin** élőfej felszolgált.  Többféleképpen elhárításához.
 
### <a name="azure-cdn-premium-from-verizon"></a>A Verizon Azure CDN-támogatás

Ahhoz, hogy ez a legjobb módszer **Azure CDN-támogatás az Verizon**, amely bizonyos speciális funkciókhoz közzététele használni. 
 
Kell [szabály](cdn-rules-engine.md) létrehozásához jelölje be az **Origin** élőfej a kérést.  Ha egy érvényes origin kerül, a szabály az **Access-vezérlőelem-engedélyezése – Origin** élőfej fog beállított az origin leírt a kérést.  Az **Origin** fejlécében meghatározott origin visszacsatolás nem engedélyezett, ha a a szabály kell elhagyja a **Access-vezérlőelem-engedélyezése – Origin** fejléc, amelyek hatására a böngészőben, ha el szeretné utasítani a kérést. 
 
Kétféleképpen ehhez a szabályok motor.  Mindkét esetben a forráskiszolgálóval a fájlt az **Access-vezérlőelem-engedélyezése – Origin** élőfej teljesen figyelmen kívül hagyja, a CDN szabályok motor teljesen kezeli a megengedett CORS forrásokból.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Az összes érvényes forrásokból egy kifejezéssel
 
Ebben az esetben kell létrehoznia a reguláris kifejezéseket, amely tartalmazza az összes engedélyezni szeretné a forrásokból: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Azure CDN Verizon a** [Perl kompatibilis reguláris kifejezések](http://pcre.org/) használja, mint a reguláris kifejezésekkel motort.  Például a [Szokásos kifejezések 101](https://regex101.com/) eszköz használatával ellenőrzése a normál kifejezést.  Figyelje meg, hogy a "/" karakter érvényes a reguláris kifejezésekkel és nem szükséges, meg kell jelölni, azonban, hogy karakter kijutását javasoljuk egy és várakozások szerint néhány regex érvényesítőket.

A kifejezéssel megfelel, a szabály helyére kerül az **Access-vezérlőelem-engedélyezése – Origin** fejléc (ha van ilyen), a származási, amely a kérést küldte el.  További CORS fejlécek, például **Access-ellenőrzési-engedélyezése – módszereket**is hozzáadhat.

![Szabályok példa reguláris kifejezésekkel együtt](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Az egyes származási élőfej szabály kérheti.

Reguláris kifejezések, nem pedig annak egy külön szabályt minden származás használata a [feltételnek megfelelő](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1) **Kérése élőfej helyettesítő** engedélyezni szeretné helyette létrehozása. Akárcsak a reguláris kifejezésekkel módszer a szabályok motor egyedül a CORS fejlécek állítja be. 
  
![Szabályok példában nélküli reguláris kifejezésekkel](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] Helyettesítő karakter használata a fenti példában a * közli a szabályok motort HTTP és a HTTPS megfelelően.
 
### <a name="azure-cdn-standard"></a>Azure CDN Standard

Azure CDN szabványos profilok az csak eljárás, amely lehetővé teszi a helyettesítő származási használata nélkül több forrásokból az használata a [lekérdezési karakterlánc gyorsítótárazás](cdn-query-string.md).  Engedélyezze a lekérdezési karakterlánc beállítást a CDN-végpont és egy egyedi lekérdezési karakterláncban az engedélyezett tartományokhoz kérései szüksége. Ezzel a gyorsítótár minden egyedi lekérdezési karakterlánc egy külön objektumot CDN eredményezi. Ezt a megközelítést nem ideális, azonban, az a CDN gyorsítótárazott ugyanannak a fájlnak több példányban eredményez.  

