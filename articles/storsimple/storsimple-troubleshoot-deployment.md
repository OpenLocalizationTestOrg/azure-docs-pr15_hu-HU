<properties 
   pageTitle="StorSimple telepítési problémák elhárítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként diagnosztizálása és megoldása, amikor először telepít StorSimple előforduló hibák."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>StorSimple eszköz telepítési problémák elhárítása

## <a name="overview"></a>– Áttekintés

Ez a cikk a Microsoft Azure StorSimple üzembe hasznos hibaelhárítási útmutatót tartalmaz. Gyakori problémák, a lehetséges okok és a ajánlott lépéseket az StorSimple konfigurálásakor esetleg tapasztalható hibák elhárításához azt mutatja be. Ezt az információt a StorSimple helyszíni fizikai eszközök és a StorSimple virtuális eszköz is érinti.

> [AZURE.NOTE] Eszköz konfigurációs kapcsolatos problémákat, előfordulhat, hogy a arcra akkor fordulhat elő, az első alkalommal telepíti a az eszközt, vagy azok akkor fordulhat elő, később, ha az eszköz műveleti. Ez a cikk koncentrál első telepítési problémáinak. Egy műveleti eszközön elhárításához [fellépő eszköz működési problémák](storsimple-troubleshoot-operational-device.md)megnyitásához.

Ez a cikk is ismerteti a StorSimple telepítések hibaelhárítási eszközök, és a részletes hibaelhárítási példa.

## <a name="first-time-deployment-issues"></a>Első telepítési problémák

Ha első alkalommal az eszköz telepítésekor belefutna egy problémába, vegye figyelembe a következőket:

- Ha hibaelhárítási fizikai eszközt, ellenőrizze, hogy a hardver telepítve és beállítva [a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md)leírtak szerint.
- Jelölje be a Előfeltételek, telepítés. Győződjön meg arról, hogy van-e a [telepítési konfigurációs feladatlista](storsimple-deployment-walkthrough.md#deployment-configuration-checklist)ismertetett minden információt.
- Tekintse át a StorSimple kibocsátási megjegyzések megjelenítéséhez, ha a probléma leírása. Kibocsátási megjegyzések az ismert telepítési problémái megoldásai tartalmazzák. 

Eszköz a telepítés során a leggyakoribb hibák arc fordulhat elő, felhasználók által a beállítási varázsló futtatásakor, és ha azokat az eszközön a Windows PowerShell regisztrálhat StorSimple. (Használhatja a Windows PowerShell-StorSimple regisztrálhat, és állítsa be az StorSimple eszközt. Az eszköz regisztrációs kapcsolatos további tudnivalókért lásd: [a 3: konfigurálása és a Windows Powershellen keresztül az eszköz regisztrálhat StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

Az alábbi szakaszok segíthetnek az első alkalommal a StorSimple eszköz konfigurálásakor fellépő problémák megoldása.

## <a name="first-time-setup-wizard-process"></a>Első beállítási varázsló folyamat

Az alábbi lépéseket a beállítási varázsló folyamat foglalja össze. A telepítő részletes információt című témakörben talál [Deploy helyszíni StorSimple eszközére](storsimple-deployment-walkthrough.md).

1. Futtassa [Invoke-HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) kattintva indítsa el a beállítási varázslót, amely végigvezeti Önt a hátralévő lépéseket. 
2. A hálózat beállítása: a beállítási varázsló segítségével a adatok 0 hálózati kapcsolaton hálózat beállításainak konfigurálása StorSimple eszközén. Ezeket a beállításokat az alábbiak:
  - Virtuális IP (virtuális), alhálózat maszk és az átjáró: A [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) parancsmagot a háttérben végrehajtása. Akkor állítja be az IP-cím, alhálózat maszk és az adatok 0 hálózati kapcsolat átjáró StorSimple eszközén.
  - Elsődleges DNS-kiszolgáló – a [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) parancsmagot a háttérben végrehajtása. Akkor állítja be a DNS-beállítások a StorSimple megoldás.
  - A háttérben NTP server – a [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) parancsmag végrehajtása. Konfigurálja a a StorSimple megoldás a NTP kiszolgáló beállításait.
  - Nem kötelező webes proxy – a [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) parancsmagot a háttérben végrehajtása. Akkor állítja be, és lehetővé teszi, hogy a StorSimple megoldás webes proxybeállításait.
3. A jelszavak beállítása: a következő lépésként az eszköz rendszergazdáját és StorSimple pillanatkép Manager jelszavak beállítása. Ha frissítés 1 futnak, majd, nem kötelező a StorSimple pillanatkép Manager jelszó beállításához.
  - Jelentkezzen be az eszközre az eszközt rendszergazdai jelszó szolgál. Az alapértelmezett eszköz jelszava **Jelszo1**.
  - A StorSimple pillanatkép Manager jelszó szükség, amikor egy eszközt StorSimple pillanatkép Manager használatához konfigurálja. Kell először a varázslóban adja meg a jelszót, és ezután beállítása és módosításáról a StorSimple Manager szolgáltatásból. Erre a jelszóra hitelesíti a StorSimple pillanatkép kezelő eszközt.
 
    > [AZURE.IMPORTANT] Jelszavak bejegyzése előtt gyűjtött, de csak azt követően sikerült regisztrálni az eszközre alkalmazott. Ha alkalmazni a jelszó nem sikerült, a rendszer kéri adni újra a jelszót, amíg a szükséges jelszavak (vagyis a összetettsége követelményeknek) begyűjtési.

4. Regisztráljon az eszközön: az utolsó lépésként regisztrálja az eszköz a Microsoft Azure-ban futó StorSimple kezelő szolgáltatás. A regisztráció megköveteli [a szolgáltatás regisztrációs kulcs](storsimple-manage-service.md#get-the-service-registration-key) beszerzése az Azure klasszikus portálról, és adja meg azt a varázslóban. Az eszköz sikeresen regisztráció után egy szolgáltatás adatok titkosítási kulcs kapja meg azt. Ügyeljen arra, hogy a titkosítási kulcs megbízható helyen tartani, hiszen szükséges összes további eszközök regisztrálhatja a szolgáltatással.

## <a name="common-errors-during-device-deployment"></a>Eszköz telepítése során gyakran előforduló hibák

Az alábbi táblázatokban felsoroljuk, hogy előforduló mikor kapcsolatos gyakori hibákra:

- A szükséges hálózat beállításainak konfigurálása
- A választható proxykiszolgáló beállításainak megadása.
- Állítsa be az eszköz rendszergazdáját és StorSimple pillanatkép Manager jelszavakat. 
- Regisztráljon az eszközt. 

## <a name="errors-during-the-required-network-settings"></a>A szükséges hálózati beállítások hibák

| nem.| Hibaüzenet jelenik meg | Lehetséges okai | Javasolt teendő |
| ---| ------------- | --------------- | ------------------ |
| 1  | Meghívása HcsSetupWizard: Ez a parancs csak futtatható az aktív vezérlő. | Konfigurációs passzív vezérlőn végrehajtása.| Ez a parancs lebonyolítása az aktív vezérlő. További tudnivalókért lásd: [azonosítása egy aktív vezérlő az eszközön](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Meghívása HcsSetupWizard: Nem áll készen eszköz. | Az adatok 0 a hálózati kapcsolat problémák lépnek fel.| Jelölje be a fizikai a hálózati kapcsolat adatok 0.|
| 3 | Meghívása HcsSetupWizard: Van, akkor a hálózaton más rendszerrel IP-cím ütközést (kivétel HRESULT: 0x80070263). | Az IP-adatok 0 megadott már egy másik rendszert használja. | Adja meg egy új IP, amely nem használja.|
| 4 | Meghívása HcsSetupWizard: Fürt erőforrás sikertelen volt. (Kivétel HRESULT: 0x800713AE). | Ismétlődő virtuális. A megadott IP már használatban van.| Adja meg egy új IP, amely nem használja.|
| 5 | Meghívása HcsSetupWizard: Érvénytelen IPv4-címet. | Az IP-cím helytelen formátumú megadva.| Jelölje be a formátumot, és adja meg újra az IP-címének. További tudnivalókért lásd: a [Ipv4-címek][1]. |
| 6 | Meghívása HcsSetupWizard: Érvénytelen IPv6 address. | Az IP-cím helytelen formátumú megadva.| Jelölje be a formátumot, és adja meg újra az IP-címének. További tudnivalókért lásd: a [Ipv6-címek][2].|
| 7 | Meghívása HcsSetupWizard: Érhetők el további végpontok végpont. (Kivétel HRESULT: 0x800706D9) | A fürt funkció nem működik. | [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) , a következő lépéseket.

## <a name="errors-during-the-optional-web-proxy-settings"></a>A választható webes proxybeállítások hibák

| nem.| Hibaüzenet jelenik meg | Lehetséges okai | Javasolt teendő |
| ---| ------------- | --------------- | ------------------ |
| 1  | Meghívása HcsSetupWizard: Érvénytelen paraméter (kivétel HRESULT: 0x80070057) | A proxybeállítások megadott paraméterek egyike nem érvényes.| A URI nem szerepel a megfelelő formátumban. Használja a következő formátumot: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Meghívása HcsSetupWizard: RPC kiszolgáló nem érhető el (kivétel HRESULT: 0x800706BA jelű) | A legfelső szintű oka a következők valamelyikét:<ol><li>A fürt nem legfeljebb.</li><li>A passzív vezérlő nem lehet kommunikálni, az az aktív vezérlő, és a parancsot futtatja a passzív vezérlő.</li></ol> | Attól függően, hogy a kiváltóok:<ol><li>[Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) , és ellenőrizze, hogy a fürt be-e.</li><li>Az aktív vezérlő a művelet végrehajtása Ha szeretne a parancsot a a passzív vezérlő, szüksége lesz annak érdekében, hogy a passzív vezérlő az aktív vezérlő is kommunikálhatnak. Ha megszakad a kapcsolat kell kapcsolatfelvétel a [Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) .</li></ol> |
| 3 | Meghívása HcsSetupWizard: Nem sikerült RPC-(kivétel HRESULT: 0x800706be) | Fürt nem működik. | [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) , és ellenőrizze, hogy a fürt be-e.|
| 4 | Meghívása HcsSetupWizard: Az erőforrás nem található fürt (kivétel HRESULT: 0x8007138f) | A rendszer nem található. Ez akkor fordulhat elő, ha a telepítés nem helyes. | Előfordulhat, hogy az eszköz gyári alapértelmezett beállításainak visszaállítása. [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) , és hozzon létre egy fürt erőforrást.|
| 5 | Meghívása HcsSetupWizard: Fürt erőforrás online nem (kivétel HRESULT: 0x8007138c)| Erőforrások nem érhetők el. | [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) , a következő lépéseket.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Az eszköz rendszergazdáját és StorSimple pillanatkép Manager jelszavak kapcsolatos hibák

Az alapértelmezett eszközt rendszergazdai jelszó **Jelszo1**. A jelszó lejárta után az első bejelentkezéskor; ezért szüksége lesz a beállítási varázsló használatával módosítja. Amikor regisztrál az eszközt, először meg kell adnia egy új eszközt rendszergazdai jelszó. 

Ha a StorSimple pillanatkép Managert a Windows Server állomáson futó használatával kezelheti az eszközt, majd meg kell adnia StorSimple pillanatkép Manager jelszó első regisztráció során. 

Győződjön meg arról, hogy a jelszavak az alábbi követelményeknek:

- Az eszköz rendszergazdai jelszó 8 és 15 karakter hosszú között kell lennie.
- StorSimple pillanatkép Manager jelszavát 14 vagy 15 karakter hosszú kell lennie.
- Jelszavak kell tartalmaznia az alábbi típusú 4 karakter a 3: kisbetűre cseréli le, a nagybetűs, a numerikus és a speciális. 
- A jelszó nem lehet ugyanaz, mint a legutóbbi 24 jelszavakat.

Ezenkívül ne feledje, hogy a jelszavak évente lejár, és módosítható, csak azt követően sikerült regisztrálni az eszközt. A regisztráció nem sikerül valamilyen okból, a jelszavak nem módosíthatók. Az eszköz rendszergazdáját és StorSimple pillanatkép Manager jelszavak további tudnivalókért olvassa el az [használata a StorSimple kezelő szolgáltatás StorSimple jelszavának módosítása](storsimple-change-passwords.md).

Ha az eszköz rendszergazdáját és StorSimple pillanatkép Manager jelszavak beállítása előfordulhatnak végre az alábbi hibák.

| nem.| Hibaüzenet jelenik meg | Javasolt teendő |
| ---| ------------- | ------------------ | 
| 1  | A jelszó megengedettnél. |Ezeknek a követelményeknek megfelelő jelszót használja:<ul><li>Az eszköz rendszergazdai jelszó 8 és 15 karakter hosszú között kell lennie.</li><li>A StorSimple pillanatkép Manager jelszavát 14 vagy 15 karakter hosszú kell lennie.</li></ul> | 
| 2 | A jelszó nem felel meg a szükséges hossz. | Ezeknek a követelményeknek megfelelő jelszót használja:<ul><li>Az eszköz rendszergazdai jelszó 8 és 15 karakter hosszú között kell lennie.</li><li>A StorSimple pillanatkép Manager jelszavát 14 vagy 15 karakter hosszú kell lennie.</lu></ul> |
| 3 | A jelszó kisbetűk kell tartalmaznia. | Jelszavak kell tartalmaznia az alábbi típusú 4 karakter a 3: kisbetűre cseréli le, a nagybetűs, a numerikus és a speciális. Győződjön meg arról, hogy a jelszó felel meg ezeknek a követelményeknek. |
| 4 | A jelszó számokból kell tartalmaznia. | Jelszavak kell tartalmaznia az alábbi típusú 4 karakter a 3: kisbetűre cseréli le, a nagybetűs, a numerikus és a speciális. Győződjön meg arról, hogy a jelszó felel meg ezeknek a követelményeknek. |
| 5 | A jelszó különleges karaktereket tartalmazhat. | Jelszavak kell tartalmaznia az alábbi típusú 4 karakter a 3: kisbetűre cseréli le, a nagybetűs, a numerikus és a speciális. Győződjön meg arról, hogy a jelszó felel meg ezeknek a követelményeknek. |
| 6 | A jelszó kell tartalmaznia az alábbi típusú 4 karakter a 3: nagybetűsre, kisbetűsre, numerikus és különleges. | A jelszó nem tartalmaz a szükséges karakterek típusú. Győződjön meg arról, hogy a jelszó felel meg ezeknek a követelményeknek. |
| 7 | A paraméter értéke nem egyezik meg megerősítő. | Győződjön meg arról, hogy a jelszó megfelel az összes és, hogy helyesen írta be. |
| 8 | A jelszó nem egyezik meg az alapértelmezett. | Az alapértelmezett jelszava *Jelszo1*. Szeretné módosítani a jelszavát, az első bejelentkezés után. |
| 9 | A megadott jelszót nem egyezik meg az eszköz jelszót. Írja be újra a jelszót. | Jelölje be a jelszót, és írja be újra. |

Jelszavak begyűjtési, mielőtt az eszköz van regisztrálva, de csak azt követően a sikeres regisztrációt alkalmazva vannak. A jelszó helyreállítási munkafolyamat regisztrálását az eszköz szükséges. 

> [AZURE.IMPORTANT] Az általános jelszó alkalmazása kísérlete sikertelen, majd a szoftver többször megkísérli összegyűjtése a jelszót, amíg nem sikerül. Ritkán a jelszó nem alkalmazható. Ebben az esetben regisztrálni az eszközt, és folytassa, azonban a jelszavak nem változik. Kap arról, hogy a jelszó nem változott jelzés – az eszköz rendszergazdai jelszó vagy a StorSimple pillanatkép Manager jelszava. Akkor fordul elő, ebben az esetben, ha azt javasoljuk, hogy módosítsa a két jelszavakat.

A jelszót az Azure Service StorSimple Manager klasszikus portálon. További információért lépjen: 

- [Az eszköz rendszergazdai jelszó módosítása](storsimple-change-passwords.md#change-the-device-administrator-password).
- [A StorSimple pillanatkép Manager jelszó módosítása](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Hibák eszköz regisztráció során

A Microsoft Azure-ban futó StorSimple kezelő szolgáltatás segítségével regisztrálni az eszközt. Egy vagy több az alábbi problémák sikerült előforduló eszköz regisztráció során.

| nem.| Hibaüzenet jelenik meg | Lehetséges okai | Javasolt teendő |
| ---| ------------- | --------------- | ------------------ |
| 1  | Hiba 350027: Nem sikerült regisztrálni az eszközt a StorSimple kezelő. |  | Várjon néhány percet, és próbálkozzon újra a műveletet. Ha a probléma továbbra is fennáll, [lépjen kapcsolatba a Microsoft-támogatást](storsimple-contact-microsoft-support.md).|
| 2  | 350013 hiba: Egy hiba történt a Regisztrálás az eszközt. Ennek oka lehet helytelen szolgáltatás regisztrációs billentyűt. | | Regisztráljon az eszköz újra a megfelelő szolgáltatás regisztrációs kulccsal. További tudnivalókért lásd: [beszerzése a szolgáltatás regisztrációs kulcsa.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | 350063 hiba: A hitelesítési átadott StorSimple-kezelő szolgáltatás, de a regisztrációs sikertelen volt. A művelet után egy kis időt, próbálkozzon újra. | Ez a hiba azt jelzi, hogy ACS hitelesítéssel eltelte külső.FÜGGV hívást a szolgáltatás nem sikerült. Ez lehet egy hálózati kapcsolata időről-időre működési hiba eredménye. | Ha a probléma nem szűnik meg, jelentkezzen be a [lépjen kapcsolatba a Microsoft-támogatást](storsimple-contact-microsoft-support.md). |
| 4 | 350049 hiba: A szolgáltatás nem érhető el regisztráció során. | A hívás a szolgáltatás történik, ha egy webes kivétel érkezik. Bizonyos esetekben ez lehet első rögzített, a művelet később újra próbálkozik. | Ellenőrizze az IP-cím és a DNS-nevét, és próbálja meg a műveletet. Ha a probléma nem szűnik meg, [lépjen kapcsolatba a Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | 350031 hiba: Az eszköz már regisztrálva van. | | Nem szükséges lépéseket. |
| 6 | Hiba 350016: Nem sikerült regisztrálni. | |Győződjön meg arról, hogy helyesek-e a regisztrációs billentyűt. |
| 7 | Meghívása HcsSetupWizard: Hiba történt az eszköz; regisztrálásakor. Ez lehet miatt nem a megfelelő IP-cím vagy a DNS-nevét. Ellenőrizze a hálózati beállításokat, és próbálkozzon újra. Ha a probléma továbbra is fennáll, [lépjen kapcsolatba a Microsoft-támogatást](storsimple-contact-microsoft-support.md). (Hiba 350050) | Győződjön meg arról, hogy az eszköz a külső hálózat pingelése. Ha nincs kapcsolat külső hálózatot, a regisztráció meghiúsulhat, az alábbihoz hasonló szövegű. Ez a hiba lehet kombinációi az alábbiak közül:<ul><li>Helytelen IP</li><li>Helytelen alhálózat</li><li>Hibás átjáró</li><li>Helytelen DNS-beállítások</li></ul> | A lépések [részletes hibaelhárítási példa](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Meghívása HcsSetupWizard: Az aktuális művelet [0x1FBE2] szolgáltatás belső hiba miatt sikertelen volt. A művelet valamikor után próbálkozzon újra. Ha a probléma továbbra is fennáll, lépjen kapcsolatba a Microsoft Support. | Ez a szolgáltatás vagy ügynökök az összes felhasználó nem látható hiba elő általános hiba. A leggyakoribb oka lehet, hogy a ACS hitelesítése sikerült. A lehetséges a hiba oka, hogy a NTP kiszolgáló beállításával kapcsolatos problémák merülnek fel, és időt az eszköz nincs megfelelően beállítva. | Javítsa ki az idő (Ha problémák merülnek fel), és próbálkozzon újra a regisztrációs művelet. Ha a Set-HcsSystem - időzóna paranccsal az időzóna beállítása, szókezdő nagybetűk az időzónát (például "óceáni idő").  Ha a probléma nem szűnik meg, [a partnert a Microsoft támogatási](storsimple-contact-microsoft-support.md) lépések. |
| 9 | Figyelmeztetés: Nem sikerült aktiválja az eszköz. Az eszköz rendszergazdáját és StorSimple pillanatkép Manager jelszavak nem változott. | Ha nem sikerül a regisztráció, az eszköz rendszergazdáját és StorSimple pillanatkép Manager jelszavak nem változnak. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>StorSimple telepítések hibaelhárítási eszközök

StorSimple kapcsolatos hibák elhárítása a StorSimple megoldás használható eszközöket tartalmaz. Ezek a következők:

- Támogatási csomag és eszköz naplók 
- A parancsmagok kifejezetten – hibaelhárítás 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Csomagok és eszköz naplók elérhető támogatása – hibaelhárítás

A támogatás csomag a vonatkozó naplókat, amely segíthet a Microsoft Support csapatának az eszköz problémák megoldásához. A Windows PowerShell-StorSimple létrehozni egy titkosított támogatási csomagot, majd megoszthat szakemberei is használhatja.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>A naplókat és a támogatási csomag tartalmának megtekintése

1. A támogatási csomag készítése a ismertetett módon StorSimple a Windows PowerShell használatával [létrehozása és kezelése a támogatási csomag](storsimple-create-manage-support-package.md).

2. Töltse le a [Visszafejtés parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) helyileg az ügyfélszámítógépen.

3. Ez az [eljárás lépésről lépésre](storsimple-create-manage-support-package.md#edit-a-support-package) segítségével megnyitása és visszafejtése a támogatási csomagot.

4. A támogatás visszafejtett csomag naplók esemény-nyomkövetés/etvx formátumban vannak. Az alábbi lépésekkel tekinteni a fájlokat a Windows eseménynaplójának végezheti el:
  1. A Windows-ügyfél **eventvwr** parancsot. Az Eseménynapló elindítja.
  2. Kattintson a **Műveletek** ablaktáblában kattintson a **Naplófájl megnyitása** , és mutasson a naplófájlok etvx/esemény-nyomkövetés formátuma (a támogatási csomagra). Megtekintheti a fájlt. Miután megnyitotta a fájlt, kattintson a jobb gombbal, és mentse a fájlt a szövegként.
   
    > [AZURE.IMPORTANT] A **Get-WinEvent** parancsmagot a fájlok megnyitása a Windows PowerShellben is használhatja. További tudnivalókért lásd: a [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) a Windows PowerShell-parancsmag hivatkozás dokumentációjában.

5. A naplókat meg az Eseménynapló nevet, a következő naplók, amelyeknél az eszköz konfigurációval kapcsolatos problémákat keresse:

  - hcs_pfconfig/működési napló
  - hcs_pfconfig/Config

6. A naplófájlok keressen rá a hívja meg a beállítási varázsló parancsmagok kapcsolódó karakterláncok. Lásd: az [első beállítási varázsló folyamat](#first-time-setup-wizard-process) parancsmagok listáját. 

7. Ha nem tudja megállapítása a probléma okát, akkor [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) a következő lépéseket. [Létrehozás támogatási kérelmet](storsimple-contact-microsoft-support.md#create-a-support-request) ismertetett lépésekkel hozhatja létre, ha a Microsoft Support kérjen segítséget.

## <a name="cmdlets-available-for-troubleshooting"></a>Elérhető az alábbiakkal parancsmagok

Az alábbi Windows PowerShell-parancsmagok használata csatlakozási hibák feltárása.

- `Get-NetAdapter`: Ezzel a parancsmaggal használható hálózati kapcsolatok állapotának észlelésére. 

- `Test-Connection`: Ezzel a parancsmaggal használható a hálózati kapcsolat belüli és kívüli a hálózat ellenőrzésére.

- `Test-HcsmConnection`: Ezzel a parancsmaggal használható sikeresen regisztrált eszköz kapcsolatot ellenőrzésére.

Ha az StorSimple eszközön futtatja a frissítés 1, a következő diagnosztikai parancsmagok is elérhetők.

- `Sync-HcsTime`: Ezzel a parancsmaggal megjelenítéséhez eszköz idő és használja kényszeríti NTP-kiszolgálóval való idő szinkronizálást.

- `Enable-HcsPing`és `Disable-HcsPing`: lehetővé teszi a hosts a hálózati kapcsolatok StorSimple eszközén pingelése parancsmagok használatával. Alapértelmezés szerint a StorSimple hálózati kapcsolatok nem ping kérelmekre válaszolni.

- `Trace-HcsRoute`: Ez a parancsmag használata egy nyomkövetési útvonalat eszközt. A végleges célhelyének úgy minden útválasztó egy időszakra vetített állapotával csomagok küld, és majd kiszámítja az egyes csomagok alapuló eredményeket. Mivel a `Trace-HcsRoute` csomagvesztés bármely adott útválasztó vagy a hivatkozás, az mértékét adja meg, mely útválasztó tudja vagy hivatkozások hálózati problémák okozhatják. 

- `Get-HcsRoutingTable`: Ezzel a parancsmaggal megjelenítéséhez használja a helyi IP-útválasztási tábla.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>A Get-NetAdapter parancsmag elhárítása

Hálózati kapcsolatok első eszköz telepítés konfigurálásakor a hardver állapota nem áll rendelkezésre az StorSimple kezelő szolgáltatás felhasználói felület mivel az eszköz van még nem regisztrált a szolgáltatással. Emellett a hardver állapota lap nem feltétlenül mindig megfelelően tükrözi állapotát az eszköz, különösen akkor, ha szinkronizálási szolgáltatás befolyásoló problémák lépnek fel. Ezekben az esetekben használhatja a `Get-NetAdapter` parancsmag határozza meg, az állapot és a hálózati kapcsolatok állapotát.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Az összes hálózati adapteren listájának megtekintéséhez az eszközön

1. Indítsa el a Windows PowerShell-StorSimple, és írja be `Get-NetAdapter`. 

2. Használja a kimenet: a `Get-NetAdapter` parancsmag és az alábbi útmutatásokat megérteni a hálózati kapcsolat állapotát.
  - Ha a kapcsolat megfelelő és engedélyezett, **ifIndex** állapota **mentése**másként jelenik meg.
  - Ha a felület megfelelő, de nem csatlakozik fizikailag (által a hálózati kábel le), a **ifIndex** **letiltott**formában jelenik meg.
  - Ha a kapcsolat megfelelő, de nincs engedélyezve, a **ifIndex** állapot **NotPresent**formában jelenik meg.
  - Ha a kapcsolat nem létezik, azt nem jelenik meg a listában. A felhasználói felület-StorSimple kezelő szolgáltatás továbbra is megjeleníti a kapcsolat egy hibás állapotú.

További információt a parancsmag használatát nyissa meg a [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) és a Windows PowerShell-parancsmag hivatkozás. 

Az alábbi szakaszok kimenetét minták megjelenítése a `Get-NetAdapter` parancsmag. 

 Ezeket a mintákat vezérlő 0 volt a passzív vezérlőre, és a konfigurálták, az alábbi képlettel történik:

- ADATOK 0, adatai 1, adatok 2 és 3 adatok hálózati felületek volt az eszközön.
- ADATOK 4-es és 5 adatok hálózati kártya nem volt jelen; Emiatt nem szerepelnek az eredményben.
- ADATOK 0 engedélyezték.

1 vezérlő az aktív vezérlő volt, és konfigurálták, az alábbi képlettel történik:

- ADATOK 0, adatok 1, adatok 2, adatok 3, adatok 4-es és adatok 5 hálózati kapcsolatok volt az eszközön.
- ADATOK 0 engedélyezték.

**Minta kimeneti – vezérlő 0**

Az alábbiakban kimenetét a vezérlő 0 (szenvedő vezérlőt). ADATOK 1, adatok 2 és 3 adatok nem csatlakozik. ADATOK 4-es és 5 adatok nem szerepelnek, mert azok nem szerepelnek az eszközön. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Minta kimeneti – 1 vezérlő**

Az alábbiakban kimenetét a vezérlő 1 (az aktív vezérlő). Csak az adatok 0 van konfigurálva a hálózati kapcsolat az eszközön és kezeléséről.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>A kapcsolat tesztelése parancsmag elhárítása

Használhatja a `Test-Connection` parancsmag határozza meg, hogy az StorSimple eszköz a külső hálózat csatlakozhat. Ha a hálózati paramétereket, beleértve a DNS-beállítási varázsló helyesen van beállítva, akkor használhatja a `Test-Connection` parancsmag egy ismert cím, például Outlook.com-fiókkal a hálózaton kívüli pingelése. 

Engedélyeznie kell a ping problémáinak kapcsolódási ezzel a parancsmaggal ha ping le van tiltva.

Lásd: az alábbi példák kimenetét a `Test-Connection` parancsmag. 

> [AZURE.NOTE] Az első mintában az eszköz egy helytelen DNS-sel van beállítva. DNS-Rekordokat a második minta helyességét.
 
**Minta kimeneti – helytelen DNS**

A következő példa nem nincs kimenet IPV4 és az IPv6-címek, amely azt jelzi, hogy nem oldja meg a DNS. Ez azt jelenti, hogy nincs kapcsolat a külső hálózatot, és a megfelelő DNS kell megadni. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Példa a kimeneti – DNS javítása**

Az alábbi példa a DNS ad vissza, a IPv4-cím, jelezve, hogy helyesen van-e beállítva a DNS-Rekordokat. Ez megerősíti, hogy van-e a külső hálózati kapcsolat. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>A próba-HcsmConnection parancsmag elhárítása

Használja a `Test-HcsmConnection` szolgáló már csatlakozik, és a StorSimple kezelő szolgáltatás regisztrált eszközt. Ezzel a parancsmaggal segít, hogy ellenőrizze a bejegyzett eszközök és a megfelelő StorSimple kezelő szolgáltatás közötti kapcsolatot. Ez a parancs a Windows PowerShell-StorSimple futtatása. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>A próba-HcsmConnection parancsmag futtatásához

1. Győződjön meg arról, hogy az eszköz regisztrálva van-e.

2. Az eszköz állapotának ellenőrzéséhez. Ha az eszköz inaktiválva, karbantartási módban, vagy offline állapotban, az alábbi hibák jelenhet meg: 

   - ErrorCode.CiSDeviceDecommissioned – Ez azt jelzi, hogy az eszköz nincs aktiválva.
   - ErrorCode.DeviceNotReady – Ez azt jelzi, hogy az eszköz karbantartási üzemmódban van.
   - ErrorCode.DeviceNotReady – Ez azt jelzi, hogy az eszköz nem érhető el.

3. Győződjön meg róla, hogy fut-e a StorSimple kezelő szolgáltatás (a [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) parancsmag használata). Ha a szolgáltatás nem fut, az alábbi hibák jelenhet meg:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – Ez azt jelzi, hogy történt kivétel Get-ClusterResource futtatásakor.

4. Jelölje be az Access-vezérlő szolgáltatást (ACS) jogkivonathoz. Ha webes kivételt okoz azt, akkor lehetséges átjáró probléma, a hiányzó proxyhitelesítés, egy helytelen DNS vagy hitelesítési hiba eredményét. Az alábbi hibák jelenhetnek meg:

   - ErrorCode.CiSApplianceGateway – Ez azt jelzi, HttpStatusCode.BadGateway kivételt: a név feloldó szolgáltatás nem sikerült az állomásnév feloldani. 
   - ErrorCode.CiSApplianceProxy – Ez azt jelzi, HttpStatusCode.ProxyAuthenticationRequired kivételt (HTTP állapotkód 407): az ügyfélnek nem sikerült hitelesíteni a proxykiszolgáló használata. 
   - ErrorCode.CiSApplianceDNSError – Ez azt jelzi, egy WebExceptionStatus.NameResolutionFailure kivétel: a név feloldó szolgáltatás nem sikerült az állomásnév feloldani.
   - ErrorCode.CiSApplianceACSError – Ez azt jelzi, hogy a szolgáltatás hitelesítési hibát adja vissza, de nincs kapcsolat.
   
    Ha ez nem lépett egy webes kivétel, keressen a ErrorCode.CiSApplianceFailure. Ez azt jelzi, hogy a készülék sikertelen volt.

5. Jelölje be a felhőalapú szolgáltatás kapcsolat. Ha a szolgáltatás webes kivételt okoz, az alábbi hibák jelenhet meg:

  - ErrorCode.CiSApplianceGateway – Ez azt jelzi, HttpStatusCode.BadGateway kivételt: egy köztes proxykiszolgáló egy másik proxy vagy az eredeti kiszolgálótól kapott hibás kérés.
  - ErrorCode.CiSApplianceProxy – Ez azt jelzi, HttpStatusCode.ProxyAuthenticationRequired kivételt (HTTP állapotkód 407): az ügyfélnek nem sikerült hitelesíteni a proxykiszolgáló használata. 
  - ErrorCode.CiSApplianceDNSError – Ez azt jelzi, egy WebExceptionStatus.NameResolutionFailure kivétel: a név feloldó szolgáltatás nem sikerült az állomásnév feloldani.
  - ErrorCode.CiSApplianceACSError – Ez azt jelzi, hogy a szolgáltatás hitelesítési hibát adja vissza, de nincs kapcsolat.
  
    Ha ez nem lépett egy webes kivétel, keressen a ErrorCode.CiSApplianceSaasServiceError. Ez a probléma a StorSimple kezelő szolgáltatás mutatja.
 
6. Ellenőrizze a Azure Service Bus kapcsolatot. ErrorCode.CiSApplianceServiceBusError azt jelzi, hogy az eszköz nem tud csatlakozni a szolgáltatás Bus.
 
A naplófájlok CiSCommandletLog0Curr.errlog, és CiSAgentsvc0Curr.errlog lesz további információt, például a kivétel részleteit. 

További információ arról, hogy miként parancsmag, nyissa meg a [Próba-HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) a Windows PowerShell dokumentáció hivatkozást.

> [AZURE.IMPORTANT] Ezzel a parancsmaggal az aktív, mind a passzív vezérlő futtatását is lehetővé teszi. 
 
Lásd: az alábbi példák kimenetét a `Test-HcsmConnection` parancsmag. 

**Példa a kimeneti – StorSimple verzióját futtatja sikeresen regisztrált eszköz (2014-es július)**

Az első mintában származik, de nincs csatlakozási problémákat tapasztal a StorSimple kezelő szolgáltatás sikeresen regisztrált és az eszközt. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Minta kimeneti – sikeresen regisztrált eszköz futtatása StorSimple frissítés 1**

Ha frissítés 1 futtatja a StorSimple eszközén, nincs szüksége lesz a részletes kapcsolóval futtatásához.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Példa a kimeneti – StorSimple verzióját futtatja a kapcsolat nélküli eszköz (2014-es július)**

Ez a példa a eszközről, amelynek az Azure klasszikus portálon **Offline** állapotban van.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Az eszköz nem sikerült csatlakozni, az aktuális webhely proxy konfiguráció használata. Ennek oka lehet a webes proxy beállítások vagy a hálózati kapcsolat problémája problémát. Ebben az esetben gondoskodnia kell arról, hogy a webes proxybeállításokat is helyes, és a webes proxykiszolgálók pedig online érhető el. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>A szinkronizálási-HcsTime parancsmag elhárítása

Ezzel a parancsmaggal használható meg az eszköz időt. Ha az eszköz idő NTP-kiszolgálóval való eltolás, majd segítségével ezzel a parancsmaggal hatályba – az idő szinkronizálása NTP kiszolgálóját. Ha az eszköz és NTP-kiszolgáló között az eltolás nagyobb, mint az 5 perc, egy figyelmeztetés jelenik meg. Ha az eltolás meghaladja a 15 percet, majd az eszköz fog offline módba lépne. Továbbra is használhatja a parancsmag kényszerítése idő szinkronizálást. Jó helyen jár Ha az eltolás meghaladja a 15 óra, majd nem tudja hatályba szinkronizálása az időt és a megjelenő hibaüzenet jelenik meg.

**Minta kimeneti – kényszerített idő szinkronizálás a szinkronizálási-HcsTime használatával**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Az Enable-HcsPing és letiltása-HcsPing parancsmagokat – problémamegoldás

A parancsmagok segítségével győződjön meg arról, hogy a hálózati kapcsolatokat az eszközön az ICMP ping kérelmekre válaszolni. Alapértelmezés szerint a StorSimple hálózati kapcsolatok nem ping kérelmekre válaszolni. Ez a parancsmag használata a legkönnyebben úgy, hogy ha az eszköz online állapotú, és elérhető.  

**Minta – engedélyezése – HcsPing és letiltása-HcsPing kimeneti**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>A nyomkövetés-HcsRoute parancsmag elhárítása

Ezzel a parancsmaggal használata egy nyomkövetési útvonalat eszközt. A végleges célhelyének úgy minden útválasztó egy időszakra vetített állapotával csomagok küld, és majd kiszámítja az egyes csomagok alapuló eredményeket. A parancsmag csomagvesztés bármely adott útválasztó vagy hivatkozás a mértékét jeleníti meg, mert tudja, hogy melyik útválasztó, vagy előfordulhat, hogy a hivatkozások hálózati problémát okozó.

**Nyomon követheti a nyomkövetés-HcsRoute csomagot az útvonal ábrázoló minta kimeneti**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>A Get-HcsRoutingTable parancsmag elhárítása

Ez a parancsmag használatával az útválasztási táblázat StorSimple mobileszközére megtekintése. Útválasztási táblázat, amely segít a határozza meg, ahol adatokat csomagok út közben az Internet Protocol (protokoll) (IP-) hálózaton irányítja szabályhalmaz. 

Útválasztási táblázatban látható, a kapcsolatok és az átjáró, amely a megadott hálózatok továbbítja az adatokat. Is kínál a útválasztási mérőszám, amely a döntési maker az elérési út elérni egy adott célt venni. Az alsó a útválasztási mérőszám, annál nagyobb a beállítás. 

Ha például 2 hálózati kapcsolatok esetén adatok 2 és 3 adatok csatlakozik az internethez. Esetén a útválasztási adatok 2 és 3 adatok mérési módja miatt 15 és 261 a kurzor az alsó útválasztási metrikus adatok 2 a használni kívánt felület eléréséhez az internethez.

Frissítés 1 StorSimple eszközén futtat, ha az adatok 0 hálózati kapcsolatnak a legmagasabb preferencia-sorrend az a felhő forgalmat. Ez azt jelenti, hogy akkor is, ha vannak más felhő engedélyezni kapcsolatok, a felhőben forgalom lesz átirányítva 0 adatai között. 

Ha a `Get-HcsRoutingTable` parancsmag megadása nélkülire paramétereket (mint az alábbi példában látható), a parancsmag kimeneti fog IPv4 és az IPv6 útválasztási táblák. Azt is megteheti, megadhatja `Get-HcsRoutingTable -IPv4` vagy `Get-HcsRoutingTable -IPv6` megszerezni a megfelelő útválasztási táblához.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Részletes StorSimple hibaelhárítási példa

A következő példa bemutatja, hogy egy StorSimple telepítési lépésenkénti hibaelhárítás. Példa esetben eszköz regisztrációs sikertelen, és egy hiba üzenet azt jelzi, hogy a hálózati beállítások vagy a DNS-neve helytelen.

A visszaadott hibaüzenet a következő:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

A hiba oka lehet által az alábbiak egyikét:

- Helytelen hardver telepítése
- Hibás hálózati viteldíjjelző illesztőegységeihez
- Helytelen IP-címet, alhálózat maszk, átjáró, elsődleges DNS-kiszolgáló vagy web-proxy
- Helytelen regisztrációs kulcs
- Helytelen tűzfal beállításai

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Keresse meg és a eszköz regisztrációs probléma megoldásához

1. Az eszköz konfigurációjának ellenőrzése: az aktív vezérlő futtatása `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] A beállítási varázsló az aktív vezérlő kell futtatnia. Ha ellenőrizni szeretné, hogy az aktív vezérlő csatlakozik, tekintse meg a szalagcímen a soros konzol a található. A szalagcímen azt jelzi, hogy csatlakozva van vezérlő 0 és 1 vezérlő, és hogy a vezérlő aktív vagy passzív. További információért lépjen [azonosítása egy aktív vezérlő az eszközön](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Győződjön meg arról, hogy az eszköz megfelelően van-e csatlakoztatva: jelölje be a hálózati kábelek vissza sík az eszközön. A kábelek az eszköz adatmodellhez. További információért lépjen [a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md).

     > [AZURE.NOTE] Ha 10 GbE hálózati portok használata esetén szüksége lesz használja a megadott QSFP-SFP adaptereken és SFP kábelek. További tudnivalókért lásd: a [kábelek, a kapcsolók, és a OEM szállító Mellanox portok által javasolt adó listája](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. A hálózati kapcsolat állapotának ellenőrzése:

   - A Get-NetAdapter parancsmag használata a hálózati kapcsolatok állapotának észlelésére adatok 0. 
   - Ha a hivatkozás nem működik, **ifindex** állapotát jelzi, hogy a felületen nem működik. Ezután jelölje be a port a készülék pedig a Váltás a hálózati kapcsolat kell. Akkor is kell zárja ki a hibás kábelek. 
   - Ha azt gyanítja, hogy az adatok 0 nem sikerült az aktív vezérlő portjához győződhet meg az adatok 0 portjához 1 vezérlő útján. Ennek ellenőrzéséhez a hálózati kábel le az eszköz a vezérlő 0 hátterében, csatlakoztassa a kábelét vezérlő 1, és futtassa újra a Get-NetAdapter parancsmag. 
   Ha az adatok 0 portjához egy vezérlő nem sikerül, [forduljon a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) a következő lépéseket. Szükség lehet a rendszer a vezérlő cseréje.
 
4. A Váltás a kapcsolatának ellenőrzéséhez:
   - Győződjön meg arról, hogy adatokat 0 hálózati kapcsolatok vezérlő 0 és 1 adatkezelő az elsődleges ház az azonos alhálózat. 
   - Jelölje be a központi vagy útválasztóhoz. Mindkét vezérlők csatlakozzon általában az azonos központi vagy az útválasztó. 
   - Győződjön meg arról, hogy van-e a kapcsolatot használja a kapcsolók adat 0 mindkét vezérlők az azonos virtuális.
   
5. A felhasználó hibák megszüntetése:

  - Futtassa a beállítási varázsló újra (futtatása **Invoke-HcsSetupWizard**), és adja meg ismét a győződjön meg arról, hogy nincsenek-e hibák az értékeket. 
  - Ellenőrizze a regisztráció használt billentyűt. Az azonos regisztrációs kulcsa StorSimple kezelő szolgáltatás csatlakozhat különféle eszközökön használható. Az [első a szolgáltatás regisztrációs billentyűt](storsimple-manage-service.md#get-the-service-registration-key) az eljárás használatával győződjön meg arról, hogy a helyes regisztrációs kulcsa használ.

    > [AZURE.IMPORTANT] Ha több szolgáltatás fut, szüksége lesz annak érdekében, hogy használt-e a regisztrációs billentyűt a megfelelő szolgáltatás regisztrálni az eszközt. Ha egy eszközt a megfelelő StorSimple Manager szolgáltatással van regisztrálva, szüksége lesz [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) a következő lépéseket. Előfordulhat, hogy végezhető el a gyári alaphelyzetbe állítása az eszköz (amely az adatok elvesztését eredményezhet), majd csatlakoztassa a tervezett szolgáltatást.

6. A kapcsolat tesztelése parancsmag használatával ellenőrizheti, hogy a külső hálózat kapcsolatot. További információért lépjen [a kapcsolat tesztelése parancsmagot a hibaelhárítás](#troubleshoot-with-the-test-connection-cmdlet).

7. Ellenőrizze, hogy a tűzfal zavaró. Ellenőrizte, amely a virtuális IP (virtuális), az alhálózathoz, az átjáró és a DNS-beállítások az összes helyességét, továbbra is látni fogja csatlakozási problémákat tapasztal, majd lehetséges, hogy a tűzfal blokkolja a és a külső hálózat közötti kommunikációt. Szüksége annak érdekében, hogy 80-as és 443-as StorSimple eszközén kimenő kommunikáció számára érhető el. További tudnivalókért olvassa el a [hálózati StorSimple eszközére vonatkozó követelmények](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)című témakört.

8. Nézze meg a naplókat. Nyissa meg a [támogatási csomag és eszköz naplók hibaelhárítási érhető el](#support-packages-and-device-logs-available-for-troubleshooting).

9. Ha a fenti lépések nem oldja meg a problémát, segítségért [forduljon a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) .

## <a name="next-steps"></a>Következő lépések
[Megtudhatja, hogy miként egy műveleti eszköz hibáinak elhárítása](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
