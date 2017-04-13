<properties 
   pageTitle="A webhely virtuális Magánhálózati átjáró kapcsolatok Azure virtuális hálózatok VPN eszközök |} Microsoft Azure"
   description="Ez a cikk azt ismerteti, hogy a virtuális Magánhálózati eszközök és a paraméterek IPsec S2S virtuális Magánhálózati átjáró kapcsolatokhoz és konfigurációs utasításokat és a minták mutató hivatkozásokat tartalmaz."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Tudnivalók a webhely virtuális Magánhálózati átjáró kapcsolatok virtuális Magánhálózati eszközök

Állítsa be a webhely (S2S) virtuális Magánhálózati kapcsolat VPN-eszközhöz szükséges. Hely közötti kapcsolatok hibrid megoldás létrehozása, fel, illetve jelenik meg, ha azt szeretné, a biztonságos kapcsolat a helyszíni és a virtuális hálózat között. Ez a cikk azt ismerteti, hogy kompatibilis virtuális Magánhálózati eszközök és konfigurációs paramétereket. 

>[AZURE.NOTE] Hely közötti kapcsolat beállításakor a virtuális Magánhálózati eszköz szükség egy nyilvános IPv4 IP-címet.                                                                                                                                                                               

Ha az eszköz nem jelenik meg a [érvényesített virtuális Magánhálózati eszközök](#devicetable) táblázatban, olvassa el a a Ez a cikk [nem érvényesített virtuális Magánhálózati eszközök](#additionaldevices) szakaszát. Akkor lehet, hogy az eszköz továbbra is működnek az Azure. Virtuális Magánhálózati eszköztámogatás kérjük, forduljon a számítógépgyártó.

**Megjegyzés: a táblázatok megtekintésekor elemek:**

- Statikus és dinamikus útválasztás terminológia változás történt. Valószínűleg be mindkét kifejezés futtatásával. Nem használható funkciók körét változnak, csak a neveket változnak.
    - Statikus útválasztás = PolicyBased
    - A dinamikus útválasztás = RouteBased
- Nagy teljesítmény virtuális Magánhálózati átjáró és RouteBased virtuális Magánhálózati átjáró jellemzői megegyeznek példáiban. Ha például RouteBased VPN átjárók alkalmazással kompatibilis érvényesített VPN eszközök kompatibilisek is az Azure nagy teljesítmény VPN átjáró. 


## <a name="devicetable"></a>Érvényesített virtuális Magánhálózati eszközök 

Eszköz szállítókkal azt van érvényesíteni partnerség szabványos virtuális Magánhálózati eszközök csoportja. Az eszköz családok, az alábbi listában szereplő összes eszközről Azure virtuális Magánhálózati átjárók együtt kell működniük. Lásd: a [Virtuális Magánhálózati átjáró](vpn-gateway-about-vpngateways.md) ellenőrizze az átjáró, létre kell hoznia a megoldáshoz be szeretné állítani a típusát. 

Szeretné beállítani a virtuális Magánhálózati eszközt, olvassa el a hivatkozásra, hogy a megfelelő eszköz család megfelelnek. Virtuális Magánhálózati eszköztámogatás kérjük, forduljon a számítógépgyártó.



| **Szállítói**                      | **Eszköz család**                                        | **Minimális operációs rendszer verziója**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Rokon Telesis                  | AR sorozat VPN útválasztó                                    | 2.9.2                                              | hamarosan                                                                                                                                                                                                                                          | Nem kompatibilis                                                                                                                                                                                               |
| Barracuda hálózatok, Inc.        | Az F-sorozat barracuda NextGen tűzfal             | PolicyBased: 5.4.3, RouteBased: 6.2.0  | [Konfigurációs utasításokat](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Konfigurációs utasításokat](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda hálózatok, Inc.        |  Barracuda NextGen tűzfal X-sorozat                 | Barracuda tűzfal 6.5 rendszeren | [Barracuda tűzfal](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Nem kompatibilis                                                                                                                                                                                               |
| Brokát                         | Vyatta 5400 vRouter                                      | Virtuális útválasztó 6.6R3 kiadás                            | [Konfigurációs utasításokat](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Nem kompatibilis                                                                                                                                                                                               |
| Jelölőnégyzet pont                     | Biztonsági átjáró                                         | R75.40, R75.40VS                                     | [Konfigurációs utasításokat](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Konfigurációs utasításokat](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Cisco minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Nem kompatibilis                                                                                                                                                                                               |
| Cisco                           | AUTOMATIKUS RENDSZER-HELYREÁLLÍTÁS                                                      | IOS 15.1 (PolicyBased), az IOS 15.2 (RouteBased)                | [Cisco minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Cisco minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (PolicyBased), az IOS 15.1 (RouteBased *)               | [Cisco minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco minták *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX, SDX, VPX      |10.1 és a fenti                                           | [Integráció utasításokat.](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Nem kompatibilis                                                                                                                                                                                               |
| Dell SonicWALL                  | Nemzeti sorozat, SuperMassive sorozat, az E-osztály nemzeti sorozat TZ adatsorokban | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Utasítások - SonicOS 6.2.](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Utasítások - SonicOS 5.9.](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Utasítások - SonicOS 6.2.](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Utasítások - SonicOS 5.9.](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| AZ F5                              | Az IP-nagy sorozat                                 |           12.0                                            | [Konfigurációs utasításokat](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Konfigurációs utasításokat](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Konfigurációs utasításokat](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Konfigurációs utasításokat](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internetes kezdeményezésére japán (IIJ) | SEIL sorozat                                              | SEIL / 4.60, SEIL/B1 4.60, SEIL/x86 3.20 X            | [Konfigurációs utasításokat](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Nem kompatibilis                                                                                                                                                                                               |
| Boróka                         | SRX-LEÍRÁSBAN                                                      | JunOS 10.2 (PolicyBased), JunOS 11,4 (RouteBased)            | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Boróka                         | J-sorozat                                                 | JunOS 10.4r9 (PolicyBased), JunOS 11,4 (RouteBased)          | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Boróka                         | ISG                                                      | ScreenOS 6.3 (PolicyBased és RouteBased)                  | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Boróka                         | SSG                                                      | ScreenOS 6.2 (PolicyBased és RouteBased)                  | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Boróka minták](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| A Microsoft                       | Útválasztás és távoli szolgáltatás                        | A Windows Server 2012                                | Nem kompatibilis                                                                                                                                                                                                                                       | [Microsoft minták](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Nyissa meg a rendszerek AG | Küldetés vezérlő biztonsági átjáró | A #HIÁNYZIK | [Telepítési útmutatója](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Telepítési útmutatója](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Hamarosan)                                                                                                                                                                                                                                        | Nem kompatibilis                                                                                                                                                                                               |
| Palo Alto hálózatok              | Összes eszközön, operációs PÁSZTÁZÁS     | 6.1.5 PÁSZTÁZÁS-OS vagy újabb verzióját (PolicyBased), 7.0.5 PÁSZTÁZÁS-OS vagy újabb verzióját (RouteBased)       | [Konfigurációs utasításokat]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Konfigurációs utasításokat](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| WatchGuard                      | Az összes                                                      | Fireware XTM v11.x                                 | [Konfigurációs utasításokat](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Nem kompatibilis                                                                                                                                                                                               |

(*) Csak a ISR 7200 sorozat útválasztó támogatott PolicyBased VPN adatai.

## <a name="additionaldevices"></a>Virtuális Magánhálózati eszközök nem érvényesítése

Az eszköz érvényesített VPN eszközök táblázatban nem látható, ha még mindig előfordulhat, hogy működik a webhely kapcsolatot. Győződjön meg arról, hogy a virtuális Magánhálózati eszköz megfelel a minimális az átjáró vonatkozó követelményei című témakört a [Virtuális Magánhálózati átjárók](vpn-gateway-about-vpngateways.md#gateway-requirements) tagolt. Megfelel a minimális eszközöket is együtt kell működniük jól VPN átjárók. A számítógépgyártó forduljon a támogatási és konfigurációs további tudnivalókat.


## <a name="editing-device-configuration-samples"></a>Eszköz konfigurációs minta szerkesztése

Miután letöltötte a megadott VPN eszköz konfigurációs minta, kell néhány a környezet beállításainak megfelelően az értékek lecserélése. 

**Minta szerkesztése:**

1. Nyissa meg a minta Jegyzettömbbel. 
1. Keressen, és cserélje ki az összes <*szöveg*> szöveg az abban a környezetben. Ügyeljen arra, amelyet fel szeretne venni < és >. Ha egy mappanevet, a nevet, akkor jelölje be egyedinek kell lennie. A parancs nem működik, ha az eszköz dokumentációjában gyártó.

| **Mintaszöveget**                | **Módosítása**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Az objektum a választott nevét. Példa: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Az objektum a választott nevét. Példa: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Az objektum a választott nevét. Példa: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Az objektum a választott nevét. Példa: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Az objektum a választott nevét. Példa: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Adja meg a tartomány. Példa: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Adja meg a alhálózat maszk. Példa: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Adja meg, hogy a helyszíni tartomány. Példa: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Adja meg, hogy a helyszíni alhálózat maszk. Példa: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Ezek az információk bizonyos a virtuális hálózathoz és **Gateway IP-**cím az adatkezelési portálon található. |
| &lt;SP_PresharedKey&gt;                | Ez az információ szerinti virtuális hálózatához, és az kulcs kezelése az adatkezelési portál található.          |



## <a name="ipsec-parameters"></a>IPsec paraméterei

>[AZURE.NOTE] Az alábbi táblázatban szereplő értékeket az Azure virtuális Magánhálózati átjáró által támogatott, de jelenleg nincs lehetőség, amellyel megadhatja, vagy jelöljön ki egy adott kombinációt az Azure virtuális Magánhálózati átjáró. Meg kell adnia a helyszíni VPN eszközről korlátozó. Ezeken kívül MSS kell kapocs 1350 elemre.

### <a name="ike-phase-1-setup"></a>IKE fázis 1 beállítása

| **A tulajdonság**                                       | **PolicyBased** | **RouteBased és a normál vagy a nagy teljesítmény VPN átjáró** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE verziója                                        | IKEv1                          | IKEv2                                                            |
| Kulcsalap csoport                               | Csoport 2 (1024 bit)             | Csoport 2 (1024 bit)                                               |
| Hitelesítési módszer                              | Előre megosztott kulcs                 | Előre megosztott kulcs                                                   |
| Titkosítási algoritmust                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| A kivonatolás algoritmus                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Fázis 1 biztonsági társítás élettartama (idő) | 28,800 másodpercig                 | 10,800 másodpercig                                                   |


### <a name="ike-phase-2-setup"></a>IKE fázis 2 beállítása

| **A tulajdonság**                                                             | **PolicyBased**                 | **RouteBased és a normál vagy a nagy teljesítmény VPN átjáró**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE verziója                                                              | IKEv1                                          | IKEv2                                                              |
| A kivonatolás algoritmus                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| 2 fázis biztonsági társítás élettartama (idő)                        | 3600 másodpercet                                  | 3600 másodpercet                                                                  |
| 2 fázis biztonsági társítás élettartama (sebesség)                  | 102,400,000 KB                                 | -                                                                  |
| IPsec Nyelvű titkosítás és a hitelesítési ajánlatok (elsőbbségi) | 1. ESP-AES256 2. ESP AES128 3. ESP 3DES 4. A #HIÁNYZIK | Lásd: a *RouteBased átjáró IPsec biztonsági társítás kínál* (lásd alább) |
| Tökéletes előre titoktartási (beállítás)                                            | nem                                             | Nincs (*)|
| Nem Peer kimutatására                                                      | Nem támogatott                                  | Támogatott                                                          |

(*) Azure átjáró IKE válaszoló, ideiglenesen elfogadhatja beállítás DH csoport 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>RouteBased átjáró IPsec biztonsági társítás ajánlatok

Az alábbi táblázat IPsec Nyelvű titkosítást és a hitelesítési kínál. Ajánlatok jelennek meg a sorrendben, hogy az ajánlat mutatják be, vagy elfogadott.

| **IPsec Nyelvű titkosítást és a hitelesítési ajánlatok** | **Azure átjáró kezdeményező szerint**                               | **Azure átjáró válaszadók szerint**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | A ESP AES_128 a null HMAC AH SHA1                      |
| 5                                                 | A ESP AES_256 a null HMAC AH SHA1                      | A null HMAC a ESP 3_DES AH SHA1                        |
| 6                                                 | A ESP AES_128 a null HMAC AH SHA1                      | A null HMAC, nem javasolt élettartama a ESP 3_DES AH MD5 |
| 7                                                 | A null HMAC a ESP 3_DES AH SHA1                        | A ESP 3_DES SHA1, nincs élettartama AH SHA1                    |
| 8                                                 | A null HMAC, nem javasolt élettartama a ESP 3_DES AH MD5 | ESP 3_DES MD5, nincs élettartama AH MD5                     |
| 9                                                 | A ESP 3_DES SHA1, nincs élettartama AH SHA1                    | ESP DES MD5                                                  |
| 10                                                | ESP 3_DES MD5, nincs élettartama AH MD5                     | ESP DES SHA1, nincs élettartama                                   |
| 11                                                | ESP DES MD5                                                  | Ahogy LÁTHATJA a null HMAC ESP DES, SHA1 nincs élettartama javasolt        |
| 12                                                | ESP DES SHA1, nincs élettartama                                   | Ahogy LÁTHATJA a null HMAC ESP DES, MD5 nincs élettartama javasolt        |
| 13-mal                                                | Ahogy LÁTHATJA a null HMAC ESP DES, SHA1 nincs élettartama javasolt        | A ESP DES SHA1, nincs élettartama AH SHA1                      |
| 14                                                | Ahogy LÁTHATJA a null HMAC ESP DES, MD5 nincs élettartama javasolt        | ESP DES MD5, nincs élettartama AH MD5                       |
| 15                                                | A ESP DES SHA1, nincs élettartama AH SHA1                      | ESP SHA, nincs élettartama                                        |
| 16                                                | ESP DES MD5, nincs élettartama AH MD5                       | ESP MD5, nincs élettartama                                        |
| 17                                                | -                                                            | Ahogy LÁTHATJA SHA, nincs élettartama                                         |
| 18                                                | -                                                            | AH MD5, nincs élettartama                                        |


- A RouteBased és nagy teljesítmény VPN átjárók IPsec ESP NULL titkosítási is megadhat. NULL alapján titkosítás nem nyújt védelmet a hálózaton átvitt adatok, és csak ha maximális használható átviteli és minimális időtartama szükség.  Ügyfelek dönthet: Ezzel a VNet-VNet kommunikációs helyzetekben, vagy ha titkosítási máshol van alkalmazva a megoldás.

- Idegen helyszíni connectivity az interneten keresztül használja az alapértelmezett Azure virtuális Magánhálózati átjáró beállításait a titkosítási és a kritikus kommunikációs biztonsága érdekében a táblázatokban a fent felsorolt ujjlenyomat algoritmusok.





