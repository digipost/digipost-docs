.. _egenPortalIntegrasjon:

API-integrasjon for signering i portalflyt
********************************************

Dette integrasjonsmønsteret passer for tjenesteeiere som ønsker å opprette :ref:`signeringsoppdrag i portalflyt <signering-i-portalflyt>`. Signeringsseremonien gjennomføres av sluttbruker i Signeringsportalen, og tjenesteeier vil deretter kunne polle på status og hente ned det signerte dokumentet.

Dette scenariet er utviklet med tanke på å støtte en flyt hvor det er behov for å innhente signaturer fra flere enn én undertegner.

Meldingsformatet i APIet er XML, og reelevante typer finnes i filen `portal.xsd <https://github.com/digipost/signature-api-specification/blob/master/schema/xsd/portal.xsd>`_.

|portalflytskjema|
 **Flytskjema signeringsoppdrag i portalflyt:** *skjemaet viser at avsender sender inn et oppdrag, starter polling, at undertegner(e) signerer oppdraget, og avsender får oppdatert status via polling, og laster ned signert dokument. Dersom du sender et oppdrag til kun én undertegner, kan du se bort i fra den første "steg 4"-seksjonen. Heltrukne linjer viser brukerflyt, mens stiplede linjer viser API-kall.*

.. _egenPortalIntegrasjonSteg1:

Steg 1: Opprette signeringsoppdraget
======================================
Flyten begynner ved at tjenesteeier gjør et API-kall for å opprette signeringsoppdraget. Dette kallet gjøres som en multipart-request, der den ene delen er dokumentpakken og den andre delen er metadata.

-  Kallet gjøres som en ``HTTP POST`` mot ressursen ``<rot-URL>/portal/signature-jobs``.
-  Dokumentpakken legges med multipart-kallet med mediatypen ``application/octet-stream``. Se :ref:`informasjonOmDokumentpakken` for mer informasjon om dokumentpakken.
-  Metadataene som skal sendes med i dette kallet er definert av elementet ``portal-signature-job-request``. Disse legges i multipart-kallet med mediatypen ``application/xml``.

Følgende er et eksempel på metadata for et signeringsoppdrag i portalflyt:

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
   <portal-signature-job-request xmlns="http://signering.posten.no/schema/v1">
       <reference>123-ABC</reference>
       <polling-queue>custom-queue</polling-queue>
   </portal-signature-job-request>

Følgende er et eksempel på ``manifest.xml`` fra dokumentpakken for et signeringsoppdrag som skal signeres av fire undertegnere:

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
   <portal-signature-job-manifest xmlns="http://signering.posten.no/schema/v1">
       <signers>
           <signer order="1">
               <personal-identification-number>12345678910</personal-identification-number>
               <signature-type>ADVANCED_ELECTRONIC_SIGNATURE</signature-type>
               <notifications>
                   <!-- Override contact information to be used for notifications -->
                   <email address="signer1@example.com" />
                   <sms number="00000000" />
               </notifications>
           </signer>
           <signer order="2">
               <personal-identification-number>10987654321</personal-identification-number>
               <signature-type>AUTHENTICATED_ELECTRONIC_SIGNATURE</signature-type>
               <notifications>
                   <email address="signer2@example.com" />
               </notifications>
           </signer>
           <signer order="2">
               <personal-identification-number>01013300001</personal-identification-number>
               <signature-type>AUTHENTICATED_ELECTRONIC_SIGNATURE</signature-type>
               <notifications-using-lookup>
                   <!-- Try to send notifications in both e-mail and SMS using lookup -->
                   <email/>
                   <sms/>
               </notifications-using-lookup>
           </signer>
           <signer order="3">
               <personal-identification-number>02038412546</personal-identification-number>
               <signature-type>AUTHENTICATED_ELECTRONIC_SIGNATURE</signature-type>
               <notifications-using-lookup>
                   <email/>
               </notifications-using-lookup>
           </signer>
       </signers>
       <sender>
           <organization-number>123456789</organization-number>
       </sender>
       <document href="document.pdf" mime="application/pdf">
           <title>Tittel</title>
           <nonsensitive-title>Sensitiv tittel</nonsensitive-title>
           <description>Melding til undertegner</description>
       </document>
       <required-authentication>4</required-authentication>
       <availability>
           <activation-time>2016-02-10T12:00:00+01:00</activation-time>
           <available-seconds>864000</available-seconds>
       </availability>
       <identifier-in-signed-documents>PERSONAL_IDENTIFICATION_NUMBER_AND_NAME</identifier-in-signed-documents>
   </portal-signature-job-manifest>

Undertegnere
--------------

Du bør se :ref:`varsler` og :ref:`adressering-av-undertegner` før du starter med dette kapitlet.

Undertegnere kan adresseres og varsles på ulike måter:

..  tabs::

    ..  tab:: E-post

        ..  code-block:: xml

            <signer>
                <identified-by-contact-information/>
                <notifications>
                    <email address="email@example.com"/>
                </notifications>
                <on-behalf-of>SELF</on-behalf-of>
            </signer>

    ..  tab:: Mobil

        ..  code-block:: xml

            <signer>
                <identified-by-contact-information/>
                <notifications>
                    <sms number="00000000" />
                </notifications>
                <on-behalf-of>SELF</on-behalf-of>
            </signer>

    ..  tab:: E-post og mobil

        ..  code-block:: xml

            <signer>
                <identified-by-contact-information/>
                <notifications>
                    <email address="email@example.com"/>
                    <sms number="00000000" />
                </notifications>
                <on-behalf-of>SELF</on-behalf-of>
            </signer>

    ..  tab:: Fødselsnummer

        Med varsling til gitt epostadresse:

        ..  code-block:: xml

            <signer>
                <personal-identification-number>12345678910</personal-identification-number>
                <notifications>
                    <email address="email@example.com"/>
                </notifications>
                <on-behalf-of>SELF</on-behalf-of>
            </signer>


        Med varsling som offentlig virksomhet:

        ..  NOTE::
            Som offentlig virksomhet skal oppslag gjøres vha. Kontakt- og Reservasjonsregisteret.

        ..  code-block:: xml

            <signer>
                <personal-identification-number>12345678910</personal-identification-number>
                <notifications>
                    <notifications-using-lookup/>
                </notifications>
                <on-behalf-of>SELF</on-behalf-of>
            </signer>

    ..  tab:: På vegne av

        En avsender kan velge om undertegner signerer på vegne av seg selv eller i kraft av en rolle. Dette gjøres ved å sette attributtet ``on-behalf-of`` til enten ``SELF`` eller ``OTHER``.

        Dersom man signerer på vegne av noen andre, vil det i praksis bety at signert dokument ikke sendes videre til undertegners postkasse. For offentlige virksomheter brukes heller ikke Kontakt- og reservasjonsregisteret, og man må adressere undertegner på egenvalgt telefonnummer og e-postadresse.

        ..  code-block:: xml

            <signer>
                <personal-identification-number>12345678910</personal-identification-number>
                <notifications>
                    <email address="email@example.com"/>
                    <sms number="00000000" />
                </notifications>
                <on-behalf-of>OTHER</on-behalf-of>
            </signer>

        ..  NOTE::
            Elementet ``notifications-using-lookup`` er kun tilgjengelig for offentlige virksomheter. Ettersom dette vil slå opp undertegners *private* kontaktinformasjon, kan man ikke samtidig angi at vedkommende signerer på vegne av noen andre. Altså, man kan ikke sette ``on-behalf-of`` til ``OTHER`` dersom man ønsker å benytte Kontakt- og reservasjonsregisteret for å adressere undertegner.


Andre innstillinger
---------------------------

Rekkefølge
^^^^^^^^^^^
``order``-attributtet på ``signer`` brukes til å angi rekkefølgen på undertegnerne. I eksempelet over vil signeringsoppdraget først kun bli tilgjengelig for undertegnerne med ``order="1"``. Når disse har signert, blir oppdraget tilgjengelig for de med ``order="2"``, og for undertegneren med ``order="3"`` når de med ``order="2"`` har signert.

Tilgjengelighet
^^^^^^^^^^^^^^^^
Elementet ``availability`` brukes til å kontrollere tidsrommet et signeringsoppdrag er tilgjengelig for undertegner(e).


..  code-block:: xml

    <availability>
        <activation-time>2016-02-10T12:00:00+01:00</activation-time>
        <available-seconds>864000</available-seconds>
    </availability>

Tidspunktet angitt i ``activation-time`` angir når jobben aktiveres, og de første undertegnerne får mulighet til å signere oppdraget. Varigheten angitt i ``available-seconds`` gjelder for alle undertegnere. Det vil si at alle undertegnere vil få like lang tid til å signere eller avvise oppdraget fra det blir tilgjengelig for dem. Dette tidsrommet gjelder altså for hvert sett med undertegnere med samme ``order``.

**Eksempel, angi 345600 sekunder (4 dager) for undertegnere med rekkefølge:**

#. Undertegnere med ``order=1`` får 4 dager fra ``activation-time`` til å signere.
#. Undertegnere med ``order=2`` vil få tilgjengeliggjort dokumentet *umiddelbart* når alle undertegnere med ``order=1`` har signert. De vil da få 4 dager fra tidspunktet de fikk oppdraget tilgjengelig.

..  NOTE::
    Dersom man utelater ``availability`` vil jobben aktiveres umiddelbart, og oppdraget vil være tilgjengelig i maks 30 dager for hvert sett med ``order``-grupperte undertegnere.

..  IMPORTANT::
    Et signeringsoppdrag utløper og stopper dersom minst én undertegner ikke signerer innenfor sitt tidsrom når oppdraget er tilgjengelig.

..  IMPORTANT::
    Jobber som angir større ``available-seconds`` enn 7 776 000 sekunder (90 dager) blir avvist av tjenesten.

Identifikator i signert dokument
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Elementet ``identifier-in-signed-documents`` brukes for å angi hvordan undertegner skal identifiseres i de signerte dokumentene. Tillatte verdier er ``PERSONAL_IDENTIFICATION_NUMBER_AND_NAME``, ``DATE_OF_BIRTH_AND_NAME`` og ``NAME``, men ikke alle er gyldige for alle typer signeringsoppdrag og avsendere. For mer informasjon, se :ref:`identifisereUndertegnere`.

Respons
--------

Som respons på dette kallet vil man få elementet ``portal-signature-job-response``. Denne responsen inneholder en ID generert av signeringstjenesten. Du må lagre denne IDen i dine systemer slik at du senere kan koble resultatene du får fra polling-mekanismen til riktig oppdrag.

.. code-block:: xml

   <portal-signature-job-response xmlns="http://signering.posten.no/schema/v1">
       <signature-job-id>1</signature-job-id>
       <cancellation-url>https://api.signering.posten.no/api/{sender-identifier}/portal/signature-jobs/1/cancel</cancellation-url>
   </portal-signature-job-response>

Steg 2: Polling på status
==========================

For å finne ut hva statusen er for de signeringsoppdragene du har opprettet, må du jevnlig sende forespørsler til signeringstjenesten (polling). Som avsender må du sjekke hvilket oppdrag statusoppdateringen gjelder for å oppdatere i ditt system og så bekrefte den.

Responsen på dette kallet vil være én av to ting:

- **statusoppdatering:** en ``200 OK``-respons som inneholder informasjon om ny status for ett oppdrag. Denne er definert av elementet ``portal-signature-job-status-change-response``.
- **ingen oppdatering tilgjengelig:** Dersom det ikke er noen oppdateringer for dine signeringsoppdrag, vil du få en ``204 No Content``-respons.

Hyppighet
----------

Responsene vil alltid inneholde HTTP-headeren ``X-Next-permitted-poll-time`` som forteller deg når du kan gjøre neste forespørsel, og det er viktig at dette tidspunktet overholdes. Dersom man sender en forespørsel før dette tidspunktet har passert, vil man få en ``429 Too Many Requests``-respons tilbake. Denne vil også inneholde headeren ``X-Next-permitted-poll-time`` med et nytt tidspunkt.

..  NOTE::
    I produksjonsmiljøet vil neste tillatte polling-tidspunkt være om 10 minutter om køen er tom, mens for testmiljøer vil det være mellom 5 og 30 sekunder.

I praksis vil tidspunktet for neste tillatte polling-forespørsel være umiddelbart så lenge man får en respons som inneholder en statusoppdatering.


Integrasjon
------------


For å polle, så gjør du en ``HTTP GET`` mot ``<rot-URL>/portal/signature-jobs``. Oppdrag som ikke er lagt på en spesifikk kø vil havne på en standard-kø. Hvis signeringsoppdraget er lagt på en spesifikk kø, så må også query-parameteret ``polling_queue`` settes til navnet på køen, f.eks. ``<rot-URL>/portal/signature-jobs?polling_queue=custom-queue``. Du skal ikke ha med noen request-body på dette kallet.

Følgende er et eksempel på en respons der en del av signeringsoppdraget har blitt fullført:

.. code-block:: xml

   <portal-signature-job-status-change-response xmlns="http://signering.posten.no/schema/v1">
       <signature-job-id>1</signature-job-id>
       <status>IN_PROGRESS</status>
       <confirmation-url>https://api.signering.posten.no/api/{sender-identifier}/portal/signature-jobs/1/complete</confirmation-url>
       <signatures>
           <signature>
               <status since="2017-01-23T12:51:43+01:00">SIGNED</status>
               <personal-identification-number>12345678910</personal-identification-number>
               <xades-url>https://api.signering.posten.no/api/{sender-identifier}/portal/signature-jobs/1/xades/1</xades-url>
           </signature>
           <signature>
               <status since="2017-01-23T12:00:00+01:00">WAITING</status>
               <personal-identification-number>98765432100</personal-identification-number>
           </signature>
           <pades-url>https://api.signering.posten.no/api/{sender-identifier}/portal/signature-jobs/1/pades</pades-url>
       </signatures>
   </portal-signature-job-status-change-response>


Statusoppdateringer du henter vil forsvinne fra køen. Dette gjør det mulig å spørre om statusoppdateringer i parallell, og du vil ikke få samme statusoppdatering to ganger. Det er derfor viktig at du bekrefter mottak av hver statusoppdatering så raskt som mulig, for dersom det likevel skulle skje en feil under overføring eller prosessering, så vil kvitteringen legges på køen igjen etter 10 minutter. Mer informasjon om hvordan du bekrefter en kvittering er beskrevet i :ref:`egen-integrasjon-steg-4`.

Steg 3: Laste ned PAdES eller XAdES
=====================================

Responsen i forrige steg inneholder lenkene ``xades-url`` og ``pades-url``. Disse kan du gjøre en ``HTTP GET`` på for å laste ned det signerte dokumentet i de to formatene. For mer informasjon om format på det signerte dokumentet, se :ref:`signerte-dokumenter`.

XAdES-filen laster du ned per undertegner, mens PAdES-filen lastes ned på tvers av alle undertegnere. Denne vil inneholde signeringsinformasjon for alle undertegnere som frem til nå har signert oppdraget. I de aller fleste tilfeller er det ikke aktuelt å laste ned denne før alle undertegnerne har statusen ``SIGNED``.

..  _egen-integrasjon-steg-4:

Steg 4: Bekrefte ferdig prosessering
======================================

Til slutt gjør du et ``HTTP POST``-kall mot ``confirmation-url`` for å bekrefte at du har mottatt/persistert statusoppdateringen. Dersom statusen indikerer at oppdraget er helt ferdig, så vil dette kallet også bekrefte at du er ferdig med å prosessere hele oppdraget.
Hvis :ref:`langtidslagring` benyttes vil dette markere oppdraget som ferdig og lagret, ellers vil oppdraget slettes fra signeringstjenesten.

..  |portalflytskjema| image:: https://raw.githubusercontent.com/digipost/signature-api-specification/master/integrasjon/flytskjemaer/asynkron-maskin-til-maskin.png
    :alt: Flytskjema for portalintegrasjon
