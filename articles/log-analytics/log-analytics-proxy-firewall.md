<properties
    pageTitle="A proxykiszolgáló és tűzfalbeállításokat konfigurálása a napló Analytics |} Microsoft Azure"
    description="Amikor a ügynökök vagy MOBILE szolgáltatások kell használnia az adott portok proxy és tűzfal beállításainak konfigurálása"
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
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="banders;magoedte"/>

# <a name="configure-proxy-and-firewall-settings-in-log-analytics"></a>A napló Analytics proxy és a tűzfal beállításainak konfigurálása

Műveletek szükséges proxybeállítások, és a MOBILE napló Analytics tűzfal beállításai különböznek egymástól a Operations Manager és a ügynökök és a Microsoft figyelése ügynökök, csatlakoztassa közvetlenül a kiszolgáló használata esetén. Tekintse át az alábbi szakaszok az Ön által használt ügynök típusú.

## <a name="configure-proxy-and-firewall-settings-with-the-microsoft-monitoring-agent"></a>A Microsoft figyelése Agent proxy és a tűzfal beállításainak konfigurálása

A Microsoft figyelése Agent való kapcsolódáshoz és regisztrálhatja a MOBILE szolgáltatással akkor a tartományok és az URL-címek port száma hozzáféréssel kell rendelkeznie. Ha a agent és a MOBILE szolgáltatás közötti kommunikáció proxykiszolgálót használ, kell érhetők el, hogy a megfelelő erőforrások biztosítására. Ha tűzfalat használva korlátozhatja a hozzáférést az interneten, állítsa be a tűzfalat, hogy engedélyezze a hozzáférést a MOBILE szeretne. Az alábbi táblázatokban felsoroljuk a portokat MOBILE szükséges.

|**Ügynök erőforrás**|**Portok**|**HTTPS-ellenőrzési figyelmen kívül hagyása**|
|--------------|-----|--------------|
|\*. ods.opinsights.azure.com|443-as|igen|
|\*. oms.opinsights.azure.com|443-as|igen|
|\*. blob.core.windows.net|443-as|igen|
|ods.systemcenteradvisor.com|443-as| |

A következő eljárással konfigurálása a Microsoft figyelése Agent Vezérlőpultján proxybeállításait. Minden kiszolgáló eljárással kell. Ha sok kiszolgáló, amely kell beállítania, akkor előfordulhat, hogy egyszerűbb parancsfájl használatával automatizálhatja a folyamathoz. Ha igen, olvassa el a következő eljárást [a Microsoft figyelése Agent parancsfájl használatával a proxykiszolgáló beállításainak konfigurálása](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script)című témakört.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-control-panel"></a>A Microsoft figyelése Agent Vezérlőpultján a proxykiszolgáló beállításainak konfigurálása

1. Nyissa meg a **Vezérlőpultot**.

2. Nyissa meg **a Microsoft Agent figyelése**.

3. Kattintson a **Proxy beállításai** fülre.<br>  
  ![Proxybeállítások lap](./media/log-analytics-proxy-firewall/proxy-direct-agent-proxy.png)

4. Jelölje be **a proxykiszolgáló használata** , és írja be az URL-címet, és portszámot, ha szükséges, a megjelenített példához hasonló. Ha a proxykiszolgáló hitelesítést igényel, írja be a felhasználónevét és jelszavát a proxykiszolgáló eléréséhez.

Az alábbi eljárással hozzon létre egy PowerShell-parancsprogramot egyes ügynök, amely közvetlenül a kiszolgálókhoz csatlakozik a proxykiszolgáló beállításainak megadása futtatva.

### <a name="to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script"></a>A Microsoft figyelése Agent parancsfájl használatával a proxykiszolgáló beállításainak konfigurálása

Másolja a következő példa, frissítése, adatai a környezetben, PS1 fájlnévkiterjesztésű mentse és futtassa a parancsfájl összes olyan számítógépen, amely közvetlenül a MOBILE szolgáltatás csatlakozik.

        
    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get the Health Service configuration object.  We need to determine if we
    #have the right update rollup with the API we need.  If not, no need to run the rest of the script.
    $healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

    $proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

    if (!$proxyMethod)
    {
         Write-Output 'Health Service proxy API not present, will not update settings.'
         return
    }

    Write-Output "Clearing proxy settings."
    $healthServiceSettings.SetProxyInfo('', '', '')

    $ProxyUserName = $cred.username

    Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
        

## <a name="configure-proxy-and-firewall-settings-with-operations-manager"></a>A proxykiszolgáló és tűzfalbeállításokat Operations Manager konfigurálása

Egy Operations Manager kezelése csoport való kapcsolódáshoz és a MOBILE szolgáltatás regisztrálása akkor a tartományok és URL-ek a portszámokat hozzáféréssel kell rendelkeznie. Ha a Operations Manager management server és a MOBILE szolgáltatás közötti kommunikáció proxykiszolgálót használ, kell érhetők el, hogy a megfelelő erőforrások biztosítására. Ha tűzfalat használva korlátozhatja a hozzáférést az interneten, állítsa be a tűzfalat, hogy engedélyezze a hozzáférést a MOBILE szeretne. Még ha az Operations Manager management server nem egy proxykiszolgáló, annak ügynökök lehet. Ebben az esetben a proxykiszolgáló kell beállítania ugyanúgy engedélyezése, és lehetővé teszi a biztonsági ügynökök és Log kezelési megoldás adatok beolvasása a MOBILE küldeni webszolgáltatás.

Ahhoz, hogy a MOBILE szolgáltatás kommunikáció Operations Manager ügynökök a Operations Manager infrastruktúra (beleértve a ügynökök) kell tartalmaznia a helyes-e a proxybeállítások és verzióját. A proxy ügynökök beállítása a Operations Manager konzolban van megadva. A verzió kell az alábbiak egyikét:

- Műveletek Manager 2012 SP1 frissítőcsomag 7 vagy újabb verzió
- Műveletek Manager 2012 R2 frissítőcsomag 3 és újabb verziók


Az alábbi táblázatokban felsoroljuk a portokat kapcsolódó az alábbi műveleteket.

>[AZURE.NOTE] A következő forrásokhoz említeni Advisor és műveleti elemzéseket, mindkét voltak MOBILE korábbi verzióiban. Azonban a jövőben változik a felsorolt erőforrásokat.

Az alábbiakban ügynök erőforrások és a portokra listája:<br>

|**Ügynök erőforrás**|**Portok**|
|--------------|-----|
|\*. ods.opinsights.azure.com|443-as|
|\*. oms.opinsights.azure.com|443-as|
|\*.BLOB.Core.Windows.NET/\*|443-as|
|ods.systemcenteradvisor.com|443-as|
<br>
Az alábbiakban management server erőforrásaira és portokat:<br>

|**Management server erőforrás**|**Portok**|**HTTPS-ellenőrzési figyelmen kívül hagyása**|
|--------------|-----|--------------|
|Service.systemcenteradvisor.com|443-as| |
|\*. service.opinsights.azure.com|443-as| |
|\*. blob.core.windows.net|443-as|igen| 
|Data.systemcenteradvisor.com|443-as| | 
|ods.systemcenteradvisor.com|443-as| | 
|\*. ods.opinsights.azure.com|443-as|igen| 
<br>
Az alábbiakban a MOBILE és a Operations Manager konzol erőforrások és a portokra listáját.<br>

|**MOBILE és a Operations Manager konzol erőforrás**|**Portok**|
|----|----|
|Service.systemcenteradvisor.com|443-as|
|\*. service.opinsights.azure.com|443-as|
|\*. live.com|80 és 443-as port|
|\*. microsoft.com|80 és 443-as port|
|\*. microsoftonline.com|80 és 443-as port|
|\*. mms.microsoft.com|80 és 443-as port|
|login.Windows.NET|80 és 443-as port|
<br>

Az alábbi eljárások segítségével regisztrálhatja a Operations Manager-kezelés csoport MOBILE szolgáltatással. Ha problémába ütközik a kapcsolati a kezelés csoport és a MOBILE szolgáltatás között, a MOBILE szolgáltatás adatátvitel megvizsgálhatja az érvényesítési eljárásokat.

### <a name="to-request-exceptions-for-the-oms-service-endpoints"></a>Kivételek hatóköre a MOBILE service végpontok igénylése

1. Lehet, hogy az első tábla annak érdekében, hogy az Operations Manager management Server szükséges forrásokat bármely tűzfalon keresztül érhető el a korábban benyújtott adatait szeretné használni.
2. Lehet, hogy a második tábla annak érdekében, hogy a műveletek konzol Operations Manager és a MOBILE szükséges forrásokat bármely tűzfalon keresztül érhető el a korábban benyújtott adatait szeretné használni.
3. Ha a proxykiszolgáló használata az Internet Explorer, győződjön meg arról, hogy be van állítva, és most megfelelően működik. Ha ellenőrizni szeretné, megnyithatja a webhely biztonságos kapcsolat (HTTPS), például [https://bing.com](https://bing.com). Ha a webhely biztonságos kapcsolatot a böngészőben nem működik, akkor valószínűleg nem használhatók a Operations Manager management console webszolgáltatásokhoz a felhőben.

### <a name="to-configure-the-proxy-server-in-the-operations-manager-console"></a>A proxykiszolgáló beállítása a Operations Manager konzolban

1. Nyissa meg az Operations Manager konzolt, és válassza a **felügyeleti** munkaterület.

2. Bontsa ki a **Működési Hírcsatornájában**, és válassza a **Működési háttérismeretek kapcsolat**.<br>  
    ![Műveletek Manager MOBILE kapcsolat](./media/log-analytics-proxy-firewall/proxy-om01.png)
3. A MOBILE kapcsolatot nézetben kattintson a **Proxykiszolgáló beállítása**.<br>  
    ![Műveletek Manager MOBILE kapcsolatot proxykiszolgáló beállítása](./media/log-analytics-proxy-firewall/proxy-om02.png)
4. Az üzemeltetési háttérismeretek beállítások varázsló: Proxykiszolgáló, jelölje be **a hírcsatornájában műveleti webszolgáltatás eléréséhez proxykiszolgáló használata**és írja be az URL-címe a port száma, például **http://myproxy:80**.<br>  
    ![Műveletek Manager MOBILE proxycímére.](./media/log-analytics-proxy-firewall/proxy-om03.png)


### <a name="to-specify-credentials-if-the-proxy-server-requires-authentication"></a>Hitelesítő adatok megadásához, ha a proxykiszolgáló hitelesítést igényel
 A proxykiszolgáló hitelesítő adatait, és a beállítások kell propagálása felügyelt számítógépekre, amely a jelentést MOBILE tesz. A *Microsoft rendszer központ Advisor figyelése Server csoport*azokat a kiszolgálókat kell lennie. Az egyes kiszolgálók csoportjában a beállításjegyzékben titkosított hitelesítő adatokat.

1. Nyissa meg az Operations Manager konzolt, és válassza a **felügyeleti** munkaterület.
2. **RunAs konfigurációs**válassza a **profilok**lehetőséget.
3. Nyissa meg a **System Center Advisor futtatása, profil Proxy** profilt.  
    ![a rendszer központ Advisor futtatása mint Proxy profil képe](./media/log-analytics-proxy-firewall/proxy-proxyacct1.png)
4. A következőképpen profil varázslót kattintson a **Hozzáadás** Futtatás mint-fiókkal gombra. Hozzon létre egy új Futtatás mint fiókot, vagy egy meglévő fiók használatával. Ehhez a fiókhoz át a proxykiszolgáló megfelelő engedélyekkel kell szerepelnie.  
    ![a következőképpen profil varázslót képe](./media/log-analytics-proxy-firewall/proxy-proxyacct2.png)
5. A fiók kezelése beállításához válassza **A kijelölt osztály, csoport vagy objektum** kattintva nyissa meg az objektum keresőmezőbe.  
    ![a következőképpen profil varázslót képe](./media/log-analytics-proxy-firewall/proxy-proxyacct2-1.png)
6. Keressen rá, majd jelölje be a **Microsoft rendszer központ Advisor figyelése Server csoport**.  
    ![az objektum Keresés mező képe](./media/log-analytics-proxy-firewall/proxy-proxyacct3.png)
7. Kattintson **az OK gombra** kattintva zárja be a Futtatás mint fiók mező.  
    ![a következőképpen profil varázslót képe](./media/log-analytics-proxy-firewall/proxy-proxyacct4.png)
8. A varázsló, és mentse a módosításokat.  
    ![a következőképpen profil varázslót képe](./media/log-analytics-proxy-firewall/proxy-proxyacct5.png)


### <a name="to-validate-that-oms-management-packs-are-downloaded"></a>Csomagok letöltési ellenőrzéséhez, hogy MOBILE kezelése

Megoldások MOBILE hozzáadta, ha megtekinthesse őket az Operations Manager konzolban management csomagok **felügyelete**szerint. Keresse meg a *System Center Advisor* gyorsan megtalálhatja őket.  
    ![adatkezelési csomagok letöltése](./media/log-analytics-proxy-firewall/proxy-mpdownloaded.png) vagy is ellenőrizni MOBILE felügyeleti csomag az Operations Manager management server használatával az alábbi Windows PowerShell-parancsot:

    ```
    Get-ScomManagementPack | where {$_.DisplayName -match 'Advisor'} | select Name,DisplayName,Version,KeyToken
    ```

### <a name="to-validate-that-operations-manager-is-sending-data-to-the-oms-service"></a>Az adott Operations Manager érvényességét adatokat küld a MOBILE szolgáltatás

1. A Operations Manager management server a teljesítmény Monitor (perfmon.exe), és nyissa meg **Teljesítmény Monitor**.
2. Kattintson a **Hozzáadás**gombra, és válassza az **Állapot szolgáltatás felügyeleti csoportok**.
3. Adja hozzá a **HTTP**kezdődő összes számláló.  
    ![Számláló hozzáadása](./media/log-analytics-proxy-firewall/proxy-sendingdata1.png)
4. Ha a Operations Manager konfigurációs jó, látni fogja a tevékenység állapota Szolgáltatáskezelés számláló események és egyéb adatok elemeket, a kezelés csomagok MOBILE és a webhelycsoport házirend konfigurálva a hozzáadott alapján.  
    ![Tevékenység a teljesítmény figyelése megjelenítő](./media/log-analytics-proxy-firewall/proxy-sendingdata2.png)


## <a name="next-steps"></a>Következő lépések

- [Log Analytics hozzáadása megoldások a megoldástárból](log-analytics-add-solutions.md) funkciókat és a szükséges adatok összegyűjtése.
- Ismerkedés a [napló keresések](log-analytics-log-searches.md) megoldások által gyűjtött részletes információk megtekintése.
