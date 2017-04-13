<properties
    pageTitle="Csatlakozás számítógépeken és eszközökön a MOBILE átjáró MOBILE |} Microsoft Azure"
    description="Csatlakoztassa a MOBILE kezelt eszközök és a számítógépen futó Operations Manager ellenőrizni a MOBILE átjáró adatokat küld a MOBILE szolgáltatás, amikor nincs internetkapcsolata."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Csatlakozás a MOBILE átjáró MOBILE számítógépeken és eszközökön

A dokumentum ismerteti, hogyan a MOBILE kezelt eszközök és a System Center műveletek Manager SCOM ellenőrizni számítógépek küldhet adatokat a MOBILE szolgáltatás amikor nincs internetkapcsolata. A MOBILE átjáró is összegyűjti az adatokat, és küldje el a MOBILE service akik a nevében.

Az átjáró, a HTTP tunneling a HTTP csatlakozás paranccsal támogató előre HTTP-proxy. Az átjáró futtatásakor a 4-core processzor, a Windows operációs rendszert futtató 16 GB-os kiszolgáló akár 2000 MOBILE egyidejűleg csatlakoztatott eszközök képes kezelni.

Példaként a vállalati vagy a nagyobb szervezetek előfordulhat, hogy a hálózati kapcsolat kiszolgálók, de nem lehet, hogy rendelkezik internetkapcsolattal. A példában egy másik lehet, hogy sok pont értékesítését (POS) eszközökön felügyeleti őket közvetlenül nem eszközökkel. És a példában egy másik Operations Manager használata a MOBILE átjáró proxykiszolgálón. Ezekben a példákban a MOBILE átjáró adatokat is át az alábbi kiszolgálókon vagy MOBILE POS eszközök telepített ügynökök.

Minden egyes ügynök küldő adat helyett közvetlenül az MOBILE, és kezelésük is közvetlen internetkapcsolat minden ügynök adat helyett küldi el egyetlen internetkapcsolattal rendelkező számítógépen keresztül. Számítógép, ahol telepítése és használata az átjáró. Ebben az esetben minden olyan számítógépen, amelyre adatokat szeretne gyűjteni ügynökök telepítheti. Az átjáró majd át adatokat az ügynökök a MOBILE közvetlenül – az átjáró nem elemezheti az átvitt adatok közül.

Figyelheti a MOBILE átjáró, és a kiszolgáló, amelyen telepítve van a teljesítmény vagy esemény elemzéséhez, telepítenie kell a MOBILE agent a számítógépen, amelyen az átjáró is telepítve van.

Az átjáró feltölteni az adatokat a MOBILE Internet-hozzáféréssel kell rendelkeznie. Minden egyes ügynök kell rendelkeznie a hálózati kapcsolat, hogy az átjáró, hogy ügynökök automatikusan adatátviteli és az átjáró onnan. A legjobb eredmény érdekében nem telepíteni az átjáró egy olyan számítógépen, amely is a tartományvezérlőnek.

Az alábbiakban a diagram, amely megjeleníti a közvetlen ügynökök adatfolyam MOBILE.

![közvetlen ügynök diagram](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Az alábbiakban a diagram, amely megjeleníti az Operations Manager adatfolyam MOBILE.

![Műveletek Manager diagram](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>A MOBILE átjáró telepítése

Az átjáró a korábbi verzióival (Log Analytics továbbító) telepítve van ez az átjáró telepítése váltja fel.

Előfeltételek: .net-keretrendszer 4.5, Windows Server 2012 R2 SP1 vagy újabb

1. Töltse le a MOBILE átjárót a legújabb verzióját a [Microsoft letöltőközpontból](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi).
2. A telepítés megkezdéséhez kattintson duplán a **MOBILE Gateway.msi**.
3. A Kezdőlap lapon **Tovább gombra**.  
    ![Átjáró beállítása varázsló](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. A licencszerződés lapon jelölje be **az elfogadom a licencszerződés** lenyomásával fogadja el a LICENCSZERZŐDÉST majd a **Tovább gombra**.
5. A port és a proxy cím lapon:
    1. Írja be a TCP-port száma az átjáró használható. A telepítő a portszámot a Windows tűzfal nyitja meg. Az alapértelmezett érték 8080.
    Az érvényes tartományon, a portszámot 1 – 65535. Ha a bemeneti nem tartozik a tartományba, hibaüzenet jelenik meg.
    2. Ha szükséges Ha a kiszolgáló, amelyen telepítve van-e az átjáró a proxykiszolgáló használata, írja be a proxycím, ahol az átjáró csatlakoznia. Ha például `http://myorgname.corp.contoso.com:80` Ha nincs megadva, az átjáró megpróbálja közvetlenül csatlakozik az internethez. Az átjáró ellenkező esetben a proxy csatlakozik. Ha a proxykiszolgáló hitelesítést igényel, írja be a felhasználónevét és jelszavát.
        ![Átjáró varázsló proxybeállítások](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Kattintson a **Tovább** gombra
6. Ha nem rendelkezik Microsoft Updates engedélyezve, a Microsoft Update lapja jelenik meg, kiválaszthatja ahhoz, hogy Microsoft Updates. Jelöljön ki egy elemet, és kattintson a **Tovább**gombra. Egyéb esetben folytassa a következő lépéssel.
7. A célmappát lapon az alapértelmezett mappa **%ProgramFiles%\OMS átjáró** hagyja, vagy írja be a hely, amelyre telepíteni az átjárót, és kattintson a **Tovább gombra**.
8. A felkészült alkalmazástelepítési oldalon kattintson a **telepítés**gombra. A felhasználói fiókok felügyelete telepítéséhez engedély kérelmezése jelenhetnek meg. Ha igen, kattintson az **Igen**gombra.
9. A telepítés után kattintson a **Befejezés gombra**. Ellenőrizheti, hogy a szolgáltatás működik, és nyissa meg a services.msc beépülő modult, és ellenőrizze, hogy a **MOBILE átjáró** megjelenik a szolgáltatások listában.  
    ![Szolgáltatások – MOBILE átjáró](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Ügynököt eszközökön

Ha szükséges, olvassa el a [napló Analytics csatlakoztatása a Windows számítógépek](log-analytics-windows-agents.md) információ arról, hogy miként telepítheti az közvetlenül csatlakozik ügynökök beállítása című témakört. A cikkből megtudhatja, hogyan telepítheti a beállítási varázsló agent vagy a parancssor használatával.

## <a name="configure-oms-agents"></a>MOBILE ügynökök beállítása

Lásd: [Microsoft figyelése ügynök proxy és a tűzfalat beállítások konfigurálása](log-analytics-proxy-firewall.md) a Ez esetben proxykiszolgáló használata ügynököt konfigurálásáról az átjárót.

Műveletek Manager ügynökök bizonyos adatok, például Operations Manager értesítések, konfigurációs értékelési, példány terület és kapacitás adatoknak a Management Server küldése. Egyéb nagy mennyiségű adatok, például az IIS naplók, a teljesítmény és a biztonsági közvetlenül a MOBILE átjáró küldött. Megoldásainak [napló Analytics hozzáadása a megoldástárból](log-analytics-add-solutions.md) minden csatornán keresztül küldött adatok teljes listáját.

>[AZURE.NOTE]
Ha az átjáró használata a terheléselosztás, olvassa el a [másik lehetőségként a hálózati terheléselosztási konfigurálása](#optionally-configure-network-load-balancing)című témakört.

## <a name="configure-a-scom-proxy-server"></a>SCOM proxykiszolgáló beállítása

Az átjáró használni kívánt a proxykiszolgáló hozzáadása Operations Manager konfigurálása Ha a proxy konfigurációja, akkor a proxy konfigurációjának automatikusan érvényesül Operations Manager jelentéshez ügynökök beállítása.

Az átjáró a támogatási Operations Manager használatához van szükség:

- A Microsoft figyelése Agent (ügynök verzió – **8.0.10900.0** és újabb verzióiban) az átjáró-kiszolgálón telepítette és beállította a MOBILE munkaterületek, amellyel kommunikálni szeretne.
- Az átjáró van internetkapcsolat, vagy a proxykiszolgáló fejlesztő csatlakoznia.

### <a name="to-configure-scom-for-the-gateway"></a>Az átjáró SCOM konfigurálása

1. Nyissa meg az Operations Manager konzolt, és a **Tevékenységek kezelése csomagja**, kattintson a **kapcsolat** , és válassza a **Proxykiszolgáló beállítása**:  
    ![Operations Manager – proxykiszolgáló beállítása](./media/log-analytics-oms-gateway/scom01.png)
2. Válassza **a műveletek Management csomagja eléréséhez a proxykiszolgáló használata** , és írja be a MOBILE átjáró kiszolgáló IP-címét. Győződjön meg róla, indítsa el a a `http://` előtag:  
    ![Operations Manager – proxykiszolgáló címe](./media/log-analytics-oms-gateway/scom02.png)
3. Kattintson a **Befejezés gombra**. A MOBILE munkaterület az Operations Manager kiszolgáló csatlakozik.

## <a name="configure-network-load-balancing"></a>A terheléselosztás konfigurálása

Beállíthatja, hogy az átjáró magas elérhetőség hálózati terheléselosztási fürt létrehozásával. A fürt forgalmat a ügynökök a átirányítása a kért kapcsolatokat a Microsoft figyelése ügynökök végig a csomópontok kezeli. Ha egy átjáró kiszolgáló megszakad, a forgalmat a csomóponton átirányítását.

1. Nyissa meg a Hálózati terheléskiegyenlítő, és hozzon létre egy fürt.
2. Kattintson a jobb gombbal a fürt átjárók felvétele előtt, és válassza a **Fürt tulajdonságai.** Állítsa be a fürt saját IP-címe:  
    ![Hálózati terheléskiegyenlítő – fürt IP-címei](./media/log-analytics-oms-gateway/nlb01.png)
3. A Microsoft figyelése Agent egy MOBILE átjáró server kapcsolódni, kattintson a jobb gombbal a fürt IP-címét, és válassza a **Állomás hozzáadása fürthöz**.  
    ![Hálózati betöltése terheléselosztó Manager – állomás hozzáadása fürthöz](./media/log-analytics-oms-gateway/nlb02.png)
4. Adja meg, amelyhez csatlakozni az átjárót kiszolgáló IP-címe:  
    ![Hálózati terheléselosztás kezelője – állomás hozzáadása fürthöz: csatlakozás](./media/log-analytics-oms-gateway/nlb03.png)
5. Operációs rendszerű számítógépeknél, amely nem rendelkezik internetkapcsolattal, ne felejtse el a fürt IP-címe akkor használhatók, amikor célszerű konfigurálni a **Microsoft Agent figyelése tulajdonságai**:  
    ![Microsoft Agent tulajdonságai – a proxybeállítások figyelése](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Automatizálási hibrid dolgozók konfigurálása

Ha automatizálási hibrid dolgozók van a környezetben, az alábbi lépéseket adja meg a támogatási őket az átjáró beállítása kézi, ideiglenes megoldásokat alkalmazhatja.

A következőképpen kell tudnia a Azure régió, ahol az automatizálási fiókját. Keresse meg azt a helyet:

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Jelölje be az automatizálási Azure szolgáltatás.
3. Jelölje ki a megfelelő Azure automatizálási fiókot.
4. A **hely**területi megtekintése.  
    ![Azure portál – automatizálási fiókjának helye](./media/log-analytics-oms-gateway/location.png)

Használja az alábbi táblázatokban minden olyan helyen, URL-címe azonosítása:

**Feladat futtatókörnyezet adatok szolgáltatás URL-címei**

| **hely** | **URL-CÍME** |
| --- | --- |
| A központi Észak-amerikai | ncus-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| Nyugati Európa | a Microsoft-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| A központi Dél-Amerikai Egyesült Államok | scus-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| Kelet-Amerikai Egyesült Államok | eus-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| Közép Kanadán | másolatot kap – jobruntimedata-termékkatalógus-su1.azure-automation.net |
| Észak-Európa | Ne-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| Dél-kelet-ázsiai | tengeri-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| A központi India | CID-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| Japán | jpe-jobruntimedata-termékkatalógus-su1.azure-automation.net |
| Ausztrália | ASE-jobruntimedata-termékkatalógus-su1.azure-automation.net |

**Ügynök szolgáltatás URL-címei**

| **hely** | **URL-CÍME** |
| --- | --- |
| A központi Észak-amerikai | ncus-agentservice-termékkatalógus-1.azure-automation.net |
| Nyugati Európa | a Microsoft-agentservice-termékkatalógus-1.azure-automation.net |
| A központi Dél-Amerikai Egyesült Államok | scus-agentservice-termékkatalógus-1.azure-automation.net |
| Kelet-Amerikai Egyesült Államok | eus2-agentservice-termékkatalógus-1.azure-automation.net |
| Közép Kanadán | másolatot kap – agentservice-termékkatalógus-1.azure-automation.net |
| Észak-Európa | Ne-agentservice-termékkatalógus-1.azure-automation.net |
| Dél-kelet-ázsiai | tengeri-agentservice-termékkatalógus-1.azure-automation.net |
| A központi India | CID-agentservice-termékkatalógus-1.azure-automation.net |
| Japán | jpe-agentservice-termékkatalógus-1.azure-automation.net |
| Ausztrália | ASE-agentservice-termékkatalógus-1.azure-automation.net |

A számítógép azonosítóként regisztrált egy hibrid dolgozó automatikus javítása, használja a frissítés projektmenedzsment megoldás az, ha használja az alábbi lépéseket:

1. A feladat futtatókörnyezet adatok szolgáltatás URL-címek hozzáadása az engedélyezett Host listához, MOBILE átjáró. Példa: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Indítsa újra az átjáró MOBILE szolgáltatás az alábbi PowerShell-parancsmag használatával:`Restart-Service OMSGatewayService`

Ha a számítógépen lévő-szállt Azure az automatizálás a hibrid dolgozó regisztrációs parancsmaggal, akkor ezeket a lépéseket:

1. Az ügynök regisztrációs URL-cím felvétele a megengedett Host listára, MOBILE átjáró. Példa:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. A feladat futtatókörnyezet adatok szolgáltatás URL-címek hozzáadása az engedélyezett Host listához, MOBILE átjáró. Példa: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Indítsa újra az átjáró MOBILE szolgáltatást.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Hasznos PowerShell-parancsmagok

A parancsmagok segíthetnek végezze el a MOBILE átjáró konfigurációs beállítások frissítése szükséges feladatokat. Mielőtt használni őket, ne felejtse el:

1. Telepítse a MOBILE átjáró (MSI).
2. Nyissa meg a PowerShell-ablakot.
3. A modul importálásához ezt a parancsot:`Import-Module OMSGateway`
4. Ha nem történt hiba az előző lépésben, a modul sikeresen importálta, és a parancsmaggal használható. Típus`Get-Module OMSGateway`
5. Módosítások befejezése után az parancsmag használatával, győződjön meg arról, hogy újra kell indítania az átjáró szolgáltatással.

Ha nem sikerül a 3, a modul nem lett importálva. A hiba akkor fordulhat elő, ha PowerShell nem található a modul. Megtalálhatja az átjáró telepítési elérési út: C:\Program File\Microsoft MOBILE Gateway\PowerShell.

| **Parancsmag** | **Paraméterek** | **Leírás** | **Példák** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Kulcs (kötelező) <br> Érték | A szolgáltatás a konfiguráció módosítása | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Kulcs | A szolgáltatás a konfiguráció módja | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Cím <br> Felhasználónév <br> Jelszó | Állítja be a továbbítás (felsőbb szintű) proxy címet (és hitelesítő adatok) | 1. a válasz a proxykiszolgáló és a hitelesítő adatok beállítása:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. a válasz proxy, nem hitelesítés beállítása:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. Törölje a jelet a válasz-proxy beállításai, ez azt jelenti, hogy nem kell egy válasz proxy:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | A cím a továbbítási (felsőbb szintű) proxy módja | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | A Host (kötelező) | A host a engedélyezési listájára hozzáadása | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`Kétmintás t | A Host (kötelező) | A host eltávolítása az engedélyezett listájáról | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | A jelenleg engedélyezett host kap (csak a helyben beállított engedélyezett host, ne kerüljön bele automatikusan letöltött engedélyezett hosts) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | A tárgy (kötelező) | Az ügyfél tanúsítványát a engedélyezési listájára fizetnie hozzáadása | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | A tárgy (kötelező) | Az ügyfél tanúsítvány téma eltávolítása az engedélyezett listájáról | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Az éppen engedélyezett ügyfél kapja a tanúsítvány tárgyak (csak a helyben beállított engedélyezett tárgyak, ne kerüljön bele automatikusan letöltött engedélyezett téma szerint rendezve) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Kapcsolatos hibák elhárítása

Azt javasoljuk, hogy telepítse a MOBILE agent operációs rendszerű számítógépeknél, az átjáró telepítve van. A ügynök gyűjthetők össze az események, az átjáró szerint be van jelentkezve majd használható.

![Az Eseménynapló – MOBILE átjáró napló](./media/log-analytics-oms-gateway/event-viewer.png)

**MOBILE átjáró esemény azonosítók és leírások**

A következő táblázat mutatja az esemény azonosítók és a MOBILE átjáró naplóbeli események leírását.

| **AZONOSÍTÓ** | **Leírás** |
| --- | --- |
| 400 | Bármely, amely nem tartalmaz egy egyedi Azonosítót alkalmazáshiba |
| 401 | Helytelen konfigurációja. Példa: listenPort = "szöveg" helyett egy egész szám |
| 402 | Kivétel TLS handshake üzenetek elemzése |
| 403 | Hálózati hiba. Példa: cél kiszolgálóhoz való kapcsolódást |
| 100 | Általános információk |
| 101 | Szolgáltatás indítása |
| 102 | Szolgáltatás leállt. |
| 103 | Csatlakozás HTTP parancsot kapott ügyfélprogramból |
| 104 | Nem egy HTTP csatlakozás parancs |
| 105 | Cél kiszolgáló nem engedélyezett listában vagy a célport nem biztonságos port (443-as) <br> <br> Győződjön meg arról, hogy az átjáró-kiszolgálóra a MMA ügynök és a kommunikáció az átjáró ügynökök csatlakoztatott azonos napló Analytics munkaterületen.|
| 105 | Hiba TcpConnection – érvénytelen ügyfél tanúsítvány: CN = átjáró <br><br> Győződjön meg arról, hogy: <br> <br> & #149; A verziószám 1.0.395.0 használja, az átjáró-s vagy újabb. <br> & #149; Az átjáró-kiszolgálóra a MMA ügynök és a kommunikáció az átjáró ügynökök csatlakoztatott azonos napló Analytics munkaterületen. |
| 106 | Bármilyen okból, amely a TLS-munkamenet gyanús és elutasított |
| 107 | A TLS-munkamenet ellenőrizte |

**A gyűjtendő teljesítmény számláló**

Az alábbi táblázat a teljesítmény számláló érhető el a MOBILE átjáró jeleníti meg. A számláló teljesítmény Monitor használatával is hozzáadhat.

| **név** | **Leírás** |
| --- | --- |
| MOBILE átjáró/aktív Ügyfélkapcsolat | Aktív ügyfél (TCP) hálózati kapcsolatok száma |
| MOBILE átjáró/hibaszám | Hibák száma |
| MOBILE átjáró/csatlakoztatott ügyfél | A kapcsolódó ügyfelek száma |
| MOBILE átjáró/elutasítás száma | Bármely TLS adatérvényesítési hiba miatt elutasítása száma |

![MOBILE átjáró teljesítmény számláló](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Segítség kérése

Ha meg van bejelentkezve beépülő modul az Azure-portálra, a támogatási kérelem a MOBILE átjáró vagy más Azure szolgáltatás vagy a szolgáltatás funkciója is létrehozhat.
Kérhet segítséget, kattintson a portál jobb felső sarokban a kérdőjelet ábrázoló szimbólumot, és kattintson az **új támogatási kérelem**gombra. Ezután hajtsa végre az új támogatási kérelem űrlap.

![Új támogatási kérelem](./media/log-analytics-oms-gateway/support.png)

A a [Microsoft Azure Visszajelzési fórum](https://feedback.azure.com/forums/267889)is hagyhatja, hogy a MOBILE vagy napló Analytics.

## <a name="next-steps"></a>Következő lépések

- [Hozzáadás adatforrások](log-analytics-data-sources.md) adatgyűjtés a MOBILE munkaterület csatlakoztatott adatforrásból, és a MOBILE tárházba tárolja.
