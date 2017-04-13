<properties
   pageTitle="Az SQL Azure-adatraktár hitelesítés |} Microsoft Azure"
   description="Azure SQL-adatraktár Azure Active Directory (AAD) és az SQL Server hitelesítést."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Adatraktár Azure SQL-hitelesítés

> [AZURE.SELECTOR]
- [Biztonsági – áttekintés](sql-data-warehouse-overview-manage-security.md)
- [Hitelesítés](sql-data-warehouse-authentication.md)
- [Titkosítás (Portal)](sql-data-warehouse-encryption-tde.md)
- [Titkosítás (T – SQL)](sql-data-warehouse-encryption-tde-tsql.md)

SQL-adatraktár szeretne csatlakozni, hitelesítési célokból a hitelesítő adatokat kell eltelnie. A kapcsolat létrehozása után bizonyos kapcsolat beállításai a lekérdezés-munkamenet létrehozásának részeként.  

Biztonság és a adatraktár kapcsolatok engedélyezése a további tudnivalókért lásd: [biztonságos lévő adatraktár SQL-adatbázis][].

## <a name="sql-authentication"></a>SQL-hitelesítés
Csatlakozás SQL adatraktár, meg kell adnia az alábbi adatokat:

- Teljesen minősített kiszolgálónév
- Adja meg az SQL-hitelesítés
- Felhasználónév
- Jelszó
- Alapértelmezett adatbázis (nem kötelező)

Alapértelmezés szerint a kapcsolat a *fő* adatbázist, és nem a felhasználó adatbázishoz csatlakozik. Adatbázishoz való csatlakozáshoz a felhasználó, a két lehetőség közül választhat:

- Adja meg az alapértelmezett adatbázis, a kiszolgáló Intézővel az SQL Server objektum SSDT, SSMS, vagy az alkalmazás kapcsolati karakterláncban szereplő regisztrálásakor. Például az ODBC-kapcsolat InitialCatalog paraméter.
- Jelölje ki a felhasználói adatbázis SSDT a munkamenet létrehozása előtt.

> [AZURE.NOTE] Az adatbázis-kapcsolat módosításához a Transact-SQL-utasítás **Használata adatbázis;** nem támogatott. Csatlakozás SQL adatraktár SSDT az útmutatást [lekérdezés, amelynek a Visual Studio][] cikke nyújt útmutatást.

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD) hitelesítési

[Azure Active Directory] [ What is Azure Active Directory] hitelesítés egy mechanizmusa Csatlakozás Microsoft Azure SQL-adatraktár identitások az Azure Active Directory (Azure Active Directory) használatával. Azure Active Directory hitelesítéssel központilag kezelheti az adatbázis-felhasználói identitások és az egyéb Microsoft-szolgáltatásokhoz egy központi helyen. Központi azonosító irányítás adatraktár SQL-felhasználók kezelése egyetlen helyet biztosít, és leegyszerűsíti a engedélykezelés. 

### <a name="benefits"></a>Legfontosabb előny

Azure Active Directory előnyökkel jár a következők:

- Az SQL Server-hitelesítés alternatív biztosít.
- Segít adatbázis kiszolgálóin állítsa le a száma korlátok, felhasználói identitások között.
- Lehetővé teszi, hogy a jelszó Forgatás egy helyen
- Külső (AAD) csoportokkal adatbázis-engedélyek kezelése.
- Integrált Windows-hitelesítés és más Azure Active Directory által támogatott hitelesítéstípusok engedélyezésével megszünteti a tárolását jelszavakat.
- Felhasználási tárolt adatbázis-felhasználói identitások adatbázis szintre hitelesítést végezni.
- Csatlakozás SQL adatraktár alkalmazások jogkivonat-alapú hitelesítés támogatja.
- Többtényezős hitelesítés hitelesítéssel Active Directory univerzális támogatja az SQL Server Management Studio. Többtényezős hitelesítés listájáért lásd a [SSMS támogatja az Azure Active Directory többtényezős SQL-adatbázis és SQL adatraktár](../sql-database/sql-database-ssms-mfa-authentication.md).

> [AZURE.NOTE] Azure Active Directory továbbra is viszonylag és némi korlátokkal rendelkezik. Győződjön meg róla, hogy Azure Active Directory-környezetben közelítését, lásd: [Azure AD-szolgáltatások és érvényes korlátozások][], konkrétan a további szempontok.

### <a name="configuration-steps"></a>Konfigurálási lépéseket

Kövesse ezeket a lépéseket követve konfigurálása az Azure Active Directory.

1. Hozzon létre és Azure Active Directory feltöltése
2. Nem kötelező: Társítása vagy az active directory jelenleg társított az Azure-előfizetés módosítása
3. Azure Active Directory-rendszergazda az Azure SQL-adatraktár létrehozása.
4. Állítsa be az ügyfélszámítógépen
5. A tárolókban található adatbázis-felhasználók létrehozása az Azure Active Directory azonosítási megfeleltetve adatbázis
6. Csatlakozás a adatraktár Azure AD-identitások használatával

Azure Active Directory-felhasználók jelenleg nem jelennek a SSDT objektum Intézőben. Kerülő megtekintheti a felhasználók [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
  
### <a name="find-the-details"></a>Az adatai
- Hajtsa végre a részletes lépéseket. A részletes lépéseket, beállítása és használata az Azure Active Directory authentication majdnem megegyeznek az Azure SQL-adatbázis és Azure SQL-adatraktár. Hajtsa végre a következő témakörben: [Csatlakozás SQL-adatbázis vagy SQL adatok raktári által használatával Azure Active Directory-hitelesítés](../sql-database/sql-database-aad-authentication.md)a részletes lépéseket.
- Egyéni adatbázis szerepköröket hozhat létre, és a felhasználók hozzáadása a szerepköröket. Ezután részletes hozzáférési engedélyek megadása a szerepköröket. További tudnivalókért olvassa el az [Ismerkedés a adatbázis motor engedélyekkel](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Következő lépések

Szeretné kezdeni a adatraktár Visual Studio és más alkalmazások lekérdezésére, olvassa el a [lekérdezés, amelynek a Visual Studio][]című témakört.

<!-- Article references -->
[Az SQL adatraktár az adatbázis védelmét]: ./sql-data-warehouse-overview-manage-security.md
[A Visual Studio lekérdezés]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure Active Directory-szolgáltatások és érvényes korlátozások]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
