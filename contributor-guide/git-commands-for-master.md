<properties pageTitle="Mely számjegy-parancsok egy új témakör létrehozása vagy egy meglévő cikk frissítése" description="A műszaki Azure használata lépései GitHub tárházakban tartalom." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="01/16/2015" ms.author="tysonn" />

# <a name="git-commands-for-creating-a-new-article-or-updating-an-existing-article"></a>Mely számjegy-parancsok egy új témakör létrehozása vagy egy meglévő cikk frissítése


## <a name="standard-process-working-from-master"></a>Szokásos folyamat (mesteralakzatból használata)
Kövesse az ebben a cikkben hozzon létre egy helyi munka ág a számítógépen, hogy hozzon létre egy új cikk azure.microsoft.com a műszaki dokumentáció szakasz, vagy egy meglévő cikk frissítése.

1. Indítsa el, mely számjegy Bash (vagy a parancssori eszköz, mely számjegy-et).

 **Megjegyzés:** Ha a nyilvános adattárban dolgozik, akkor módosítsa azure-tartalom-ár azure-tartalmat a minden parancs.

2. Azure-tartalom-ár módosítása:

        cd azure-content-pr
3. Nézze meg a fő ág:

        git checkout master

4. A fő ág származás friss helyi munka ág létrehozása:

        git pull upstream master:<working branch>


5. Az új munkaidő ág áthelyezése:

        git checkout <working branch>

6. Tudassa az, hogy a helyi munka ág létrehozott elágazás:

        git push origin <working branch>

7. Az új hozzászólás létrehozása és módosítása meglévő cikket. A Windows Intézőben nyissa meg az új markdown fájlok létrehozása és Atom (http://atom.io) használja a markdown szerkesztésére. Miután létrehozott vagy módosított cikk és a képeket, lépjen a következő lépéssel.

8. Adja hozzá, és hajtsa végre az elvégzett módosításokat:

        git add .
        git commit –m "<comment>"
        
   Vagy csak az adott hozzáadása fájlok módosítani:

        git add <file path>
        git commit –m "<comment>"

   Ha a törölt fájlokat, akkor ezzel a:
   
        git add --all
        git commit -m "<comment>"

9. A helyi munka ág frissítése a felsőbb szintű által végzett módosításokkal:

        git pull upstream master

10. Küldeni a a módosításokat a elágazás GitHub meg:

        git push origin <working branch>

12. Amikor készen áll a felsőbb szintű fő ág átmeneti tárolására, adatérvényesítési, és a közzététel, a tartalom küldése a GitHub felhasználói felület, hozzon létre ki kérelmének a elágazás a fő ág.

13. Ha Ön egy alkalmazott a magánjellegű tárházba dolgozik a módosítások elküldése automatikusan előkészített vannak, és a leküldéses kérésre írott átmeneti tárolásra szolgáló hivatkozás. Tekintse át a szakaszos tartalmat, és jelentkezzen a leküldéses kérelem megjegyzések a **#sign kikapcsolása** Megjegyzés hozzáadása.  Ez azt jelzi, hogy a módosítások kell nyomni élő készen áll.  Ha nem szeretné, hogy a leküldéses kérelem elfogadott - Ha csak a módosításokat – átmeneti tárolására megjegyzést a **#hold ki** a leküldéses kérésre.

14. A leküldéses összehívást elfogadó ki kérését áttekinti, visszajelzés biztosít, és/vagy fogadja el a leküldéses kérelmet. 

15. Másik lehetőségként ellenőrizze a a közzétett cikk vagy a módosítások

 http://Azure.microsoft.com/Documentation/articles/*name-of-your-article-without-the-MD-extension*

## <a name="publishing"></a>Közzététel

- Cikkek közzétett körülbelül 10:00 de és 3:00 du. csendes-óceáni idő, hétfőtől péntekig. Eltarthat legfeljebb 30 percig cikkekben a közzététel után online jelennek meg. Ne feledje, hogy a leküldéses kérelem van egyesítendő a leküldéses kérelem véleményező szerint, a változtatások beépíthetők a következő ütemezett közzétételi futtatása előtt. Meg kell használnia a leküldéses kérelem véleményezők, időszakokra annak érdekében, hogy ki kérelem futtatása egy adott közzétételi egyesített. Egyéb esetben PRs ellenőrzi a témák tartalmát abban a sorrendben benyújtásának sorrendjét.

- Ön a magánjellegű adattárban dolgozó alkalmazott, az összes ki kérelem is fizetnie kell megoldást, mielőtt a leküldéses kérelem egyesíthető adatérvényesítési szabályokat. 

## <a name="working-with-release-branches"></a>Megjelenés ágak használata

A Megjelenés ág dolgozik, a legjobb mód a Megjelenés ág egy helyi munkaidő-fiók létrehozása esetén ez a parancs szintaxis használatára:

    git checkout upstream/<upstream branch name> -b <local working branch name>

Ez a helyi ág közvetlenül a felsőbb szintű ág, helyi egyesítés elkerülése hoz létre.

