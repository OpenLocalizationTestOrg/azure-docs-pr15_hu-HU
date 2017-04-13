
<properties
    pageTitle="Azure Active Directory használatával csatlakozás állapota az AD FS |} Microsoft Azure"
    description="Ez az az Azure Active Directory csatlakozás állapota oldal figyelése a helyszíni Active Directory összevonási szolgáltatások infrastruktúra."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Azure Active Directory használatával kapcsolatba állapot Active Directory összevonási szolgáltatások
A következő dokumentáció az az Active Directory összevonási szolgáltatások infrastruktúra az Azure Active Directory csatlakozás állapot ellenőrzése. Azure AD Connect (szinkronizálás) az Azure Active Directory csatlakozás rendszerállapot figyelése a további tudnivalókért lásd [Használatával Azure Active Directory csatlakozás állapota szinkronizálás](active-directory-aadconnect-health-sync.md). Ezenkívül Active Directory tartományi szolgáltatások az Azure Active Directory csatlakozás rendszerállapot figyelése a további tudnivalókért lásd [Használatával Azure Active Directory csatlakozás állapota az Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Active Directory összevonási szolgáltatások vonatkozó értesítések
Az Azure Active Directory csatlakozás állapot riasztások szakaszban a aktív figyelmeztetések listája találja. Minden riasztás vonatkozó információkat, a megoldási lépéseinek és a kapcsolódó dokumentáció mutató hivatkozásokat tartalmaz.

Aktív vagy megoldani, hogy jelzést egy új lap további információkkal, nyissa meg a lépéseket a riasztás és a hivatkozások úgy, hogy a megfelelő dokumentumokat eljuttatja duplán kattinthat. Is megtekintheti figyelmeztetések, amelyek a múltbeli megoldott korábbi adatok.

![Azure AD Connect egészségügyi portál](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Active Directory összevonási szolgáltatások használati elemzéséhez
Azure Active Directory csatlakozás állapot használati elemzi a hitelesítési forgalom az összevonási kiszolgálók. Kattintson duplán a használatát analytics párbeszédpanelen számos mértékek és csoportokból kiindulva láthatók használatát analytics lap megnyitásához.

>[AZURE.NOTE] Active Directory összevonási szolgáltatások használati használatához győződjön meg arról, hogy engedélyezve van-e az Active Directory összevonási szolgáltatások naplózás. További tudnivalókért olvassa el a [Active Directory összevonási szolgáltatások naplózásának engedélyezése](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs)című témakört.

![Azure AD Connect egészségügyi portál](./media/active-directory-aadconnect-health/report1.png)

Válassza a további metrikus, adjon meg egy időtartományt, vagy módosíthatja a csoportosítás, kattintson a jobb gombbal a használati analitikát diagram, és válassza ki a diagram szerkesztése. Ezután adja meg a időtartomány, jelölje be a másik metrikus, és módosítsa a csoportosítás. Az eloszlás értékét különböző "mérési módja miatt" hitelesítési forgalom megtekintése, és minden metrikus, az alábbi táblázatban ismertetett megfelelő "Csoportosítás" paramétereket alkalmaz a csoport:

| Metrikus | Csoportosítási szempont | Azt jelenti, hogy milyen a csoportosítás, és miért célszerű? |
| ------ | -------- | -------------------------------------------- |
| Teljes kér: Száma az összevonási szolgáltatás által feldolgozott kérések | Az összes | Csoportosítás nélkül kérések száma számát mutatja. |
|  | Alkalmazás | Csoportok kérések teljes a célként megadott megbízó fél alapján. A csoportosítás akkor hasznos, ha meg szeretné érteni, hogy melyik alkalmazás az összes forgalom mennyi százalékos postaládájába. |
|  | Kiszolgáló | Csoportok kérések teljes alapján a kiszolgáló által feldolgozott a kérést. A csoportosítás akkor hasznos, a teljes forgalom terheléselosztási megértéséhez. |
|  | Munkahelyi illesztés | Csoportok a kérések teljes alapján, hogy lesz azokat az eszközökről, amelyek az illesztés munkahelye (ismert). A csoportosítás akkor hasznos, ha meg szeretné érteni, ha az erőforrások érhetők el, amelyek ismeretlen, az Identitáskezelés infrastruktúra-eszközt használva. |
|  | Hitelesítési módszer | Csoportok kérések teljes hitelesítéshez használt hitelesítési módszer alapján. A csoportosítás akkor lehet hasznos hitelesítéshez használt kap közös hitelesítési mód megértéséhez. Az alábbiakban a lehetséges hitelesítési módszereket <ol> <li>Windows-hitelesítés (Windows)</li> <li>Űrlapalapú hitelesítés (űrlap)</li> <li>Egyszeri bejelentkezés (egyszeri bejelentkezéses)</li> <li>X509 tanúsítvány-hitelesítés (tanúsítvány)</li> <br>Ha az összevonási kiszolgálók kapja meg a kérelmet,-féle egyszeri Bejelentkezést cookie-k, kérelem számít egyszeri bejelentkezés (egyszeri bejelentkezés) Ebben az esetben ha a cookie-k érvényes, a a felhasználó nem kéri hitelesítő adatait, és zökkenőmentesen elvégezhető az alkalmazás hozzáférést kap. Ez a probléma közös, ha több megbízó felek védi az összevonási kiszolgálók van. |
|  | Hálózati hely | Csoportok kérések teljes annak a felhasználónak a hálózati hely alapján. Lehet vagy intranetes vagy extranetes. A csoportosítás célszerű tudni fogja, a forgalom hány százaléka intranetes vagy extranetes. |
| Nem sikerült kérések teljes: A teljes oldalszámot az összevonási szolgáltatás által feldolgozott kérések sikertelen volt. <br> (Ez metrikus megtalálható csak a Windows Server 2012 R2 rendszer AD FS)| Hiba típusa | Előre definiált hiba alapuló hibák száma látható. A csoportosítás akkor hasznos lehet ahhoz, hogy a gyakori hibák típusai. <ul><li>Helytelen felhasználónevet és jelszót: hibák miatt nem a megfelelő felhasználónevet és jelszót.</li> <li>"Extranetes zárolása": hibák miatt a kérések kapott extranetes át lett zárolva felhasználóról </li><li> "A jelszó lejárt": hibák miatt felhasználók naplózása be egy lejárt jelszavával.</li><li>"A fiók letiltva": hibák miatt letiltott fiókkal bejelentkező felhasználók.</li><li>"Eszköz hitelesítési": hibák miatt a felhasználók adatkapcsolat hitelesítést végezni az eszköz hitelesítés használatával.</li><li>"Felhasználóhitelesítés tanúsítvány": hibák miatt a felhasználók adatkapcsolat-érvénytelen tanúsítvány miatt hitelesítést végezni.</li><li>"MFA": hibák miatt felhasználó sikertelenül hitelesítést végezni, többtényezős hitelesítés használatával.</li><li>"Egyéb hitelesítő": "Kiadási engedélyezési": engedélyezési hibák miatt sikertelen.</li><li>"Kiállítási meghatalmazás": kiállítási meghatalmazás hibák miatt sikertelen.</li><li>"Token elfogadó": hibák miatt a jogkivonat egy harmadik fél identitásszolgáltató elvetésével ADFS.</li><li>"Protocol": protocol hibák miatt sikertelen.</li><li>"Ismeretlen": elfog minden. Más hibák, amelyek nem illeszkednek a definiált kategóriába sorolhatók.</li> |
|  | Kiszolgáló | Csoportok a hibák, a kiszolgáló alapján. A csoportosítás akkor lehet hasznos lehet áttekinteni a hiba eloszlás kiszolgálóin. Páratlan terjesztési lehet egy hibás állapotú kiszolgáló mutatója. |
|  | Hálózati hely | Csoportok a hibák, a hálózati helyet a kérelmek (intranetes viewben extranetes) alapján. A csoportosítás akkor lehet hasznos, amely nem működnek kérések típusú megértéséhez. |
|  | Alkalmazás | Csoportok a hibák, a célként megadott alkalmazás (megbízó fél) alapján. A csoportosítás akkor hasznos, ha meg szeretné érteni, hogy melyik célzott alkalmazás lát hibák a legtöbb száma. |
| Az egyedi a rendszer az aktív felhasználók felhasználószám: átlagos száma | Az összes | Ez a mérőszám átlagos száma az összevonási szolgáltatás használata a kijelölt időtartomány szeletet felhasználók számának biztosít. A felhasználók nincsenek csoportosítva. <br>Az átlag attól függ, hogy a kijelölt szeletre idő. |
|  | Alkalmazás | Csoportok átlagos száma felhasználókat a célként megadott alkalmazás (megbízó fél) alapján. A csoportosítás akkor hasznos, ha meg szeretné érteni, hogy hány felhasználóm melyik alkalmazást használ. |


## <a name="performance-monitoring-for-ad-fs"></a>Teljesítményét Active Directory összevonási szolgáltatások ellenőrzése
Azure Active Directory csatlakozás állapot teljesítményét figyelve információk felügyeleti mérési módja miatt. Jelölje be a figyelés jelölőnégyzetet, megnyílik egy új lap részletes információkat a mérési módja miatt.


![Azure AD Connect egészségügyi portál](./media/active-directory-aadconnect-health/perf1.png)


A lap tetején a lehetőség kiválasztásával kiszolgálón, hogy egy adott kiszolgáló mértékek szerint is szűrheti. Mértékek módosításához kattintson a jobb gombbal a megfigyeléssel diagram csoportban az ellenőrzési lap, és válassza a diagram szerkesztése. Majd a fel a megnyíló új lap, az a legördülő listában válassza a további metrikus, és adjon meg egy időtartományt a teljesítményadatok megtekintése.

## <a name="reports-for-ad-fs"></a>Jelentések, az Active Directory összevonási szolgáltatások
Azure Active Directory csatlakozás állapot itt észleléséről készült jelentések megtekinté tevékenység és AD FS teljesítményét. Ezeket a jelentéseket a rendszergazdák az AD FS-kiszolgálókon tevékenységek bepillantást segítségével.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>A sikertelen felhasználónevével és jelszavával bejelentkezések 50 felhasználó legfontosabb

A gyakori okai az AD FS-kiszolgáló sikertelen hitelesítési kérelmek egy kérelmet érvénytelen hitelesítő adatokkal, ez azt jelenti, hogy a megfelelő felhasználónevet és a jelszót. Összetett jelszavakat, elfelejtett jelszó és írta miatt felhasználók általában történik.

De más oka lehet annak, hogy az AD FS-kiszolgálók, például: kezelt kérések helytelen számú eredményezheti: olyan alkalmazás, amely gyorsítótárát felhasználói hitelesítő adatokat, és a hitelesítő adatok lejár vagy rosszindulatú felhasználó szeretne jelentkezni a jól ismert jelszavak adatsorral-fiókba. Ezek a példák két okból érvényes, amely egy hullámzó jellegű összehívásokban vezethet.

ADFS az Azure Active Directory csatlakozás állapot felső 50 felhasználó érvénytelen felhasználónevet és jelszót miatt sikertelen bejelentkezések a jelentést tartalmaz. Ez a jelentés megvalósítani a a farmok összes AD FS-kiszolgáló által létrehozott események feldolgozása

![Azure AD Connect egészségügyi portál](./media/active-directory-aadconnect-health-adfs/report1a.png)

Ez a jelentés belül könnyen elérheti a következő információkat vehet van:

- Az elmúlt 30 nap megfelelő felhasználónevével és jelszavával sikertelen kérések száma
- Átlagos száma a felhasználóknál, akik nem sikerült egy hibás felhasználónevével és jelszavával jelentkezzen be a naponta.


Ebben a részben kattintás megnyitja a fő jelentés lap, ahol további részleteket. Ez a lap trendfigyelésre információt találhat a megfelelő felhasználónevet és jelszót kérések kapcsolatos alapterv létrehozása egy diagramot tartalmaz. Ezenkívül biztosít a felső 50 Felhasználólista számára sikertelen kísérletek száma.

A diagram az alábbi információk találhatók:

- A hibás felhasználónevével és jelszavával nap alapon miatt sikertelen bejelentkezések száma.
- A sikertelen bejelentkezések nap alapon egyedi felhasználók száma.
- Ügyfél IP-címét az utolsó kérés

![Azure AD Connect egészségügyi portál](./media/active-directory-aadconnect-health-adfs/report3a.png)

A jelentés az alábbi információk találhatók:

| Jelentés elem | Leírás
| ------ | -------- |
|Felhasználói azonosító| A használt felhasználói azonosító látható. Ez az érték a felhasználó begépelt, amely bizonyos esetekben-e a megfelelő felhasználói azonosító használatban.|
|Sikertelen kísérletek| Sikertelen kísérletek száma látható, hogy bizonyos felhasználói azonosítójával. A táblázat a legtöbb számú sikertelen kísérletek csökkenő sorrendben vannak rendezve.|
|Utolsó hiba| A időbélyeg akkor jeleníti meg, ha az utolsó hiba történt.
|Hiba utolsó IP | Az ügyfél IP-címét a legújabb hibás kérés látható.|



>[AZURE.NOTE] Ez a jelentés automatikusan frissül az új információkkal két óránként összegyűjtése adott idő alatt után. Emiatt az utolsó két órán belül bejelentkezések nem szerepelhetnek a jelentésben.



## <a name="related-links"></a>Kapcsolódó hivatkozások

* [Azure AD Connect állapota](active-directory-aadconnect-health.md)
* [Azure AD Connect állapot ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect állapot műveletek](active-directory-aadconnect-health-operations.md)
* [Azure Active Directory csatlakozás állapota szinkronizálás használata](active-directory-aadconnect-health-sync.md)
* [Azure Active Directory segítségével kapcsolatba állapot Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect állapot – gyakori kérdések](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect állapot korábbi verziók](active-directory-aadconnect-health-version-history.md)
