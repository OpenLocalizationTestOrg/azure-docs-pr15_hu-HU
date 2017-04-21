<properties
pageTitle="Telepítse és állítsa be GitHub létrehozására szolgáló eszközök"
description="Az eszközök és a megfelelő az Azure tartalmának GitHub szerzői lépést."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Telepítse és állítsa be GitHub létrehozására szolgáló eszközök

Kövesse az ebben a cikkben a Azure műszaki dokumentációt járult hozzá az eszközök beállítása. Alkalmi és alkalmi munkatársak valószínűleg használhatja a felhasználói felület lépés: 2 ismertetett GitHub.

Ha tudja pontosan, mely számjegy, érdemes lehet áttekinteni néhány mely számjegy terminológiájában: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Ezenkívül a StackOverflow szál tartalmazza-e egy fogja a lépéscsoportra előforduló mely számjegy szószedet: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Tartalom

- [GitHub fiók létrehozása és beállítása](#create-a-github-account-and-set-up-your-profile)
- [Regisztrálás az Disqus](#sign-up-for-disqus)
- [Állapítsa meg, hogy valóban szükséges hajtsa végre az alábbi lépéseket a többi](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Engedélyek az GitHub](#permissions-in-github)
- [Telepítse a Windows mely számjegy](#install-git-for-windows)
- [Kétfaktoros hitelesítés engedélyezése](#enable-two-factor-authentication)
- [Telepítse egy markdown szerkesztő](#install-a-markdown-editor)
- [Atom-alapú konfigurálása](#configure-atom)
- [A tár ágaznak, és másolja a vágólapra a számítógépen](#fork-the-repository-and-copy-it-to-your-computer)
- [Állítsa be a felhasználónevét és a helyi meghajtóra az e-mailben](#configure-your-user-name-and-email-locally)
- [Következő lépések](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>GitHub fiók létrehozása és beállítása

Azure műszaki tartalma hozzájárul szüksége lesz egy [GitHub](http://www.github.com) fiókot.

Ha egy Microsoft közreműködői, állítsa be a GitHub fiók, hogy egy Microsoft-alkalmazott egyértelműen esetén elkülönítésének szeretne. Állítsa be a profil az alábbi képlettel történik:

- **Profilkép**: (kötelező) Ha kép
- **Név**: az első és utolsó név (kötelező)
- **E-mail**: a Microsoftos e-mail cím (nem kötelező)
- **Vállalati**: Microsoft Corporation (kötelező)
- **Hely**: (nem kötelező) a hely lista

A profil profil kell hasonló:

<p align="center">
 ![GitHub profil példa](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Regisztrálás az Disqus

Minden közzétett Azure technikai cikket a Disqus szolgáltatás által biztosított Megjegyzés adatfolyam tartalmaz.

 ![Discus embléma](./media/tools-and-setup/discus.png)

Ha egy Microsoft-alkalmazott, és a szerzőjének vagy egy közös munka egy cikk, kell regisztrál Disqus, vehet részt a Megjegyzés falának olvashatók.

1. A fiókjához a [http://www.disqus.com/](http://www.disqus.com/) regisztráció
2. Töltse ki a profil az alábbi képlettel történik:

 - **Teljes név**: a teljes neve megjelenik a Microsoft cím könyv nevére a partnerlistában, valamint a zárójeles információ, amely a alias plusz @MSFT. Formátum: *Utónév [alias@MSFT] *
 - **Hely**: A hely
 - **Rövid Bio**: A cím

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Állapítsa meg, hogy valóban szükséges hajtsa végre az alábbi lépéseket a többi

Előfordulhat, hogy nem szükséges követnie a jelen cikkben ismertetett lépéseket. A tartalom hozzájárulása szükséges vagy szeretné beszúrni függ.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Küldjön egy meglévő cikk csak szöveg módosítása

Ha csak van szüksége, vagy egy meglévő cikk szöveges frissítéseket szeretne, akkor valószínűleg nem szükséges követnie a további műveletek elvégzéséhez. A módosítások küldhetik GitHub a webes markdown szerkesztő is használhatja. Egyszerűen csak kattintson a módosítani kívánt témakört a GitHub hivatkozás:

 ![GitHub profil példa](./media/tools-and-setup/contributetogit.png)

 Kattintson a Szerkesztés ikonra a cikk GitHub verziójában

 ![GitHub profil példa](./media/tools-and-setup/editicon.PNG)

 A könnyen használható web-szerkesztő, amely megkönnyíti a módosítások elküldése megnyitó. Akkor nem szükséges követnie a jelen cikkben ismertetett lépések.

###<a name="all-other-changes"></a>Minden módosítás
A felhasználói felület GitHub támogatja az új fájlok és húzása a képek létrehozását. Azonban a felhasználói felület használatakor ágak kezelése kiigazodni, telepítse az eszközöket, és ismerje meg, hozhat létre és kezelhet cikkek szolgáló parancsok a szokásos javasoljuk. Ha a felhasználói felület használni kívánt, látható:

- [Github fájlok létrehozása](https://github.com/blog/1327-creating-files-on-github)
- [Fájlok feltöltése a tárházakban](https://github.com/blog/2105-upload-files-to-your-repositories)

Az alábbi elemeket betűrendbe munka, az azt ajánljuk telepítése, és ismerje meg, hogyan eszközeivel:

 - Egy cikk fő módosítása
 - Létrehozásáról és közzétételéről egy új cikk
 - Új kép hozzáadása vagy frissítése a képek
 - Egy cikk frissítése egy adott időszakra vonatkozóan a közzétételi módosítások nélkül minden napig e napok
 - Az egyes egyszerre csak egy bizonyos nap ugorhat tartalmazó kiadásokban tartalom létrehozása

##<a name="permissions-in-github"></a>Engedélyek az GitHub

Bárki GitHub fiókkal hozzájárulhat a nyilvános tárházba [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content)címen keresztül Azure műszaki tartalmat. Nincs speciális engedélyek szükségesek.

Ha egy Microsoft PM, vagy ha a minden tartalom kell dolgozik a magánjellegű Azure tartalomtár azure-tartalom-ár dolgozik. Látogasson el a [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) kérni az olvasási engedélyek, amely lehetővé teszi az adományok a magánjellegű repó - től jelentkezzen be a gombjával GitHub > Azure kattintson > kattintson a **csoport csatlakozás** vagy **egy másik csoport csatlakozás**, és keresse meg és az **azure tartalom olvasható** csoport.

## <a name="install-git-for-windows"></a>Telepítse a Windows mely számjegy

Mely számjegy telepítse a Windows [http://git-scm.com/download/win](http://git-scm.com/download/win). A letöltés telepíti a mely számjegy rendszerhez, és mely számjegy Bash, a parancssori alkalmazás, amely a használni kívánt használata a helyi mely számjegy tárházba való telepítésekor.

Elfogadhatja az alapértelmezett beállítások; Ha azt szeretné, hogy a parancsok, a Windows parancssori belül elérhetővé szeretné tenni, válassza ki a beállítást, amely lehetővé teszi a.

<p align="center">
 ![GitHub profil példa](./media/tools-and-setup/gitbashinstall.png)

(Megjegyzés: Ez nem ugyanaz, mint "Github a Windows". "A Windows Github" egy másik grafikus-alapú eszköz, ha el szeretné olvasni a saját maga is működő. [https://windows.github.com/](https://windows.github.com/).)

## <a name="enable-two-factor-authentication"></a>Kétfaktoros hitelesítés engedélyezése

Engedélyeznie kell az két tényező authentication (2FA) a GitHub-fiókban, ha a saját tartalomtár dolgozik. Szükség van a saját tárat.

Ha engedélyezni szeretné a, kövesse a mind a következő GitHub súgótémaköröket:

- [Névjegy Kétfaktoros hitelesítés](https://help.github.com/articles/about-two-factor-authentication/)
- [A hozzáférési token parancssori használatra létrehozása](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

A token létrehozásakor, jelölje be az összes rendelkezésre álló token létrehozásához felhasználói felület ([minden hatókör részletes](https://developer.github.com/v3/oauth/#scopes)) hatókörök

Miután engedélyezte 2FA, akkor GitHub összegyűjti a parancssorból eléréséhez használatakor, adja meg a hozzáférési jogkivonat helyett a parancssorablakban GitHub jelszavát. A hozzáférési jogkivonat nem jelenik a szöveges üzenet beállítása 2FA hitelesítési kódot. A hosszú karakterlánc, amely néz ki: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. A megjegyzések néhány ezzel kapcsolatban:

- A hozzáférési jogkivonat létrehozásakor mentse egy szövegfájlt, hogy legyen könnyen hozzáférhető szükség esetén.

- Később Ha illessze be a token kell, hogy kétféleképpen illessze be a parancssor:

 - Kattintson a parancssor ablak bal felső sarkában található ikonra > Szerkesztés > Beillesztés parancsot.
 - Kattintson a jobb gombbal az ablak bal felső sarkában lévő ikonra, majd a Tulajdonságok > Beállítások > Gyorsszerkesztés. Ezzel a parancssorból a, kattintson a jobb gombbal a parancssor ablakban illesztheti.

## <a name="install-a-markdown-editor"></a>Telepítse egy markdown szerkesztő

Azt a szerző tartalom meglehetősen egyszerű "markdown" jelölés használata a fájlokat, mint a komplex, "haszonkulcs" (HTML, XML stb.). Igen kell telepíteni egy markdown szerkesztő.

- **Atom-alapú**: a legtöbb szolgáltatás GitHub's Atom markdown-szerkesztő használata: [http://atom.io](http://atom.io). Vállalati használatra, nincs szükség egy licencet. A helyesírás-ellenőrzés rendelkezik.

- **A Jegyzettömb**: használhatja a Jegyzettömb egy nagyon könnyű lehetőségre.

- **Prose**: Ez az a könnyű, elegáns, online és Megnyitás markdown Forrásszerkesztő, amely felajánlja a előnézetét. Látogasson el a [http://prose.io](http://prose.io) , és engedélyezheti a tárban tárolnak Prose.

- **[Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)** - bejegyzést a Microsoft ezt a területet.

## <a name="configure-atom"></a>Atom-alapú konfigurálása

Atom-alapú használata esetén kell néhány dolog, amit beállítása.

- Atom-alapú alapértelmezés szerint 2 szóköz használata a lapok, de Markdown vár 4 szóközöket. A két alapértelmezett hagyja, ha a cikk fog nagyszerűen helyi előzetes verzióban, de nem mikor importálják Azure. Atom használhat 4 szóközöket, konfigurálása – ez a beállítás megtalálhatja a fájl > Beállítások > szerkesztő beállításai > lapon Length.
- A program valószínűleg is szeretné bekapcsolni ebben a szakaszban a finom tördelése túl, amelyek jelent ugyanaz, mint a Jegyzettömbben "sortörés".
- A markdown előnézeti bekapcsolásához kattintson a csomagok > Markdown előnézeti > váltása előnézeti. Váltás az előnézet HTML-nézet Ctrl-Shift-M is használhatja.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>A tár ágaznak, és másolja a vágólapra a számítógépen

1. Hozzon létre egy, a tárházba elágazás GitHub - nyissa meg a lap felső sarkában, és kattintson a elágazás gombra. Ha a rendszer kéri, jelölje ki a fiókját, a helyet, ahol hozható létre a elágazás. Ezzel létrehozott egy példányát a tárat, mely számjegy központi fiókja belül. Általánosságban elmondható technikai szövegírói és a program menedzserek kell azure-tartalom-ár, a személyes repó elágazás. Munkatársak közösségi elágazás azure-tartalom, a nyilvános repó kell. Csak akkor van szüksége egy alkalommal; asztalig első telepítése után a elágazás másolása másik számítógépre, ha csak akkor kövesse az ebben a szakaszban a repó másolása a számítógépen a parancsokat.  Ha úgy dönt, hogy a mindkét tárházakban vasvillák létrehozása, szüksége lesz egy elágazó minden tárhoz létrehozásához.

2. Másolja a személyes hozzáférési jogkivonat [https://github.com/settings/tokens](https://github.com/settings/tokens)kapott. Elfogadhatja az alapértelmezett engedélyek a jogkivonat.  Mentse a személyes hozzáférési jogkivonat szövegfájlba későbbi használatra.

3. Ezután másolja a vágólapra a tárat a számítógépen a parancsot a beágyazott hitelesítő adatokkal.  Ehhez nyissa meg a mely számjegy Bash, és indítsa el a számítógépre rendszergazdaként. A parancssorba írja be a következő parancsot.  Ez a parancs könyvtárat hoz létre azure-content(-pr) a számítógépen.  Ha az alapértelmezett hely használata esetén a c:\users legyen<your Windows user name>\azure-content(-pr).

Nyilvános repó:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

A magánjellegű repó:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Például ez adatfeliratsor parancsot sikerült valami hasonló látható:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Távoli tárházba kapcsolat és konfigurálása a hitelesítő adatok

Hozzon létre hivatkozást a legfelső szintű tárházba ezek a parancsok beírásával. Állít be a tárházba GitHub-kapcsolatot, hogy a legutóbbi változások beolvasása a helyi számítógépre, és a módosítások vissza leküldéses GitHub szeretne. Ez a parancs is a token beállítja a helyi meghajtóra, hogy a név és jelszó megadására szolgáló minden alkalommal, amikor megpróbál hozzáférni a felsőbb szintű repó és a elágazás a GitHub nincs.

Nyilvános repó:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

A magánjellegű repó:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Ez általában egy ideig tart. Ezt követően, nem kell újra ágaznak, vagy írja be újból a hitelesítő adatait. Csak másolja a vágólapra a vasvillák helyi számítógépre ismét Ha beállította az eszközök egy másik számítógépen kellene.


## <a name="configure-your-user-name-and-email-locally"></a>Állítsa be a felhasználónevét és a helyi meghajtóra az e-mailben

Annak érdekében, hogy megfelelően munkatársként szerepelnek, kell konfigurálása a felhasználó nevét és e-mail helyileg a mely számjegy.

1. Indítsa el a mely számjegy Bash, és azure-tartalom vagy azure-tartalom-ár válthat:

   ````
   cd azure-content
   ````

 vagy

   ````
   cd azure-content-pr
   ````

2. Állítsa be a felhasználónevét, akkor megegyezik a név, állítsa be a GitHub profilhoz:

    ````
    git config --global user.name "John Doe"
    ````
3. A levelezés konfigurálása, hogy a megegyezik az elsődleges e-mail kijelölt GitHub profiljában; Ha egy MSFT alkalmazotthoz, kell MSFT e-mail címe:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Típus `git config -l` tekintse át a helyi ahhoz, hogy a felhasználó nevét, és az e-mailek, a konfigurációban is helyes.

##<a name="next-steps"></a>Következő lépések

- A műszaki tartalom repó tartozó tartalomtípust megértését és, hogy mit nem tartozik. További tudnivalók a [tartalom csatorna útmutatást](./content-channel-guidance.md)!
- Kövesse [ezeket a lépéseket követve hozzon létre vagy módosíthatja egy cikk, és küldje el a közzétételi](./git-commands-for-master.md).
- Másolja [a markdown sablon](../markdown templates/markdown-template-for-new-articles.md) alapjául szolgáló új cikket.
- [Ezzel az ellenőrzőlistával ki kérését ellenőrzéséhez megfelel-e a minőség feltétel](./contributor-guide-pr-criteria.md) használata űrlapegyesítéshez.


###<a name="contributors-guide-navigation"></a>Munkatársak naptárának útmutató navigációs

- [A cikk – áttekintés](./../README.md)
- [Index útmutatást cikkek](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
