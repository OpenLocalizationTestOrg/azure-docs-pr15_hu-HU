<properties
    pageTitle="Azure Active Directory – gyakori kérdések |} Microsoft Azure"
    description="Azure Active Directory Feltett kérdésekre ad választ együtt Azure és Azure Active Directory elérésével kapcsolatos jelszó kezelési és az alkalmazás hozzáférést."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Azure Active Directory – gyakori kérdések

Azure Active Directory áll egy teljes identitás személyazonosságáról, a hozzáférés-kezelés, és a biztonsági részletekbe menően nyúló szolgáltatás (IDaaS) megoldást.


További részletekért lásd: [Mi az Azure Active Directory?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Hozzáférés Azure és Azure Active Directory


**Kérdés: Miért kapok "nincs előfizetések található" jelenik meg az Azure klasszikus portálon (https://manage.windowsazure.com) Azure Active Directory eléréséhez?**

**A:** Minden felhasználó rendelkezik engedélyekkel Azure-előfizetéshez az Azure klasszikus portál megnyitása igényel. Egy fizetős Office 365- vagy nyissa meg azt a [http://aka.ms/accessAAD](http://aka.ms/accessAAD) egyszeri aktiválási lépés az Azure Active Directory Ha, egyébként szüksége lesz egy teljes [Azure próbaverzió](https://azure.microsoft.com/pricing/free-trial/) vagy kifizetett előfizetés aktiválása. 

További részletekért olvassa el:

- [Azure előfizetések Azure Active Directory társított található.](active-directory-how-subscriptions-associated-directory.md)

- [Az Office 365-előfizetés Azure-ban a címtárban kezelése](active-directory-manage-o365-subscription.md)

---

**Kérdés: Mi az Azure Active Directory, a viszonya az Office 365-höz és Azure?**

**A:** Azure Active Directory közös identitás- és az access minden Microsoft online services lehetőségeket biztosít. Office 365-ben, a Microsoft Azure, Intune vagy mások szolgáltatást használ, hogy már esetén az Azure Active Directory engedélyezheti bejelentkezés, és elérheti az összes, e szolgáltatások kezelése. 

Erre valójában engedélyezte a Microsoft Online services az összes felhasználót meghatározásuk szerint egy vagy több Azure Active Directory-példány a felhasználói fiókok. A fiókok ingyenesen Azure Active Directory funkciók például alkalmazás felhőelérés engedélyezése
 
Emellett az Azure Active Directory kifizetett szolgáltatások (például: Azure AD-basic, Premium, EMS, stb.) kiegészítése más Online szolgáltatások, például az Office 365-ben és a Microsoft Azure megoldásokkal átfogó vállalati skála kezelési és biztonsági.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Hibrid Azure Active Directory – első lépések


**Kérdés: Hogyan tudok csatlakozni a helyszíni címtár Azure Active Directory?**

**A:** **Azure AD Connect**használatával Azure AD a helyszíni címtárában csatlakozhat. 

További részletekért olvassa el [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).


---

**Kérdés: Hogyan állíthatom be SSO helyszíni címtár és a felhő alkalmazások között?**

**A:** Csak kell egyszeri bejelentkezés beállítása a helyszíni könyvtár és Azure Active Directory között. Mindaddig, amíg a felhőben alkalmazások keresztül Azure Active Directory érhetők el, a szolgáltatás automatikusan meghajtók a felhasználóknak, hogy helyesen hitelesítse magát, a helyi hitelesítő adatokkal.

Egyszeri bejelentkezés végrehajtása a helyszíni könnyen érhető el a összevonási megoldásokkal, például az ADFS vagy jelszó-szinkronizálás kivonat konfigurálásával. Az Azure AD Connect varázslóval mindkettőt egyszerűen telepítheti.
  

További részletekért olvassa el [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
  

---

**Kérdés: Azure Active Directory nyújt a önkiszolgáló portál a szervezet felhasználói számára?**

**A:** Igen, Azure Active Directory teszi lehetővé az [Azure Active Directory Access Panel](http://myapps.microsoft.com) önkiszolgáló felhasználók és az alkalmazás hozzáférést. Ha Ön Office 365-előfizetése, talál számos, az azonos funkciók az Office 365-portálon. 

További tudnivalókért lásd: [Bevezetés az Access panelre](active-directory-saas-access-panel-introduction.md). 



---

**Kérdés: eladja Azure Active Directory segítség a helyszíni kezelését?**

**A:** Igen, tartalmaz. Az Azure Active Directory Premium edition biztosít a **Csatlakozás állapota**. Azure Active Directory csatlakozás állapot segít figyelésére és a helyszíni identitás infrastruktúra és a szinkronizálási szolgáltatások bepillantást.  

További információ című témakör tartalmaz [a helyszíni Monitor identitás infrastruktúra és a szinkronizálás a felhőben](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Jelszó kezelése

**Kérdés: van lehetőség a Azure Active Directory jelszó írási-vissza jelszó-szinkronizálás nélkül? (Más NÉVEN, szeretném jelszó írási-vissza az Azure Active Directory SSPR használja, de nem szeretném megjeleníteni a jelszavakat tárolja a cloud?)**

**A:** Nem kell az Azure Active Directory AD jelszavak szinkronizálása ahhoz, hogy írható-vissza. A szövetséges környezetben Azure Active Directory SSO a helyszíni könyvtár csatlakozó felhasználók hitelesítését támaszkodik. Ebben az esetben nem kell-e az Azure Active Directory követhetők helyszíni jelszót.

---

**Kérdés: hogyan ideig tart a jelszót ahhoz, hogy újra AD a helyszíni?**

**A:** Valós idejű működik írási-vissza jelszavát. 

További részletekért olvassa el [a jelszavak kezelését az első lépések](active-directory-passwords-getting-started.md) 


---

**Kérdés: van lehetőség írási-vissza jelszavát a rendszergazda által kezelt jelszavakkal?**

**A:** Igen, ha engedélyezve írási-vissza jelszavát, a rendszergazda által végzett műveletek jelszó kerülnek vissza a helyszíni környezetben.  

További válaszokat találhat, a jelszó kapcsolódó kérdések, lásd: a [Jelszó Management gyakori kérdésekre](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Az access alkalmazásban


**Kérdés: hol találhatók, amelyek az Azure Active Directory előre integrált alkalmazások listájának és azok a Funkciók?**

**A:** Azure AD a Microsofttól, az alkalmazás szolgáltatók vagy a partnerek fölötti 2600 előre integrált alkalmazások tartalmaz. Az összes előre integrált alkalmazások támogatja az egyszeri Bejelentkezést. Egyszeri bejelentkezés lehetővé teszi a szervezeti hitelesítő adatok használata az alkalmazások elérésére. Az alkalmazások is egyesek automatikus kiépítési és vonja kiépítése

Az előre integrált alkalmazások listáját olvassa el a az [Active Directory piactér](https://azure.microsoft.com/marketplace/active-directory/)című témakört.


---

**Kérdés: Mi a teendő, ha van szükség az alkalmazás nem szerepel az Azure Active Directory piactéren?**

**A:** Az Azure Active Directory prémium verzióval hozzáadása, és állítsa be a bármely-alkalmazást. Attól függően, hogy az alkalmazás lehetőségeit és a beállítások beállíthatja az egyszeri bejelentkezés, és az automatizált kiépítési.  

További részletekért olvassa el:

- [Egyszeri bejelentkezés, amelyek nem az Azure Active Directory-alkalmazás gyűjteményben alkalmazások beállítása](active-directory-saas-custom-apps.md)
- [Engedélyezése a felhasználók és csoportok az Azure Active Directory alkalmazások automatikus kiépítési SCIM használatával](active-directory-scim-provisioning.md) 


---

**Kérdés: felhasználók hogyan tegye jelentkezzen be az Azure Active Directory használó alkalmazásai?**
 
**A:** Azure Active directory Itt a felhasználók számára és nem érheti például a saját alkalmazások többféle módon:

- Az Azure Active Directory access panel

- Az Office 365-alkalmazásindító

- Közvetlen bejelentkezéses szövetséges-alkalmazás

- Szövetséges, a jelszó-alapú mély hivatkozások, illetve meglévő alkalmazások

További tudnivalókért olvassa el a [üzembe helyezés Azure Active Directory integrált alkalmazásokat olyan felhasználókhoz](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)című témakört.


---

**Kérdés: Mi az Azure Active Directory módon is lehetővé teszi, hogy a hitelesítésről és az egyszeri bejelentkezés alkalmazások?**
 
**A:** Azure Active Directory-hitelesítés és az engedélyezés, például a SAML 2.0-s, OpenID csatlakozni, OAuth 2.0-s és Webszolgáltatás-összevonás sok szabványos protokollt is támogat. Azure Active Directory jelszó vaulting és az automatizált bejelentkezési funkciók is űrlapalapú hitelesítés támogató támogatja.  

További tudnivalókért lásd:

- [Az Azure Active Directory Authentication esetek](active-directory-authentication-scenarios.md)

- [Az Active Directory Authentication protokollok](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Hogyan egyszeri bejelentkezés Azure Active Directory munkahelyi?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**K: vonhatok alkalmazások helyszíni fut?**

**A:** Azure Active Directory szolgáltatásalkalmazás-Proxy úgy dönt, hogy a helyszíni webalkalmazások egyszerűen és biztonságos hozzáférést biztosít. Ezeket az alkalmazásokat igénybe veszi a szoftver-alkalmazásokat az Azure Active Directory megegyező módon érheti el. Nincs szükség, virtuális Magánhálózaton vagy módosítása a hálózati infrastruktúrára vonatkozó nem.  

További részletekért tájékozódhat [a helyszíni környezetbe applications távoli biztonságos hozzáférés nyújtása](active-directory-application-proxy-get-started.md).


--- 

**Kérdés: hogyan MFA megkövetelése felhasználóját egy adott alkalmazás?**

**A:** Az Azure Active Directory feltételes Access-szel hozzárendelheti egy egyedi hozzáférési házirendet az egyes alkalmazások. A házirend kérheti MFA mindig, vagy ha a felhasználók nem csatlakozik a helyi hálózaton.  

További részletekért olvassa el [az Office 365-ben és egyéb alkalmazások csatlakozik az Azure Active Directory biztonságossá tétele elérését](active-directory-conditional-access.md).


---

**Kérdés: Mi az felhasználói kiépítési automatikus-szoftver alkalmazások?**

**A:** Azure Active Directory lehetővé teszi, hogy automatizálhatja a létrehozási, karbantartási és felhasználói identitások számos népszerű felhő (szoftver) alkalmazás eltávolítását. 

További információt a [kiépítési automatizálhatja a felhasználói és a szoftver-alkalmazások és az Azure Active Directory Deprovisioning](active-directory-saas-app-provisioning.md) témakörben talál.

---



