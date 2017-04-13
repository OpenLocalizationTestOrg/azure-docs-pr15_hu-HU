<properties
   pageTitle="Az Azure biztonság otthon átlátszó titkosítás engedélyezéséhez |} Microsoft Azure"
   description="A dokumentum bemutatja, hogyan az Azure biztonság otthon ajánlást **Átlátszó adatok titkosítása engedélyezése**végrehajtásához."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Az Azure biztonság otthon átlátszó titkosítás engedélyezéséhez

Azure biztonság otthon javasol engedélyezése átlátszó adatok titkosítási (TDE) a SQL-adatbázisait Ha TDE már nem engedélyezett. TDE védi az adatokat, és segít, hogy titkosítja a adatbázis, társított biztonsági mentés és a többi, tranzakció naplófájlok anélkül, hogy az alkalmazás módosításainak megfelelőségi követelményeknek. További információ [Az Azure SQL-adatbázis átlátszó adatok titkosítása](https://msdn.microsoft.com/library/dn948096).

Az Azure SQL szolgáltatás csak; vonatkozik a javaslat a virtuális gépeken futó SQL nem tartalmazza.

> [AZURE.NOTE] A dokumentum egy példa telepítési használatával vezet be a szolgáltatást.  Ez a cikk részletesen ismerteti nem.

## <a name="implement-the-recommendation"></a>Az ajánlási megvalósítása

1. Jelölje ki a **javaslatok** lap **Átlátszó adatok titkosítása engedélyezése**.
![Áttetsző titkosítás engedélyezéséhez][1]

2. Ekkor megnyílik a **Engedélyezése átlátszó adatok titkosításának SQL-adatbázisait** lap. Jelölje ki a TDE engedélyezéséhez kattintson egy SQL-adatbázisban.
![Jelölje ki az SQL-adatbázis TDE engedélyezése][2]
3. Az **áttetsző adatok titkosítása** lap a adatok titkosítása csoportjában jelölje be a **Tovább** , és válassza a **Mentés** a felső menüszalagján, a lap.
![TDE bekapcsolása][3]

  Miután TDE engedélyezve van a kijelölt SQL-adatbázishoz, a **titkosítási állapot** **titkosított**változik.    

  ![Titkosítás állapota][4]

## <a name="see-also"></a>Lásd még:

Ez a cikk mutatott megvalósításáról a biztonság otthon ajánlási "Átlátszó titkosítás engedélyezéséhez." SQL-TDE kapcsolatos további tudnivalókért olvassa el az alábbiakat:

- [Áttetsző adatok titkosítás Azure SQL-adatbázis](https://msdn.microsoft.com/library/dn948096)
- [Első lépések az áttetsző adatok titkosítási (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Többet szeretne tudni a biztonság otthon, olvassa el az alábbiakat:

- [Az Azure biztonság otthon biztonsági házirendek beállítása](security-center-policies.md) – a Azure előfizetések és erőforrás-csoportok biztonsági házirendek konfigurálásának ismertetése.
- [Kezelése biztonsági ajánlások az Azure biztonság otthon](security-center-recommendations.md) névre hallgat – ismerje meg, hogyan javaslatok segítenek az Azure erőforrások védelme.
- [Biztonsági állapot ellenőrzése Azure biztonság otthon](security-center-monitoring.md) – útmutató a Lync-állapota az Azure erőforrások.
- [Kezelése és válaszol a biztonsági figyelmeztetések az Azure biztonság otthon](security-center-managing-and-responding-alerts.md) – megtudhatja, hogy miként kezelheti, és a biztonsági figyelmeztetések válaszolni.
- [Figyelés partnermegoldások Azure biztonsági központ](security-center-partner-solutions.md) – megtudhatja, hogy miként figyelheti a partnerek megoldásaival állapotának.
- [Azure biztonsági központja – gyakori kérdések](security-center-faq.md) – keresés gyakran ismételt kérdések a szolgáltatással.
- [Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – legújabb Azure biztonsági híreket és információkat.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
