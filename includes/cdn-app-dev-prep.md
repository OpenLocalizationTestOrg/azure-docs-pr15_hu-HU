## <a name="prerequisites"></a>Előfeltételek

Mielőtt azt is kódírás CDN kezelése, végezze el az egyes előkészítése ahhoz, hogy a kódot tartalmazó az Azure erőforrás-kezelő interaktív szükség.  Ehhez kell:

* Hozzon létre egy erőforrás csoportot, amely tartalmazhat a hozzunk létre, ebben az oktatóanyagban CDN-profil
* Azure Active Directory, a saját alkalmazás hitelesítés konfigurálása
* Engedélyek alkalmazása az erőforráscsoport, hogy csak hitelesített felhasználók az Azure AD-bérlő kommunikálhat a CDN-profil

### <a name="creating-the-resource-group"></a>Az erőforráscsoport létrehozása

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. Kattintson a bal felső sarokban, majd **kezeléséről**, és **Erőforráscsoport**az **Új** gombra.
    
    ![Új erőforráscsoport létrehozása](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Az erőforráscsoport *CdnConsoleTutorial*hívja.  Jelölje ki azt az előfizetést, és válasszon egy helyet közelében.  Ha kívánja, akkor előfordulhat, hogy a jelölőnégyzetre **PIN-kód irányítópult** az erőforráscsoport az irányítópult rögzítése a portálon.  Ezzel megkönnyíti megtalálása.  Miután elvégezte a megfelelő beállításokat, kattintson a **Létrehozás**gombra.

    ![Az erőforráscsoport elnevezése](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Az erőforrás csoport létrehozását követően Ha a rendszer nem rögzít, akkor az irányítópult, kattintson a **Tallózás gombra**, majd **Az erőforrás csoportok**megtalálhatja.  Kattintson az erőforrás csoportra a megnyitáshoz.  Jegyezze fel a **Előfizetés azonosítója**.  Azt fogja később szükség lenne rá.

    ![Az erőforráscsoport elnevezése](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Az Azure Active Directory-alkalmazás létrehozása és engedélyeinek alkalmazásával

Kétféleképpen az Azure Active Directory-alkalmazás hitelesítés: az egyéni felhasználók vagy a szolgáltatás egyszerű. Egy egyszerű szolgáltatás hasonlít a Windows szolgáltatásfiók.  Helyett egy felhasználó a CDN-profilokat vezérléséhez engedélyek megadása, azt inkább a hozzáférési engedélyek megadása a fő szolgáltatást.  Automatikus, nem interaktív folyamatok szolgáltatás rendszerbiztonsági általában segítségével.  Akkor is, ha ez az oktatóanyag ír interaktív konzolon alkalmazás, a fogja a fő-megközelítés koncentráljon.

A szolgáltatás egyszerű létrehozása, ahol több lépésből, beleértve az Azure Active Directory-alkalmazás létrehozása.  Ehhez megyünk [kövesse az ebben az oktatóanyagban](../articles/resource-group-create-service-principal-portal.md).

> [AZURE.IMPORTANT] Ügyeljen arra, hogy hajtsa végre a [csatolt oktatóprogram](../articles/resource-group-create-service-principal-portal.md)lépéseit.  Azt is *rendkívül fontos* , hogy akkor fejezze be pontosan leírt módon.  Ügyeljen arra, hogy jegyezze fel a **bérlői azonosítója**, **bérlői tartománynevet** (gyakran egy *. onmicrosoft.com* tartomány kivéve, ha a korábban megadott egyéni tartományt), **ügyfél-azonosító**, és az **ügyfél hitelesítési kulcsot**, ezek újabb lesz szükség szerint.  Ügyeljen arra, hogy nagyon védelme érdekében az **ügyfél-azonosító** és a **ügyfél hitelesítési kulcsot**, ezek a hitelesítő adatok használható bárki műveletek végrehajtása a szolgáltatás egyszerű, mint. 
>   
> Ha a [több elem bérlői alkalmazás konfigurálása](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application)nevű lépéssel, válassza a **nincs**lehetőséget.
> 
> Ha az [alkalmazás szerepkör hozzárendelése](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role)lépésre, használja a korábbi verzióiban létrehozott *CdnConsoleTutorial*erőforráscsoport, de helyett az **olvasó** szerepkör hozzárendelése az a **CDN-profil munkatársi** szerepkörök.  Ebben az oktatóanyagban hozzárendelése után az alkalmazás a **CDN-profil munkatársi** szerepkörök a az erőforráscsoport, lépjen vissza. 

Egyszer létrehozta a szolgáltatás fő, és a **CDN-profil munkatársi** szerepkörök hozzárendelve, a **felhasználók** lap a erőforráscsoport hasonlóan kell kinéznie ez.

![Felhasználók lap](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Interaktív felhasználói hitelesítés

Ha a szolgáltatás egyszerű helyett inkább lenne interaktív egyes felhasználói hitelesítés, a folyamat nagyon hasonlít, amely a szolgáltatás rendszerbiztonsági tag számára.  Szüksége lesz valójában, hajtsa végre ugyanezt az eljárást, de néhány kisebb módosítani szeretné.

> [AZURE.IMPORTANT] Csak kövesse a következő lépéseket, egyéni felhasználói hitelesítés használata helyett egy egyszerű szolgáltatás kiválasztása.

1. Az alkalmazás helyett **Webalkalmazás**létrehozása esetén válassza ki a **natív alkalmazást**. 
    
    ![Natív alkalmazás](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. A következő lapon kéri az **átirányítás URI**.  A URI nem érvényesíthetők, de ne feledje, hogy mi a beírt.  Újabb mobiltelefon szükséges. 

3. Nincs szükség az **ügyfél hitelesítési kulcs**létrehozásához.

4. A szolgáltatás egyszerű hozzárendelése a **CDN-profil munkatársi** szerepkörök, hanem megyünk hozzárendelése a felhasználókhoz és csoportokhoz.  Ebben a példában látható, hogy e hozzárendelte *CDN bemutató felhasználó* a **CDN-profil munkatársi** szerepkörök.  
    
    ![Egyes felhasználói hozzáférés](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

