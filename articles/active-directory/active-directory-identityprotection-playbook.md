<properties
    pageTitle="Azure Active Directory identitás védelmet playbook |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure Active Directory-identitás védelem lehetővé teszi, hogy korlátozza a támadó kihasználni egy biztonságos identitás vagy eszközön, és biztonságos identitás vagy korábban gyanús vagy kell sérül ismert eszközt."
    services="active-directory"
    keywords="Azure active directory identitás védelmét, cloud app feltárás, alkalmazásokat, biztonsági, kockázat, kockázat szint, rés, biztonsági házirendek kezelése"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory identitás védelem playbook 

Ez a playbook nyújt segítséget:

- Az identitás-védelem környezetben adatok feltöltése amelyek kockázat események és biztonsági
- Feltételes hozzáférés kockázata-alapú házirendek beállítása és tesztelése a milyen következményekkel járnak ezek a házirendek


## <a name="simulating-risk-events"></a>A kockázat események amelyek

Ez a témakör azt lépésekkel megtekintheti, a következő kockázati esemény típusú:

- A Névtelen bejelentkezések IP-cím (egyszerű)
- Bejelentkezési bővítmények ismeretlen helyről (közepes)
- Lehetséges utazási atipikus helyekre (nehezen ismerhető)

Más kockázat eseményeket nem lehet szimulált biztonságos módon.


### <a name="sign-ins-from-anonymous-ip-addresses"></a>A Névtelen bejelentkezések IP-címek

A kockázati esemény típusa azonosítja a felhasználók, akik a névtelen proxy IP-címként azonosították IP-címet sikeres aláírták. Ezek a proxyk azok, akik az eszköze IP-cím elrejtése és rosszindulatú is felhasználhatók.

**A bejelentkezés a névtelen IP-címre, hasonlóan a következő lépésekkel**:

1.  Töltse le a [Szabályok böngészőben](https://www.torproject.org/projects/torbrowser.html.en).
2.  A szabályok böngésző használata esetén keresse meg [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3.  Írja be a **bejelentkezési bővítmények névtelen IP-címek a** jelentés megjeleníteni kívánt a fiók hitelesítő adatait.

A bejelentkezés megjelenik az identitás-védelem irányítópulton 5 percen belül. 


###<a name="sign-ins-from-unfamiliar-locations"></a>Bejelentkezési bővítmények ismeretlen helyről

Az ismeretlen helyek kockázat egy bejelentkezési helyek múltbeli tekinti valós idejű bejelentkezési kiértékelési mechanizmusok (IP-hez, a szélesség és hosszúság és ASN) határozza meg az új / ismeretlen helyek. A rendszer tárolja az előző IP-címei, szélesség és hosszúság és a felhasználó ASN-EK és számításba veszi-e ezekre a már jól ismert helyekre kell. Bejelentkezési hely ismeretlen számít, ha a bejelentkezési helyre nem egyezik meg a meglévő már jól ismert helyeken.

Azure Active Directory-identitás védelme:  

 - az oktatóközpont kezdő időtartam 14 nap alatt, amely azt nem jelző ismeretlen helyként minden olyan új helyre tartozik.
 - figyelmen kívül hagyja a már jól ismert eszközök és a helyek, amelyek földrajzilag közelébe meglévő már jól ismert hely bejelentkezések.

Ismeretlen helyek hasonlóan, hogy jelentkezzen be a fájl helyét és a fiók még nincs bejelentkezve a előtt eszközt. 


**A bejelentkezés ismeretlen helyről, hasonlóan a következő lépésekkel**:

1.  Válassza a fiók, amelynek legalább egy bejelentkezési előzmények 14 nap. 

2.  Módokon lehetséges:
    
    egy. Virtuális Magánhálózattal használatakor [https://myapps.microsoft.com](https://myapps.microsoft.com) keresse meg, és adja meg a kívánt hasonlóan a kockázati esemény fiók hitelesítő adatait.

    b. Kérje meg egy társítása egy másik helyre való bejelentkezéshez (nem javasolt), a fiók hitelesítő adataival.

A bejelentkezés megjelenik az identitás-védelem irányítópulton 5 percen belül.
 
### <a name="impossible-travel-to-atypical-location"></a>Lehetséges utazási atipikus helyre
A lehetséges utazási feltétel amelyek oka az nehéz algoritmus gépi tanulási gyomláláskor hamis-pozitív, például ismerős eszközökről lehetetlenné utazási vagy a többi felhasználó a címtárban által használt VPN bejelentkezések, hogy használja. Emellett a algoritmus szükséges a 3-14 napig bejelentkezési előzményeivel a felhasználó kockázati esemény létrehozása megkezdése előtt.

**Hasonlóan egy lehetetlenné utazási atipikus helyre, hajtsa végre az alábbi lépéseket**:

1.  A szokásos böngésző használata esetén keresse meg [https://myapps.microsoft.com](https://myapps.microsoft.com).  

2.  Adja meg a lehetséges utazási kockázati esemény létrehozásához használni kívánt fiók hitelesítő adatait.

3.  Módosítsa a felhasználói ügynököt. Megjelenítésének módosítása az Internet Explorerben felhasználói ügynököt a Fejlesztőeszközök, vagy módosítsa a felhasználói ügynököt a Firefox vagy a Chrome-ban egy felhasználó-agent kapcsoló bővítmény használatával.

4.  Az IP-címének módosítása. Az IP-címének virtuális Magánhálózattal egy szabályok bővítmény használatával, illetve pörgő be egy új számítógépre egy másik adatközpont az Azure-ban módosíthatja.

5.  Bejelentkezés [https://myapps.microsoft.com](https://myapps.microsoft.com) előtt, használja a hitelesítő adatait, és az előző bejelentkezés után néhány percen belül.

A bejelentkezés megjelenik az identitás-védelem irányítópult 2-4-es órán belül.<br>
Az összetett gépi tanulási a modellek érintett, mert nincs azt nem első kiválasztott mentése lehetőséget.<br> Érdemes lehet való replikáció ezeket a lépéseket az Azure Active Directory-fiókok több.


## <a name="simulating-vulnerabilities"></a>Biztonsági amelyek 
Biztonsági gyengeségek Azure AD-környezetben, amely egy hibás szereplő ki. Jelenleg biztonsági 3 típusú vannak jelenik meg, hogy kihasználhatja az Azure Active Directory egyéb szolgáltatásai Azure Active Directory-identitás védelem. Ezek a biztonsági jelennek meg az identitás-védelem irányítópulton automatikusan ezek a funkciók beállítása után.

-   Azure Active Directory [többtényezős hitelesítést?](../multi-factor-authentication/multi-factor-authentication.md)
-   Azure Active Directory [Cloud App feltárás](active-directory-cloudappdiscovery-whatis.md).
-   Azure AD- [Yammerhez Identitáskezelés](active-directory-privileged-identity-management-configure.md). 



##<a name="user-compromise-risk"></a>Felhasználói biztonságos kockázat

A **felhasználó biztonságos kockázatot, hajtsa végre az alábbi lépéseket**:

1.  Jelentkezzen be az- [https://portal.azure.com](https://portal.azure.com) globális rendszergazdai hitelesítő adatokkal az bérlői webhelyen.

2.  Nyissa meg azt a **azonosító adatok védelmét**. 

3.  A fő **Azure Active Directory-identitás védelem** a lap kattintson a **Beállítások**gombra. 

4.  A **Portál beállításai** lap a **biztonsági szabályok**csoportban kattintson **felhasználói behatolhatnak kockázat**. 

5.  **Jelentkezzen be a kockázat** lap kapcsolja ki a **szabály engedélyezése** , és kattintson a beállítások **mentése** gombra.

6.  Egy adott felhasználói fiók hasonlóan egy ismeretlen helyekre vagy névtelen IP kockázati esemény. Ez a felhasználói kockázat szintjén az adott felhasználó fog **Közepes**szintű.

7.  Várjon néhány percet, és ellenőrizze, hogy a felhasználó számára a felhasználói szintű **Közepes**.

8.  A **Portál beállításai** lap megnyitásához.

9.  A **Felhasználó biztonságos kockázat** lap **szabály engedélyezése**, csoportban jelölje be **a** . 

10. Válasszon az alábbi lehetőségek közül:

    egy. Le szeretné tiltani, jelölje be a **Közepes** **Jelentkezzen be a továbbfejlesztett fájlblokkolás**csoportban.

    b. Engedélyezi a biztonságos jelszó módosítása, jelölje be a **Közepes** a **többtényezős hitelesítést igényel**.

13. Kattintson a **Mentés**gombra.

14. Most már tesztelheti feltételes hozzáférés kockázata-alapú jelentkezzen be a felhasználó használata egy jogú kockázat szintet. Ha a felhasználó kockázat közepes, attól függően, hogy a házirend konfigurációja, a bejelentkezés blokkolhatja, akár vagy meg kelljen módosíthatja a jelszavát. 
<br><br>
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
<br>

 
##<a name="sign-in-risk"></a>Bejelentkezés a kockázat

 
**A bejelentkezési kockázatot, hajtsa végre az alábbi lépéseket:**

1.  Jelentkezzen be az- [https://portal.azure.com](https://portal.azure.com) globális rendszergazdai hitelesítő adatokkal az bérlői webhelyen.

2.  Nyissa meg azt a **azonosító adatok védelmét**.

3.  A fő **Azure Active Directory-identitás védelmet** a lap kattintson a **Beállítások**gombra. 

4.  A **Videóportál beállításai** lap a **biztonsági szabályok**csoportban kattintson a **Jelentkezzen be a kockázat**.

5.  **Jelentkezzen be a kockázat **lap jelölje be **a** **szabály engedélyezése**csoportban. 

7.  Válasszon az alábbi lehetőségek közül:

    egy. Le szeretné tiltani, jelölje be a **Közepes** **Jelentkezzen be a továbbfejlesztett fájlblokkolás** területen

    b. Engedélyezi a biztonságos jelszó módosítása, jelölje be a **Közepes** a **többtényezős hitelesítést igényel**.

8.  Le szeretné tiltani, jelölje ki a közepes tiltása a bejelentkezés csoportban.

9.  Követelni többtényezős hitelesítést, jelölje be a **Közepes** a **többtényezős hitelesítést igényel**.

10. Kattintson a **Mentés**.

11. Most már tesztelje a kockázat alapuló feltételes hozzáférés az ismeretlen helyekre vagy névtelen IP kockázati esemény amelyek, mert azok mindkét **Közepes** kockázati esemény.

<br>
![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")
<br>


## <a name="see-also"></a>Lásd még:

 - [Azure Active Directory identitás-védelem](active-directory-identityprotection.md)
