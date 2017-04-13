<properties title="" pageTitle="Azure műszaki tartalom csatorna útmutató" description="A Microsoft content csatornát, melyet a alkalmazottak, a partnerek és a munkatársak közösségi kell használni Azure műszaki tartalom közzététele ismerteti." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/06/2015" ms.author="tysonn" />

# <a name="azure-technical-content-channel-guidance"></a>Azure műszaki tartalom csatorna útmutató

GitHub is viszonylag egyszerűen (Ha a mely számjegy hump keresztül letölthető) tartalom-előállítás és -közzététel műszaki tartalma. De szükség győződjön meg arról, hogy a tartalom ne lépje túl a határokat műszaki dokumentáció - vannak más forrású más típusú információkat.

##<a name="technical-content-that-belongs-in-the-azure-content-pr-repository"></a>Az azure-tartalom-ár adattárban tartozó műszaki tartalom

**Műszaki cikkek használatáról a termék** tartozik a kiadvány http://azure.microsoft.com/documentation/articles az azure-tartalom és azure-tartalom-ár tárházakban található. Azok a megértése és használata a termék szükséges elvi vagy lépésről adatokat tartalmazzák. A műszaki tartalom csatorna olyan műszaki tartalom megjelenítő személyek **hogyan** szeretne elhelyezni. Beszélni a "mi" és a "Miért" munkában leképezés megértéséhez, de a cikkek kell kiemelése a tényleges tartalom jelzi személyek végezze el a feladatot, és töltse ki az alkalmazási példát.

##<a name="technical-content-that-does-not-belong-in-the-azure-content-pr-repository"></a>Az azure-tartalom-ár adattárban nem tartozó műszaki tartalma

**Segédletekre**: hivatkozás, REST API-hoz, PowerShell parancsmaggal súgó, séma hivatkozás és hiba felügyelt összefoglaló tevékenységének az MSDN hatókörű Azure library (http://msdn.microsoft.com/library/azure/). Csomópont fonetikus és más nyelv segédletekre a http://azure.github.io/ tartozik.

A következő típusú tartalmakhoz érkeznek más Azure vagy a Microsoft content csatornák; egyes esetekben bizonyos típusú tartalmakat nem vesznek részt a tartalom stratégia.

- **Blogbejegyzéseket**: blogbejegyzések általában kapcsolódnak hirdetmények és promóciók, és az első személy hang általában írt. A rendezés tartalom általában az Azure blogjában tartozik. A blog mélyen technikai vagy lépésről tartalmát nem lép.

- **Esettanulmányok/ügyfél írások**: esettanulmányok és az ügyfél írások nagyon konkrét termék, amely megy keresztül marketing, a saját folyamat és irányelveket, és az adott ügyfelek és partnerek Előjegyzések létrehozott. Azok https://customers.microsoft.com/ vannak közzétéve. Nem hívja valamit, amit egy esettanulmány vagy a vevő szövegegység kivéve, ha azt a részét a formális eset tanulmányi folyamat, és ne tegye közzé a technikai tartalomtár.

- **Kód és a project minták**: kód és a project minták lépjen a minták tárházakban található, és a minta gyűjtemény a kiemelt vannak.

- **Közösségi reflektorfény/közösségi források**: közösségi projektek vásárolt cikkek. A repó műszaki tartalom használatáról a Microsoft persective terméke, nem az emberek hogyan használja a termék szolgál. Ez a marketing, vagy esetleg blog tartalmat. Vagy engedélyezni a Közösség megállapítani, hogy azt, hogy a közösségi tetszésnyilvánítások legjobb helyen saját szövegegység!

- **Megfelelőségi**: iparági szabványok és a megfelelőségi információk az Azure-szolgáltatások fel kell adni a https://www.microsoft.com/en-us/TrustCenter/Compliance?service=Azure#Icons. Ide tartoznak a hitelesítő, például az ISO, országfüggő szabványok és kormányzati tanúsítványok, banki, állapot vagy egyéb tanúsítványok.

- **Letölthető fájlok**: műszaki dokumentumok kézbesítési cikkekben, nem letöltéseket. Nem hozható létre letölthető PDF-tartalom a műszaki tartalom tárat. A letöltési központ downloabable egyebek helyezzem.

- **Visszajelzés - visszajelzés keresztül az e-mail címek szüksége**: A jóváhagyott visszajelzés elérési utak Azure tartalom szerepeltetni a visszajelzés hivatkozásra a webhely lábléc, a elégedettséget minősítést és szó vezérlő, a Disqus megjegyzéseket, közvetlen cikk adományok keresztül GitHub ki kérések és a UserVoice webhelyet. Kérjük, nem adom hozzá a csatornák formátum azzal, hogy a személyek, küldjön visszajelzést mailben.

- **Jövőbeli termék csomagok**: műszaki dokumentáció a kimutatások jövőbeli termék csomagokról nem tehető közzé. Műszaki dokumentáció illik csak mi az a végleges termék lehetséges.

- **Jogi feltételek**: vannak a teljes Azure jogi feltételek: https://azure.microsoft.com/en-us/support/legal/

- **Marketingtartalmak**: tartalmat, amely a magas szintű szolgáltatás/kedvezménye leírását tartalmazza, vagy, amely csak listák magas szintű a szolgáltatás funkcióinak valószínűleg van marketingtartalmak. A webhely részeinek marketing tartozik. Marketingtevékenység tartalom közzététele, fájl munka azure.microsoft.com kérelmének.

- **Árinformációkat**: beszélgethet arról, hogyan befolyásolják technikai lehetőségek általános módon költség rendelkezik, de tegye nem ajánlat adott részletek árak technikai cikkekben. Ehelyett a szükséges a árak lapra mutató hivatkozás a beszélgetésben van a szolgáltatás.

- **Mutató cikkek letöltések**: mutató semmi, de a letöltés mutató hivatkozást tartalmazó kis oldalakat, hanem csak csatolása a letöltés a a kapcsolódó műszaki tartalmat.

- **Adatvédelmi információkat**: egy teljes adatvédelmi nyilatkozat – Microsoft Online Services, az összes Azure eltakaró van. Műszaki tartalomként nem "adatvédelmi nyilatkozatokat" be kell mutatni az adatvédelmi információkat egy adott szolgáltatáshoz. Lásd: https://azure.microsoft.com/en-us/support/legal/.

- **Cikkek átirányítása**: Ha töröl egy cikk tartalma, akkor ne hagyja a cikk az új tartalom mutató hivatkozást tartalmazó közzé. A valós átirányítást használja.

- **Fontos tudnivalók**: kivéve, ha egy SDK cikk vagy egy StorSimple a cikk a Hardver frissítése, ilyen típusú adatok kell csak az adott technikai tartalmakat beágyazva vagy a szolgáltatás frissítéseiről csatornában szereplő.

- **A megjelenés vagy a szolgáltatás újdonságai**: listák vagy a kibocsátás vagy a szolgáltatás újdonságai leírását a szolgáltatásfrissítések csatorna megnyitásához.
