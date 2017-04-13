<properties
     pageTitle="Hogyan Azure Tartalomkézbesítési hálózati (CDN) tartalma feleltesse meg egy egyéni tartományt |} Microsoft Azure"
     description="Ez a témakör a CDN-tartalom feleltesse meg egy egyéni tartományt szemléltetik."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Egyéni tartomány megfeleltetése tartalom kézbesítési hálózati (CDN) végpont módjáról
Egyéni tartományt ahhoz, hogy az URL-ek a saját tartománynév használatával gyorsítótárban tárolt tartalom helyett a azureedge.net altartományt képezhető le egy CDN-végpontot.

Az egyéni tartomány hozzárendelése egy CDN-végpontot két módja van:

1. [Hozzon létre egy CNAME rekordot a tartományregisztrálója, és feleltesse meg az egyéni tartomány és altartomány az a CDN-végpontot](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    Egy CNAME rekordot a DNS-szolgáltatása, amely a forrás tartományt, például megfeleltetések `www.contosocdn.com` vagy `cdn.contoso.com`, az egy e-céltartományban. Ebben az esetben a forrástartomány a saját tartomány és a subdomain (altartomány, például a **www-t** vagy a **cdn** mindig szükség). A céltartományban az a CDN-végpontot.  

    A folyamaton, az egyéni tartomány hozzárendelése a CDN-végpontot, azonban okozhat a tartomány legrövidebb leállás rövid időtartam közben regisztrál a tartományt az Azure-portálon.

2. [A **cdnverify** egy köztes regisztrációs lépés hozzáadása](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Ha az egyéni tartomány van jelenleg támogató szolgáltatói szerződést (SLA), amely szükségessé teszi, hogy nincs nincs legrövidebb leállás és az alkalmazások, majd is használhatja az Azure **cdnverify** altartomány köztes regisztrációs lépésnek számára, hogy a felhasználók hozzáadhassák a tartomány a DNS-leképezés kerül sor közben eléréséhez.  

Miután regisztrálta az egyéni tartomány használata a fenti eljárás egyikét, érdemes ellenőrizze [, hogy az egyéni altartomány hivatkozik-e a CDN-végpontot](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] A tartományregisztrálónál tartománya hozzárendelését a CDN-végpont egy CNAME rekordot is létre kell hoznia. CNAME rekordok például feleltesse meg bizonyos altartományokat `www.contoso.com` vagy `cdn.contoso.com`. Még nem lehet feleltesse meg egy CNAME rekordot a legfelső szintű tartományt, például: `contoso.com`.
>    
> Altartomány csak társítható egy CDN-végpontot. A CNAME rekord létrehozott minden forgalom az altartomány a megadott végpont címezve irányítja.  Ha például társítania `www.contoso.com` az a CDN-végpontot, majd nem társítja azt más Azure végpontok, például egy tároló fiók végpontot, vagy egy felhőalapú szolgáltatás végpontjának. Azonban használható különböző altartományokat ugyanabban a tartományban lévő másik szolgáltatáscsaládba végpontok. Különböző altartományt is megfeleltetése az azonos CDN-végpontot.
>
> **Azure CDN a Verizon** (normál és Premium) végpontok figyelje meg, hogy foglal **90 perc** az egyéni tartomány módosítás CDN él csomópontjainak propagálása.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Egyéni tartomány Azure CDN zárólap regisztrálása

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2.  Kattintson a **Tallózás gombra**, majd **CDN-profilokat**, majd a hozzá kívánja rendelni egy egyéni tartományra végponttal CDN profil.  
3.  Kattintson az a **CDN-profil** lap az CDN-végpontot, amellyel az altartomány a hozzárendelni kívánt.
4.  Az endpoint a lap tetején a **Saját tartomány hozzáadása** gombra.  Az **Egyéni tartomány felvétele** lap látni fogja a végpontot állomásnév esetén a CDN végpontot, hozzon létre egy új CNAME rekordot használandó származik. A host név cím formátumát fog megjelenni ** &lt;EndpointName >. azureedge.net**.  A CNAME rekord létrehozásához használni ezt a host name másolhatja.  
5.  Keresse meg a tartományregisztráló webhelyén, és keresse meg a DNS-rekordok létrehozására vonatkozó szakaszt. Egy szakasz, például a **Tartomány neve**, **DNS**vagy **Név Server Management**Észreveheti.
6.  Keresse meg a szakasz CNAME kezelésére szolgáló. Előfordulhat, hogy nyissa meg a Speciális beállítások lapot, és keresse meg a szavakat, CNAME, Alias vagy altartományokat.
7.  Hozzon létre egy új CNAME rekordot, amely a választott altartományt (például a **www-t** vagy a **cdn**) hozzárendeli a megadott az **Egyéni tartomány felvétele** lap állomásneve.
8.  Térjen vissza az **Egyéni tartomány felvétele** lap, és írja be az egyéni tartomány, a altartomány, beleértve a párbeszédpanelen. Például adja meg a tartomány nevét a formátumban `www.contoso.com` vagy `cdn.contoso.com`.   

    Azure ellenőrzi, hogy létezik-e a CNAME rekordot a megadott tartomány nevét. Ha a CNAME helyes, az egyéni tartomány érvényességét.  **Azure CDN a Verizon** (normál és Premium) végpontok az egyéni tartomány beállítások azonban az összes CDN él csomóponthoz terjesztése legfeljebb 90 percig is eltarthat.  

    Figyelje meg, hogy bizonyos esetekben időt igénybe vehet a CNAME rekord névkiszolgálók az interneten a szülőtől. Ha a tartomány nem azonnal érvényesíteni, és úgy gondolja, hogy helyesek-e a CNAME rekordot, várjon néhány percet, és próbálkozzon újra.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Egyéni tartomány használata a közvetítő cdnverify altartomány Azure CDN zárólap regisztrálása  

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **Tallózás gombra**, majd **CDN-profilokat**, majd az egyéni tartomány megfeleltetni kívánt végponttal CDN profil.  
3. Kattintson az a **CDN-profil** lap az CDN-végpontot, amellyel az altartomány a hozzárendelni kívánt.
4. Az endpoint a lap tetején a **Saját tartomány hozzáadása** gombra.  Az **Egyéni tartomány felvétele** lap látni fogja a végpontot állomásnév esetén a CDN végpontot, hozzon létre egy új CNAME rekordot használandó származik. A host név cím formátumát fog megjelenni ** &lt;EndpointName >. azureedge.net**.  A CNAME rekord létrehozásához használni ezt a host name másolhatja.
5. Keresse meg a tartományregisztráló webhelyén, és keresse meg a DNS-rekordok létrehozására vonatkozó szakaszt. Előfordulhat, hogy keresett elem, például a **Tartománynevet**, **DNS**vagy **Név kiszolgáló kezelése**szakaszban.
6. Keresse meg a szakasz CNAME kezelésére szolgáló. Előfordulhat, hogy akkor nyissa meg a Speciális beállítások lapot, és keresse meg a **CNAME**, szavakat **Alias**vagy **altartományokat**.
7. Hozzon létre egy új CNAME rekordot, és adja meg a egy altartomány aliast, amely tartalmazza az **cdnverify** altartomány. Az altartomány megadott például a formátum **cdnverify.www** vagy **cdnverify.cdn**lesz. Közölje az a CDN végpont **formátumban állomásnév cdnverify.&lt; EndpointName >. azureedge.net**.
8. Térjen vissza az **Egyéni tartomány felvétele** lap, és írja be az egyéni tartomány, a altartomány, beleértve a párbeszédpanelen. Például adja meg a tartomány nevét a formátumban `www.contoso.com` vagy `cdn.contoso.com`. Fontos tudni, hogy ebben a lépésben, nem található az altartomány a **cdnverify**.  

    Azure ellenőrzi, hogy létezik-e a CNAME rekord adta meg, a cdnverify tartomány nevét.
9. Ezen a ponton Azure ellenőrizte az egyéni tartomány, de a forgalmat a tartomány van még irányítása nem az a CDN-végpontot. Után elég ideig várakozási engedélyezni propagálása az a CDN él csomópontok ( **Verizon az Azure CDN**és az **Azure CDN Akamai az**1-2 percig, amíg a 90 perc), az egyéni tartomány beállítások térjen vissza a DNS-szolgáltató webhelyén, és egy másik CNAME rekord létrehozására, amely rendeli hozzá az altartomány a CDN-végpontot. Például adja meg a **www-t** vagy **cdn**, és a hostname (állomásnév), mint a altartomány ** &lt;EndpointName >. azureedge.net**. Ezt a lépést az egyéni tartomány a regisztráció befejeződött.
10. Végül törölheti a CNAME rekord **cdnverify**, használatával létrehozott, csak közvetítő lépéseként szükséges volt.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Ellenőrizze, hogy az egyéni altartomány hivatkozik-e a CDN-végpontok

- Az egyéni tartomány a regisztráció után a CDN-végpont az egyéni tartomány használatával érheti el a tartalom gyorsítótárazott.
Először győződjön meg arról, hogy rendelkezik-e a végpont gyorsítótárazott nyilvános tartalmat. Például, ha a CDN-végpontot tároló fiókkal tartozik a CDN gyorsítótárát nyilvános blob tárolók a tartalmat. Az egyéni tartomány ellenőrzéséhez győződjön meg arról, hogy a tároló nyilvános elérésének van beállítva, és legalább egy blob tartalmaz.
- A böngészőben nyissa meg az egyéni tartomány használata a blob címét. Ha például az egyéni tartomány van `cdn.contoso.com`, gyorsítótárazott blob URL-CÍMÉT az alábbi URL-cím hasonló lesz: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Lásd még:

[Azure engedélyezése a Tartalomkézbesítési hálózatai (CDN)](./cdn-create-new-endpoint.md)  
