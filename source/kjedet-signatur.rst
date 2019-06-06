..  _kjedet-signering:

Kjedet signering
*****************

I tillegg til å kunne tilby :ref:`signering-i-direkteflyt` og :ref:`signering-i-portalflyt`, så har Posten signering lagt opp til å støtte mer avanserte flyter.

Avsendere som integrerer gjennom API og :ref:`bruker portalflyt <signering-i-portalflyt>` kan spesifisere en signeringsrekkefølge for undertegnerne. Når alle undertegnerne i en gruppe har signert, vil oppdraget bli tilgjengelig for neste gruppe. En gruppe undertegnere er alle som har samme rekkefølge (:code:`order`) i APIet og kan bestå av én eller flere undertegnere som skal signere i parallell.

..  TIP::
    For kjedete signeringsoppdrag gjelder aktiveringstidspunktet for *første gruppe*.

Eksempel
_________

Kjedet signering kan være ønskelig i flere tilfeller. La oss ta en eiendomsmegler som eksempel. Man kan se for seg at det er ønskelig at en megler bare skal bruke tid på å signere et dokument først etter at kjøper av boligen har signert. Dette vil være tidsbesparende og fornuftig, ettersom forutsetningen for at megler skal signere er at kjøper har signert. Ved å sette kjøper til å ha :code:`order=0` og selger ha :code:`order=1` så vil man få denne oppførselen.

..  TIP::
    Oppdraget vil være like lenge tilgjengelig for alle parter ved kjedet signering. I eksemplet over betyr det at hvis levetiden på signeringsoppdraget settes til 1 uke, og kjøper signerer etter 3 dager, så vil megler fra dette tidspunktet få 1 uke på å signere. Siste dag megler kan signere er derfor 10 dager etter opprettelsestidspunkt (3+7 dager).

Terminerende handlinger for kjedete signeringsoppdrag
_______________________________________________________

En terminerende handling på et kjedet signeringsoppdrag vil føre til at oppdraget avsluttes for alle undertegnere som enda ikke har signert, inkludert de undertegnere som ikke har fått oppdraget tilgjengeliggjort enda.

Hvis en undertegner i første gruppe avviser oppdraget eller signeringsfristen går ut, vil oppdraget aldri tilgjengeliggjøres for undertegnerne i de senere gruppene og avsender blir varslet om at oppdraget er fullført med en feilende status.
