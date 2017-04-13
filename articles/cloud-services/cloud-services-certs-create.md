<properties 
    pageTitle="Cloud Services és a kezelés tanúsítványok |} Microsoft Azure" 
    description="Megtudhatja, hogy miként hozhat létre, és a Microsoft Azure tanúsítványok használata" 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Az Azure Cloud Services a tanúsítványok – áttekintés
Tanúsítványok az Azure szolgálnak felhőszolgáltatások ([service tanúsítványok](#what-are-service-certificates)) és a hitelesítő-ös az API-val ([management Tanúsítványok](#what-are-management-certificates) az Azure klasszikus portálját, és nem ARM használata esetén). Ez a témakör áttekintést egy általános mindkét tanúsítvány típusú [létrehozása](#create) és [telepítése](#deploy) az Azure őket.

Azure-ban használt tanúsítványok x.509 v3 tanúsítványokat, és egy másik megbízható tanúsítvány kell aláírnia vagy önaláírt is lehet. Önaláírt tanúsítvány jelentkezett-e saját készítő, és emiatt, alapértelmezés szerint nem megbízható. A legtöbb böngészőben figyelmen kívül hagyhatja a. Önaláírt tanúsítványok csak használható kell keresnie őket fejlesztés és a felhőbeli szolgáltatások tesztelés során. 

Azure által használt tanúsítványok magánjellegű vagy egy nyilvános kulcs is tartalmazhat. Tanúsítványok egy ujjlenyomat, amely lehetővé teszi megkönnyítése érdekében egyértelművé módon van. A ujjlenyomat szolgál az Azure [konfigurációs fájl](cloud-services-configure-ssl-certificate.md) azonosítása melyik tanúsítványt egy felhőalapú szolgáltatásba kell használnia. 

## <a name="what-are-service-certificates"></a>Mik azok a tanúsítványok szolgáltatás?
Szolgáltatás-tanúsítványok cloud services és és a szolgáltatás közötti biztonságos kommunikáció engedélyezése vannak csatolva. Például ha egy webes szerepkör telepítette, volna szeretne megadni egy tanúsítványt, amely hitelesíthet elérhető HTTPS-végpont. Szolgáltatás-tanúsítványok szolgáltatás definíciójának meghatározott automatikusan a virtuális géphez, amely a szerepkör egy példánya fut van telepítve. 

Szolgáltatás-tanúsítványok is feltölthet a Azure klasszikus portálra vagy az Azure klasszikus portálon vagy a szolgáltatás felügyeleti API segítségével. Szolgáltatás a tanúsítványok egy adott felhőszolgáltatásba társított és a szolgáltatás-definíciós fájl telepítéshez hozzárendelt.

Szolgáltatás tanúsítványok a a szolgáltatások külön-külön kezelhető, és előfordulhat, hogy más személyek kell kezelniük. A Fejlesztőeszközök előfordulhat, hogy például az informatikai vezető korábban feltöltötte Azure tanúsítvány hivatkozó szolgáltatás csomag feltöltése. Az informatikai vezető kezelheti és a szolgáltatás beállításait megváltoztatja anélkül, hogy az új szolgáltatás csomag feltöltése tanúsítvány megújításához. Ennek oka lehet, hogy a logikai nevét a tanúsítvány és a tár nevét és helyét megadott szolgáltatás-definíciós fájl, miközben a tanúsítvány ujjlenyomatot a szolgáltatás konfigurációs fájl a megadott. A tanúsítvány frissítéséhez gomb csak akkor tölthet fel egy újat és módosítja a szolgáltatás konfigurációs fájl ujjlenyomatot értékét szükséges.

## <a name="what-are-management-certificates"></a>Mik azok a tanúsítványok kezelése
Adatkezelési tanúsítványok teszi lehetővé az Azure klasszikus által nyújtott szolgáltatás felügyeleti API-val hitelesítést végezni. Számos programok és eszközök (például a Visual Studio vagy az Azure SDK) fogja használni a tanúsítványok automatizálása konfigurálása és üzembe Azure szolgáltatást. Ezek nem igazán kapcsolódnak a felhőszolgáltatásokba. 

>[AZURE.WARNING] légy óvatos! Tanúsítványok az ilyen típusú engedélyezése bárki, aki hitelesíti a velük társított azokat az előfizetés kezeléséhez. 

### <a name="limitations"></a>Korlátozások
Legfeljebb 100 management Tanúsítványok előfizetésenként van. Is van legfeljebb 100 management Tanúsítványok kapcsolódó összes előfizetéshez az egy adott szolgáltatás rendszergazdai felhasználói azonosítójával. Ha a fiók rendszergazdák számára a felhasználói azonosító már használatban van 100 management webhelyben, és további tanúsítványok szükség van, a közös rendszergazdája, a további webhelyben is hozzáadhat. 

Mielőtt felvenne 100-nál több tanúsítványok, olvassa el a Ha, így újból felhasználhatja egy meglévő tanúsítvány című témakört. Esetleg a szükségtelen összetettsége használ, további rendszergazdák felvétele a tanúsítvány kezelése során.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Új önaláírt tanúsítvány létrehozása
Bármelyik rendelkezésére önaláírt tanúsítvány létrehozása, feltéve hogy igazodjon a beállítások eszköz használható:

* Egy X.509-tanúsítványt.
* A titkos kulcs tartalmazza.
* Létre fő exchange (.pfx fájl).
* Meg kell egyeznie a felhőbeli szolgáltatástól eléréséhez használt tartomány. 
    > Egy SSL-tanúsítvány a cloudapp.net (vagy bármely kapcsolódó Azure) tartomány; nem finomítása a tanúsítvány tulajdonosneve egyeznie kell az egyéni tartománynevet, amellyel elérhető az alkalmazás. Például **contoso.net**, **contoso.cloudapp.net**.
* Minimális 2048 bites titkosítást.
* **Csak a tanúsítvány szolgáltatás**: ügyféloldali tanúsítvány kell lennie, a *személyes* tanúsítvány tárban.

Kétféleképpen egyszerűen a Windows, a tanúsítványok létrehozásáról a `makecert.exe` segédprogram vagy az IIS.

### <a name="makecertexe"></a>MakeCert.exe

A segédprogram elavult, és már nem itt ismertetett. Olvassa el [a MSDN-cikk](https://msdn.microsoft.com/library/windows/desktop/aa386968) további információt.

### <a name="powershell"></a>A PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Ha szeretné használni a tanúsítványt egy tartomány helyett IP-címet, használja az IP-cím - Dns_név paraméter.


Ha ezzel a [tanúsítvánnyal az adatkezelési portálon](../azure-api-management-certs.md), exportálása **.cer** fájl:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Az Internet Information Services (IIS)

Sok weblap van az interneten, amely ennek módjáról az IIS terjed ki. [Itt](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) nem található, valamint megtudhatja számításba remek egy. 

### <a name="java"></a>Java
Java segítségével [tanúsítvány létrehozása](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[Ez](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) a cikk ismerteti, hogyan SSH tanúsítványok létrehozását.

## <a name="next-steps"></a>Következő lépések

[A szolgáltatás tanúsítványt az Azure klasszikus portálon feltöltése](cloud-services-configure-ssl-certificate.md) (vagy az [Azure portálra](cloud-services-configure-ssl-certificate-portal.md)).

Töltse fel [adatkezelési API-tanúsítványt](../azure-api-management-certs.md) az Azure klasszikus portálra.

>[AZURE.NOTE] Az Azure portálon nem management Tanúsítványok eléréséhez használja az API, de inkább a felhasználói fiókokat használ.
