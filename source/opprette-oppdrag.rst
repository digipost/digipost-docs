Opprette signeringsoppdrag
===========================

Ved opprettelse av signeringsoppdrag kan følgende felter angis:

+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Felt                      | Direkteflyt             | Portalflyt        | Ekstra informasjon                                            |
+===========================+=========================+===================+===============================================================+
| Dokument                  | **Obligatorisk**        | **Obligatorisk**  |                                                               |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Undertegner(e)            | **Obligatorisk**        | **Obligatorisk**  | se :ref:`adressering-av-undertegner`                          |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Tittel                    | **Obligatorisk**        | **Obligatorisk**  |                                                               |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Signaturtype              | Valgfritt               | Valgfritt         | se :ref:`signaturtype`                                        |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Sikkerhetsnivå            | Valgfritt               | Valgfritt         | se :ref:`sikkerhetsnivå`                                      |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Melding til mottaker(e)   | Valgfritt               | Valgfritt         |                                                               |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Undertegners identifikator| Valgfritt               | Valgfritt         | se :ref:`adressering-av-undertegner`                          |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Aktiveringstidspunkt      | Ikke overstyrbar [#f1]_ | Valgfritt         |                                                               |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Levetid                   | Ikke overstyrbar [#f2]_ | Valgfritt         |                                                               |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| E-postadresse             | Ikke relevant           | **Obligatorisk**  | se :ref:`varsler`                                             |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Mobilnummer               | Ikke relevant           | Valgfritt         | se :ref:`varsler`                                             |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+
| Rekkefølge                | Ikke relevant           | Valgfritt         | se :ref:`kjedet-signering`                                    |
+---------------------------+-------------------------+-------------------+---------------------------------------------------------------+

.. rubric:: Footnotes

.. [#f1] Signeringsoppdrag i direkteflyt blir alltid aktivert øyeblikkelig etter opprettelse. *Standardverdi* er *øyeblikkelig etter opprettelse*.
.. [#f2] Signeringsoppdrag i direkteflyt har alltid 30 dagers levetid for å unngå at et dokument blir signert uhensiktsmessig lenge etter opprettelsen av oppdraget. Eventuell frist fra avsenders perspektiv må kommuniseres og håndteres i avsenders tjenester.

For implementasjon for signeringsoppdrag i portalflyt, se  :ref:`portal-flow`, og for signeringsoppdrag i direkteflyt, se :ref:`direct-flow`.

Begrensninger
______________

Antall undertegnere
^^^^^^^^^^^^^^^^^^^^^

Et signeringsoppdrag kan ha flere undertegnere. Tjenesten tillater maksimalt 10 undertegnere pr. oppdrag.

Hastighet
^^^^^^^^^^^

Tjenesten tillater maksimalt 10 API-kall i sekundet per organisasjonsnummer. Hvis en avsender overskrider denne grensen vil API-et returnere :code:`HTTP 429 Too Many Requests`, og avsenderen vil bli blokkert i 30 sekunder.


..  _dokumentformat:

Dokumentformat
^^^^^^^^^^^^^^^^^

Tjenesten støtter dokumenter av typen ren tekst (:code:`.txt`) og PDF (:code:`.pdf`). Både PDF og PDF/A aksepteres av tjenesten. Det signerte dokumentet vil være av samme type som originaldokumentet.
Et originaldokument som er PDF/A gir et signert PAdES-dokument som er PDF/A, og et originaldokument som er PDF versjon 1.1 – 1.7 gir et signert PAdES-dokument som er PDF versjon 1.7.
For PDF/A vil tjenesten alltid produsere signerte PAdES-dokumenter av typen PDF/A-3b, uavhengig av PDF/A-versjon og -konformitetsnivå på originaldokumentet.

For arkivering av signerte dokumenter anbefaler vi å bruke originaldokumenter av typen PDF/A. Dette er et krav hvis det signerte dokumentet skal avleveres til Riksarkivet.

..  NOTE::
    Filen kan maksimalt være 3 MB (:code:`3 145 728 bytes`) stor. PDF-versjoner som støttes er PDF 1.1-1.7.

I PAdES vil dokumentet alltid presenteres i A4- og portrett-format. For best resultat anbefales det at det innsendte dokumentet også har dette formatet.

..  DANGER::
    Passordbeskyttede dokumenter (begrenset lese- og/eller skrive-tilgang) er ikke støttet av tjenesten og vil gi feilmelding først ved nedlasting av dokumentet.

Aktiveringstidspunkt
^^^^^^^^^^^^^^^^^^^^^^

Angir tidspunkt for når signeringsoppdraget skal tilgjengeliggjøres for undertegner(e). Dersom aktiveringstidspunktet er i fortiden, blir oppdraget tilgjengelig øyeblikkelig etter opprettelse.

Signeringsoppdrag i direkteflyt blir alltid aktivert øyeblikkelig etter opprettelse.

Oppdragets levetid
^^^^^^^^^^^^^^^^^^^^

Angir hvor lenge *etter aktivering* et signeringsoppdrag er tilgjengelig for undertegner før det utløper. Kan maksimalt være 90 dager etter aktivering.

Signeringsoppdrag i direkteflyt har alltid 30 dagers levetid for å unngå at et dokument blir signert uhensiktsmessig lenge etter opprettelsen av oppdraget. Eventuell frist fra avsenders perspektiv må kommuniseres og håndteres i avsenders tjenester.

