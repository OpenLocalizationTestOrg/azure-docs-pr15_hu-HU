<properties
    pageTitle="Biztonságos, felhőalapú erőforrások és Azure MFA Active Directory összevonási szolgáltatások"
    description="Ez a az Azure többtényezős hitelesítés oldalt, amely bemutatja, hogy miként veheti használatba Azure MFA és AD FS a felhőben."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Azure többtényezős hitelesítés és AD FS felhő erőforrások biztonságossá tétele

Ha a szervezet és az Azure Active Directory identitásszolgáltatóval összevont, használatával Azure többtényezős hitelesítés vagy az Active Directory összevonási szolgáltatások biztonságos információforrások, amelyek az Azure Active Directory férnek hozzá. Az alábbi eljárással biztonságos Azure Active Directory-erőforrások Azure többtényezős hitelesítés vagy az Active Directory összevonási szolgáltatásokat.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Az Active Directory összevonási szolgáltatásokat használó Azure AD-erőforrások biztosítása

A felhőben erőforrás biztonságos, először engedélyezése a felhasználók egy fiókot, majd követelések szabály beállítása. Ezzel az eljárással elemre a műveletek elvégzéséhez:

1. [Többtényezős hitelesítés kisebb kapcsolási a felhasználók számára](active-directory/multi-factor-authentication-get-started-cloud.md#turn-on-multi-factor-authentication-for-users) című témakörben ismertetett lépésekkel hozhatja létre ahhoz, hogy egy fiókot.
2. Indítsa el az Active Directory összevonási szolgáltatások kezelése konzolt.
![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-cloud/adfs1.png)
3. Nyissa meg **Fél megbízik használna** , majd kattintson a jobb gombbal a használna fél meghatalmazási. Válassza a **Szerkesztés állítást szabályok...**
4. Kattintson a **… szabály hozzáadása**
5. A legördülő listában válassza **a Küldés egyéni szabály alkalmazásával követelések** , és kattintson a **Tovább**gombra.
6. Írja be a felelős szabály nevét.
7. Egyéni szabály alapján: Adja hozzá a következő szöveggel:

    ```
    => issue(Type = "http://schemas.microsoft.com/claims/authnmethodsreferences", Value = "http://schemas.microsoft.com/claims/multipleauthn");
    ```

    Megfelelő felelős:

    ```
    <saml:Attribute AttributeName="authnmethodsreferences" AttributeNamespace="http://schemas.microsoft.com/claims">
    <saml:AttributeValue>http://schemas.microsoft.com/claims/multipleauthn</saml:AttributeValue>
    </saml:Attribute>
    ```

8. Kattintson **az OK gombra** , majd a **Befejezés gombra**. Zárja be az Active Directory összevonási szolgáltatások kezelése konzolt.

Felhasználók majd hajthatják végre bejelentkezés a helyszíni módszerrel (például az intelligens kártya).

## <a name="trusted-ips-for-federated-users"></a>A szövetséges felhasználók megbízható IP-címei
Megbízható IP-címei segítségével a rendszergazdák azt kétlépcsős hitelesítési adott IP-címeket, illetve a saját intraneten belül származó kérések rendelkező szövetséges felhasználók. Az alábbi szakaszok ismertetik, hogyan Azure többtényezős hitelesítést megbízható IP-címei konfigurálása azt kétlépcsős hitelesítési és a szövetséges felhasználók, ha a szövetséges felhasználók intraneten belül származik kérést. Ez egy átadó vagy Szűrősablon egy bejövő igénylése a vállalati hálózaton belül állítást típusú Active Directory összevonási szolgáltatások konfigurálásával érhető el.

Ebben a példában az Office 365-ben a saját használna fél megbízik.

### <a name="configure-the-ad-fs-claims-rules"></a>Az Active Directory összevonási szolgáltatások követelések szabályok konfigurálása

A legfontosabb dolog, végezze el szükség konfigurálása az Active Directory összevonási szolgáltatások követelések. Azt hoz létre két követelések szabályok, az egyik a vállalati hálózaton belül állítást típus, a további egy, a felhasználók bejelentkezve megőrzési.

1. Nyissa meg az Active Directory összevonási szolgáltatások kezelése.
2. A bal oldali jelölje ki a **Külső megbízik használna**.
3. Kattintson a jobb gombbal a **Microsoft Office 365-ben identitás Platform** , és válassza a **Szerkesztés állítást szabályok...** 
 ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)
4. Kattintson kiállítási átalakítása szabályok **szabály hozzáadása.** 
 ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)
5. **Áthúzás vagy a szűrő egy bejövő állítást** jelölje ki a legördülő hozzáadása átalakítása állítást szabály varázsló, és kattintson a **Tovább**gombra.
![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)
6. Állítást szabály neve melletti mezőbe nevezze el a szabályt. Példa: InsideCorpNet.
7. A legördülő listában a mellett a bejövő formál típusa, jelölje be a **Vállalati hálózaton belül**.
![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip4.png)
8. Kattintson a **Befejezés gombra**.
9. Kattintson az átalakítás kiállítási szabályok **Szabály hozzáadása**.
10. **Küldés követelések egy egyéni szabály használatával** jelölje ki a legördülő hozzáadása átalakítása állítást szabály varázsló, és kattintson a **Tovább**gombra.
11. A felelős szabály neve alatti mezőbe: Adja meg a *Megtartása felhasználók jelentkezett be*.
12. Egyéni szabály mezőbe írja be:

        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
![Felhőalapú](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip5.png)
13. Kattintson a **Befejezés gombra**.
14. Kattintson a **alkalmazása**gombra.
15. Kattintson az **OK gombra**.
16. Zárja be az Active Directory összevonási szolgáltatások kezelése.



### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Állítsa be az Azure többtényezős hitelesítést megbízható szövetséges felhasználók IP-címei
Most, hogy a jogcímalapú helyen, azt is beállíthatja a megbízható IP-címei.

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2. A bal oldalon kattintson **Az Active Directory**.
3. Címtár csoportban jelölje be a címtárban, amelyhez megbízható IP-címei beállításához.
4. A könyvtár választotta kattintson a **Konfigurálás**gombra.
5. Többtényezős hitelesítés csoportban kattintson a **szolgáltatás-beállítások kezelése**lehetőséget.
6. A szolgáltatás beállításai lapon, a megbízható IP-címei, jelölje be a **ugorja át a multi-factor-authentication iránti kérelmek a szövetséges felhasználó az intraneten.** 
 ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip6.png)
7. Kattintson a **Mentés**gombra.
8. Miután a frissítések alkalmazott, kattintson a **Bezárás**gombra.


Az egész! Ezen a ponton szövetséges Office 365-felhasználóknak csak kell MFA melyikkel igényt kívül a vállalati intranetes származik.
