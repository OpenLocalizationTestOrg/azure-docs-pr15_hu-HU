<properties
    pageTitle="A saját tartománynév beállítása a Cloud Services |} Microsoft Azure"
    description="Megtudhatja, hogy miként kattintva jelenítse meg az adatokat az internethez, a saját tartomány vagy az Azure alkalmazás DNS-beállítások konfigurálásával.  Az alábbi példákban az Azure-portálra."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Egyéni tartománynevet az Azure felhőalapú szolgáltatás konfigurálása

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-custom-domain-name-portal.md)
- [Azure klasszikus portál](cloud-services-custom-domain-name.md)

Amikor létrehoz egy felhőalapú szolgáltatásba, Azure rendeli az **cloudapp.net**altartomány. Ha például a felhőalapú szolgáltatás neve "contoso", ha a felhasználók lesz hozzáférhet az alkalmazásokat, például http://contoso.cloudapp.net URL-cím. Azure is rendel egy virtuális IP-címet.

Jó helyen jár akkor is is jelenítik meg az alkalmazás a saját tartománynevet (például **contoso.com)**. Ez a cikk ismerteti, hogyan lefoglalása, vagy állítsa be a Felhőbeli szolgáltatástól webes szerepkörök egyéni tartománynevet.

Mit már milyen CNAME és A rekordok undestand? [Ugrás a explaination múltbeli](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Ebben a feladatban eljárásokat alkalmazni az Azure Cloud Services. Az alkalmazás-szolgáltatások lásd [a](../app-service-web/web-sites-custom-domain-name.md). Tárterület-fiók esetén című témakör tartalmaz [a](../storage/storage-custom-domain-name.md).

<p/>

> [AZURE.TIP]
> Gyorsabb – áttekintés használata új Azure [interaktív útmutató](http://support.microsoft.com/kb/2990804)!  Van egy egyéni tartománynevet és védelme kommunikáció (SSL) társítása Azure Cloud Services vagy az Azure webhelyek egy pillanat alatt.

## <a name="understand-cname-and-a-records"></a>Dátumtáblázatok ismertetése és az A CNAME rekordok

CNAME (vagy a rekordok alias) és A rekordok, mindkét lehetővé teszi a tartománynév hozzárendelése egy adott kiszolgálóhoz (vagy a szolgáltatás ebben az esetben) azonban ezek másképp működnek. Is adott megfontolandó A rekordok használatakor a kiválasztásához, amelyek használata előtt vegye figyelembe Azure felhőszolgáltatásaihoz.

### <a name="cname-or-alias-record"></a>Alias vagy a CNAME rekord

Egy CNAME rekordot a *adott* tartományt, például: **contoso.com** vagy **www.contoso.com**, megfelelteti a kanonikus tartománynév. Ebben az esetben a kanonikus tartománynév van-e a **[SajátPr] .cloudapp .net** tartománynevét az Azure alkalmazás is. Amikor létrejött, a CNAME alias hoz létre a **[SajátPr] .cloudapp .net**. A CNAME bejegyzés fog úgy, hogy az IP-címét a **[SajátPr] .cloudapp .net** szolgáltatás automatikusan, ha megváltozik a felhőbeli szolgáltatástól IP-címét, nem kell tennie semmit.

> [AZURE.NOTE]
> Néhány tartományregisztrálói csak teszi altartományokat feleltesse meg egy CNAME rekordot, például www.contoso.com, és nem legfelső szintű nevek (például contoso.com) használata esetén. További információt a CNAME rekordot a tartományregisztráló, [a CNAME rekord Wikipedia-bejegyzés](http://en.wikipedia.org/wiki/CNAME_record)vagy a [IETF tartománynevek - végrehajtása és a specifikációja](http://tools.ietf.org/html/rfc1035) dokumentum által biztosított dokumentációjában.

### <a name="a-record"></a>Rekord létrehozása

Az *A* rekord rendeli hozzá a tartományt, például: **contoso.com** vagy **www.contoso.com**, *vagy egy helyettesítő tartomány* például ** \*. contoso.com**, IP-cím. Ha egy Azure felhőalapú szolgáltatás, a virtuális IP a szolgáltatás. A fő az A rekord fölé egy CNAME rekordot előnye, hogy egy bejegyzést, például egy helyettesítő karaktert használó lehet, \* **. contoso.com**, amely volna kezelje a több alárendelt tartományok, például a **mail.contoso.com**, **login.contoso.com**vagy **www.contso.com**kér.

> [AZURE.NOTE]
> Az A rekord van rendelve egy statikus IP-cím, mivel azt nem lehet automatikusan változtatásainak feloldása a felhőalapú szolgáltatás IP-címére. Az a felhő szolgáltatás által használt IP-cím van hozzárendelve egy üres tárolóhely (gyártási vagy átmeneti.) történő telepítése első alkalommal Ha törli a tárolóhely telepítését, az IP-cím Azure által kiadott, és minden későbbi telepítéshez való a tárolóhely új IP-cím lehet adni.
>
> Állandó kényelmesen, egy adott telepítési tárolóhely (a termelési vagy átmeneti) IP-címét a Ha csere előkészítése és munkakörnyezeti telepítések vagy a meglévő telepítés helyi frissítés végrehajtása között. További tájékoztatást a következő műveletek elvégzéséhez megtudhatja, [hogy miként kezelheti a felhőszolgáltatások](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Az egyéni tartomány CNAME rekord hozzáadása

A CNAME rekord létrehozására, hozzá kell adnia egy új bejegyzést a DNS-táblázat az egyéni tartományához tartozó a szolgáltató által biztosított eszközeivel. Minden tartományregisztráló még egy CNAME rekordot tartalmazó hasonló, de kissé eltérő módszer, de a fogalmak megegyeznek.

1. Az alábbi módszerek egyikét segítségével keresse meg a **. cloudapp.net** a felhőalapú szolgáltatáshoz rendelt tartománynevet.

    * Jelentkezzen be az [Azure portálon]válassza a felhőalapú szolgáltatás, tekintse meg a **Essentials** szakaszt, és keresse meg a **Webhely URL-címe** bejegyzés.

        ![fontos szakasz megjelenítése a webhely URL-címe][csurl]
            
        **VAGY**
  
    * Telepítse és állítsa be a [Azure Powershell](../powershell-install-configure.md), és használja a következő parancsot:

        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Mentse a tartomány nevét az URL-cím mindkét módszer, által visszaadott használják szüksége lesz, amikor egy CNAME rekord létrehozásához.

1.  Jelentkezzen be a DNS-szolgáltató webhelyén, és nyissa meg a DNS kezelése lapot. Keresse meg a hivatkozásokat, vagy a **Tartománynevet**, a **DNS-**vagy a **Név kiszolgáló kezelése**felirata webhely részeinek.

2.  Ha jelölje ki vagy adja meg a CNAME-féle most megkeresése Előfordulhat, hogy ki kell válassza a record type a egy legördülő listában, vagy nyissa meg a Speciális beállítások lapot. **Alias**vagy **altartományokat**a szavak **CNAME**, meg kell kinéznie.

3.  Meg kell adnia a tartomány és altartomány alias, például a **www-t** a CNAME Ha létre szeretne hozni egy aliast **www.customdomain.com**. Ha szeretne a gyökértartomány alias létrehozása, akkor előfordulhat, hogy fog szerepelni a "**@**" a tartományregisztráló DNS-eszközök a szimbólumot.

4. Ezután meg kell adnia a kanonikus állomásnév, amely az alkalmazás **cloudapp.net** tartomány ebben az esetben.

Ha például a következő CNAME rekord továbbítja minden forgalom **www.contoso.com** **contoso.cloudapp.net**, az egyéni tartomány nevét a telepített alkalmazások:

| Alias vagy állomásnév oszlopban név/altartomány | Kanonikus tartomány     |
| ------------------------- | -------------------- |
| www                       | contoso.cloudapp.NET |

> [AZURE.NOTE]
A látogatók a **www.contoso.com** soha nem jelenik meg igaz a host (contoso.cloudapp.net), ezért a továbbítás folyamat nem látható a végfelhasználó.

> A fenti példában csak a **www** altartomány forgalom vonatkozik. CNAME rekordok nem használhat helyettesítő karaktereket, mivel létre kell hoznia egy CNAME minden tartomány és altartomány. Ha azt szeretné, irányítsa át a forgalmat a altartományokat, mint például *., akkor a contoso.com a cloudapp.net címét, beállíthatja a * *URL-átirányítási* * vagy a * *Továbbítás URL-cím** a DNS-beállítások elemére, vagy hozzon létre egy A rekordot.


## <a name="add-an-a-record-for-your-custom-domain"></a>Az egyéni tartományát A rekord létrehozása

Az A rekord létrehozásához először meg kell keresnie a felhőalapú szolgáltatás virtuális IP-címét. Ezután vegyen fel egy új egyéni tartománya DNS-táblázatban a szolgáltató által biztosított eszközeivel. Minden tartományregisztráló egy A rekordot megadni a hasonló, de némileg eltérő módszer van, de a fogalmak megegyeznek.

1. Az alábbi módszerek segítségével beszerzése a felhőalapú szolgáltatás IP-címét.

    * Jelentkezzen be az [Azure portálon], jelölje ki a felhőalapú szolgáltatást, tekintse meg a **Essentials** szakasz, és keresse meg a **nyilvános IP-címek** bejegyzés.

        ![a virtuális megjelenítő fontos szakasz][vip]

        **VAGY**

    * Telepítse és állítsa be a [Azure Powershell](../powershell-install-configure.md), és használja a következő parancsot:

        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Az IP-cím mentése lesz szükség szerint egy rekord létrehozása

1.  Jelentkezzen be a DNS-szolgáltató webhelyén, és nyissa meg a DNS kezelése lapot. Keresse meg a hivatkozásokat, vagy a **Tartománynevet**, a **DNS-**vagy a **Név kiszolgáló kezelése**felirata webhely részeinek.

2.  Ha jelölje ki vagy írja be A rekord most megkeresése Előfordulhat, hogy ki kell válassza a record type a egy legördülő listában, vagy nyissa meg a Speciális beállítások lapot.

3. Jelölje ki vagy adja meg a tartomány vagy altartomány, amely használja ezt A rekordot. Ha például kijelölése **www** Ha létre szeretne hozni egy aliast **www.customdomain.com** Ha az összes altartományokat helyettesítő szöveg létrehozása szeretne, adja meg a "__*__". Ez az összes alárendelt tartományok, például **mail.customdomain.com** **login.customdomain.com**és **www.customdomain.com**kiterjed.

    Ha létrehoz egy A rekordot a gyökértartomány szeretne, akkor előfordulhat, hogy fog szerepelni a "**@**" a tartományregisztráló DNS-eszközök a szimbólumot.

4. A megadott mezőben írja be a felhőalapú szolgáltatás IP-címét. Ez a a tartomány bejegyzés használt az A rekordot a felhőalapú szolgáltatás üzembe az IP-címmel társít.

Ha például az egy rekordot minden forgalom továbbítja **contoso.com** **137.135.70.239**, a telepített alkalmazások IP-címét az alábbi:

| A Host név/altartomány | IP-cím     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |


Ez a példa bemutatja, hogy hozzon létre egy A rekordot a legfelső szintű tartomány. Ha ki szeretne, hogy pontosan illeszkedjen összes altartományokat helyettesítő szöveg létrehozása, írná be "__*__", az altartomány.

>[AZURE.WARNING]
>IP-címek Azure-ban olyan dinamikus alapértelmezés szerint. Program valószínűleg használni kívánt a [fenntartott IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md) annak érdekében, hogy az IP-címének nem változik.

## <a name="next-steps"></a>Következő lépések

* [Cloud Services kezelése](cloud-services-how-to-manage.md)
* [Hogyan CDN-tartalom hozzárendelése az egyéni tartomány](../cdn/cdn-map-content-to-custom-domain.md)
* [A felhőalapú szolgáltatás általános konfigurálása](cloud-services-how-to-configure-portal.md).
* Megtudhatja, hogyan [egy felhőalapú szolgáltatás üzembe](cloud-services-how-to-create-deploy-portal.md).
* [Az ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md)beállítása.

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure portál]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
 