.. _varsler-regler-for-utsending:

Varsler: Regler for utsending
=============================

For å sørge for at undertegner mottar varsler og påminnelser i passende tidsrom, er tidspunktene for når varsler blir sendt ut avhengig av signeringsfristen for et oppdrag.

Hvis avsender har lagt til både e-postadressen og mobilnummeret til undertegner, sendes det ut inntil 3 varsler om signeringen: Et førstegangsvarsel umiddelbart etter aktivering, påminnelse på e-post, og til slutt en siste påminnelse på SMS. Bakgrunnen for dette oppsettet, er at e-post da benyttes som primær varslingskanal og at SMSen skal fungere som et siste, eskalerende varsel.

=============== ================= ================= =================
Signeringsfrist 1. varsel: e-post 2. varsel: e-post 3. varsel: SMS
=============== ================= ================= =================
0-24 timer      Ved aktivering                      Ved aktivering
2-3 dager       Ved aktivering    1 dag før frist   1 dag før frist
4-5 dager       Ved aktivering    2 dager før frist 1 dag før frist
6-9 dager       Ved aktivering    3 dager før frist 2 dager før frist
10+ dager       Ved aktivering    5 dager før frist 2 dager før frist
=============== ================= ================= =================

.. raw:: html

   <!-- Tabellen er generert vha. http://www.tablesgenerator.com/markdown_tables -->

Hvis avsender har lagt til kun e-postadressen eller kun mobilnummeret til undertegner, sendes det ut inntil 2 varsler om signeringen: Et førstegangsvarsel umiddelbart etter aktivering, og én påminnelse på henholdsvis e-post eller SMS. 

=============== ===================== =====================
Signeringsfrist 1. varsel: e-post/SMS 2. varsel: e-post/SMS
=============== ===================== =====================
0-24 timer      Ved aktivering
2-3 dager       Ved aktivering        1 dag før frist
4-5 dager       Ved aktivering        2 dager før frist
6-9 dager       Ved aktivering        3 dager før frist
10+ dager       Ved aktivering        5 dager før frist
=============== ===================== =====================

.. raw:: html

   <!-- Tabellen er generert vha. http://www.tablesgenerator.com/markdown_tables -->

.. NOTE:: SMS sendes ikke ut mellom klokken 22:00 og 08:00, med mindre oppdraget opprettes på natten og fristen er så kort at det er nødvendig med umiddelbar utsending.

.. NOTE:: Hvis avsender *utvider signeringsfristen* slettes alle planlagte varsler for oppdraget. Det blir da generert nye varsler som sendes ut på relative tidspunkt knyttet til den nye fristen.

.. CAUTION:: Får å unngå at vi kommer i skade for å sende varsler til reelle mottakere i testmiljøer, er det lagt inn noen sikkerhetsmekanisme i difitest og difiqa: e-postvarsler inkluderer en setning som indikerer at varselet kommer fra et testmiljø: "Dette er en test-e-post sendt for Difi fra Postens signeringstjeneste" og SMS-varslene erstattes i sin helhet med setningen: "Dette er en test-SMS sendt for Difi fra Postens signeringstjeneste".


Varseltekster for undertegnere
===============================

Oppsettet på varslene som blir sendt ut er predefinert og ikke mulig å endre på for deg som avsender, men du kan legge til en tittel/beskrivelse av dokumentet som skal signeres. 

Innholdet i varselet vil variere ut fra om

- adresseringen til undertegner er med eller uten fødselsnummer
- hvilken kanal de sendes i (e-post/SMS)
- sektor som avsender sender fra (privat eller offentlig)
- antall undertegnere på oppdraget

Nedenfor vises de ulike variantene av varslene som sendes på e-post og SMS.


Varsel om dokument til signering, ved adressering med fødselsnummer
____________________________________________________________________

..  tabs::

    ..  tab:: E-post 1. varsel

        **Emne**: Dokument til signering fra [*Avsender*]

        Hei!

        Du har fått en forespørsel om å signere et dokument fra [*Avsender*]: [*Tittel på dokumentet*].
        
        Dokumentet er nå signert av [*antall*] og må signeres innen [*signeringsfrist*] / Dokumentet må signeres innen [*signeringsfrist*].
        
        Du kan signere med [*disse elektroniske e-IDene*].

        Logg deg inn på [*signering.posten.no/logginn*] for å signere dokumentet.

        Hilsen Posten
    
      
    ..  tab:: E-post 2. varsel

        **Emne**: Påminnelse: Dokument til signering fra [*Avsender*]

        Hei!

        Vi vil minne om at du fortsatt har et dokument til signering fra [*Avsender*]: [*Tittel på dokumentet*].
        
        Dokumentet er nå signert av [*antall*] og må signeres innen [*signeringsfrist*] / Dokumentet må signeres innen [*signeringsfrist*].
        
        Du kan signere med [*disse elektroniske e-IDene*].

        Logg deg inn på [*signering.posten.no/logginn*] for å signere dokumentet.

        Rekker du ikke å signere innen fristen? Usignerte dokumenter slettes når fristen går ut. Kontakt [*Avsender*] for å få dokumentet tilsendt på nytt.

        Hilsen Posten


..  tabs::

    ..  tab:: SMS 1. varsel

        Du har et dokument til signering fra [*Avsender*]. Logg inn og signer på [*signering.posten.no/logginn*] innen [*signeringsfrist*].
         
    ..  tab:: SMS 2./3. varsel

        Du har et usignert dokument fra [*Avsender*]. Logg inn og signer på [*signering.posten.no/logginn*] innen [*signeringsfrist*].
         
.. _varslerUtenFødselsnummer:

Varsel om dokument til signering, ved adressering uten fødselsnummer
____________________________________________________________________

..  tabs::
         
    ..  tab:: E-post 1. varsel

        **Emne**: Dokument til signering fra [*Avsender*]

        Hei!
        Du har fått en forespørsel om å signere et dokument fra [*Avsender*]: [*Dokumenttittel*].
        
        Dokumentet er nå signert av [*antall*] og må signeres innen [*signeringsfrist*] / Dokumentet må signeres innen [*signeringsfrist*].
        
        Du kan signere med *disse elektroniske ID-ene*.
        
        Slik signerer du:
        1) Klikk på lenken under
        2) Skriv inn sikkerhetskode XXXX
        3) Les og signer dokumentet
        
        [*https://signering.posten.no/uniklenke*]
        
        Hilsen Posten
         
    ..  tab:: E-post 2. varsel

        **Emne**: Dokument til signering fra [*Avsender*]
        
        Hei!
        Vi vil minne om at du fortsatt har et dokument til signering fra [*Avsender*]: [*Dokumenttittel*].
        
        [*Dokumentet er nå signert av [*antall*] og må signeres innen [*signeringsfrist*] / Dokumentet må signeres innen [*signeringsfrist*].
               
        Du kan signere med [*disse elektroniske ID-ene*].
        
        Slik signerer du:
        1) Klikk på lenken under
        2) Skriv inn sikkerhetskode [*XXX*]
        3) Les og signer dokumentet
        
        [*https://signering.posten.no/uniklenke*]
        
        Rekker du ikke å signere innen fristen?
        Usignerte dokumenter slettes når fristen går ut. Kontakt [*Avsender*] for å få dokumentet tilsendt på nytt.
               
        Hilsen Posten

.. tabs::
         
    ..  tab:: SMS 1. varsel

        Hei! [*Avsender*] ber deg signere et dokument. Bruk kode [*XXXX*] på [*https://signering.posten.no/uniklenke*] før [*signeringsfrist*].
         
    ..  tab:: SMS 2./3. varsel

        Hei! Husk signering for [*Avsender*]. Bruk kode [*XXXX*] på [*https://signering.posten.no/uniklenke*] før [*signeringsfrist*].



Etter signering: Varsel om oppsalg til digital postkasse
________________________________________________________

Etter at en undertegner har signert et dokument, vil hun i *disse tilfeller* få mulighet til å opprette en digital postkasse. Hvis avsender er privat, vil undertegner få mulighet til å opprette konto hos Digipost, og hvis avsender er offentlig vil undertegner kunne velge digital postkasse på Norge.no.

Innholdet i dette varselet er ulikt avhengig av hvor mange undertegnere som skal signere dokumentet, og om avsender er privat eller offentlig.

Private avsendere
^^^^^^^^^^^^^^^^^^^

..  tabs::

    ..  tab:: E-post, én undertegner

        **Emne**: Motta det signerte dokumentet i Digipost

        Hei!

        Du har nettopp signert et dokument fra [*Avsender*] gjennom Posten signering.

        Hvis du oppretter en konto i Digipost innen 7 dager, sendes dokumentet du signerte automatisk dit. Da har du det              lett tilgjengelig når du trenger det!
         
        Registrer deg i Digipost: https://www.digipost.no/app/registrering ,

        Hilsen Posten
    
    ..  tab:: E-post, flere undertegnere

        **Emne**: Motta det signerte dokumentet i Digipost

        Hei!

        Du har tidligere signert et dokument fra [*Avsender*] gjennom Posten signering. Nå har alle undertegnerne signert, og avsender har mottatt det ferdigsignerte dokumentet.

        Hvis du også ønsker å motta dokumentet med alle signaturer, må du opprette en konto i Digipost innen 7 dager. Da sendes dokumentet automatisk dit, så har du det lett tilgjengelig når du trenger det.

        Registrer deg i Digipost: https://www.digipost.no/app/registrering ,
         
        Hilsen Posten
        
        
..  tabs::

    ..  tab:: SMS, én undertegner
       
        Hei, du har nettopp signert et dokument fra [*Avsender*] gjennom Posten signering.
        Hvis du oppretter en konto i Digipost innen 7 dager, sendes dokumentet du signerte automatisk dit: https://www.digipost.no/app/registrering

    ..  tab:: SMS, flere undertegnere
       
        Hei! Du har tidligere signert et dokument fra [*Avsender*] gjennom Posten signering.

        Nå har alle undertegnerne signert. Hvis du også ønsker å motta dokumentet med alle signaturer, må du opprette en konto i Digipost innen 7 dager. Da sendes dokumentet automatisk dit, så har du det lett tilgjengelig når du trenger            det: https://www.digipost.no/app/registrering


Offentlige avsendere
^^^^^^^^^^^^^^^^^^^^^
      
..  tabs::
      
    ..  tab:: E-post, én undertegner
       
        **Emne**: Motta det signerte dokumentet i din digitale postkasse

        Hei!

        Du har nettopp signert et dokument fra [*Avsender*] gjennom den nasjonale fellesløsningen e-Signering.

        Hvis du oppretter en konto i Digipost innen 7 dager, sendes dokumentet du signerte automatisk dit. Da har du det lett tilgjengelig når du trenger det!

        Opprett digital postkasse:
        https://www.norge.no/velg-digital-postkasse
 
    ..  tab:: E-post, flere undertegnere
       
        **Emne**: Motta det signerte dokumentet i din digitale postkasse

        Hei!

        Du har tidligere signert et dokument fra [*Avsender*] gjennom den nasjonale fellesløsningen e-Signering. Nå har alle undertegnerne signert, og avsender har mottatt det ferdigsignerte dokumentet. Hvis du også ønsker å motta dokumentet          med alle signaturer, må du opprette en digital postkasse innen 7 dager. Da sendes dokumentet automatisk dit, så har du det tilgjengelig når du trenger det!
         
        Opprett digital postkasse:
        https://www.norge.no/velg-digital-postkasse
        
..  tabs::
      
    ..  tab:: SMS, én undertegner
       
        Hei, du har nettopp signert et dokument fra [*Avsender*] gjennom den nasjonale fellesløsningen e-Signering.
        Hvis du oppretter en digital postkasse innen 7 dager, sendes dokumentet du signerte automatisk dit:                            https://www.norge.no/velg-digital-postkasse

    ..  tab:: SMS, flere undertegnere
       
        Hei, du har tidligere signert et dokument fra [*Avsender*] gjennom den nasjonale fellesløsningen e-Signering. Nå har alle undertegnerne signert. Hvis du også ønsker å motta dokumentet med alle signaturer, må du opprette en digital postkasse innen 7 dager. Da sendes dokumentet automatisk dit, så har du det lett tilgjengelig når du trenger det: https://www.norge.no/velg-digital-postkasse


Varsel om kansellert oppdrag
_______________________________

Hvis avsender *kansellerer* et signeringsoppdrag, blir det sendt ut et varsel til undertegner om dette:

..  tabs::
      
    ..  tab:: E-post
       
        **Emne**: Kansellert: Dokument til signering fra [*Avsender*]
        
        Hei!
        [*Avsender*] har trukket tilbake forespørselen om signering av [*Dokumenttittel*].
        Kontakt [*Avsender*] om du lurer på hvorfor de kansellerte, eller om du ønsker dokumentet tilsendt på nytt.
        
        Hilsen Posten


.. _varseltekster-for-avsendere:

