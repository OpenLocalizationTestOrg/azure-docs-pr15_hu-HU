<properties 
    pageTitle="Felosztása és egyesítése biztonsági beállításai |} Microsoft Azure" 
    description="Állítsa be x409 titkosítás tanúsítványai" 
    metaKeywords="Elastic Database certificates security" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng" />


# <a name="split-merge-security-configuration"></a>Felosztása és egyesítése biztonsági beállításai  

A felosztott vagy egyesített szolgáltatást használ, biztonsági megfelelően meg kell adnia. A szolgáltatás a skála rugalmas szolgáltatást, a Microsoft Azure SQL-adatbázis része. További információ a [skála rugalmas az osztott és egyesítése szolgáltatás oktatóprogram](sql-database-elastic-scale-configure-deploy-split-and-merge.md)témakörben talál.

## <a name="configuring-certificates"></a>Tanúsítványok beállítása

Tanúsítványok kétféleképpen vannak beállítva. 

1. [Az SSL-tanúsítvány beállítása](#To-Configure-the-SSL#Certificate)
2. [Ügyfél-tanúsítványok beállítása](#To-Configure-Client-Certificates) 

## <a name="to-obtain-certificates"></a>Tanúsítványok beszerzése

Tanúsítványok szerezhető be nyilvános hitelesítésszolgáltatók (CA) vagy a [Windows-alapú tanúsítvány szolgáltatást](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Ezek az előnyben részesített módszerek tanúsítványok juthat.

Ha nem érhetők el ezeket a beállításokat, **önaláírt tanúsítványok**hozhat létre.
 
## <a name="tools-to-generate-certificates"></a>Tanúsítványok létrehozása eszközök

* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="to-run-the-tools"></a>Az eszközök futtatása

* A Fejlesztőeszközök parancssort az vizuális Studios, lásd: a [Visual Studio parancssor](http://msdn.microsoft.com/library/ms229859.aspx) 

    Ha telepítve van, akkor nyissa meg:

        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 

* A WDK az első [Windows 8.1: Töltse le a Kit és eszközök](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="to-configure-the-ssl-certificate"></a>Az SSL-tanúsítvány beállítása
A kommunikáció titkosításához, és a hitelesítési a kiszolgáló SSL-tanúsítvány van szükség. Válassza a legtöbb esetben az alábbi három jelenik meg, és hajtsa végre az összes lépést:

### <a name="create-a-new-self-signed-certificate"></a>Új önaláírt tanúsítvány létrehozása

1.    [Önaláírt tanúsítvány létrehozása](#Create-a-Self-Signed-Certificate)
2.    [PFX fájl önaláírt SSL-tanúsítvány létrehozása](#Create-PFX-file-for-Self-Signed-SSL-Certificate)
3.    [Töltse fel a felhőbe szolgáltatás SSL-tanúsítvány](#Upload-SSL-Certificate-to-Cloud-Service)
4.    [Az SSL-tanúsítvány szolgáltatás konfigurációs fájl módosítása](#Update-SSL-Certificate-in-Service-Configuration-File)
5.    [Importálás SSL hitelesítésszolgáltató](#Import-SSL-Certification-Authority)

### <a name="to-use-an-existing-certificate-from-the-certificate-store"></a>Meglévő tanúsítvány használni a tanúsítvány áruházból
1. [Az SSL-tanúsítvány exportálása tanúsítvány áruházból](#Export-SSL-Certificate-From-Certificate-Store)
2. [Töltse fel a felhőbe szolgáltatás SSL-tanúsítvány](#Upload-SSL-Certificate-to-Cloud-Service)
3. [Az SSL-tanúsítvány szolgáltatás konfigurációs fájl módosítása](#Update-SSL-Certificate-in-Service-Configuration-File)

### <a name="to-use-an-existing-certificate-in-a-pfx-file"></a>Meglévő tanúsítvány használata a PFX-fájlba

1. [Töltse fel a felhőbe szolgáltatás SSL-tanúsítvány](#Upload-SSL-Certificate-to-Cloud-Service)
2. [Az SSL-tanúsítvány szolgáltatás konfigurációs fájl módosítása](#Update-SSL-Certificate-in-Service-Configuration-File)

## <a name="to-configure-client-certificates"></a>Ügyfél-tanúsítványok beállítása
Ügyfél tanúsítványokat annak érdekében, hogy a szolgáltatási kérelmek hitelesítést végezni. Válassza a legtöbb esetben az alábbi három jelenik meg, és hajtsa végre az összes lépést:

### <a name="turn-off-client-certificates"></a>Ügyfél-tanúsítványok kikapcsolása
1.    [Kapcsolja ki az ügyfél tanúsítvány-alapú hitelesítés](#Turn-Off-Client-Certificate-Based-Authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Új ügyfél önaláírt tanúsítványokat
1.    [Hozzon létre egy önaláírt hitelesítésszolgáltató](#Create-a-Self-Signed-Certification-Authority)
2.    [Hitelesítésszolgáltató tanúsítványát szolgáltatás cloud feltöltése](#Upload-CA-Certificate-to-Cloud-Service)
3.    [Hitelesítésszolgáltató tanúsítványát szolgáltatás konfigurációs fájl módosítása](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Ügyfél-tanúsítványokat](#Issue-Client-Certificates)
5.    [Ügyfél-tanúsítványok PFX fájlok létrehozása](#Create-PFX-files-for-Client-Certificates)
6.    [Ügyfél-tanúsítvány importálása](#Import-Client-Certificate)
7.    [Ügyfél tanúsítvány Thumbprints másolása](#Copy-Client-Certificate-Thumbprints)
8.    [A szolgáltatás konfigurációs fájl engedélyezett ügyfélprogramok beállítása](#Configure-Allowed-Clients-in-the-Service-Configuration-File)

### <a name="use-existing-client-certificates"></a>Meglévő, ügyfél tanúsítványok használata
1.    [Nyilvános kulcs hitelesítésszolgáltató megkeresése](#Find-CA-Public Key)
2.    [Hitelesítésszolgáltató tanúsítványát szolgáltatás cloud feltöltése](#Upload-CA-certificate-to-cloud-service)
3.    [Hitelesítésszolgáltató tanúsítványát szolgáltatás konfigurációs fájl módosítása](#Update-CA-Certificate-in-Service-Configuration-File)
4.    [Ügyfél tanúsítvány Thumbprints másolása](#Copy-Client-Certificate-Thumbprints)
5.    [A szolgáltatás konfigurációs fájl engedélyezett ügyfélprogramok beállítása](#Configure-Allowed-Clients-in-the-Service-Configuration File)
6.    [Állítsa be az ügyfél visszavonási állapotának ellenőrzése](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Engedélyezett IP-címek

Lehet, hogy a szolgáltatás végpontok való hozzáférést a IP-címek adott tartományok korlátozott.

## <a name="to-configure-encryption-for-the-store"></a>Titkosítás az üzlet konfigurálása

Titkosítsa a hitelesítő adatokat, a metaadat-tároló tárolt tanúsítvány van szükség. Válassza a legtöbb esetben az alábbi három jelenik meg, és hajtsa végre az összes lépést:

### <a name="use-a-new-self-signed-certificate"></a>Új önaláírt tanúsítvány használata

1.     [Önaláírt tanúsítvány létrehozása](#Create-a-Self-Signed-Certificate)
2.     [Önaláírt tanúsítvány-titkosítás PFX fájl létrehozása](#Create-PFX-file-for-Self-Signed-Encryption-Certificate)
3.     [Töltse fel a titkosítási tanúsítvány cloud szolgáltatás](#Upload-Encryption-Certificate-to-Cloud-Service)
4.     [Titkosítási tanúsítvány szolgáltatás konfigurációs fájl módosítása](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-from-the-certificate-store"></a>Meglévő tanúsítvány használja a tanúsítvány áruházból

1.     [Titkosítási tanúsítvány exportálása tanúsítvány áruházból](#Export-Encryption-Certificate-From-Certificate-Store)
2.     [Töltse fel a titkosítási tanúsítvány cloud szolgáltatás](#Upload-Encryption-Certificate-to-Cloud-Service)
3.     [Titkosítási tanúsítvány szolgáltatás konfigurációs fájl módosítása](#Update-Encryption-Certificate-in-Service-Configuration-File)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Meglévő tanúsítvány használata PFX-fájlba

1.     [Töltse fel a titkosítási tanúsítvány cloud szolgáltatás](#Upload-Encryption-Certificate-to-Cloud-Service)
2.     [Titkosítási tanúsítvány szolgáltatás konfigurációs fájl módosítása](#Update-Encryption-Certificate-in-Service-Configuration-File)

## <a name="the-default-configuration"></a>Az alapértelmezett beállítás

Az alapértelmezett beállítás letiltja a HTTP-végpont minden hozzáférést. Ez a javasolt beállításokat, mivel a részvételi ezeket a végpontokat is elvégezheti az bizalmas információkat, például adatbázis hitelesítő adatait.
Az alapértelmezett beállítás a HTTPS-végpont összes hozzáférést biztosít. Ez a beállítás további korlátozva lehet.

### <a name="changing-the-configuration"></a>A konfiguráció megváltoztatása

Access-ellenőrzési szabályok vonatkozó és végpont csoportja úgy van konfigurálva, a a **<EndpointAcls>** a **szolgáltatás konfigurációs fájl**szakaszban.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Access-beállítás csoportban a szabályok vannak beállítva egy <AccessControl name=""> a szolgáltatás konfigurációs fájl szakaszában. 

A formátum a hálózati hozzáférés-vezérlési listákat dokumentáció című cikkben ismertetett.
Például ahhoz, hogy csak az IP-címei 100.100.0.0 való 100.100.255.255 a HTTPS-végpont eléréséhez tartomány, a szabályok módon néz ki:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Szolgáltatás-megelőzés megtagadását

Van két különböző mechanizmusok észleli és megakadályozása szolgáltatás megtagadását támadások támogatott:

*    Egyidejű kérelmek egy távoli száma korlátozása (alapértelmezés szerint kikapcsolva)
*    Mennyi díjat kell egy távoli hozzáférés korlátozása (a alapértelmezés szerint)

Ezek a további dokumentált az IIS dinamikus IP-biztonsági funkciók alapulnak. Ha ez a beállítás módosítása ügyeljen arra, az alábbi tényezőket:

* A proxykiszolgálók és a távoli számítógép információ fölé hálózati címfordítást eszközök
* Minden egyes a webes szerepkör az összes erőforrást tekinthető kérelem (pl. betöltése parancsfájlok, képek, stb)

## <a name="restricting-number-of-concurrent-accesses"></a>Egyidejű bejáratok száma korlátozása

A beállítások, módosíthatja a beállítást a következők:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Módosítsa a DynamicIpRestrictionDenyByConcurrentRequests igaz ahhoz, hogy a védelmet.

## <a name="restricting-rate-of-access"></a>Ráta hozzáférés korlátozása

A beállítások, módosíthatja a beállítást a következők:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-the-response-to-a-denied-request"></a>A válasz arra a letiltott konfigurálása

A következő beállítás azt határozza meg, a válasz arra a letiltott:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Dinamikus IP-biztonság az IIS dokumentációjában találhat más támogatott érték hivatkozik.

## <a name="operations-for-configuring-service-certificates"></a>Műveletek a tanúsítványok szolgáltatás beállítása
Ez a témakör nem csak a hivatkozást. Hajtsa végre a konfigurációs című cikk lépéseit:

* Az SSL-tanúsítvány beállítása
* Ügyfél-tanúsítványok beállítása

## <a name="create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása
Hajtsa végre:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

Ha testre szeretné szabni:

*    -n szolgáltatás URL-címét. Helyettesítő karakterek ("CN = * .cloudapp .net") és a támogatott alternatív nevek ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net").
*    tanúsítvány lejárati dátummal -e olyan erős jelszót létrehozni, és adja meg, amikor a rendszer kéri.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Önaláírt SSL-tanúsítvány PFX-fájl létrehozása

Hajtsa végre:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Írja be a jelszót, és majd exportálja a tanúsítvány ezeket a beállításokat:
* Igen, a titkos kulcs exportálását
* Minden további tulajdonság exportálása

## <a name="export-ssl-certificate-from-certificate-store"></a>Az SSL-tanúsítvány exportálása tanúsítvány áruházból

* Keresse meg a tanúsítvány
* Kattintson a Műveletek -> az összes tevékenység -> Exportálás...
* A tanúsítvány exportálása egy. Az alábbi lehetőségek PFX-fájl:
    * Igen, a titkos kulcs exportálását
    * Az összes tanúsítványok szerepeltetni hitelesítő elérési * minden további tulajdonság exportálása

## <a name="upload-ssl-certificate-to-cloud-service"></a>Az SSL-tanúsítvány feltöltheti felhőszolgáltatásba

Feltöltés és a meglévő tanúsítványt, vagy hozza létre. Az SSL kulcs pár PFX fájlt:

* Írja be a jelszót, a saját kulcs adatainak védelme

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Frissítés SSL-tanúsítvány szolgáltatás konfigurációs fájl

Frissítse a következő beállítást a szolgáltatás konfigurációs fájl ujjlenyomat értékét a ujjlenyomat a tanúsítvány töltenek fel a felhőbeli szolgáltatástól:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Importálás SSL hitelesítésszolgáltató

Hajtsa végre az összes fiók/számítógép zónában, a szolgáltatás fog kommunikálni az alábbi lépéseket:

* Kattintson duplán a. A Windows Intézőben CER fájl
* A tanúsítvány párbeszédpanel tanúsítvány telepítése elemre.
* A megbízható legfelső szintű hitelesítésszolgáltatók tárolóba tanúsítvány importálása

## <a name="turn-off-client-certificate-based-authentication"></a>Kapcsolja ki az ügyfél tanúsítvány-alapú hitelesítés

Csak az ügyfélprogram tanúsítvány-alapú hitelesítés támogatott, és tiltsa le lehetővé teszi a szolgáltatás végpontok nyilvános eléréséhez kivéve, ha más mechanizmusok helyen (például a Microsoft Azure virtuális hálózati).

A szolgáltatás konfigurációs fájl a funkció kikapcsolásához hamis módosítsa a beállításokat:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Ezután másolja a azonos ujjlenyomat, mint az SSL-tanúsítvány hitelesítésszolgáltató tanúsítvány beállításban:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Hozzon létre egy önaláírt hitelesítésszolgáltató
Hajtsa végre az alábbi lépések végrehajtásával használni kívánt hitelesítésszolgáltató önaláírt tanúsítvány létrehozása:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

Ha testre szeretné szabni

*    az e - tanúsítvány lejárati dátummal


## <a name="find-ca-public-key"></a>Nyilvános kulcs hitelesítésszolgáltató megkeresése

Az összes ügyfelet tanúsítványok szolgáltatás megbízható hitelesítésszolgáltató kell bocsátotta. Keresse meg a nyilvános tanúsítványokat az ügyfél, hogy annak érdekében, hogy töltse fel a felhőbeli szolgáltatástól hitelesítéshez használt hitelesítésszolgáltató billentyűt.

Ha a fájlt a nyilvános kulccsal nem érhető el, exportálása a tanúsítvány áruházból:

* Keresse meg a tanúsítvány
    * Ügyfél által kibocsátott tanúsítványt az azonos hitelesítésszolgáltató keresése
* Kattintson duplán a tanúsítvány.
* A tanúsítvány párbeszédpanel válassza ki a hitelesítő elérési út lapot.
* Kattintson duplán az elérési út az hitelesítésszolgáltató bejegyzést.
* A tanúsítvány tulajdonságok jegyzeteket.
* A **tanúsítvány** párbeszédpanel bezárásához.
* Keresse meg a tanúsítvány
    * Keresse meg a fentiekben hitelesítésszolgáltató.
* Kattintson a Műveletek -> az összes tevékenység -> Exportálás...
* A tanúsítvány exportálása egy. Az alábbi lehetőségek CER:
    * **Nem, nem exportálja a titkos kulcs**
    * Ha lehetséges bele az összes tanúsítványok hitelesítő elérési.
    * Minden további tulajdonság exportálása.

## <a name="upload-ca-certificate-to-cloud-service"></a>Hitelesítésszolgáltató tanúsítványának feltöltése felhőalapú szolgáltatás

Feltöltés és a meglévő tanúsítványt, vagy hozza létre. A hitelesítésszolgáltató nyilvános kulcs CER fájl.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Frissítés hitelesítésszolgáltató tanúsítványát szolgáltatás konfigurációs fájl

Frissítse a következő beállítást a szolgáltatás konfigurációs fájl ujjlenyomat értékét a ujjlenyomat a tanúsítvány töltenek fel a felhőbeli szolgáltatástól:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Frissítse a következő beállítás értékét a azonos ujjlenyomat:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Ügyfél-tanúsítványokat

Minden egyes férhetnek hozzá a szolgáltatás kell rendelkeznie a egy ügyfél tanúsítvánnyal-kizáró his/hers használja, és his/hers saját erős jelszóval védheti a titkos kulcs kell választania. 

Amennyiben a önaláírt hitelesítésszolgáltató tanúsítványát létrehozott és tárolt ugyanarra a gépre a végre kell hajtani az alábbi lépéseket:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Testreszabás:

* az ezzel a tanúsítvánnyal hitelesíteni kell a ügyfél-azonosító -n
* az e - tanúsítvány lejárati dátummal
* MyID.pvk és MyID.cer az egyedi fájlnevek a ügyfél tanúsítvány

Ez a parancs kérne lehet létrehozni és majd még egyszer használt jelszót. Erős jelszót használhat.

## <a name="create-pfx-files-for-client-certificates"></a>Tanúsítványok az ügyfél PFX fájlok létrehozása

Minden egyes létrehozott ügyfél tanúsítvány végrehajtása:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Testreszabás:

    MyID.pvk and MyID.cer with the filename for the client certificate

Írja be a jelszót, és majd exportálja a tanúsítvány ezeket a beállításokat:

* Igen, a titkos kulcs exportálását
* Minden további tulajdonság exportálása
* Az egyes, akinek a tanúsítvány ki kell választania a Exportálás jelszó

## <a name="import-client-certificate"></a>Ügyfél-tanúsítvány importálása

Minden egyes, akinek egy ügyfél kibocsátott igazolás importálnia kell a főbb átlagostól való a gép az általa használatával kommunikál a szolgáltatást fogja használni:

* Kattintson duplán a. A Windows Intézőben PFX-fájl
* Tanúsítvány importálása be személyes tárolni legalább ezt a beállítást:
    * Minden további tulajdonság jelölőnégyzet szerepeltetése

## <a name="copy-client-certificate-thumbprints"></a>Ügyfél tanúsítvány thumbprints másolása
Minden egyes, akinek egy ügyfél kibocsátott igazolás az alábbi lépésekkel annak érdekében, hogy az ujjlenyomatot his/hers a tanúsítvány, amely a szolgáltatás konfigurációs fájl hozzáadódik:
* Certmgr.exe futtatása
* Jelölje be a személyes lap
* Kattintson duplán az ügyfél tanúsítványának hitelesítéshez
* A megnyíló tanúsítvány párbeszédpanelen jelölje be a részletek lapon
* Ellenőrizze, hogy a megjelenítése, amelyen az összes
* Jelölje ki a mezőt ujjlenyomat nevű a listában
* Minden szóközt az ujjlenyomatot **előtti utolsó karaktere után nem látható Unicode-karakterek törlése** törlés értékének másolása

## <a name="configure-allowed-clients-in-the-service-configuration-file"></a>A szolgáltatás konfigurációs fájl engedélyezett ügyfélprogramok beállítása

Frissítse a szolgáltatás konfigurációs fájl a következő beállítás értékét a thumbprints férhet hozzá a szolgáltatás ügyfél tanúsítványok vesszővel tagolt listáját:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Állítsa be az ügyfél visszavonási állapotának ellenőrzése

Az alapértelmezett beállítás nem kérdezze meg a hitelesítésszolgáltató ügyfél tanúsítvány-visszavonási állapotát. Bekapcsolásához ellenőrzést, ha az ügyfél-tanúsítványok kiadó hitelesítésszolgáltató támogatja az ilyen ellenőrzések módosítása a következő beállítást a X509RevocationMode felsorolás megadott értékek egyikét:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>PFX fájl titkosítási önaláírt tanúsítvány létrehozása

Egy titkosítási tanúsítvány végrehajtása:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Testreszabás:

    MyID.pvk and MyID.cer with the filename for the encryption certificate

Írja be a jelszót, és majd exportálja a tanúsítvány ezeket a beállításokat:
*    Igen, a titkos kulcs exportálását
*    Minden további tulajdonság exportálása
*    A tanúsítvány a felhőbeli szolgáltatástól feltöltésekor szüksége lesz a jelszót.

## <a name="export-encryption-certificate-from-certificate-store"></a>Titkosítási tanúsítvány exportálása tanúsítvány áruházból

*    Keresse meg a tanúsítvány
*    Kattintson a Műveletek -> az összes tevékenység -> Exportálás...
*    A tanúsítvány exportálása egy. Az alábbi lehetőségek PFX-fájl: 
  *    Igen, a titkos kulcs exportálását
  *    Az összes tanúsítványok szerepeltetni a hitelesítő elérési út 
*    Minden további tulajdonság exportálása

## <a name="upload-encryption-certificate-to-cloud-service"></a>Töltse fel a titkosítási tanúsítvány a felhőbeli szolgáltatástól

Feltöltés és a meglévő tanúsítványt, vagy hozza létre. A titkosítási kulcs pár PFX fájlt:

* Írja be a jelszót, a saját kulcs adatainak védelme

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Titkosítási tanúsítvány szolgáltatás konfigurációs fájl módosítása

A szolgáltatás konfigurációs fájl az alábbi beállítások ujjlenyomat értékének frissítse az ujjlenyomatot a tanúsítvány töltenek fel a felhőbeli szolgáltatástól:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Gyakori műveletek

* Az SSL-tanúsítvány beállítása
* Ügyfél-tanúsítványok beállítása

## <a name="find-certificate"></a>Keresse meg a tanúsítvány

Kövesse az alábbi lépéseket:

1. Futtassa a mmc.exe.
2. Fájl -> beépülő modul hozzáadása és eltávolítása...
3. Jelölje be a **tanúsítványok**.
4. Kattintson a **hozzáadása**gombra.
5. Tanúsítvány áruházból helyének kiválasztása.
6. Kattintson a **Befejezés gombra**.
7. Kattintson az **OK gombra**.
8. Bontsa ki a **tanúsítványok**.
9. Bontsa ki a tanúsítvány áruházból csomópontot.
10. Bontsa ki a tanúsítvány gyermek csomópontot.
11. Jelöljön ki egy tanúsítványt a listában.

## <a name="export-certificate"></a>Tanúsítvány exportálása
A **tanúsítvány exportálás varázsló**:

1. Kattintson a **Tovább**gombra.
2. Válassza az **Igen gombra**, majd **a titkos kulcs exportálását**.
3. Kattintson a **Tovább**gombra.
4. Jelölje ki a kívánt kimeneti formátumban.
5. Jelölje be a kívánt beállításokat.
6. Jelölje be a **jelszót**.
7. Írjon be egy erős jelszót, és erősítse meg.
8. Kattintson a **Tovább**gombra.
9. Írja be vagy tallózással keresse meg a fájl nevét a tanúsítvány helyének (használata egy. PFX kiterjesztésű).
10. Kattintson a **Tovább**gombra.
11. Kattintson a **Befejezés gombra**.
12. Kattintson az **OK gombra**.

## <a name="import-certificate"></a>Tanúsítvány importálása

A tanúsítvány importálása varázsló:

1. Válassza a tár helyet.

    * Jelölje ki az **Aktuális felhasználót** , ha csak az aktuális felhasználói futó folyamatok érik el a szolgáltatás
    * Válassza a **Helyi számítógép zónában** , ha más ezen a számítógépen a folyamatok érik el a szolgáltatás
2. Kattintson a **Tovább**gombra.
3. Ha importálása fájlból, erősítse meg a fájl elérési útját.
4. Ha az importálást egy. PFX fájl:
    1.     Írja be a jelszót a védelem, a titkos kulcs
    2.     Jelölje be az importálási beállítások
5.     Jelölje ki a következő tár "Hely" tanúsítványok
6.     Kattintson a **Tallózás gombra**.
7.     Jelölje ki a kívánt tárolót.
8.     Kattintson a **Befejezés gombra**.
       
    * A megbízható legfelső szintű hitelesítésszolgáltató tároló választotta, kattintson az **Igen**gombra.
9.     Kattintson az **OK gombra** az összes párbeszédpanel a Windows.

## <a name="upload-certificate"></a>Töltse fel a tanúsítvány

Az [Azure portál](https://portal.azure.com/)

1. Jelölje ki a **Cloud Services**.
2. Jelölje ki a felhőalapú szolgáltatást.
3. A felső menüben kattintson a **tanúsítványok**gombra.
4. Az alsó sávján kattintson a **Feltöltés**elemre.
5. Jelölje ki a tanúsítvány-fájlt.
6. Ha van egy. PFX fájlt, írja be jelszavát a titkos kulcs.
7. Ha kész, másolja a tanúsítvány ujjlenyomat az új bejegyzést a listában.

## <a name="other-security-considerations"></a>Egyéb biztonsági kérdései
 
A dokumentum ismertetett SSL-beállítások titkosítsa a szolgáltatás és az ügyfelek közötti kommunikáció, a HTTPS-végpont segítségével. Ez óta hitelesítő adatokat, az adatbázis eléréséhez és a kommunikáció más potenciálisan kényes információkat tartalmazza. Ne feledje, hogy a szolgáltatás továbbra is fennáll belső állapotát, a hitelesítő adatait, beleértve a a Microsoft Azure SQL-adatbázishoz, amely a metaadat-tároló megadott a Microsoft Azure-előfizetésben lévő belső táblázatok. A következő beállítást a szolgáltatás konfigurációs fájl részeként definiált arra az adatbázisra (. CSCFG fájl): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

A titkosított ebben az adatbázisban tárolt hitelesítő adatokhoz. Azonban legjobb módszer, győződjön meg arról, hogy a szolgáltatás telepítési környezetének web- és dolgozó szerepkörök naprakészen tartani és biztonságos, mint azokat is hozzáférhet az a metaadat-alapú és az titkosítást és tárolt hitelesítő adatok visszafejtése használt tanúsítvány. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
