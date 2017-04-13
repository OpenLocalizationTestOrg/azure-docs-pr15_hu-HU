<properties
    pageTitle="Az egyéni tartománynév hozzáadása Azure Active Directory előzetes verzió |} Microsoft Azure"
    description="A vállalat tartománynevek hozzáadása az Azure Active Directory, és hogyan lehet ellenőrizni a tartománynevet."
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
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Azure Active Directory előzetes egyéni tartománynév hozzáadása

> [AZURE.SELECTOR]
- [Azure portál](active-directory-domains-add-azure-portal.md)
- [Azure klasszikus portál](active-directory-add-domain.md)

Egy vagy több üzleti a szervezete használ, így a felhasználók jelentkezzen be a vállalati tartomány nevét használja a vállalati hálózathoz tartománynevek is megtalálja. Azure Active Directory (Azure Active Directory) előzetes verzió használata esetén vehet a vállalati tartomány neve, valamint az Azure AD. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) Ez lehetővé teszi, hogy rendeljen a felhasználóneveket a címtárban, hogy ismeri a felhasználókhoz, például: ‘alice@contoso.com.’ a folyamat nem egyszerű:

1. Az egyéni tartománynév hozzáadása a címtárhoz
2. A tartomány DNS-bejegyzés hozzáadása neve tartományregisztrálónál
3. Győződjön meg az egyéni tartománynevet az Azure Active Directory

## <a name="how-do-i-add-a-domain-name"></a>Hogyan vehetek fel tartománynév helyett?

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg az **Azure Active Directory** a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)

3. A ***Címtár-neve*** lap jelölje be **a tartománynevek**.

4. A ** *Címtár-neve* - Domain names** lap jelölje be a **Hozzáadás** parancsot.

  ![Az Add parancs kiválasztása](./media/active-directory-domains-add-azure-portal/add-command.png)

5. A **tartomány nevét** a lap adja meg az egyéni tartomány nevét a mezőbe (például "contoso.com"), és válassza a **Tartomány hozzáadása**. Győződjön meg arról, ha meg szeretné jeleníteni a .com, .net vagy más legfelső szintű bővítmény.

6. A ***tartománynév*** lap (Ez azt jelenti, hogy a lap, amely megnyitja, amelynek címe az új tartománynevet) Ismerkedés a DNS-bejegyzés adatait, amelyekkel a Azure Active Directory ellenőrizze, hogy a szervezet tulajdonában van-e az egyéni tartománynevet.

  ![DNS-bejegyzés adatok beolvasása](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Most, hogy hozzáadta a tartomány nevét, Azure Active Directory ellenőriznie kell, hogy a szervezet tulajdonában van-e a tartomány nevét. Azure Active Directory ezt az ellenőrzést végezhet, mielőtt hozzá kell adnia a DNS-bejegyzés az a DNS-zónafájl a tartománynév. Ezt a feladatot a webhelyén, a tartománynév tartományregisztrálóhoz történik.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>A DNS-bejegyzés hozzáadása a tartomány neve tartományregisztrálónál

Az egyéni tartománynév használata az Azure Active Directory lépés a tartományhoz tartozó DNS-zónafájl frissítéséhez. Ezzel az Azure Active Directory, ellenőrizze, hogy a szervezet tulajdonában van-e az egyéni tartománynevet.

1.  Jelentkezzen be a tartomány tartományregisztrálótól. Ha nincs frissítése a DNS-bejegyzés való hozzáférést, kérje meg a személy vagy csoport, aki hozzáfér a lépés: 2 befejezéséhez, és amely közli, amikor elkészült.

2.  A tartományhoz tartozó DNS-zónafájl frissítse a DNS-bejegyzés Azure Active Directory által biztosított hozzáadásával. A DNS-bejegyzés lehetővé teszi, hogy a tartomány tulajdonjogának ellenőrzéséhez az Azure AD. A DNS-bejegyzés bármely viselkedések, például a levelezés továbbításában szerepet vagy a webes szolgáltatója nem változik.

Ez a DNS-bejegyzés hozzáadása segítséget olvassa el az [utasításokat a népszerű DNS-tartományregisztrálókkal a DNS-bejegyzés hozzáadása](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Ellenőrizze a tartománynevet az Azure Active Directory

Miután elhelyezte a DNS-bejegyzés, készen áll ellenőrizni a tartománynevet az Azure Active Directory.

A tartománynév ellenőrizhető csak azt követően a DNS-rekordok propagált van. A terjesztési gyakran csak másodpercig tart, de időnként eltarthat egy óra további. Ha ellenőrzése az első alkalommal nem működik, próbálja meg később újra.

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Válassza a **Tallózás gombra**, adja meg a felhasználók kezelése a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Válassza a **felhasználók kezelése – Domain names** lap az ellenőrizni kívánt nem ellenőrzött tartománynevet.

4. Lap (Ez azt jelenti, hogy a lap, amely megnyitja, amelynek címe az új tartománynevet), a ***tartománynév*** **tulajdonjogának igazolása** az ellenőrzés elvégzéséhez jelölje be.

Most már rendelhet [az egyéni tartománynevet tartalmazó felhasználóneveket](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Hibaelhárítás

Ha egyéni tartománynevet nem tudja ellenőrizni, próbálkozzon az alábbiakkal. Hogy miként leggyakoribb kezdődik, és lefelé legalább közös munka.

1.  **Várjon egy óra**. DNS-rekordok propagálása előtt Azure AD a tartomány ellenőrzéséhez szükséges. Ez egy óra vagy több vehet igénybe.

2.  **Ellenőrizze a DNS-rekord írt be, és az, hogy az**. Ezt a lépést a webhelyén a tartományregisztrálóhoz, a tartomány. Azure Active Directory nem tudja ellenőrizni a tartománynevet, ha a DNS-bejegyzés nem szerepel a DNS-zónafájl, vagy ha nem az a DNS-bejegyzés pontos egyezést, hogy Azure Active Directory, feltéve. Ha nincs hozzáférése a tartományregisztrálóhoz a tartományhoz tartozó DNS-rekordok frissítése, a DNS-bejegyzés oszthat meg a személy vagy csoport a szervezetnél a hozzáféréssel rendelkező személyek közül, és kérje meg, hogy a DNS-bejegyzés hozzáadása.

3.  **Törölje az Azure Active Directory egy másik címtárból a tartomány nevét**. A tartománynév ellenőrizhető csak egyetlen könyvtárában található. Ha egy másik címtárban a tartománynév korábban ellenőrizve, azt törölni kell van előtt új címtárában ellenőrizheti. Törlés tartománynevek kapcsolatos további tudnivalókért olvassa el az [egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása

Ha szervezete több egyéni tartománynevet, például "contoso.com" és 'contosobank.com', használja a 900 tartománynevek maximum felveheti őket. Kövesse ezeket a lépéseket a jelen cikkben egyes a tartománynevet.

## <a name="next-steps"></a>Következő lépések

[Egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md)
