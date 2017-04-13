<properties
    pageTitle="Azure Active Directory identitás védelmet szószedet |} Microsoft Azure"
    description="Azure Active Directory identitás védelem szószedet"
    services="active-directory"
    keywords="Azure active directory identitás védelmét, cloud app feltárás, alkalmazásokat, biztonsági, kockázat, kockázat szint, rés, biztonsági házirendje, szószedet kezelése"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory identitás védelem szószedet 


### <a name="at-risk-user"></a>A kockázatok (felhasználói)  
Egy vagy több aktív kockázati esemény rendelkező felhasználó. 

### <a name="atypical-sign-in-location"></a>Atipikus bejelentkezés helye   
A bejelentkezés, amely nem az adott felhasználó, hasonló felhasználóknak vagy a bérlő tipikus földrajzi helytől.

### <a name="azure-ad-identity-protection"></a>Azure Active Directory-identitás-védelem    
Biztonsági modul az Azure Active Directory kockázati esemény, és egy szervezet identitások érintő potenciális biztonsági összevont nézetben.

### <a name="conditional-access"></a>Feltételes hozzáférés  
Egy házirendet, az erőforrások való hozzáférés biztonságossá tétele. Feltételes hozzáférési szabályokat az Azure Active Directory vannak tárolva, és az erőforráshoz való hozzáférés engedélyezése előtt Azure Active Directory értékeli ki.  Példa szabályokat tartalmaz, a felhasználói hely alapján hozzáférés korlátozása eszköz állapot vagy a felhasználó hitelesítési módszert.

### <a name="credentials"></a>Hitelesítő adatok     
Azonosításra és vásárlási azonosítása és való hozzáférés a helyi hálózati erőforrások használt információkat. Hitelesítő adatok példák a felhasználóneveket és jelszavakat, intelligens kártya és a tanúsítványok.

### <a name="event"></a>Esemény   
Az Azure Active Directory tevékenység rögzíteni.

### <a name="false-positive-risk-event"></a>Hamis pozitív (kockázati esemény) 
Az identitás védelmet felhasználója, jelezve, hogy a kockázati esemény lett vizsgálni, és nem megfelelően lett megjelölve kockázati esemény manuálisan állítsa kockázati esemény állapota.

### <a name="identity"></a>Azonosító    
Személy vagy entitás, amely a hitelesítés, például a jelszó vagy a tanúsítvány feltételek alapján történő ellenőriznünk kell.

### <a name="identity-risk-event"></a>Identitás kockázati esemény     
AAD esemény lett megjelölve anomalous identitás Protection és jelezheti, hogy az identitás napvilágra kerülhetett.

### <a name="ignored-risk-event"></a>Figyelmen kívül hagyja a (kockázati esemény)    
Az identitás-védelem felhasználója, jelezve, hogy a kockázati esemény anélkül, hogy egy remediation műveleteket végezhet bezárta manuálisan állítsa kockázati esemény állapota.

### <a name="impossible-travel-from-atypical-locations"></a>Lehetséges utazási atipikus helyekről   
Kockázati esemény indul el, ha két bejelentkezési-bővítmények ugyanazt a felhasználót a észlel, ahol legalább az egyik helyről atipikus bejelentkezési, és ha a bejelentkezési bővítmények között eltelt idő rövidebb, mint a minimális időt szeretné tartani a fizikailag utazási ezekre a helyekre között.  

###<a name="investigation"></a>Vizsgálat    
A tevékenységek, naplók és egyéb vonatkozó információk döntse el, hogy remediation vagy kezelési lépések szükségesek, kockázati esemény véleményezési eljárás megértéséhez, ha, és hogyan az identitás jutott, és a biztonságos identitás használatának ismertetése.

### <a name="leaked-credentials"></a>Elvesztett hitelesítő adatok  

Ha az aktuális felhasználói hitelesítő adatokkal (felhasználónév és jelszó) talál közzétett nyilvánosan sötét a webhelyen a kutatók által indított kockázati esemény.

### <a name="mitigation"></a>Kezelési  
Korlátozni vagy támadó lehetőségét, hogy egy biztonságos identitás vagy eszköz kihasználhatja a identitás vagy eszközön biztonságos állapotba visszaállítással kiküszöbölheti művelet. A kezelési nem oldja meg a korábbi, az Identitáskezelés vagy más eszközhöz kapcsolatos kockázat események.

### <a name="multi-factor-authentication"></a>Többtényezős hitelesítés 
Egy hitelesítési módszer, amely csak két vagy több hitelesítési módszereket, amely tartalmazhat valamit, amit a felhasználó van, például egy tanúsítványt; egy üzenetet a felhasználó tudja, például a felhasználóneveket, a jelszavak vagy fázis mondatok; fizikai tulajdonságait, például egy ujjlenyomat; és a személyes tulajdonságait, például egy személyes aláírását.

### <a name="offline-detection"></a>Kapcsolat nélküli kimutatására   
Rendellenességeit és egy eseményt, például a bejelentkezési kísérletet a kockázata értékelése az esemény, amely már tartalmaz történt utólag kimutatása.

### <a name="policy-condition"></a>Házirend feltétel    
A biztonsági szabályt, amely meghatározza az a (csoportok, felhasználók, alkalmazások, eszköz platformokon, eszköz államok, IP-tartományokat, lekérdezésiügyfél-típusok) szerepel a házirend vagy szervezetek zárni egy részét.

### <a name="policy-rule"></a>Házirend szabály     
A biztonsági szabályt, amely ismerteti, amelyek a házirendet, és a házirend elindításakor a megtett szeretné elindítani az esetekben része.

### <a name="prevention"></a>Megelőzése  
Művelet sérülések megakadályozására visszaélés-azonosító vagy eszközt a szervezet gyanús vagy sérül kell tudnia. Egy megelőzése műveletet az eszközt, vagy az Identitáskezelés nem biztonságos, és a lépések nem oldották meg az előző kockázati esemény.

### <a name="privileged-user"></a>A védett (felhasználói)
Egy felhasználó kockázati esemény, időben állandó vagy ideiglenes rendszergazdai engedélyekkel egy vagy több erőforrás volt az Azure Active Directory, például egy globális rendszergazdai számlázási adminisztrátor, a szolgáltatás-rendszergazda, a felhasználónak rendszergazdai és jelszókezelő. 

###<a name="real-time"></a>Valós idejű    
Lásd: valós idejű észlelési.

###<a name="real-time-detection"></a>Valós idejű kimutatására  
Rendellenességeinek kimutatása és a kockázata egy eseményt, például az esemény előtt bejelentkezési kísérletet értékelése engedélyezett folytatásához.

### <a name="remediated-risk-event"></a>Remediated (kockázati esemény)     
Identitás védelmét, jelezve, hogy a kockázati esemény remediated volt ilyen típusú kockázati esemény szabványos remediation művelettel automatikusan beállítja kockázati esemény állapota. Ha például az, amikor a felhasználó jelszavának alaphelyzetbe állítása sok kockázati esemény, amely jelzi, hogy az előző jelszót jutott is automatikusan remediated.

### <a name="remediation"></a>Remediation     
Művelet biztonságossá tétele identitás vagy korábban gyanús vagy kell sérül ismert eszközt. Egy remediation művelet visszaállítása a identitás vagy eszközön biztonságos állapotba, és úgy oldja fel az azonosító vagy más eszközhöz tartozó korábbi kockázat események.

### <a name="resolved-risk-event"></a>A rendszer (kockázati esemény)   
A kockázat esemény állapot beállítása manuálisan az identitás-védelem felhasználó által zárni, jelezve, hogy a felhasználó PERT-megfelelő remediation kívüli azonosító adatok védelmét, és, hogy a kockázati esemény kell figyelembe venni.

###<a name="risk-event-status"></a>Kockázati esemény állapota    
Kockázati esemény tulajdonsága, jelezve, hogy az esemény aktív, és ha megnyitva, az az oka bezárása.

###<a name="risk-event-type"></a>Kockázati esemény típusa  
A kockázati esemény, jelezve az esemény figyelembe veendő kockázatos okozó rendellenességet típusú kategóriát.

###<a name="risk-level-risk-event"></a>A kockázat szint (kockázati esemény)  
A műveletek fontossági identitás védelem segítségnyújtás érdekében a kockázati esemény súlyosságát megjelölése (magas, közepes vagy alacsony) venniük a kockázatok csökkentésére a szervezet számára. 

###<a name="risk-level-sign-in"></a>A kockázat szint (bejelentkezés) 
Feltüntetése (magas, közepes vagy alacsony) annak valószínűsége, hogy egy adott bejelentkezés, az valaki más próbálja használni a felhasználói azonosító.

###<a name="risk-level-user-compromise"></a>A kockázat szint (felhasználói biztonságos) 
Feltüntetése (magas, közepes vagy alacsony) annak valószínűsége, hogy az identitás napvilágra kerülhetett.

### <a name="risk-level-vulnerability"></a>A kockázat szint (biztonsági)  
A műveletek rangsorolása identitás védelem segítségnyújtás érdekében a rés súlyosságát megjelölése (magas, közepes vagy alacsony) venniük a kockázatok csökkentésére a szervezet számára.

### <a name="secure-identity"></a>Biztonságos (azonosító)   
Készíthet remediation művelet, például a jelszó módosítása vagy gépi reimaging a potenciálisan biztonságos identitás-biztonsága sértetlen állapotának visszaállításához.

### <a name="security-policy"></a>Eszközbiztonsági házirend beállítása
Szabályok és feltétel gyűjteménye. A házirend például felhasználók, csoportok, alkalmazások, eszközök, eszköz platformokon, eszköz államok, IP-tartományokat és Auth2.0 lekérdezésiügyfél-típusok szervezetek alkalmazhatók. A házirend engedélyezve van, valahányszor a házirend szereplő entitás kibocsátott egy erőforrás egy token lesznek kiértékelt.

### <a name="sign-in-v"></a>Jelentkezzen be (v) 
Az Azure Active Directory identitás hitelesíteni.

### <a name="sign-in-n"></a>Bejelentkezés (n) 
A folyamat vagy művelet hitelesítése identitás az Azure Active Directory és az eseményt, amely jellemzi ezt a műveletet.

###<a name="sign-in-from-anonymous-ip-address"></a>Bejelentkezés a névtelen IP-cím    
Kockázati esemény elindítja a sikeres bejelentkezés után IP-címekről érkező azonosították névtelen proxy IP-címként.

###<a name="sign-in-from-infected-device"></a>Bejelentkezés fertőzött eszközről 
A bejelentkezés egy IP-címet, amelyek egy vagy több biztonságos eszközök, amelyek aktívan próbál kommunikálni bot kiszolgálóval való használatra ismert származik elindított kockázati esemény.

###<a name="sign-in-from-ip-address-with-suspicious-activity"></a>Jelentkezzen be az IP-cím gyanús tevékenység 
Kockázati esemény elindítja a sikeres bejelentkezés az IP-címre cím nagy számú sikertelen bejelentkezések keresztül több felhasználói fiókot fölé egy rövid idő után.

###<a name="sign-in-from-unfamiliar-location"></a>Bejelentkezés ismeretlen helyről 
Kockázati esemény indul el, ha egy felhasználó sikeresen bejelentkezik az új helyre (IP, földrajzi szélesség és hosszúság és ASN).

###<a name="sign-in-risk"></a>Bejelentkezés a kockázat     
Lásd: a kockázat szint (bejelentkezés)

###<a name="sign-in-risk-policy"></a>Bejelentkezés a kockázat házirend  
Egy feltételes access házirendet, amely kiértékeli a kockázatot egy konkrét bejelentkezés, és alkalmazza a megoldásokkal kapcsolatban, előre megadott feltételek és szabályok alapján.

###<a name="user-compromise-risk"></a>Felhasználói biztonságos kockázat     
Lásd: a kockázat szint (felhasználói biztonságos)

###<a name="user-risk"></a>Felhasználói kockázat    
Lásd: a kockázat szint (felhasználói biztonságos).

###<a name="user-risk-policy"></a>Felhasználói kockázat házirend     
Egy feltételes hozzáférési házirendet, hogy a bejelentkezés tekinti, és alkalmazza a megoldásokkal kapcsolatban, előre megadott feltételek és szabályok alapján.

###<a name="users-flagged-for-risk"></a>A kockázat menügombon felhasználók   
A kockázat események aktív vagy remediated rendelkező felhasználók

###<a name="vulnerability"></a>Biztonsági    
Konfigurációs vagy feltétel az Azure Active Directory, amely lehetővé teszi a címtárban hasznos érzékeny vagy veszélyek.


## <a name="see-also"></a>Lásd még:

- [Azure Active Directory identitás-védelem](active-directory-identityprotection.md)
