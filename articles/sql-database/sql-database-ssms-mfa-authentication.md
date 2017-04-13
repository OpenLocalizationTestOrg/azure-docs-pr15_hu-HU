<properties
   pageTitle="Azure Active Directory MFA SQL-adatbázis és SQL adatraktár támogatása SSMS |} Microsoft Azure"
   description="Több figyelembe venni hitelesítési használata SSMS SQL-adatbázis és SQL adatraktár."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>Azure Active Directory MFA SQL-adatbázis és SQL adatraktár SSMS támogatása

Azure SQL-adatbázis és Azure SQL-adatraktár támogatják az SQL Server Management Studio (SSMS) kapcsolatok *Active Directory univerzális hitelesítés*használatával. Az Active Directory univerzális hitelesítése áll egy interaktív munkafolyamat, amely támogatja az *Azure többtényezős hitelesítés* (MFA). Azure MFA segít védelmét hozzáférés az adatokhoz és alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben. Egyszerű ellenőrzési beállítások tartományba erős hitelesítés kézbesíti – telefonhívás, a szöveges üzenetben, az intelligens kártya PIN-kód vagy mobilalkalmazásban értesítés – így a felhasználók kiválaszthatja azt a módot előnyben részesített. Többtényezős hitelesítés listájáért lásd a [Többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication.md).

SSMS támogatja:

- Interaktív MFA az előugró párbeszédpanel lista érvényesítése az esetleges az Azure AD.
- Nem interaktív az Active Directory-jelszó és az Active Directory integrált hitelesítési módszereket (ADO.NET, JDBC, ODBC stb.) számos különböző alkalmazásban használható. E két módszer soha nem eredményezi az előugró párbeszédpanel.

Ha a felhasználói fiók van konfigurálva MFA interaktív hitelesítési munkafolyamata szükséges a további felhasználói beavatkozás keresztül előugró párbeszédpanelek, intelligens kártya használata stb. MFA a felhasználói fiók van konfigurálva, amikor a felhasználó válasszon Azure univerzális hitelesítési való csatlakozáshoz. Ha a felhasználói fiók MFA nem igényel, a felhasználó továbbra is használhatja a két Azure Active Directory-hitelesítés lehetőség.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Az SQL-adatbázis és SQL adatraktár univerzális hitelesítési korlátai

- SSMS egyetlen eszköze hitelesítéssel Active Directory univerzális MFA jelenleg engedélyezve.
- Egyetlen Azure Active Directory-fiókot is jelentkezzen be egy példányának SSMS univerzális hitelesítés használatával. Jelentkezzen be egy másik Azure AD-fiókot, egy másik példányát SSMS kell használnia. (Ezt a korlátozást az Active Directory univerzális hitelesítése korlátozva; az Active Directory jelszó-hitelesítést, az Active Directory beépített hitelesítését vagy SQL Server-hitelesítés használatával különböző kiszolgálók bejelentkezhetnek).
- Objektum Explorer, a Lekérdezésszerkesztő és a lekérdezés áruházból képi megjelenítés az Active Directory univerzális hitelesítése SSMS támogatja.
- DacFx, sem a séma Tervező univerzális hitelesítés támogatására.
- Az Active Directory univerzális hitelesítése MSA fiókok nem támogatott.
- Az Active Directory univerzális hitelesítése az SSMS nem támogatja a felhasználóknak, az aktuális Active Directory más Azure Active könyvtárak programba importált. Ezek a felhasználók nem támogatottak, mert a fiókok érvényesítéséhez bérlői Azonosítóra kell hozzá szeretne, és arról, hogy semmilyen módszerrel nem.
- Nincsenek az Active Directory-univerzális hitelesítés külön szoftver követelmények azzal a különbséggel, hogy az SSMS verzióját kell használnia.

## <a name="configuration-steps"></a>Konfigurálási lépéseket

Többtényezős hitelesítés végrehajtásához szükséges négy alapvető lépések.

1. **Beállítás az Azure Active Directory** – további tudnivalókért lásd: [integrálása az Azure Active Directory címtárral a helyszíni identitások](../active-directory/active-directory-aadconnect.md), [az Azure AD a saját tartománynév](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [most már a Microsoft Azure támogatja a Windows Server Active Directory összevonási](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [az Azure Active directory felügyelete](https://msdn.microsoft.com/library/azure/hh967611.aspx)és [kezelése a Windows PowerShell használatá Azure AD](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **MFA konfigurálása** – lépésenkénti útmutatásért lásd: [Microsoft Azure többtényezős hitelesítés beállítása](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **SQL-adatbázis konfigurálása vagy SQL adatraktár az Azure Active Directory Authentication** – lépésenkénti útmutatásért olvassa el a [Csatlakozás SQL-adatbázis vagy SQL adatok raktári által használatával Azure Active Directory-hitelesítés](sql-database-aad-authentication.md).

4. **Töltse le a SSMS** – az ügyfélszámítógépen, töltse le a legújabb SSMS (legalább augusztus 2016), a [Töltse le az SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Kapcsolódás a SSMS univerzális hitelesítés használatával

A következő lépések bemutatják, hogyan SQL-adatbázis vagy SQL adatraktár a legújabb SSMS használatával csatlakozhat.

1. Csatlakozás hitelesítéssel univerzális, a **Serverhez való csatlakozás** párbeszédpanel, jelölje be **Az Active Directory univerzális hitelesítése**.
![1mfa univerzális csatlakoztatása][1]

2. A szokásos módon az SQL-adatbázis és SQL adatraktár kell kattintson a **Beállítások** gombra, és adja meg az adatbázist a **Beállítások** párbeszédpanel. Kattintson a **Csatlakozás**.
3. A **Bejelentkezés a fiókjába** párbeszédpanel megjelenésekor adja meg a fiók és az Azure Active Directory-identitás jelszavát.
![2mfa-bejelentkezés][2]

    > [AZURE.NOTE] Egy olyan fiókkal, amelyre nincs szükség a MFA univerzális hitelesítéshez kapcsolódni ezen a ponton. A felhasználók MFA igénylő folytassa az alábbi lépéseket.
 
4. MFA-beállítások párbeszédpanel két jelenhetnek meg. Ez egy alkalommal művelet függ, hogy a beállítás MFA-rendszergazda, és így előfordulhat, hogy nem kötelező. Engedélyezett MFA tartomány esetén ez a lépés előfordul, hogy nem előre definiált (például a tartomány szükségessé teszi a felhasználó és az intelligens kártya PIN-kód).  
![a telepítő 3mfa][3]

5. A második lehetséges egyszeri párbeszédpanel lehetővé teszi, jelölje ki a hitelesítési mód részleteit. A lehetséges beállításokat a rendszergazda által.
![4mfa ellenőrzése 1][4]
 
6. Az Azure Active Directory, a információkkal küldi. Ha az ellenőrzőkódot kap, az **ENTER billentyűt ellenőrzőkódot** mezőbe írja be, és kattintson a **Bejelentkezés**gombra.
![5mfa ellenőrzése 2][5]

Ellenőrzés befejeződésekor SSMS csatlakozik, a szokásos módon feltéve, hogy érvényes hitelesítő adatokkal és a tűzfalat access.

##<a name="next-steps"></a>Következő lépések  

Adjon hozzáférést másokkal az adatbázist: [SQL-adatbázis hitelesítés és engedélyezési: hozzáférés megadását](sql-database-manage-logins.md)  
Győződjön meg arról, hogy mások a tűzfalon keresztül kapcsolódhatnak: [egy Azure SQL-adatbázis-kiszolgálói szintű tűzfal szabályt, az Azure portálon konfigurálása](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

