<properties 
    pageTitle="Ismert hálózatok |} Microsoft Azure" 
    description="Ismert hálózatok konfigurálásával elkerülheti a szervezet a bejelentkezési a több geographies és bejelentkezési modulokat a gyanús tevékenység jelentések IP-címek szereplő tulajdonában IP-címek problémákat." 
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
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Ismert hálózatok


Azure Active Directory access és a használati jelentések segítségével betekintést kap abba, hogy a integritás és a szervezet címtár biztonsága szerezni. Az adatokkal a címtár-rendszergazdai jobban megállapítható hol lehetséges biztonsági kockázatot elhelyezkednie előfordulhat, hogy azok megfelelően tervezi, hogy e kockázatok csökkentésében.

Lehetséges, hogy a "*több geographies bejelentkezési modulok*" és "*bejelentkezési modulok IP-címek gyanús tevékenység a*" jelentéseket helytelenül jelző IP-címek a szervezet ténylegesen tulajdonában. 

Ez akkor, ha például fordulhat elő, ha: 

- Az office van bejelentkezve távolról a San Francisco az Adatközpont Bostoni felhasználó elindítja a "Jelentkezzen be a több geographies modulok" jelentés 

- Egy felhasználó a szervezet megpróbálja bejelentkezés után többször a egy hibás jelszómegadás indítók a "Modulok jelentkezzen IP-címek gyanús tevékenység a" jelentés 

Ezekben az esetekben megakadályozásához félrevezető biztonsági-jelentések létrehozása, ismert IP-címtartományok kell hozzáadása a listához a szervezet nyilvános IP-cím.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>A szervezet nyilvános IP-címtartományok felvenni, hajtsa végre az alábbi lépéseket: 

1.  Bejelentkezés az [Azure kezelőportálja segítségével](https://manage.windowsazure.com).

2.  A bal oldali ablaktáblában kattintson az **Active Directory**. <br><br>![Hogyan működik a Cloud App feltárás](./media/active-directory-known-networks/known-netwoks-01.png)

3.  A **címtár** -lapon jelölje ki a címtárban.

4.  A felső sávon kattintson a **Konfigurálás**gombra. <br><br>![Hogyan működik a Cloud App feltárás](./media/active-directory-known-networks/known-netwoks-02.png)

5.  A beállítás lapon lépjen **a szervezet nyilvános IP-címtartományok** <br><br>![Hogyan működik a Cloud App feltárás](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Kattintson a **ismert IP-címtartományok hozzáadása**gombra.

7.  A megjelenő párbeszédpanelen adja hozzá a címtartományok, és válassza az ellenőrzés gombra, ha be szeretné fejezni. <br><br>![Hogyan működik a Cloud App feltárás](./media/active-directory-known-networks/known-netwoks-04.png)


**További források**


* [A hozzáférési és használati jelentések megtekintése](active-directory-view-access-usage-reports.md)
* [A gyanús tevékenység IP-címek bejelentkezési modulok](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Jelentkezzen be a több geographies modulok](active-directory-reporting-sign-ins-from-multiple-geographies.md)


