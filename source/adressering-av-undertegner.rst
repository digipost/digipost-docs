.. _adressering-av-undertegner:

Adressering av undertegner
***************************
Måten undertegner adresseres og identifiseres på, er litt forskjellig avhengig av om han signerer i direkteflyt eller portalflyt, og om avsender bruker API eller avsenderportalen.

..  TIP::
    Hvis du ønsker å lese en introduksjon til de forskjellige flytene en undertegner kan signere i, se :ref:`signeringsflyt`.

..  CAUTION::
    Måten du adresserer undertegner på, påvirker :ref:`innholdet og utseendet på undertegners signatur <identifisereUndertegnere>`.

Posten signering sender ut varsel om signeringsoppdrag til undertegner på e-post og/eller SMS, hvis signering skjer i portalflyt. Ved signering i direkteflyt er det avsender som står for varsling til undertegner.

**Avsender som bruker API kan**

- adressere med fødselsnummer
- adressere uten fødselsnummer
- adressere med egenvalgt identifikator (eksempelvis kundenummer)

**Avsender som bruker avsenderportal på web kan**

- adressere med fødselsnummer
- adressere uten fødselsnummer (N.B: Kun hvis privat virksomhet)


Det signerte dokumenter vil inneholde navn på undertegner og hvilken elektronisk ID som ble brukt. Man kan for øvrig velge hvilken :ref:`identifikator man ønsker i signerte dokumenter <identifisereUndertegnere>`.


Adressering med fødselsnummer
===============================
For å adressere med fødselsnummer må du vite undertegners fødselsnummer, samt e-postadresse og/eller mobilnummer. Denne måten å adressere på gir en sikker identifisering av undertegner, siden kun én bestemt person har mulighet til å åpne og signere dokumentet.

Ved :ref:`Signering i portalflyt med fødselsnummer <signering-i-portalflyt-med-fødselsnummer>` må undertegner logge inn i signeringsportalen før han kan se og signere dokumentet. Dette gjør altså at kun riktig person har mulighet til å lese og signere. Se signering i portalflyt, adressering med f.nr her.

Ved :ref:`signering-i-direkteflyt` er undertegner logget inn i avsenders system, og det er da avsender som er ansvarlig for å sende undertegner til riktig lenke i signeringstjenesten.

Når du som avsender velger adressering med fødselsnummer, kan du velge å inkludere fødselsnummeret i det signerte dokumentet.


Adressering uten fødselsnummer
===============================

Ved adressering uten fødselsnummer blir ikke signeringsoppdraget knyttet til én spesifikk person. Derfor krever signeringstypen ingen innlogging, og du trenger ikke vite undertegners fødselsnummer. I stedet sender du *kun* inn e-postadressen og/eller mobilnummeret som skal motta varsel om signeringen.  Se :ref:`varsler <varslerUtenFødselsnummer>` for å se hvordan varselet ser ut.

Ved :ref:`Signering i portalflyt uten fødselsnummer <signering-i-portalflyt-uten-fødselsnummer>` får undertegner en unik lenke og en kode, og han kan se og signere dokumentet uten innlogging.

Ved :ref:`signering-i-direkteflyt`  så sender du fra API. Da har du mulighet til å bruke en egenvalgt identifikator i stedet for e-postadresse og mobilnummer. Dette kan f.eks. være et kundenummer.

Når du som avsender velger adressering uten fødselsnummer, vil ikke fødselsnummeret bli inkludert i det signerte dokumentet, av personvernmessige hensyn. Du vil fortsatt få med navn og eventuelt fødselsdato i det signerte dokumentet.

..  IMPORTANT::
    Selve signaturen er like sikker og gyldig som når du adresserer med fødselsnummer, men du som avsender er ansvarlig for at riktig person åpner og signerer dokumentet.

.. _varsler:

Kontaktinformasjon
*********************

 - Alle undertegnere må ha minst én av e-postadresse og mobilnummer.
 - Sending av SMS er frivillig og kan bestilles av tjenesteeieren, dette koster 40 øre per SMS.
 - Dersom en undertegner har mobilnummer og ikke e-postadresse vil det alltid bli sendt SMS.
 - Tjenesten støtter kun norske mobilnumre.

Som **bedrift** må du selv vite og legge til e-postadressen og/eller mobilnummeret til undertegner. Det er ikke mulig å bruke Kontakt- og reservasjonsregisteret.

For **offentlige virksomheter** gjør vi oppslag i `Kontakt- og reservasjonsregisteret <https://samarbeid.difi.no/kontakt-og-reservasjonsregisteret>`_ hvis ikke kontaktinformasjon overstyres.

..  CAUTION::
    Hvis undertegnere er reservert mot digital kommunikasjon vil oppdraget bli avvist og påfølgende uthenting av status for oppdraget vil gi en feil med informasjon om hvilke undertegnere som er reservert. Undertegnere med overstyrt kontaktinformasjon blir ikke sjekket for reservasjon.

..  NOTE::
    Det er kun tillatt å overstyre kontaktinformasjon som en offentlig virksomhet hvis undertegner ikke signerer som privatperson, det vil si signerer i kraft av en rolle for en virksomhet.


Bruk av Kontakt- og reservasjonsregisteret
============================================

Ytterligere informasjon rundt bruk av Kontakt- og reservarsjonregisteret

 - Ved utsending av senere varsler (enten utsatt aktivering på grunn av kjedet signatur eller påminnelser) blir det gjort et nytt oppslag mot registeret for å hente ut den sist oppdaterte kontaktinformasjonen.
 - Dersom Oppslagstjenesten for Kontakt- og reservasjonsregisteret er utilgjengelig ved utsending av påminnelser vil resultatet fra oppslaget ved opprettelse av oppdraget bli brukt.
 - Reservasjon ved utsatte førstegangsvarsler: I scenariet der tjenesteeier har satt en kjedet rekkefølge på undertegnerne, og førstegangsvarsel skal sendes til en undertegner som i perioden mellom oppdraget ble opprettet og førstegangsvarsel skal sendes har reservert seg mot elektronisk kommunikasjon, så vil hele oppdraget feile.
 - Reservasjon ved påminnelser: Hvis sluttbrukeren har reservert seg etter at oppdraget ble opprettet, men oppdraget allerede er aktivert, vil det ikke bli sendt påminnelser (e-post/SMS), men oppdraget vil heller ikke feile før signeringsfristen eventuelt løper ut.
 - Oppdrag med overstyrt kontaktinformasjon med utenlandsk mobilnummer vil bli avvist, mens utenlandske mobilnumre fra Kontakt- og reservasjonsregisteret vil bli ignorert.
