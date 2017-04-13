<properties 
   pageTitle="Webes proxy StorSimple eszköz beállítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként StorSimple a Windows PowerShell használata a webes proxybeállításokat StorSimple mobileszközére."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-web-proxy-for-your-storsimple-device"></a>Webes proxy StorSimple mobileszközére beállítása

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogy miként StorSimple a Windows PowerShell használatával StorSimple eszköze proxybeállításait webes megtekintése és beállítása. A webes proxybeállítások használnak az StorSimple eszköz kommunikáció a felhőben. Webes proxykiszolgáló biztonsági, szűrő-tartalom könnyű sávszélességre vonatkozó követelmények vagy még akkor is segítségre analytics-gyorsítótár egy másik réteget szolgál.

Webes proxy StorSimple eszköze választható konfigurációs. Csak a Windows PowerShell keresztül web proxy StorSimple konfigurálhatja. A beállítás egy két lépésből álló folyamat az alábbi képlettel történik:

1. Először konfigurálása webes proxybeállításait a beállítási varázsló vagy a Windows PowerShell StorSimple parancsmagok.

2. Majd engedélyezze a beállított webes proxybeállításait Windows PowerShell StorSimple parancsmagok.

A webes proxy konfigurációjának befejeződése után StorSimple a Microsoft Azure StorSimple kezelő szolgáltatás és a Windows PowerShell megtekintheti a beállított webes proxybeállításokat. 

Ebben az oktatóanyagban elolvasása, után lesz képes:

- Webes proxybeállítások parancsmagok és a beállítási varázsló használatával
- Webes proxy engedélyezése parancsmagokkal
- Webes proxykiszolgáló beállításainak megtekintése az Azure klasszikus portálon
- Webes proxy konfigurálása során hibák elhárítása


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>A Windows PowerShell web proxy beállítása StorSimple

Használatával az alábbiak egyikét a proxykiszolgáló beállításainak megadása:

- A beállítási varázsló végigvezeti Önt a konfigurálási lépéseket.

- A StorSimple a Windows PowerShell-parancsmagok.

A fenti módszerek az alábbi szakaszok Budai tárgyalja.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>A beállítási varázsló keresztül web proxy beállítása

A beállítási varázsló segítségével végigvezeti Önt a webes proxy konfigurációjának lépéseit. A következő lépésekkel webes proxybeállítások az eszközön.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>A beállítási varázsló keresztül web proxy beállítása

1. A soros konzol menüben válassza az 1, **Jelentkezzen be a teljes hozzáférés** , és adja meg az **eszköz rendszergazdai jelszó**. Írja be a következő parancsot a beállítási varázsló indítása:

    `Invoke-HcsSetupWizard`

2. Ha ez az első alkalommal használt a beállítási varázsló eszköz bejegyzési, szüksége lesz az összes szükséges hálózati beállítások konfigurálása, amíg el nem éri a webes proxy konfigurációjának. Az eszköz már van regisztrálva, amíg el nem éri a webes proxy konfigurációjának fogadja el is az összes beállított hálózati beállítások. Amikor a proxykiszolgáló beállításainak megadása kéri, írja be a beállítási varázsló **Igen**.

3. A **Webes Proxy URL-címe**adja meg az IP-cím vagy a teljes tartománynevét (FQDN) a TCP-portszámot, amelyeket szeretne eszközét, hogy a felhőben való kommunikáció és a webes proxykiszolgáló. Használja a következő formátumban:

    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`

    Alapértelmezés szerint a TCP-portszámot 8080 van megadva.

4. Válassza ki a hitelesítési **NTLM**, **egyszerű**vagy **sem**. Egyszerű a proxy beállítása a legkevésbé biztonságos hitelesítés van. NT LAN Manager (NTLM) hármas levelezőrendszer (néha négy további integritását szükségessége esetén) használó igen biztonságos, összetett hitelesítési protokoll a felhasználó hitelesítést végezni. Az alapértelmezett hitelesítési NTLM használja. További tudnivalókért lásd: [egyszerű](http://hc.apache.org/httpclient-3.x/authentication.html) és [NTLM-hitelesítést](http://hc.apache.org/httpclient-3.x/authentication.html). 

    > [AZURE.IMPORTANT] **Az ellenőrző eszköz diagramok nem működnek, ha egyszerű vagy NTLM-hitelesítés engedélyezve van a proxy beállítása az eszköz a StorSimple-kezelő szolgáltatás. A munka nyomon diagramok meg kell győződjön meg arról, hogy hitelesítés nincs értékre van állítva.**

5. Ha hitelesítést használ, adja meg egy **Webes Proxy felhasználónév** és a **Webes Proxy jelszót**. Szüksége lesz is, hogy erősítse meg a jelszót.

    ![Webes Proxy StorSimple Device1 konfigurálása](./media/storsimple-configure-web-proxy/IC751830.png)

Ha első alkalommal regisztrál az eszközön, folytassa a regisztráció. Ha az eszköz már bejegyezte, a varázsló bezárul. A program menti a konfigurált beállításokat.

Webes proxy fog most már engedélyezni kell is. A [webes proxy engedélyezése](#enable-web-proxy) hagyja, és [webes proxybeállításokat megtekintése az Azure klasszikus portálon](#view-web-proxy-settings-in-the-azure-classic-portal)közvetlen megnyitásához.


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Állítsa be a webes proxyn keresztül a Windows PowerShell StorSimple parancsmagok

Egy alternatív módszer a proxykiszolgáló beállításainak megadása a Windows PowerShell-parancsmagok StorSimple található. A következő lépésekkel webes proxybeállítások.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Webes proxy keresztül parancsmagok konfigurálása

1. A soros konzol menüben válassza az 1, **Jelentkezzen be a teljes hozzáférést biztosít**. Amikor a rendszer kéri, adja meg az **eszközt rendszergazdai jelszót**. Az alapértelmezett jelszó az `Password1`.

2. A parancssorba írja be:

    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`

    Adja meg, és a jelszót, erősítse meg, alább látható módon.

    ![Webes Proxy StorSimple Device3 konfigurálása](./media/storsimple-configure-web-proxy/IC751831.png)

A webes proxy most már be van állítva, és engedélyezni kell.

## <a name="enable-web-proxy"></a>Webes proxy engedélyezése

Webes proxy alapértelmezés szerint nincs engedélyezve. Miután beállította a webes proxybeállítások StorSimple eszközén, akkor használja a Windows PowerShell-StorSimple ahhoz, hogy a webes proxybeállításokat.

> [AZURE.NOTE] **Ez a lépés nem lesz szükséges webes proxykiszolgáló beállítása a beállítási varázsló használatakor. Webes proxy automatikusan alapértelmezés szerint engedélyezve van egy beállítási varázsló munkamenet után.**

A Windows PowerShell-ahhoz, hogy az eszközön web proxy StorSimple végezze el az alábbi lépéseket:

#### <a name="to-enable-web-proxy"></a>Webes proxy engedélyezése

1. A soros konzol menüben válassza az 1, **Jelentkezzen be a teljes hozzáférést biztosít**. Amikor a rendszer kéri, adja meg az **eszközt rendszergazdai jelszót**. Az alapértelmezett jelszó az `Password1`.

2. A parancssorba írja be:

    `Enable-HcsWebProxy`

    A webes proxy konfigurációjának most a StorSimple eszközén engedélyezve van.

    ![Webes Proxy StorSimple Device4 konfigurálása](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Webes proxykiszolgáló beállításainak megtekintése az Azure klasszikus portálon

A webes proxybeállítások van konfigurálva a Windows PowerShell-kapcsolaton keresztül, és nem lehet módosítani a klasszikus portál belül. Azonban megtekinthetők a beállított beállítások a klasszikus portálon A következő lépésekkel web proxy megtekintéséhez.

#### <a name="to-view-web-proxy-settings"></a>Webes proxykiszolgáló beállításainak megtekintése
1. Nyissa meg azt **StorSimple kezelő szolgáltatás > eszközök**. Jelölje ki, és kattintson a kívánt eszközre, és kattintson a **Konfigurálás**.
1. Görgessen lefelé **a **webes proxybeállításokat** szakaszra lap** . Megtekintheti a beállított webes proxybeállítások StorSimple eszközén alább látható módon.

    ![Webhely-Proxy megtekintése az adatkezelési portálon](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)
 
## <a name="errors-during-web-proxy-configuration"></a>Webes proxy konfigurálása során hibák

Ha a webes proxybeállítások nem megfelelően van konfigurálva, hibaüzenetek jelennek meg a felhasználó a Windows PowerShell StorSimple az. Az alábbi táblázat ismerteti az egyes ezekre a hibaüzenetekre, a lehetséges okok és a javasolt műveletek.

|Válassza a soros nem.|HRESULT hiba kódot.|Lehetséges kiváltóok|Javasolt teendő|
|:---|:---|:---|:---|
|1.|0x80070001|A passzív vezérlő a parancsot futtatja, és nem tud kommunikálni az aktív vezérlő.|Az aktív vezérlő a művelet végrehajtása Futtassa a parancsot a passzív vezérlő, szüksége lesz a passzív a kiszolgálóhoz való csatlakozás aktív vezérlő javítás. Meg kell folytatására Microsoft Support, ha megszakad a kapcsolat.|
|2.|0x800710dd – a művelet azonosító nem érvényes|A proxybeállítások StorSimple virtuális eszköz nem támogatottak.|A proxybeállítások StorSimple virtuális eszköz nem támogatottak. Ezek csak beállíthatók StorSimple fizikai eszközön.|
|3.|0x80070057 - érvénytelen paraméter|A proxybeállítások megadott paraméterek egyike nem érvényes.|A URI nem szerepel a megfelelő formátumban. Használja a következő formátumban:`http://<IP address or FQDN of the web proxy server>:<TCP port number>`|
|4.|0x800706BA jelű - RPC kiszolgáló nem érhető el|A legfelső szintű oka a következők valamelyikét:</br></br>Fürt nem be.</br></br>DataPath szolgáltatás nem működik.</br></br>A parancsot futtatja a passzív vezérlő, és nem tud kommunikálni az aktív vezérlő.|Kérjük, folytatására kattintva győződjön meg arról, hogy a csoportját fel és datapath szolgáltatást futtató Microsoft Support.</br></br>Az aktív vezérlő a művelet végrehajtása Ha szeretne a parancsot a a passzív vezérlő, szüksége lesz annak érdekében, hogy az aktív vezérlő kommunikálni tudjanak a passzív vezérlő. Meg kell folytatására Microsoft Support, ha megszakad a kapcsolat.|
|5.|0X800706be - RPC hívás sikertelen|Fürt nem működik.|Kérjük, folytatására Microsoft Support kattintva győződjön meg róla, hogy a fürt be.|
|6.|0x8007138f - fürt erőforrás nem található|Platform szolgáltatás fürt erőforrás nem található. Ez akkor fordulhat elő, ha a telepítés nem megfelelő.|Előfordulhat, hogy az eszközön alaphelyzetbe gyár végrehajtásához. Előfordulhat, hogy az erőforrás platform létrehozása. Kérjük, forduljon a Microsoft Support a következő lépéseket.|
|7.|0x8007138c - fürt erőforrás nem online|Platform vagy datapath fürt erőforrások nem érhetők el.|Kérjük, forduljon a Microsoft Support érdekében győződjön meg arról, hogy online-e a datapath és a platform szolgáltatás erőforrás.|

> [AZURE.NOTE] 
> 
> -  A függvény nem teljes körű hibaüzenetek jelennek meg a fenti listáját. 
> - Webes proxybeállításokat kapcsolatos hibákat nem jelennek meg a StorSimple kezelő szolgáltatás az Azure klasszikus portálon. Ha webes proxy problémát a konfigurációs befejeződése után, az Eszközállapot **offline** módosítja a klasszikus portálon. |}

## <a name="next-steps"></a>Következő lépések

- Ha az eszköz telepítése vagy a webes proxykiszolgáló beállításainak konfigurálása közben tapasztal kapcsolatos problémák megoldásához, keresse meg [a StorSimple eszköz telepítésének hibaelhárítása](storsimple-troubleshoot-deployment.md).

- Megtudhatja, hogy miként használhatja a StorSimple kezelő szolgáltatás, keresse fel [a StorSimple Manager szolgáltatással való StorSimple eszközére](storsimple-manager-service-administration.md).
