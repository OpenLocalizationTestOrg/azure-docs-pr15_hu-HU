<properties 
    pageTitle="Egy felhőalapú szolgáltatásba állíthatja be az SSL |} Microsoft Azure" 
    description="Útmutató: Adjon meg egy webes szerepkör HTTPS zárólap és SSL-tanúsítvány, az alkalmazás biztonságos feltöltése. Az alábbi példákban az Azure-portálra." 
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
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Az alkalmazás az Azure SSL beállítása

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-configure-ssl-certificate-portal.md)
- [Azure klasszikus portál](cloud-services-configure-ssl-certificate.md)

Secure Sockets Layer (SSL) titkosítási biztonságossá tétele az interneten keresztül küldött adatok a leggyakrabban használt metódusát. A feladat végrehajtásához közös azt ismerteti, hogy egy webes szerepkör HTTPS végpont megadása és az SSL-tanúsítvány, az alkalmazás biztonságos feltöltése.

> [AZURE.NOTE] Ebben a feladatban eljárásokat alkalmazni az Azure felhőalapú szolgáltatások; az alkalmazás-szolgáltatások lásd [a](../app-service-web/web-sites-configure-ssl-certificate.md).

A feladat végrehajtásához használja a éles üzemi; Ez a témakör végén hiányzik egy átmeneti tárolásra szolgáló telepítési használatára vonatkozó információk.

Olvassa el [a](cloud-services-how-to-create-deploy-portal.md) először Ha nincs még létrehozott egy felhőalapú szolgáltatásba.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a>Lépés: 1: SSL-tanúsítvány beszerzése

SSL-alkalmazás konfigurálásához először kell SSL-tanúsítvány, amelyet aláírtak által egy tanúsítványt hitelesítésszolgáltató, akik tanúsítványokat erre a célra megbízható harmadik fél első. Ha nem már rendelkezik egy, meg kell szerezzen be egy olyan céget, az SSL-tanúsítványok eladja.

A tanúsítvány kell az alábbi követelményeknek az SSL-tanúsítványok Azure-ban:

-   A tanúsítvány egy titkos kulcs kell tartalmaznia.
-   A tanúsítvány kulcs exchange, a személyes információkat Exchange (.pfx) fájlba exportálható létre kell hozni.
-   A tanúsítvány tulajdonosneve egyeznie kell a felhőbeli szolgáltatástól eléréséhez használt tartomány. SSL-tanúsítvány nem szerzi cloudapp.net tartományhoz tartozó hitelesítésszolgáltató (CA). Meg kell beszereznie egyéni tartománynevet szeretne használni, ha a szolgáltatáshoz. Ha egy tanúsítványt kérése Hitelesítésszolgáltatótól a tanúsítvány tulajdonosneve egyeznie kell az egyéni tartománynevet, amellyel elérhető az alkalmazás. Például ha az egyéni tartománynevet a **contoso.com** meg kellene kérése tanúsítvány a Hitelesítésszolgáltatótól az * **. contoso.com** vagy * *www.contoso.com**.
-   A tanúsítvány 2048 bites titkosítás legalább kell használnia.

Teszt célokra hozhat [létre](cloud-services-certs-create.md) és önaláírt tanúsítványt használni. Önaláírt tanúsítvány nem lehet hitelesíteni hitelesítésszolgáltató keresztül, és a cloudapp.net tartomány használata a webhely URL-CÍMÉT. Például a tevékenység az alábbi használ, amelyben a közös (CN) használja a tanúsítvány neve **sslexample.cloudapp.net**önaláírt tanúsítványt.

Ezután szerepelnie kell a tanúsítvány adatait a definiált szolgáltatás és a szolgáltatás konfigurációs fájlokat.

<a name="modify"> </a>
## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Lépés: 2: A szolgáltatás meghatározása és konfigurációs fájl módosítása

Az alkalmazás kell beállítania, hogy a tanúsítványt használja, és HTTPS-végpont hozzá kell adnia. Emiatt a definiált szolgáltatás és szolgáltatás konfigurációs fájl kell frissülnek.

1.  A fejlesztői környezet nyissa meg a szolgáltatás-definíciós fájl (CSDEF), a **WebRole** szakaszok **tanúsítványok** szakasz hozzáadása és a tanúsítvány (és köztes) az alábbi információkat tartalmazzák:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    A **tanúsítványok** szakaszban határozza meg a nevét a tanúsítvány, helyéről, és hol a tár nevére.
    
    Engedélyek (`permisionLevel` attribútum) is beállítható az alábbiak egyikét:

  	| Jogosultsági érték  | Leírás |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Alapértelmezett)** A titkos kulcs összes szerepkör folyamat érheti el. |
  	| jogú          | A titkos kulcs csak jogú folyamatok érheti el.|

2.  A szolgáltatás-definíciós fájl a **Végpontok** szakaszok ahhoz, hogy HTTPS **InputEndpoint** elem hozzáadása:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  A szolgáltatás-definíciós fájl adjon hozzá egy **kötelező** elemet a **webhelyek** szakaszon belül. Ez a lehetőség egy HTTPS-végpont hozzárendelése a webhely kötés:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Az összes szolgáltatás-definíciós fájlt a szükséges módosításokat befejeződött, de van szüksége a tanúsítvánnyal kapcsolatos információk hozzáadása a szolgáltatás konfigurációs fájl.

4.  A szolgáltatás konfigurációs fájl (CSCFG), ServiceConfiguration.Cloud.cscfg, a **szerepkör** csoportban a minta ujjlenyomat értékének alább látható módon annak a tanúsítvány lecserélése belül **tanúsítványok** szakasz hozzáadása:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(A fenti példában **sha1** ujjlenyomat algoritmus. Adja meg a megfelelő értéket a tanúsítvány ujjlenyomat algoritmus.)

Most, hogy a szolgáltatás definition és szolgáltatás konfigurációs fájl frissített csomagot, a feltölthető Azure környezetben. **Cspack**használatakor nem felhasználhatja a **/generateConfigurationFile** jelző, amely felülírja az imént beszúrt tanúsítvány adatait.

## <a name="step-3-upload-a-certificate"></a>Lépés 3: Tanúsítvány feltöltése

A portál csatlakoztatása és...

1. A portálon válassza ki a felhőalapú szolgáltatásba, és válassza a **Felhőbeli szolgáltatástól**. (Ez az **összes erőforrás** szakaszában.) 
    
    ![A felhőalapú szolgáltatás közzététele](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. Kattintson a **tanúsítványok**gombra.

    ![Kattintson a tanúsítványok ikonra](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. Adja meg a **fájlt**, a **jelszót**, majd kattintson a **Feltöltés**elemre.

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Lépés: 4: A szerepkör-példány csatlakozás HTTPS használatával

Most, hogy a telepítési lépéseket az Azure-ban, csatlakozhat HTTPS használatával.
    
1.  Kattintson a gombra kattintva nyissa meg a böngészőben a **Webhely URL-CÍMÉT** .

    ![Kattintson a webhely URL-címe](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2.  A webböngészőben módosíthatja a csatolást, hogy **http**helyett **https** használja, és keresse meg a lapot.

    >[AZURE.NOTE] Alkalmazás használatakor, önaláírt tanúsítvány böngészés HTTPS-végpont, amely az önaláírt tanúsítványt, a böngészőben a tanúsítvány hiba jelenhet meg van társítva. Egy megbízható hitelesítésszolgáltató aláíró tanúsítvány használatával megszünteti a ezt a problémát. időközben figyelmen kívül hagyhatja a hibát. (Egy másik, hogy az önaláírt tanúsítványt hozzáadása a felhasználó megbízható hitelesítésszolgáltató tanúsítvány tárolóhoz.)

    ![Webhely megtekintése](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

    >[AZURE.TIP] Ha szeretne egy éles üzemi helyett átmeneti tárolásra szolgáló telepítés SSL protokoll használatára, először kell határozza meg az URL-címet az átmeneti tárolásra szolgáló telepítéshez használni. Miután lett telepítve a felhőalapú szolgáltatás, az URL-CÍMÉT a fejlesztői környezet határozza meg a **Telepítési azonosító** globálisan egyedi azonosítója ebben a formátumban:`https://deployment-id.cloudapp.net/`  
      
    >Tanúsítvány létrehozása (CN) közös nevű egyenlő globálisan egyedi azonosítója-alapú URL-CÍMÉT (például a **328187776e774ceda8fc57609d404462.cloudapp.net**). A portálon használja a tanúsítvány hozzáadása a szakaszos felhőszolgáltatásba. Ezután vegyen fel a tanúsítvánnyal kapcsolatos információk a CSDEF és CSCFG fájlok, az alkalmazás újracsomagolására és frissítése az új csomag a szakaszos üzembe.

## <a name="next-steps"></a>Következő lépések

* [A felhőalapú szolgáltatás általános konfigurálása](cloud-services-how-to-configure-portal.md).
* Megtudhatja, hogyan [egy felhőalapú szolgáltatás üzembe](cloud-services-how-to-create-deploy-portal.md).
* Állítsa be [egyéni tartománynevet](cloud-services-custom-domain-name-portal.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).
