<properties
   pageTitle="Azure Active Directory jelentéskészítés – előzetes verzió |} Microsoft Azure"
   description="Megjeleníti a különböző rendelkezésre álló jelentések Azure Active Directory előzetes verzió"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory jelentéskészítés – előzetes verzió

> [AZURE.SELECTOR]
- [Azure portál](active-directory-reporting-azure-portal.md)
- [Azure klasszikus portál](active-directory-reporting-guide.md)

*Ez a dokumentáció az [Azure Active Directory jelentéskészítés útmutató](active-directory-reporting-guide.md)része.*

Jelentéskészítés az Azure Active Directory-ban, meg kell határoznia, hogy hogyan a környezet teljesítenek-e minden információt kap. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md)

Két fő terület jelentéskészítés különböztethető meg:

- **Bejelentkezési tevékenységek** – használatról a felügyelt alkalmazások és a felhasználó bejelentkezési tevékenységek

- **Naplókat** - tevékenység rendszerinformációk felhasználók és csoportok kezelése, a kezelt alkalmazások és a címtár-tevékenységek

Attól függően, hogy a keresett adatok körét érheti el ezeket a jelentéseket vagy a szolgáltatások listában a [Azure-portálon](https://portal.azure.com)kattintson a **felhasználók és csoportok** vagy a **vállalati alkalmazások** .

## <a name="sign-in-activities"></a>Bejelentkezési tevékenységek

### <a name="user-sign-in-activities"></a>Felhasználói bejelentkezési tevékenységek

A felhasználó bejelentkezési jelentés által megadott adatokkal megtalálta kérdésekre adott válaszok például:

- Mi az, hogy a felhasználó bejelentkezési mintát?
- Hány felhasználóm felhasználók aláírták hetente keresztül?
- Mi az következő bejelentkezési bővítmények állapotát?

Az adatokat a belépési pontjához a felhasználó bejelentkezési grafikon a **felhasználók és csoportok** **áttekintése** szakaszában.

 ![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/05.png "Jelentéskészítés")

A felhasználó bejelentkezési grafikonon: bejelentkezési heti összesítések modulok minden felhasználónak egy adott időszakban. Az alapértelmezett az időszakra 30 napig tart.

![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/02.png "Jelentéskészítés")

Amikor a bejelentkezési grafikonon nap kattint, a bejelentkezési tevékenységek részletes listát kap.

![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/03.png "Jelentéskészítés")

A bejelentkezési tevékenységlistája minden egyes sorához is biztosít a részletes információkat a kijelölt bejelentkezés például:

- Ki van bejelentkezve, a?

- Mi történt a kapcsolódó (UPN)?

- Milyen alkalmazás a bejelentkezés célja volt?

- Mi az a bejelentkezés IP-címe?

- Mi lett az bejelentkezés állapotát?

### <a name="usage-of-managed-applications"></a>Felügyelt alkalmazások használatát

A bejelentkezési adatok alkalmazás kötődnek nézete például: is válaszoljon kérdésekre:

- Ki használja az alkalmazásokat?

- Mik azok a szervezet legfelső 3 alkalmazások?

- A legutóbb lehet van változatwebhelyeken egy alkalmazást. Hogyan van rá módon?


Az adatokat a belépési pontjához a legfelső 3 alkalmazásokat a szervezeten belül 30 nap az utolsó jelentés: a **vállalati alkalmazások** **áttekintése** szakaszában.

 ![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/06.png "Jelentéskészítés")


Az alkalmazás használatát graph heti összesítések, jelentkezzen be, az egy adott időszakban modulok a felső 3-alkalmazásokhoz. Az alapértelmezett az időszakra 30 napig tart.

![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/78.png "Jelentéskészítés")

Ha szeretné, beállíthatja, hogy a fókusz áthelyezése egy adott alkalmazást.

![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Jelentéskészítés")


Amikor az alkalmazás használatát grafikonon nap kattint, a bejelentkezési tevékenységek részletes listát kap.


![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Jelentéskészítés")



A **bejelentkezési-bővítmények** beállítás áttekintést teljes minden bejelentkezési eseményt az alkalmazások.

![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/85.png "Jelentéskészítés")

Az oszlop Nézetválasztó használatával jelölje ki a megjeleníteni kívánt adatok mezőket.

![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/column_chooser.png "Jelentéskészítés")



### <a name="filtering-sign-ins"></a>Bejelentkezések szűrése

Bejelentkezések megjelenített adatok mennyiségét korlátozni időintervallum szerint szűrhető.

![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/927.png "Jelentéskészítés")


A bejegyzéseket, a bejelentkezési tevékenységek szűrése található bizonyos tételeket kereséséhez.
A keresési módszer lehetővé teszi a megadott **felhasználók**, **csoportok** vagy **alkalmazások**bejelentkezési-bővítmények hatókör.


![Jelentéskészítés] (./media/active-directory-reporting-azure-portal/84.png "Jelentéskészítés")

## <a name="audit-logs"></a>Naplókat

A naplózási naplók az Azure Active Directory adja meg a rekordok rendszer tevékenységek megfelelőség céljából.

A naplózás az Azure-portálon kapcsolódó tevékenységek három fő kategóriába sorolhatók:

- Felhasználók és csoportok   

- Alkalmazások

- A címtár   


Naplózási jelentések tevékenységek teljes listáját a [jelentés események listája](active-directory-reporting-audit-events.md#list-of-audit-report-events)látható.


A bejegyzés mutasson a minden naplózási adatok **naplókat** **Azure Active Directory** **tevékenység** szakaszában.


![A naplózás] (./media/active-directory-reporting-azure-portal/61.png "A naplózás")


A napló rendelkezik a szereplők megjelenítő listanézet (akik), a tevékenységek (Mit) és a cél.


![A naplózás] (./media/active-directory-reporting-azure-portal/345.png "A naplózás")


A listanézetben elem kattintva kaphat további részleteket.

![A naplózás] (./media/active-directory-reporting-azure-portal/873.png "A naplózás")




### <a name="users-and-groups-audit-logs"></a>Felhasználók és csoportok naplókat


A felhasználók és a csoport alapú naplójelentések például: kaphat kérdéseire választ kaphat az:

- Milyen típusú frissítések lett alkalmazza a felhasználóknak?

- Hány felhasználóm módosultak?

- Hány jelszavak módosultak?

- Mi a rendszergazda megállapított könyvtárában található?

- Mik azok a csoportok felvett?

- Vannak olyan-csoportok tagságának módosítások?

- Megváltozott a csoport tulajdonosai?

- Milyen licencek vannak rendelve egy csoport vagy felhasználó?


Ha szeretné, hogy a felhasználók és csoportok kapcsolódó naplózási adatainak ellenőrzése, a **tevékenység** szakasz a **felhasználók és csoportok**a **naplókat** szerint szűrt nézet található.


![A naplózás] (./media/active-directory-reporting-azure-portal/93.png "A naplózás")


### <a name="application-audit-logs"></a>Alkalmazás naplókat

Az alkalmazás-alapú naplózási jelentések, például kérdésekre adott válaszok elérheti:

- Mik azok a hozzáadott vagy frissített alkalmazásokat?

- Mik azok a eltávolították alkalmazásokat?

- Megváltozott a szolgáltatás elve szerint az alkalmazás?

- Megváltozott a nevét, az alkalmazások?

- Kik rendelkezésére bocsátotta hozzájárulása az alkalmazás?


Ha csak szeretne az alkalmazáshoz kapcsolódó naplózási adatainak ellenőrzése, talál a **naplókat** szűrt nézet **vállalati alkalmazások** **tevékenység** szakaszában.


![A naplózás] (./media/active-directory-reporting-azure-portal/134.png "A naplózás")


### <a name="filtering-audit-logs"></a>Szűrési naplókat

A naplójelentés megjelenített adatok mennyiségét korlátozni időintervallum szerint szűrhető.

![A naplózás] (./media/active-directory-reporting-azure-portal/324.png "A naplózás")

A napló elemeit szűrése található bizonyos tételeket kereséséhez.

![A naplózás] (./media/active-directory-reporting-azure-portal/237.png "A naplózás")

## <a name="next-steps"></a>Következő lépések

Lásd: az [Azure Active Directory útmutató jelentéskészítés](active-directory-reporting-guide.md).
