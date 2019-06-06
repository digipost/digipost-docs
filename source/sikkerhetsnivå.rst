.. _sikkerhetsnivå:

Sikkerhetsnivå
***************

Som avsender har man mulighet til å angi hvilket sikkerhetsnivå signeringsoppdraget skal ha. Dette kan være 3 eller 4, og begrenser hvilke elektroniske ID-er undertegner kan bruke for å signere dokumentet.

Sikkerhetsnivået begrenser også hvilke innloggingsmetoder undertegner kan bruke for å se signeringsoppdraget og dets detaljer, samt begynne selve signeringen.

..  TIP::
    Om ingen signaturtype angis ved opprettelse av oppdraget, vil nivå 4 settes som standard.

Bedrift
========

Som en bedrift har du kun mulighet til å bruke det høyeste sikkerhetsnivået, nivå 4, og du trenger derfor ikke ta stilling til dette.

..  TIP::
    Tilgjengelige metoder for innlogging og signering er *BankID*, *BankID på mobil* og *Buypass*.

Offentlig virksomhet
=====================
Som offentlig virksomhet kan du velge sikkerhetsnivå 3 eller 4. Et signeringsoppdrag som er på nivå 4 vil kun kunne vises og signeres i sin helhet med alle e-IDer unntatt MinID, som er sikkerhetsnivå 3.

Er bruker innlogget på nivå 3 vil få en begrenset visning av signeringsoppdraget, der kun *ikke-sensitiv* tittel er synlig. For å se alle detaljer om oppdraget vil brukeren bli bedt om å logge inn på nytt på til sikkerhetsnivå 4. Brukeren vil alltid bli veiledet til den innloggingsmetoden som kreves for oppdraget som skal signeres, slik at brukeropplevelsen blir så god som mulig.

..  TIP::
    Tilgjenglige metoder for innlogging og signering er *BankID*, *BankID på mobil*, *Buypass id på smartkort*, *Buypass id i mobil* og *Commfides*.
