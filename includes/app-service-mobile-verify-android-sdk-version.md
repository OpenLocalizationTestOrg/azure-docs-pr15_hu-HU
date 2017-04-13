Folyamatos fejlesztési, mert az Android Studio Android SDK verziója nem egyeznek meg a kódot a verzióra. A hivatkozott ebben az oktatóanyagban Android SDK verziója 23, írásakor legkésőbb. A verziószám növelheti a SDK új kiadásai jelennek meg, és a rendelkezésre álló legújabb verziójának használata ajánlott.

A verzió eltéréseket két jelei a következők:

1. Javításakor hozza létre újra a projektet, például a "**nem sikerült megtalálni a cél Google Inc.:Google APIs:n**" Gradle hibaüzenetek jelenhetnek meg.

2. Szabványos Android objektumok megoldható kódban alapján `import` előfordulhat, hogy a kimutatások hibaüzenetek generálni.

Ha ezek közül jelenik meg, az Android SDK Android Studio telepített verziója nem egyeznek meg a letöltött projekt SDK célját.  Ha ellenőrizni szeretné a verziót, a következő módosításokat:


1. Android Studio, kattintson az **eszközök** => **Android** => **SDK Manager**. Ha nem telepítette a SDK Platform legújabb verzióját, majd kattintson a telepítéshez. Jegyezze fel a verziószám.

2. A Project Explorer lap **Gradle parancsfájlok**, nyissa meg a fájlt **build.gradle (modeule: alkalmazás)**. Győződjön meg arról, hogy a legújabb SDK verziót meghagy a **compileSdkVersion** és **buildToolsVersion** vannak beállítva. A címkék előfordulhat, hogy néz ki:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Az Android Studio Project Explorer kattintson a jobb gombbal a projekt csomópontját, válassza a **Tulajdonságok parancsot**, és válassza ki a bal oldali oszlopban **Android**. Győződjön meg arról, hogy a **Projekt összeállítása cél** SDK azonos verzióban, a **targetSdkVersion**értéke.

4. Az Android Studio a nyilvánvalóan fájl már nem használatos határozza meg, hogy a célhely SDK SDK legkisebb verziószáma, eltérően Holdas esetében.
