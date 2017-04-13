<properties
   pageTitle="Azure Active Directory Graph API |} Microsoft Azure"
   description="Egy – áttekintés és a quickstart útmutató útmutató az Graph API, amely lehetővé teszi az Azure Active Directory REST API-végpontok keresztül való hozzáférés."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API

> [AZURE.IMPORTANT] Azure Active Directory Graph API-funkciók [A Microsoft Graph](https://graph.microsoft.io/), egy egyesített API-val, amely tartalmazza az API-khoz más Microsoft szolgáltatásokból, például Outlook, a OneDrive, OneNote, a Csapattervező és az Office Graph elérhető keresztül egy végpontot, és egy egyetlen jogkivonat keresztül is érhető el.

Az Azure Active Directory Graph API Azure Active Directory – REST API-végpontok programozott hozzáférést biztosít. Alkalmazások a Graph API segítségével végezhet el létrehozása, olvassa el, frissítése és törlése a címtár-adatok és objektumok (CRUD) műveleteket. Például a diagram API támogatja a user objektumban a következő gyakori műveleteket:

- Hozzon létre egy új felhasználót könyvtárában található

- A felhasználó részletes tulajdonságait, például a csoportok beszerzése

- Saját helyet és egy telefonszámot, például a felhasználó tulajdonságainak módosítása vagy a jelszó módosítása

- Jelölje be a felhasználó csoporttagságát szerepköralapú hozzáférés-

- A felhasználói fiók letiltása vagy teljes törlése

Felhasználói objektum kívül egyéb objektumok, például a csoportok és alkalmazások hasonló műveleteket végezheti el. Hívja fel a Graph API könyvtárában, az alkalmazás regisztrálni kell az Azure Active Directory és beállítania, hogy a címtárban való hozzáférés engedélyezése. Ez általában a felhasználó vagy a rendszergazda jóváhagyási folyamat keresztül érhető el.

Az Azure Active Directory Graph API használatának megkezdéséhez olvassa el a [Diagram API quickstart útmutató útmutató](active-directory-graph-api-quickstart.md), vagy nézze át a [interaktív Graph API hivatkozási dokumentáció](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Szolgáltatások

A diagram API-t az alábbi szolgáltatásokat nyújtja:

- **REST API-végpont**: az Graph API segítségével szabványos HTTP-kérések használt végpontokat áll RESTful szolgáltatás. A diagram API-összehívások és válaszok XML- vagy Javascript objektum jelölés (JSON) tartalomtípusok támogatja. További tudnivalókért lásd: az [Azure Active Directory Graph REST API-hivatkozás](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Az Azure Active Directory Authentication**: a diagram API minden kérés kell hitelesítette hozzáfűzése a JSON webes jogkivonat (JWT) a kérelem engedélyezési fejlécében. Ez a token van szerezte be kérelmének tétele a token Azure AD-végpontot, valamint képzések érvényes hitelesítő adatokkal. Használhatja a OAuth 2.0 ügyfél hitelesítő adatok folyamat, vagy a engedélyezési kód adja meg az adatfolyam szerezheti be a token a diagramot meghívandó. További információ [az Azure Active Directory OAuth 2.0-s](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Szerepkör-alapú hitelesítés (RBAC)**: a biztonsági csoportok a Graph API RBAC végrehajtásához használják. Például határozza meg, hogy egy felhasználó rendelkezik-e az adott erőforráshoz való hozzáférés szeretné, ha az alkalmazás felhívhatja a [Ellenőrizze a csoporttagság (tranzitív)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) , a művelet, amely igaz vagy hamis értéket adja eredményül.

- **Megkülönböztető lekérdezés**: Ha módosítások között anélkül, hogy a gyakori lekérdezések Graph API-val, két időszakok könyvtárában található ellenőrizni szeretné, akkor is használhatja a megkülönböztető kérést. Kérés ilyen típusú csak az előző megkülönböztető lekérdezés kérelem és az aktuális kérelem között elvégzett módosítások ad eredményül. További tudnivalókért lásd: [Azure Active Directory Graph API megkülönböztető lekérdezés](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **A címtár-bővítmények**: Ha olvasása és írása címtárobjektumok egyedi tulajdonságai kell, hogy az alkalmazások fejlesztéséhez, ha regisztrálhatja és bővítmény értékek használata az Graph API segítségével. Ha az alkalmazás egy Skype-azonosító tulajdonságot minden felhasználó számára, például az új tulajdonság regisztrálhatja a címtárban, és minden felhasználó objektum elérhető legyen. További tudnivalókért lásd: az [Azure Active Directory Graph API címtár séma bővítmények](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Biztonságos jogosultsági hatókör**: AAD Graph API AAD adatok biztonságos/átadni kívánt hozzájárult e hozzáférés engedélyezése, amely támogatja az ügyfél alkalmazás adattípusokra, beleértve a jogosultsági hatókörök közzététele:
 - azokat, amelyek szerepelnek felhasználói felülettel meghatalmazott access keresztül engedélyezési adatok (delegálás) jelentkezett be felhasználói
  - azokat, amelyek alkalmazás-meghatározhatja például szolgáltatás/démon ügyfelei (alkalmazás szerepkörök) szerepköralapú hozzáférés-vezérlő

    Meghatalmazott, mind alkalmazás szerepkör jogosultsági hatókörök jelenítik meg a diagram API által elérhetővé tett jogosultság, és a regisztrációs engedélyek [lehetőségek az Azure klasszikus portálon](https://manage.windowsazure.com)keresztül ügyfélalkalmazásokban igényelhet. Ügyfelek ellenőrzéséhez a jogosultsági hatókörök nyújtott őket az fogadja a hozzáférési jogkivonat delegált az engedélyek hatókörének ("scp") állítást vizsgálata és alkalmazás szerepet az engedélyek formál a szerepkörök ("szerepkörök"). További tudnivalók az [Azure Active Directory Graph API jogosultsági hatókörök](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Felhasználási területei

A diagram API lehetővé teszi, hogy hány helyzeteket. Az alábbi esetekben is a leggyakoribb:

- **Üzleti vonal (egyetlen bérlő esetében) alkalmazás**: Ebben az esetben egy vállalati Fejlesztőeszközök működik, az Office 365-előfizetéssel rendelkező szervezeti. A Fejlesztőeszközök készíti Azure Active Directory feladatok elvégzéséhez szoftverekkel webalkalmazás ilyen ki egy licencet egy felhasználónak. Ehhez a feladathoz Graph API-val való hozzáférést, a Fejlesztőeszközök nyilvántartások az egy bérlői Azure AD-alkalmazás, és beállítja olvasási és írási engedéllyel a Graph API. Kattintson az alkalmazás van konfigurálva a saját hitelesítő adatok vagy azokat a bejelentkezés jelenleg felhasználó szerezheti be a token hívja fel a Graph API-t.

- **Szoftver szolgáltatás alkalmazásként (több bérlő esetében)**: Ebben az esetben egy független szoftverének a szállítójával (külső) szolgáltatott több bérlői tartalmazó webalkalmazás felhasználói dokumentumkezelési funkciókat nyújt az Azure Active Directory használó más szervezetekkel van fejlesztése. Ezek a szolgáltatások használatához címtárobjektumok való hozzáférést, és így az alkalmazás hívja fel a Graph API kell. A Fejlesztőeszközök regisztrál az alkalmazás az Azure Active Directory, csak olvasási és írási engedéllyel a Graph API konfigurálja és majd lehetővé teszi a külső hozzáférést, így más szervezetek is beleegyezés az alkalmazás használatához a címtárban. A szervezet egy másik felhasználó hitelesíti az alkalmazás első alkalommal, amikor az alkalmazás jogosultsággal rendelkező felhasználó hozzájárul ahhoz párbeszédpanel jelenjenek meg.  Megadása a felhasználó hozzájárul ahhoz adjon az alkalmazás azokat a kért a Graph API-hoz a felhasználói engedélyek. A felhasználó hozzájárul ahhoz keretrendszer kapcsolatos további tudnivalókért [beleegyezés keretrendszer áttekintése](active-directory-integrating-applications.md)című témakörben találhat.

## <a name="see-also"></a>Lásd még:

[Azure Active Directory Graph API quickstart útmutató útmutató](active-directory-graph-api-quickstart.md)

[Active Directory Graph többi dokumentáció](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory fejlesztői útmutató](active-directory-developers-guide.md)
