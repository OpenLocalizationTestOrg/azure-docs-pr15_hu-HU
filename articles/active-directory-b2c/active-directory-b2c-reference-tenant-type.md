<properties
    pageTitle="Azure Active Directory B2C: Gyártási nagyméretű B2C megtekintése és összehasonlítása-ös bérlői webhelyek |} Microsoft Azure"
    description="A tematikus típusokat az Azure Active Directory B2C bérlők"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-production-scale-vs-preview-b2c-tenants"></a>Azure Active Directory B2C: Gyártási nagyméretű B2C megtekintése és összehasonlítása-ös bérlői webhelyek

Írjon egy gyártási alkalmazás Azure Active Directory (Azure Active Directory) B2C szeretné, ha kell lehet, hogy rendelkezik-e a megfelelő bérlő szeretne lépni a élő "típus". Olvassa el a Mi az van, kövesse az alábbi lépéseket [a B2C szolgáltatások lap](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigáljon az Azure portálra, és keresse meg a **bérlői típusa**.

## <a name="summary"></a>Összefoglalás

Azure Active Directory B2C csak a **gyártási nagyméretű** B2C-ös bérlői webhelyek Észak-Amerikában gyártási alkalmazásokat támogatja.

| Bérlői típusa | Az ország/terület | Általában elérhető? |
| ----------- | -------------- | --------------------- |
| **Gyártási nagyméretű bérlői** | Észak-amerikai országok/régiók | igen |
| **Gyártási nagyméretű bérlői** | Ország/terület kivételével Észak-Amerika | nem |
| **Előzetes bérlői** | Ország/terület | nem |

> [AZURE.NOTE]
Azure Active Directory B2C bérlők (a fogyasztói) jelenleg nem érhető el néhány országban vagy régióban (az alkalmazottak) Azure AD-bérlők esetén érhető el. Olvassa el az alábbi szakaszok további információt.

## <a name="production-scale-b2c-tenant-in-north-america"></a>Gyártási nagyméretű B2C bérlői Észak-Amerikában

Ha [a B2C bérlői létrehozott](active-directory-b2c-get-started.md) Észak-Amerikában, tehát az egyik a következő ország vagy régió: Egyesült Államok, Kanada, Costa Rica, dominikai köztársasági, Salvadori, Guatemalai, Szlovákia, Panama, Puerto Ricó-i és Trinidad és Tobagó-i, és a B2C rendszergazdai felhasználói felület **bérlői típus** szerint **gyártás-skála**, a bérlői gyártás-alkalmazások használhatók.

> [AZURE.NOTE]
Bérlőnként fogyasztói identitások millió 100s méretezést gyártási nagyméretű bérlők képesek.

![Képernyőkép egy gyártási nagyméretű bérlői webhelyhez:](./media/active-directory-b2c-reference-tenant-type/production-scale-b2c-tenant.png)

## <a name="preview-b2c-tenant-in-any-countryregion"></a>Az ország/régió B2C bérlői megtekintése

Azure Active Directory B2C előzetes időszakban B2C bérlői webhelyre is létrehozott, ha valószínű **bérlői előnézete**szerint a **bérlői írja be** . Ez a helyzet, ha csak a fejlesztés és tesztelés céljából, és nem a termelési alkalmazások használja a bérlő.

> [AZURE.IMPORTANT]
Nincs áttelepítési útvonalat B2C bérlői előnézetét B2C gyártási nagyméretű bérlői webhelyre nem. Látható, hogy vannak ismert problémák az előnézeti B2C bérlő törlése és újbóli létrehozása egy gyártási nagyméretű B2C bérlői azonos nevű tartományban. Van egy gyártási nagyméretű B2C bérlői létrehozását egy másik tartománynevet.

![Képernyőkép: a kép bérlői webhelyre](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)

## <a name="production-scale-b2c-tenant-outside-of-north-america"></a>Gyártási nagyméretű B2C bérlői Észak-Amerikán kívül

Azure Active Directory B2C jelenleg nem általában elérhető Észak-Amerikán kívül. Azonban létrehozása és használata gyártási nagyméretű bérlők, fejlesztés és tesztelése céljából, az egyik a következő országokban vagy régiókban: algériai, Ausztria, Azerbajdzsán, Bahrein, Belarusz, Belgium, Bulgária, Horvátország, Ciprus, Cseh Köztársaság, Dánia, Egyiptomi, Észtország, Finnország, Franciaország, Németország, Görögország, Magyarország, Izland, Írország, Izrael, Olaszország, Jordan, Kazahsztán, Kenya, Kuvait, Lativa, Libanon, Liechtenstein, Lituania, Luxemburg, Macedónia FYRO, Málta, Montenegró, marokkói, Hollandia, Nigéria, Norvégia , Omán, Pakisztán, Lengyelország, Portugália, Katar, Románia, Oroszország, Szaúd-Arábia, Szerbia, Szlovákia, Szlovénia, Dél-afrikai, Spanyolország, Svédország, Svájc, tunéziai, török, Ukrajna, Egyesült Arab Emírségek, Egyesült Királyság és.

Miután az Azure Active Directory B2C bejelenti általános elérhetőség az országokban vagy régiókban fölött, ezek gyártási nagyméretű bérlők és éles a termelési alkalmazással az adatvesztés nélkül is.

## <a name="availability-of-b2c-tenants"></a>B2C bérlők elérhetősége

B2C bérlők szerepelnek jelenleg nem érhető el az alábbi országokat vagy régiókat: Afganisztán, Argentína, Ausztrália, Brazília, Chile, kolumbiai, Ecuadori, Hongkong KKT, India, indonéz, iraki, japán, koreai, Malajzia, angolra, paraguayi, Peru, Fülöp-szigeteki, szingapúri, Srí Lanka, tajvani, Thailand (ไทย), Uruguayi és venezuelai. Ha meg szeretné jeleníteni őket a jövőben tervezzük.
