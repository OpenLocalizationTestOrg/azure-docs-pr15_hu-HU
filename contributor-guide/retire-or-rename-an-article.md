# <a name="steps-to-follow-when-you-retire-or-change-the-name-of-an-acom-technical-article"></a>Itt leírt lépések kivonni vagy egy ACOM technikai cikk nevének módosítása

Ez az útmutató KKV, akik szerepelnek a kívánt cikk kell inaktívvá tenni azure.microsoft.com műszaki dokumentáció szakaszában, hogy a szerző nem. A lépések is vonatkoznak, ha a fájl neve.

Ha Ön az Azure Közösség tagjának, és úgy gondolja, hogy egy cikk valamilyen okból kell inaktívvá tenni, kérjük, Megjegyzés hagyja az Disqus Megjegyzés adatfolyam meg, hogy a szerző, valami nem stimmel a cikk az, hogy a cikket.

KKV szerzők kell több lépésekkel biztonságosan tartalmat kivonni, így a webhely felhasználóinak nincs telepítve a hibás használhatóság, amikor azt a tartalmat kivonni a webhelyről. A cikk törlésére és a nevét kell lennie, amely akkor fordul elő, utolsó lépésként!

## <a name="step-1-set-the-article-to-no-indexno-follow-and-republish-it-recommended"></a>Lépés: 1: Nincs – index/nem kövesse a cikk beállítása, és közzéteheti (ajánlott)

Az első lépés kell, tegye közzé újra a cikk következőképpen nincs – index/nem – néhány héten ténylegesen törlése előtt. Tekinteni a legjobb gyakorlat "előtti munka" használatból történő a tartalom. Ezzel eltávolítja a cikk keresési motor indexek, a személyek szót a keresés nem találja a cikk. [Lásd: További információ a belső wiki.](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx)

## <a name="step-2-manage-inbound-links-required"></a>Lépés: 2: (Kötelező), a Bejövő hivatkozások kezelése

Határozza meg, hogy vannak-e a tartalom nem Microsoft Bejövő hivatkozások. Gyakran blogok, fórumok és egyéb tartalmak a webes mutat cikkeket. Gyakran webhelyeken is megtekintheti és módosíthatja ezeket a hivatkozásokat blog tulajdonosok, és távolítsa el, vagy frissítése fóruma bejegyzések hivatkozásait. Web analytics eszközök megtudhatja, hogy vannak-e szükség lehet ilyen kezelése nagy forgalmat a Bejövő hivatkozások.

## <a name="step-3-remove-all-crosslinks-to-the-article-from-the-technical-content-repository-required"></a>Lépés 3: Eltávolítása az összes crosslinks cikke nyújt a technikai tartalomtár (kötelező)

Nem gondoskodik a további cikkek a crosslinks átirányítások támaszkodik. Mások frissítése vagy eltávolítása az idegen vannak törlése és átnevezése a cikkben hivatkozások, beleértve a cikkek tulajdonosa.

1. Győződjön meg róla, dolgozik egy naprakész helyi ág – futtatása `git pull upstream master` (vagy a megfelelő változat meg ezt a parancsot.

2.  Képet beolvasni a cikkek azure-tartalom-ár mappát, és az azure-tartalom-ár/tartalmaz mappa bármely cikkek és kivonni kívánt olvashatók, és vagy eltávolítása a crosslinks hivatkozást tartalmaz, vagy vegye fel helyettük a megfelelő új crosslinks. A Keresés és a csere segédprogram keresése a crosslinks, ha telepítve van. Ha nem, használhatja a Windows PowerShell ingyenesen! Az alábbiakban a PowerShell használata az crosslinks megkeresése:

 egy. Indítsa el a Windows PowerShell.

 b. Módosítania kell a PowerShell-parancssorában az azure-tartalom-pr\articles mappába:

 `cd azure-content-pr\articles`

 c billentyűkombinációt. Ezt a parancsot, amely felsorolja minden töröl témakört a hivatkozást tartalmazó fájlt:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Ha inkább a fájlnevekben listáját küldése szövegfájlba (Ez esetben elnevezett psoutput.txt), a következőkre van lehetősége:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Hozzáadása és a a módosítások véglegesítése, leküldéses őket a elágazás a és a elágazás a módosítások Ugrás a fő tárat a fő részlege ki kérelem létrehozása.

## <a name="step-4-update-the-fwlink-tool-required"></a>Lépés: 4: Frissítése az átirányítási hivatkozás eszköz (kötelező)

Az átirányítási hivatkozás eszköz keresni bármelyik a Skype vállalati verzióval, amely a cikk mutathat. Bármely a Skype vállalati verzióval mutasson a tartalom helyettesítő; Ha nem az alias, amely a hivatkozás tulajdonosa, csatlakozni. Ha a tulajdonosok nem frissíti a hivatkozást, a fájl az, hogy a hivatkozás megváltozott MSCOM jegy. További információ - [belső wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-5-remove-all-crosslinks-to-the-article-from-other-pages-on-azuremicrosoftcom-and-create-a-redirect-for-the-retired-page-if-appropriate-required"></a>5 lépés: Összes crosslinks cikke nyújt eltávolítása a azure.microsoft.com más lapokra, és hozzon létre egy visszavont laphoz redirect ha megfelelő (kötelező)

Az a személy, aki kezeli, és ebben a részben a szolgáltatás dokumentációja céloldal frissítése való használatra is. Ha nem tudja, hogy ki az adott személynek van, forduljon a tartalom csapat partnere. A személy, aki fenntartja és frissíti a dokumentum céloldal kell két dolgot kell tennie:

1. A Visual Studióban tekintse át a **teljes** ACOM webes megoldás kereszthivatkozásokat kivonni meg a fájlt. Távolítsa el a kereszthivatkozásokat, vagy vegye fel helyettük a frissített kereszthivatkozás. A HTML-hivatkozások, valamint a kapcsolódó erőforrás karakterláncok a HTML-hivatkozások eltávolítása kell. További információ – lásd: a [belső wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Create%20or%20edit%20a%20service%20landing%20page%20or%20left%20nav.aspx)

2. Ha egy helyettesítő cikk létezik, hozzon létre egy átirányítási. További információ – lásd: a [belső wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Remove%20published%20pages%20and%20request%20redirects.aspx).

3. Jelölje be a változások a tárat.

## <a name="step-6-retire-the-article"></a>Lépés a 6: Kivonni a cikk

Miután befejezte az előző lépéseket, és ezekhez a változtatásokhoz élő, törölheti a cikk a tárat a. 

**Fontos:** Ha a fájlok törléséhez kell használnia a `git add --all` parancsot.

## <a name="step-7-remove-links-from-msdn-required"></a>7 lépés: A hivatkozások eltávolítása (kötelező) az MSDN webhelyen

Tekintse át a tartalom kérdések és válaszok eszköz a visszavont vagy átnevezett témakörre nem működő hivatkozások, és a hivatkozások eltávolítása/fix az érintett összes MSDN témaköröket.

## <a name="step-8-remove-cached-pages-from-search-engines-only-if-absolutely-necessary"></a>8 lépés: A gyorsítótáras oldalainak eltávolítása keresőmotorok (csak ha feltétlenül szükséges)

Végezze el ezt csak, ha a tartalom távolítható el gyorsan jogi vagy nagyon gyenge hálózati minőség ügyfél problémák miatt. Ajánlott eljárások a Google, egy normál prioritású lap törlések csak természetes keresési motor folyamatok kell kezelni. Nyissa meg a weblapok keresőmotorok gyorsítótárazott weblapok eltávolítása:

[A Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Munkatársak naptárának útmutató hivatkozások

- [A cikk – áttekintés](./../README.md)
- [Index útmutatást cikkek](./contributor-guide-index.md)
