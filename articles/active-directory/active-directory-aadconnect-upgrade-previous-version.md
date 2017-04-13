<properties
   pageTitle="Azure AD Connect: Frissítése korábbi verzióról |} Microsoft Azure"
   description="Ebből a cikkből megtudhatja, a különböző módszereket frissítsen az Azure Active Directory csatlakozni, például helybeni és mozgó áttelepítési a legújabb verzióját."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Frissítése korábbi verzióról a legújabb
Ez a témakör ismerteti a különböző módszereket használhatja a Azure AD Connect-telepítés frissítése a legújabb kiadásra. Azt javasoljuk, hogy Ön saját maga naprakészen tartása az Azure AD Connect verzióiban. Az [áttelepítési éppen](#swing-migration) leírt lépéseket is használják, hogy jelentős beállítások megváltoztatását.

A DirSync frissíteni szeretné, ha jelennek meg [frissíteni az Azure Active Directory-szinkronizáló (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) .

Létezik néhány másik stratégiák Azure AD Connect frissítéséhez.

A módszer | Leírás
--- | ---
[Az automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) | Az ügyfelek az express telepítésével ez az a legegyszerűbb módszer.
[A helyi frissítés](#in-place-upgrade) | Ha egy kiszolgáló, frissítse a telepítés helyi ugyanazon a kiszolgálón.
[Mozgó áttelepítése](#swing-migration) | Két kiszolgálóval valamelyik új kiadásának vagy konfigurációs kiszolgáló előkészítése és a active server módosítása, ha készen áll.

Szükséges engedélyekkel a [frissítés szükséges engedélyekkel](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)című témakör tartalmaz.

## <a name="in-place-upgrade"></a>A helyi frissítés
A helyi frissítés helyez át a Azure AD-szinkronizálás vagy Azure AD Connect működik. Nem működik a DirSync és FIM + Azure Active Directory-összekötő megoldás.

Ez a módszer akkor előnyben részesített, ha egy kiszolgáló és a kisebb, mint körülbelül 100 000 objektumok. Ha a kimenő kész szinkronizálási szabályok módosításait, egy teljes importálás és a teljes szinkronizálást fordulhat elő, a frissítés után. Ezzel biztosíthatja, hogy az új konfiguráció alkalmaznak minden meglévő objektumok a rendszer. A szinkronizálási motor hatókör ez is eltarthat néhány órát, attól függően, hogy az objektumok számát. A normál delta szinkronizálási ütemezőt alapértelmezés 30 percenként fel van függesztve, de a jelszó-szinkronizálás továbbra is. Érdemes a helyi frissítés alatt álló hétvégét végrehajtásához. Az új Azure AD Connect verziójának megjelenése a kimenő kész konfiguráción nincs módosítás esetén, majd normál delta importálása és szinkronizálása indul el helyette.  
![A helyi frissítés](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Ha kész out szinkronizálási szabályok módosult, ezek állítja be vissza alapértelmezett beállítás a frissítés. Győződjön meg arról, hogy Ön konfigurációjának frissítések közötti legyen, ellenőrizze, hogy a változtatások történnek a [Gyakorlati tanácsok az alapértelmezett beállítások módosításával](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)leírt módon.

## <a name="swing-migration"></a>Mozgó áttelepítése
Ha egy összetett telepítési vagy nagyon sok objektum van, akkor lehetséges praktikus, az élő rendszer a helyi frissítés elvégzéséhez. Ez az egyes felhasználók is eltarthat több napon és a megadott időszakban delta módosítások feldolgozása lesz. Ez a módszer is használják, ha azt tervezi, hogy jelentős módosításokat végezni a konfiguráción, és próbálja ki láthatóságukat előtt ezeket a felhőben vannak tolódik szeretne.

A javasolt forgatókönyvekben módja a mozgó áttelepítés használni. Szüksége (legalább) két kiszolgálót, egy aktív, és egy fejlesztői kiszolgálóra. Az active server (folytonos kék vonal az alábbi képen) az aktív termelési terhelés feladata. Az átmeneti tárolásra szolgáló server (szaggatott lila vonalak az alábbi képen) készen áll az új kiadásának vagy konfigurációs, és ha teljesen készen áll a kiszolgáló lett aktív. A korábbi active server, most a régi verzióját vagy a konfigurációs telepítve van, az átmeneti tárolásra szolgáló kiszolgáló elvégzett, és frissíteni.

A két kiszolgáló használható különböző verzióival. Ha például le szeretné az active server Azure AD-szinkronizálás használata és az új átmeneti tárolásra szolgáló kiszolgáló tudja Azure AD Connect. Ha a mozgó áttelepítési fejlesztését a új konfiguráció azt jó ötlet, hogy ugyanazt a két kiszolgálókon.  
![A kiszolgáló átmeneti](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Megjegyzés: Van már jegyezni, hogy egyes ügyfelek inkább példánkban három vagy négy kiszolgálóval rendelkezik. Az átmeneti tárolásra szolgáló kiszolgáló frissítésekor nincs abban az esetben, ha egy [vészhelyreállítás](active-directory-aadconnectsync-operations.md#disaster-recovery)biztonsági kiszolgáló. Három vagy négy kiszolgálókkal az elsődleges/készenléti kiszolgálók, az új verzióval egyik készlete készítheti, mindig vannak átvétele készen áll a kiszolgáló átmeneti tárolásra szolgáló biztosítása.

Ezeket a lépéseket akkor is használható Azure AD-szinkronizálás vagy FIM + Azure Active Directory-összekötő megoldást helyezhetők át. Ezeket a lépéseket nem működnek a DirSync, de a azonos mozgó (más néven párhuzamos telepítési) áttelepítési módszer lépésekkel DirSync [Frissítése Azure Active Directory szinkronizálása (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)találhatók.

### <a name="swing-migration-steps"></a>Mozgó az áttelepítési lépések

1. Ha használja az Azure AD Connect mindkét kiszolgálókon és tervez csak a konfiguráció módosítása, győződjön meg arról, hogy az active server és a kiszolgáló átmeneti tárolására mind ugyanazon verzióját használja. Amely megkönnyíti az újabb különbségek. Azure AD-szinkronizálás frissít, ha ezeket a kiszolgálókat különböző verzióira van. Ha a régebbi Azure AD Connect verzióról frissít, ugyanazt a verzióját használja a két kiszolgáló indítson egy célszerű, de még nem szükséges.
2. Ha saját egyéni beállításokat, és az átmeneti tárolásra szolgáló kiszolgáló nem rendelkezik, kövesse a [aktív, a kiszolgáló átmeneti tárolásra szolgáló egyéni konfiguráció áthelyezése](#move-custom-configuration-from-active-to-staging-server)alapján.
3. Ha az Azure AD Connect egy korábbi verzióból származó frissít, az átmeneti tárolásra szolgáló kiszolgáló frissítése a legújabb verzióra. Ha az Azure AD-szinkronizálás áthelyezni, majd Azure AD Connect telepítse az átmeneti tárolásra szolgáló kiszolgálóra.
4. A teljes importálás és a teljes szinkronizálás futtatása az átmeneti tárolásra szolgáló kiszolgálóra szinkronizálási motor legyen.
5. Annak ellenőrzése, hogy az új konfiguráció nem okoz a lépésekkel csoportban **Ellenőrizze** [a kiszolgáló konfigurációjától](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server)igazolása váratlan módosításokat. Ha valami nem a várt módon, helyes, futtatása importálása és szinkronizálása, és ellenőrizze, amíg az adatok elégedett az eredménnyel. Ezeket a lépéseket a csatolt témakörben találhatók.
6. Váltás a fejlesztői legyen az active server kiszolgálóra. Az utolsó lépésben **kapcsoló active server** igazolása [a a kiszolgáló beállításait](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Azure AD Connect frissít, ha frissítés most a kiszolgáló átmeneti tárolására mód a legújabb kiadásra. Kövesse ezeket a lépéseket, mielőtt az adatok és a frissített konfigurációs. Azure AD-szinkronizálás frissített, ha most kikapcsolása és a régi kiszolgáló le.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Egyéni konfiguráció áthelyezése a kiszolgáló átmeneti tárolásra szolgáló aktív
Ha módosította konfigurálása az active kiszolgálóhoz, meg kell győződjön meg arról, hogy az azonos változások történnek a fejlesztői kiszolgálóhoz.

A PowerShell áthelyezheti a létrehozott egyéni szinkronizálási szabályokat. Egyéb változásokat mindkét rendszeren ugyanúgy kell alkalmazni, és nem telepíthetők át.

Lépésként meg kell győződnie arról mindkét kiszolgálókon ugyanúgy kell megadni:

- Az azonos forests kapcsolat.
- Tartomány és a szervezeti egységre szűrés.
- Azonos választható funkciókat, például a jelszó-szinkronizálás és a jelszó visszaírást.

**Szinkronizálás szabályok áthelyezése**  
Egyéni szinkronizálási szabály áthelyezéséhez végezze el az alábbiakat:

1. Nyissa meg a **Szinkronizálást szabályok szerkesztő** az aktív kiszolgálóra.
2. Jelölje ki az egyéni szabályt. Kattintson az **Exportálás**parancsra. Ekkor megjelenik a Jegyzettömb ablak. Az ideiglenes fájl mentése egy PS1 kiterjesztésű. Így egy PowerShell-parancsprogramot. Az átmeneti tárolásra szolgáló kiszolgáló másolja a ps1 fájlt.  
![Szinkronizálási szabály exportálása](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Az összekötő globálisan egyedi azonosítója eltér az átmeneti tárolásra szolgáló kiszolgálón, és meg kell változtatni. Úgy juthat az globálisan egyedi azonosítója, indítsa el a **Szinkronizálási szabályok szerkesztőt**, válasszon egyet a beépített out szabályok ugyanazt a csatlakoztatott rendszert, amely, és kattintson az **Exportálás**parancsra. Az átmeneti tárolásra szolgáló kiszolgálóról GUID cserélje ki a PS1 fájl a globálisan egyedi azonosítója.
4. Egy PowerShell-parancssorában futtassa az PS1 fájlt. Az átmeneti tárolásra szolgáló kiszolgálón ennek hoz létre az egyéni szinkronizálási szabály.
5. Ha több egyéni szabályok, ismételje meg az összes egyéni szabály.

## <a name="next-steps"></a>Következő lépések
További tudnivalók [a helyszíni identitás és Azure Active Directory integrálása](active-directory-aadconnect.md).
