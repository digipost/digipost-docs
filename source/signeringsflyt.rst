
.. _signeringsflyt:

Signeringsflyt
*******************

Et signeringsoppdrag inneholder et dokument som skal signeres, og kan adresseres til en eller flere undertegnere som skal signere. Tjenesten tilbyr i hovedsak to ulike typer signeringsflyter.

.. _signering-i-direkteflyt:

Signering i direkteflyt
========================

Signering i direkteflyt skjer når undertegner allerede er pålogget i avsenders system. En slik flyt er ideell hvis avsender ønsker at sluttbrukerne skal oppleve signeringsprosessen som en integrert del av deres nettsted.

Flyten ser typisk slik ut:

#. Undertegner er innlogget i avsenders tjeneste, og utfører en prosess der, f.eks. utfylling av et skjema er sluttresultatet
#. Avsender oppretter et signeringsoppdrag i signeringstjenesten gjennom API
#. Undertegner blir sendt til signeringstjenesten og gjennomfører signeringen
#. Undertegner blir sendt tilbake til avsenders tjeneste
#. Avsender laster ned signatur og tilbyr en kopi av det signerte dokumentet til undertegner

..  TIP::
    Se gjerne `denne bildeguiden i Google Presentation for signering i direkteflyt <https://docs.google.com/presentation/d/14Q_-YzaxcGsZOgUR6rJl7rWSwLZwujnuqgkKCrxksoA/edit#slide=id.g3922592cb8_0_0>`_.

.. _signering-i-portalflyt:

Signering i portalflyt
========================

.. _signering-i-portalflyt-med-fødselsnummer:

Adressering med fødselsnummer
______________________________

Hvis man adresserer med fødselsnummer så vil undertegner måtte logge inn i signeringsportalen for å kunne se signeringsoppdraget.

Flyten ser typisk slik ut:

#. Avsender oppretter et oppdrag gjennom API eller fra web i avsenderportalen
#. Posten signering varsler undertegner på e-post (og ev. SMS om spesifiert ved opprettelse)
#. Undertegner logger inn på signeringsportalen og gjennomfører signeringssermonien
#. Undertegner laster ned signert dokument
#. Undertegner logger ut av signeringsportalen
#. Avsender laster ned det signerte dokumentet

..  TIP::
    Se gjerne `denne bildeguiden i Google Presentation for signering i portalflyt, adressering med fødselsnummer <https://docs.google.com/presentation/d/14Q_-YzaxcGsZOgUR6rJl7rWSwLZwujnuqgkKCrxksoA/edit#slide=id.g36b93b9965_0_57>`_.

.. _signering-i-portalflyt-uten-fødselsnummer:


Adressering uten fødselsnummer
_______________________________

En signeringsflyt hvor man får tilgang til portalen ved en lenke og et engangspassord.

#. Avsender oppretter et oppdrag gjennom API eller fra web i avsenderportalen
#. Undertegner mottar en unik lenke og engangskode til oppdrag på e-post eller SMS
#. Undertegner trykker på lenken, og fyller inn engangskode til oppdrag
#. Undertegner gjennomfører signeringsseremonien
#. Sluttside som gir mulighet til å laste ned signert dokument

Når man adresserer undertegner uten fødselsnummer, er det avsenders ansvar å sjekke at det er rett eller ønsket person som har signert.

..  TIP::
    Se gjerne `denne bildeguiden i Google Presentation for signering i portalflyt, adressering uten fødselsnummer <https://docs.google.com/presentation/d/14Q_-YzaxcGsZOgUR6rJl7rWSwLZwujnuqgkKCrxksoA/edit#slide=id.g2e3b4edaeb_0_1>`_.

