<properties
    pageTitle="Adja hozzá az egyéni tartománynevet, és állítsa be a szövetséges bejelentkezéses Azure Active Directory |} Microsoft Azure"
    description="Hogyan vehet fel a vállalat tartománynevek Azure Active Directory, és hogyan lehet beállítani a szövetséges bejelentkezéses Azure Active Directory és a helyszíni összevonástól megoldás között."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Azure Active Directory az egyéni tartománynév hozzáadása

Beállíthatja az egyéni tartománynevet (például "contoso.com),", hogy a felhasználók contoso.com beállíthatja, hogy egy összevont egyszeri bejelentkezéses megoldást a vállalati hálózathoz. Ha már van az Active Directory összevonási szolgáltatások (AD FS) vagy a vállalati hálózaton különböző összevonási kiszolgáló, beállíthatja az Azure AD az Azure AD Connect eszközzel az egyéni tartománynevet szeretne használni. Azure AD Connect használatával új AD FS-környezetet, és állítsa be, hogy az összevont egyszeri bejelentkezési az Azure Active Directory is.

Ha nem rendelkezik, és üzembe az Active Directory összevonási szolgáltatások vagy egy másik összevonási kiszolgáló nem tervezi, kövesse az alábbi lépéseket: [Azure Active Directory egyéni tartománynév hozzáadása](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Egyéni tartománynév hozzáadása a címtárhoz

1. Jelentkezzen be az [Azure klasszikus portál](https://manage.windowsazure.com/) az Azure Active directory, a globális rendszergazdai fiókkal.

2. Az **Active Directory**nyissa meg a könyvtár, és kattintson a **tartományok** fülre.

3. A parancssávon válassza a **Hozzáadás gombra**, és írja be a nevét az egyéni tartomány, például "a contoso.com. Győződjön meg arról, ha meg szeretné jeleníteni a .com, .net vagy más legfelső szintű bővítmény.

4. Jelölje be a **lehet tervezi, hogy állítsa be a tartományt az egyszeri bejelentkezési a helyi Active Directory címtárral** jelölőnégyzetet.

5. Jelölje ki a **hozzáadni**.

Ismerkedés a DNS-bejegyzés, amelyekkel a Azure AD a tartomány használati jogának igazolása az Azure AD Connect eszköz futtatásával. A DNS-bejegyzés a varázslóban az **Azure Active Directory tartományi** lépésben jelenik meg. Mit láthat, lépés a varázsló néz ki [az alábbi utasításokat](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Ha nem rendelkezik az Azure AD Connect eszközzel, [Töltse le az alábbi](http://go.microsoft.com/fwlink/?LinkId=615771)is.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>A DNS-bejegyzés hozzáadása a tartomány neve tartományregisztrálónál

Az egyéni tartománynév használata az Azure Active Directory lépés a tartományhoz tartozó DNS-zónafájl frissítéséhez. Ezzel az Azure Active Directory, ellenőrizze, hogy a szervezet tulajdonában van-e az egyéni tartománynevet.

1. Jelentkezzen be a tartománynevét tartományregisztrálóhoz webhelyet. Ha nincs ehhez az access, kérje meg a személy vagy csoport a szervezet, aki hozzáfér a lépés: 2 befejezéséhez, és amely közli, amikor elkészült.

2. A tartományhoz tartozó DNS-zónafájl frissítse a DNS-bejegyzés Azure Active Directory által biztosított hozzáadásával. A DNS-bejegyzés lehetővé teszi, hogy a tartomány tulajdonjogának ellenőrzéséhez az Azure AD. A DNS-bejegyzés bármely viselkedések, például a levelezés továbbításában szerepet vagy a webes szolgáltatója nem változik.

Segítségre van szüksége ezzel a lépéssel olvassa el az [utasításokat a népszerű DNS-tartományregisztrálókkal DNS-bejegyzés hozzáadása](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Ellenőrizze a tartománynevet az Azure Active Directory

Miután elhelyezte a DNS-bejegyzés, készen áll ellenőrizni a tartománynevet az Azure Active Directory.

A tartomány ellenőrzéséhez kattintson a **Tovább gombra** az **Azure Active Directory tartományi** lépésben az Azure AD Connect varázsló. Azure AD a tartományhoz tartozó DNS-zónafájl az a DNS-bejegyzés fog megjelenni. Azure Active Directory csak ellenőrizni a tartománynevet, miután propagált van a DNS-rekordok. Gyakran propagálása csak másodpercig tart, de néha eltarthat egy óra további. Ha ellenőrzése az első alkalommal nem működik, próbálja meg később újra.

Ezután folytassa az Azure AD Connect varázsló további lépéseit. A felhasználók a Windows Server AD az Azure Active Directory szinkronizálja. Szinkronizált felhasználók az összevonáshoz konfigurált tartomány eljuthassanak egy összevont egyszeri bejelentkezéses megoldást a vállalati hálózathoz az Azure Active Directory lesz.

## <a name="troubleshooting"></a>Hibaelhárítás

Ha egyéni tartománynevet nem tudja ellenőrizni, próbálkozzon az alábbiakkal. Hogy miként leggyakoribb kezdődik, és lefelé legalább közös munka.

1.  **Várjon egy óra**. DNS-rekordok propagálása előtt Azure AD a tartomány ellenőrzéséhez szükséges. Ez egy óra vagy több vehet igénybe.

2.  **Ellenőrizze a DNS-rekord írt be, és az, hogy az**. Ezt a lépést a webhelyén a tartományregisztrálóhoz, a tartomány. Azure Active Directory nem tudja ellenőrizni a tartománynevet, ha a DNS-bejegyzés nem szerepel a DNS-zónafájl, vagy ha nem az a DNS-bejegyzés pontos egyezést, hogy Azure Active Directory, feltéve. Ha nincs hozzáférése a tartományregisztrálóhoz a tartományhoz tartozó DNS-rekordok frissítése, a DNS-bejegyzés oszthat meg a személy vagy csoport a szervezetnél a hozzáféréssel rendelkező személyek közül, és kérje meg, hogy a DNS-bejegyzés hozzáadása.

3.  **Törölje az Azure Active Directory egy másik címtárból a tartomány nevét**. A tartománynév ellenőrizhető csak egyetlen könyvtárában található. Ha egy másik címtárban a tartománynév korábban ellenőrizve, azt törölni kell van előtt új címtárában ellenőrizheti. Törlés tartománynevek kapcsolatos további tudnivalókért olvassa el az [egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása

Ha szervezete több egyéni tartománynevet, például "contoso.com" és 'contosobank.com', használja a 900 tartománynevek maximum felveheti őket. Kövesse ezeket a lépéseket a jelen cikkben egyes a tartománynevet.

## <a name="next-steps"></a>Következő lépések

-   [Egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md)
-   [Tudnivalók a Azure Active Directory domain management fogalmak](active-directory-add-domain-concepts.md)
-   [Amikor a felhasználók jelentkezzen be a cége védjegyzésének megjelenítése](active-directory-add-company-branding.md)
-   [Az Azure Active Directory a tartománynevek kezelése a PowerShell használatával](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
