<properties
    pageTitle="Azure Active Directory B2C: Felhasználói felületét testreszabási |} Microsoft Azure"
    description="A témakör a Azure Active Directory B2C felhasználói felületét testreszabási szolgáltatások"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Az Azure Active Directory B2C felhasználói felületét testreszabása

Felhasználói élmény program kiemelkedő fogyasztói elérésű alkalmazásban. Egy jó alkalmazást, és nagyszerű egy, és csak az aktív fogyasztói és valódi tevékenykedik melyeket közötti különbség. Azure Active Directory (Azure Active Directory) B2C segítségével fogyasztói-előfizetés, bejelentkezés (*Lásd a lenti megjegyzést*), a profil szerkesztése, testreszabása és a jelszó alaphelyzetbe állítása képpont tökéletes vezérlőelem-lap.

> [AZURE.NOTE]
Jelenleg helyi fiók bejelentkezési lapok, a accompaning jelszó alaphelyzetbe állítása az oldalak és ellenőrzés e-mailek testre szabható, csak a [vállalat esetleges funkció](../active-directory/active-directory-add-company-branding.md) használata nem pedig a jelen cikkben ismertetett eljárások.

Ebben a cikkben, további információ:

- A lap felhasználói felületét testreszabási szolgáltatás.
- Egy eszköz, amellyel tesztelheti a lap felhasználói felület testreszabása funkció használata a minta tartalom.
- Az alapvető allkalmazásban minden egyes lap típusát.
- Ajánlott eljárások, ha ez a funkció gyakorló.

## <a name="the-page-ui-customization-feature"></a>A lap felhasználói felület testreszabása szolgáltatás

Testre szabhatja a kinézetét és hangulatát ügyfél-előfizetés, a bejelentkezési oldal felhasználói felület testreszabása szolgáltatás, jelszó alaphelyzetbe állítása és a profil szerkesztése lapok ( [házirendek](active-directory-b2c-reference-policies.md)konfigurálásával). A fogyasztói zökkenőmentes élményt áll elő, amikor az Azure Active Directory B2C, melyet az alkalmazás és a lapok közötti navigálás.

Azure Active Directory B2C eltérően más szolgáltatásaival, ahol felhasználói felület beállításai korlátozódik vagy csak API-khoz keresztül érhető el, használja a modern (és egyszerűbb) megközelítése lap felhasználói felület testreszabása.

Hogyan működik az alábbiakban: Azure Active Directory B2C futtatja a kódot a fogyasztói böngészőben, és használja a modern megközelítés [Kereszt származási erőforrások megosztása (CORS)](http://www.w3.org/TR/cors/) nevű való betöltéséhez tartalmat egy URL-címet, adja meg, ha a házirend. Megadhatja, hogy a különböző lapokon különböző URL-eket. A kód egyesíti az Azure Active Directory B2C allkalmazásban a töltődik be az URL-CÍMEN található tartalommal, és a fogyasztói jelenít meg a lapot. Az összes kell tennie:

1. Tartalom és a HTML5-ös szabályos hozzon létre egy `<div id="api"></div>` (szót üres elem) elem található helyre, ahol a `<body>`. Ez az Azure Active Directory B2C tartalom hol beszúrt elem között.
2. Megbízhat a tartalom HTTPS-végpont (a CORS engedélyezett).
3. Stílus, a Felhasználóifelület-elemeket, amelyek a Azure Active Directory B2C szúrja be.

## <a name="test-out-the-ui-customization-feature"></a>A felhasználói felület testreszabása szolgáltatás ki tesztelése

Ha szeretne próbálja ki az a felhasználói felület testreszabása funkció használatával a minta HTML- és CSS-tartalom, biztosítunk Önnek, [egy egyszerű segítő eszköz](active-directory-b2c-reference-ui-customization-helper-tool.md) feltöltések és konfigurálásához az Azure Blob-tárolóhoz minta tartalom.

> [AZURE.NOTE]
A felhasználói felület tartalom tetszőleges üzemeltetheti: webes kiszolgálókon CDN, AWS S3, a megosztási fájlrendszer stb. Mindaddig, amíg a tartalom üzemelteti egy nyilvánosan elérhető HTTPS-végpont (engedélyezett CORS), Ön jó szeretne lépni. Azure Blob-tárolóhoz csak szemléltető célokra használja azt.

### <a name="the-most-basic-html-content-for-testing"></a>A tesztelés legalapvetőbb HTML-tartalom

Alább látható módon a tartalom használható kipróbálni ezt a lehetőséget a legalapvetőbb HTML-kódot. A korábban megadott tölthet fel, és állítsa be a tartalom a Azure Blob-tárolóban lévő azonos segítő eszköz használatával. Ezután ellenőrizheti, hogy az egyszerű, nem stilizált gombok és mezőket az egyes lapokon megjelenített és funkcionális.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Az alapvető allkalmazásban minden típusú lap

Az alábbi szakaszok, látni fogja a HTML5-ös töredékek, amely az Azure Active Directory B2C egyesíti példák a `<div id="api"></div>` a tartalom elem található. **A HTML 5 tartalmakat nem ezek töredékek beszúrni.** Az Azure Active Directory B2C szolgáltatás szúrja be őket a futásidőben. Ezek a példák segítségével saját stíluslapokat.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>A "identitás szolgáltató kiválasztására szolgáló lap" szúrt Azure Active Directory B2C tartalom

Ezen az oldalon, amelyből a felhasználók választhatnak regisztráció vagy bejelentkezés során Identitásszolgáltatók listáját tartalmazza. Ezek a közösségi Identitásszolgáltatók például Facebook és a Google +, vagy helyi fiókok (az e-mail címet vagy felhasználó neve alapján).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure Active Directory B2C tartalom illeszteni a "helyi előfizetési fióklapjának"

Ezen az oldalon, a felhasználónak töltse ki a helyi fiók egy e-mail címet vagy a felhasználónév alapján történő feliratkozás során regisztrációs űrlap tartalmazza. Az űrlap tartalmazhat más beviteli vezérlők, például beviteli mezőbe, a jelszómező bejegyzés, a választógomb, a egyetlen legördülő menük képe és a több elem kijelölése jelölőnégyzet jelölését.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure Active Directory B2C tartalom illeszteni a "" közösségi előfizetési fióklapjának"

Ezen az oldalon, amelyekben töltse ki a közösségi identitásszolgáltató, például a Facebook vagy a Google + meglévő fiókkal történő feliratkozás során az ügyfél-előfizetés űrlap tartalmazza. Ez a lap hasonlít helyi előfizetési fióklapjának (az előző szakaszban látható) kivételével a jelszó szövegbeviteli mezőben.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>A "egységesített regisztráció vagy bejelentkezés lap" szúrt Azure Active Directory B2C tartalom

Ez a lap is előfizetési, és bejelentkezés fogyasztói, például a Facebook vagy Google +, vagy helyi fiókok közösségi Identitásszolgáltatók használó kezeli.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>A "többtényezős hitelesítés lap" szúrt Azure Active Directory B2C tartalom

Ezen a lapon felhasználók regisztráció vagy bejelentkezés során ellenőrizheti a telefonszámok (használata szöveg vagy a hang).

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>A "" hibalap"szúrt Azure Active Directory B2C tartalom

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Tudnivalók a saját tartalom készítésekor

Ha azt tervezi, oldal felhasználói felület testreszabása szolgáltatással, olvassa el az alábbi gyakorlati tanácsokat:

- Nem az Azure Active Directory B2C alapértelmezett tartalom másolása, és próbálja módosítani. Célszerű az alapoktól a HTML5-ös tartalom létrehozásához és alapértelmezett hivatkozásként használni.
- Az összes lap (kivéve a hibalapjainak), melyet a bejelentkezési, előfizetési és a profil szerkesztése házirendek, Ön által stíluslapok bírálja felül az alapértelmezett stíluslapok, akkor vegye fel a következő jelennek meg kell a <head> töredékek. Az előfizetési vagy bejelentkezési és a jelszó alaphelyzetbe állítása házirendek, és minden házirendek hiba lapjainak, melyet a lapokon választania kell a szükséges összes stíluselemeket saját magának.
- Biztonsági okokból azt nem teszi lehetővé bármely JavaScript szerepeltetni a tartalmakat. Mire van szüksége a legtöbb elérhető beépített kell lennie. Ha nem, akkor kérhet új funkciók használata a [Felhasználó hangposta](http://feedback.azure.com/forums/169401-azure-active-directory) .
- Támogatott böngészők:
    - Az Internet Explorer 11
    - Az Internet Explorer 10
    - Az Internet Explorer 9 (korlátozott)
    - Az Internet Explorer 8 (korlátozott)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0
