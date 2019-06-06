.. _signaturtype:

Signaturtype
*************

I Norge har vi tre forskjellige måter å signere digitalt på, som beskrevet i E-signaturloven: autentisert, avansert og kvalifisert. Autentisert signatur er en sterk signatur, avansert signatur er enda sterkere, og kvalifisert signatur er den sterkeste signaturen.

Vi tilbyr autentisert og avansert signatur, som er rettskraftige signaturer på lik linje med håndskrift på papir. Kvalifisert signatur eksisterer ikke per i dag, men er tenkt oppfylt på lang sikt gjennom Nasjonalt ID-kort. 

..  _avansert-signatur:

Avansert signatur
==================

Ved bruk av avansert signatur så skjer signering med BankID. Det er her en sterk knytning mellom identifiseringshandling og dokument som skal signeres.

En avansert signatur har en sterkere knytning mellom signaturen og dokumentet enn en :ref:`autentisert-signatur` fordi det er kun BankID som er involvert i den prosessen.

..  TIP::
    Signering kan gjøres med BankId, Buypass eller BankID på mobil. [#footnoteSigneringsmetoderOffentlig]_

..  _autentisert-signatur:

Autentisert signatur
=====================

Ved bruk av autentisert signatur så autentiseres brukeren i ID-porten. IDporten vet ikke noe om at innloggingen er tilknyttet en signeringsseremoni og det er Signicats jobb å  knytte autentiserinen sammen med dokumentet til en signatur.

En autentisert signatur er derfor litt svakere enn en avansert signatur fordi det er flere parter involvert og at det skjer i to steg.

Offentlige virksomheter bruker ofte autentisert signatur, da den er rimeligere enn avansert signatur. 

Avsender kan velge :ref:`sikkerhetsnivå` på signeringen.

Valg av signaturtype
=====================

..  tabs::

    ..  tab:: Bedrift

        Som bedrift har du kun mulighet til å velge avansert signatur og derfor er det ikke nødvendig å sette denne eksplisitt.

    ..  tab:: Offentlig virksomhet

        Som offentlig virksomhet har du mulighet til å velge mellom :ref:`avansert-signatur` eller :ref:`autentisert-signatur`, men Difi anbefaler offentlige virksomheter å bruke autentisert signatur ettersom det er billigere og oppfyller de kravene som blir stilt i offentlig sektor.

        ..  NOTE::
            Standardverdi for signaturtype er autentisert.

.. rubric:: Fotnoter

.. [#footnoteSigneringsmetoderOffentlig] BankID på mobil er ikke tilgjengelig for avanserte oppdrag fra offentlig virksomhet.

