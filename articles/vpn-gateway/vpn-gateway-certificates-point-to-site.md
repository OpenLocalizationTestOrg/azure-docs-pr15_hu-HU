<properties 
   pageTitle="Önaláírt tanúsítványok létrehozása, az pont-webhely virtuális hálózati határokon helyszíni kapcsolatok makecert használatával |} Microsoft Azure"
   description="Ez a cikk a lépéseket követve segítségével makecert önaláírt tanúsítványok a Windows 10-es tartalmazza."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="cherylmc" />

# <a name="working-with-self-signed-certificates-for-point-to-site-connections"></a>Önaláírt tanúsítványok kapcsolatokhoz pont-webhely használata

Ez a cikk segít **makecert**használatával önaláírt tanúsítvány létrehozása, valamint majd ügyfél tanúsítványok készítése belőlük. A lépéseket a Windows 10 rendszer írták makecert. MakeCert érvényesítése megtörtént tanúsítványok, amelyek kompatibilis P2S kapcsolatok létrehozásához. 

P2S kapcsolatok esetén az előnyben részesített tanúsítványok módja vállalati tanúsítvány megoldás, ügyelve arra, hogy az ügyfél tanúsítványokat a közös neve érték formátumban, hogy használni 'name@yourdomain.com', helyett a "NetBIOS tartománynév\felhasználónév" formátum.

Ha nem egy vállalati megoldás, önaláírt tanúsítvány P2S ügyfelek egy virtuális hálózathoz csatlakozik szükség. Azt is érdemes szem előtt, hogy makecert elavult, de továbbra is érvényes metódussal P2S kapcsolatok alkalmazással kompatibilis önaláírt tanúsítvány létrehozása. Kattintson egy másik megoldást hozhat létre önaláírt tanúsítványok dolgozunk a probléma, de ekkor makecert a használni kívánt módot.

## <a name="create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása

MakeCert önaláírt tanúsítvány létrehozása egy lehetőség. Az alábbi lépésekkel végigvezetjük makecert használatával önaláírt tanúsítvány létrehozása. Ezeket a lépéseket nem telepítési-modell specifikus állnak. Erőforrás-kezelő és a klasszikus érvényesek.

### <a name="to-create-a-self-signed-certificate"></a>Önaláírt tanúsítvány létrehozása

1. A Windows 10 rendszerű számítógépről töltse le és telepítse a [Windows szoftver fejlesztési csomag (SDK) a Windows 10](https://dev.windows.com/en-us/downloads/windows-10-sdk).

2. A telepítés után is megtalálhatja a makecert.exe segédprogram csoportban az elérési út: C:\Program Files (x86) \Windows Kits\10\bin\<arch >. 
        
    Példa:`C:\Program Files (x86)\Windows Kits\10\bin\x64`

3. Ezután hozzon létre, és a tanúsítvány telepítése a számítógépen a személyes tanúsítvány tárolóban. Az alábbi példa létrehoz egy megfelelő *.cer* fájl Azure P2S konfigurálásakor feltöltése. A következő parancs futtatása rendszergazdaként. Cserélje ki a nevet, amelyet a tanúsítvány használni kívánt *ARMP2SRootCert* és *ARMP2SRootCert.cer* .<br><br>A tanúsítvány fog kerülni a tanúsítványok – aktuális User\Personal\Certificates.

        makecert -sky exchange -r -n "CN=ARMP2SRootCert" -pe -a sha1 -len 2048 -ss My "ARMP2SRootCert.cer"


###  <a name="rootpublickey"></a>A nyilvános kulcs beszerzése

A virtuális Magánhálózati átjáró konfiguráció pont a webhely-kapcsolat részét képező nyilvános kulcsa a legfelső szintű tanúsítvány feltöltött Azure.

1. A tanúsítvány .cer fájl beszerzéséhez nyissa meg a **certmgr.msc parancsot**. Kattintson a jobb gombbal a legfelső önaláírt tanúsítványt, válassza a **Minden tevékenység**, és kattintson az **Exportálás**parancsra. Ekkor megnyílik az **Exportálás varázsló tanúsítvány**.

2. A varázslóban kattintson a **Tovább**gombra, válassza a **nem, a titkos kulcs exportálni**, és kattintson a **Tovább gombra**.

3. A **Fájlformátumban exportálhatja** lapon jelölje ki az alap-64 **kódolású X.509 (. CER).** Kattintson a **Tovább gombra**. 

4. Kattintson az **exportálandó fájl**, **tallózással keresse meg** a tanúsítvány exportálása pontjára. A **fájl nevét**a tanúsítvány fájl nevét. Kattintson a **Tovább gombra**.

5. A tanúsítvány exportálása a **Befejezés** gombra.

 
### <a name="export-the-self-signed-certificate-optional"></a>Exportálja az önaláírt tanúsítványt (nem kötelező)

Érdemes exportálja az önaláírt tanúsítványt, és nyugodtan tárolja. Ha szükséges, később egy másik számítógépen telepíteni és több ügyfél-tanúsítvány létrehozása vagy egy másik .cer fájl exportálása. Ügyfél-tanúsítvány telepítve van, és hogy rendelkező számítógépről is van beállítva a megfelelő VPN-ügyfél beállításai csatlakoztathatja virtuális hálózaton keresztül P2S. Éppen ezért érdemes győződjön meg arról, hogy ügyfél tanúsítványok létrehozott, csak akkor, amikor szüksége van telepítve, és az önaláírt tanúsítványt biztonságosan található.

Exportálja az önaláírt tanúsítványt, egy .pfx, jelölje ki a legfelső szintű tanúsítvány, és használja ezeket a lépéseket [ügyfél tanúsítvány exportálása](#clientkey) ismertetett módon exportálása.

## <a name="create-and-install-client-certificates"></a>Hozzon létre és telepítsen ügyfél tanúsítványok

Közvetlenül az ügyfélszámítógépen nem telepítette az önaláírt tanúsítványt. Ügyfél-tanúsítványt az önaláírt tanúsítvány létrehozása szükség. Ezután exportálása és az ügyfél tanúsítványának telepítése az ügyfélszámítógép. Az alábbi lépésekkel sem telepítési-modell adott. Erőforrás-kezelő és a klasszikus érvényesek.

### <a name="part-1---generate-a-client-certificate-from-a-self-signed-certificate"></a>1 rész - ügyfél-tanúsítvány önaláírt tanúsítvány létrehozása

Az alábbi lépésekkel végigvezetjük lehet önaláírt tanúsítvány létrehozása ügyfél-tanúsítványt. Előfordulhat, hogy készítése a több ügyfél-tanúsítványt az ugyanazt a tanúsítványt. Minden egyes ügyfél-tanúsítvány majd exportált és az ügyfélszámítógépen telepítve. 

1. Ugyanazon a számítógépen, amellyel az önaláírt tanúsítvány létrehozása nyisson meg egy parancssorablakot rendszergazdaként.

2. Ebben a példában az önaláírt tanúsítványt létrehozott "ARMP2SRootCert" hivatkozik. 
    - Az ügyfél tanúsítványát a generál önaláírt legfelső szintű neve *"ARMP2SRootCert"* módosítása 
    - Módosítsa a *ClientCertificateName* szeretne előállítani kell ügyfél-tanúsítvány nevével. 


    Futtassa a minta ügyfél-tanúsítvány létrehozása és módosítása. Ha a következő példa azt módosítása nélkül, a eredménye a legfelső szintű tanúsítvány ARMP2SRootCert létrehozott személyes tanúsítvány áruház ClientCertificateName nevű ügyfél-tanúsítványt.

        makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "ARMP2SRootCert" -is my -a sha1

4. Az összes tanúsítványok vannak tárolva a "tanúsítványok – aktuális User\Personal\Certificates" tárolni a számítógépen. Hozhat létre, ez az eljárás alapján sok ügyfél tanúsítványok szükség szerint.

### <a name="clientkey"></a>Kijelző 2 – exportálás ügyfél-tanúsítvány

1. Ügyfél-tanúsítvány exportálni, nyissa meg a **certmgr.msc parancsot**. Kattintson a jobb gombbal az ügyfél tanúsítvány, amelyet exportálni, válassza a **Minden tevékenység**és majd az **Exportálás**gombra. Ekkor megnyílik az **Exportálás varázsló tanúsítvány**.

2. A varázslóban kattintson a **Tovább gombra**, majd válassza az **Igen, a titkos kulcs exportálását**, és kattintson a **Tovább gombra**.

3. Az **Exportálás fájlformátum** lapon hagyhatja, a kijelölt alapértelmezett. Kattintson a **Tovább gombra**. 
 
4. A **Biztonság** lapon a titkos kulcs kell védelme. Ha jelszóval, ügyeljen arra, hogy ne feledje, hogy a jelszót, amelyet meg ezt a tanúsítványt és rögzítése ki. Kattintson a **Tovább gombra**.

5. Kattintson az **exportálandó fájl**, **tallózással keresse meg** a tanúsítvány exportálása pontjára. A **fájl nevét**a tanúsítvány fájl nevét. Kattintson a **Tovább gombra**.

6. A tanúsítvány exportálása a **Befejezés** gombra.  

### <a name="part-3---install-a-client-certificate"></a>Ügyfél-tanúsítvány telepítés 3 - kijelző

Minden ügyfél pont a webhely-kapcsolat használatával csatlakozhat a virtuális hálózat, amelyet egy ügyfél tanúsítvánnyal kell rendelkeznie. A tanúsítvány, amely a virtuális Magánhálózati konfiguráció szükséges csomag mellett. Az alábbi lépésekkel ismerteti, hogy az ügyfél-tanúsítvány manuális telepítésével.

1. Keresse meg és a *.pfx* fájl másolása az ügyfélgépen. Az ügyfélgépen kattintson duplán a *.pfx* fájl telepítéséhez. Az **Aktuális**felhasználóként **Áruházból helyét** , majd kattintson a **Tovább**gombra.

2. A **fájl** importálása lapon, és nem bármilyen módosítást. Kattintson a **Tovább**gombra.

3. **Titkos kulcs védelme** lapon adja meg a tanúsítvány jelszavának Ha használja, vagy ellenőrizze, hogy helyesek-e a rendszerbiztonsági tag, amely a tanúsítványt telepítésére van beállítva, majd kattintson a **Tovább**gombra.

4. A **Tanúsítvány áruház** lapon az alapértelmezett helye, és kattintson a **Tovább gombra**.

5. Kattintson a **Befejezés gombra**. A **Biztonsági figyelmeztetés** a tanúsítvány telepítéshez kattintson az **Igen**gombra. A tanúsítvány ezzel sikeresen beimportálta.

## <a name="next-steps"></a>Következő lépések

Folytassa a pont-webhely beállításai. 

- **Erőforrás-kezelő** telepítési modell című témakör tartalmazza [a PowerShell használatá VNet pont a webhely-kapcsolat beállítása](vpn-gateway-howto-point-to-site-rm-ps.md). 
- **Klasszikus** telepítési modell című témakör tartalmazza [konfigurálása a pont-pont virtuális Magánhálózati kapcsolatot egy VNet a klasszikus portálon](vpn-gateway-point-to-site-create.md).