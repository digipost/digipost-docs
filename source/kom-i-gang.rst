..  _kom-i-gang:

..  DANGER::
    Dette nettstedet er under arbeid. Vennligst se https://www.digipost.no/plattform for oppdatert Digipost teknisk dokumentasjon.

Kom i gang med API-et til Digipost på 1-2-3
********************************************

Det er veldig enkelt å integrere med Digipost og sende ditt første brev. Vi foreslår at du benytter et av våre klientbiblioteker for å komme raskt igang. Følgende tre steg er alt som må til:

1. Registrer deg for sending i Digipost
________________________________________

Alle virksomheter som ønsker å sende brev i Digipost må ha en virksomhetskonto. Du kan få tilgang til en test-konto ved å sende en e-post til support@digipost.no med følgende informasjon:

- Navn på virksomheten
- Organisasjonsnummer
- Telefonnummer
- E-postadresse
- Digipostadressene til de som skal ha tilgang

2. Ta i bruk Digipost klientbibliotek for ditt språk
_____________________________________________________

Ta i bruk klientbiblioteket for enten Java eller .NET slik at du kan enkelt integrere med Digipost som avsender. Du kan selvsagt også gjøre en manuell integrasjon mot selve APIet, men vi anbefaler at du benytter våre klientbibliotek.

.. tabs::

   .. tab:: C#

      https://www.nuget.org/packages?q=digipost-api-client

   .. tab:: Java

      http://search.maven.org/#search%7Cga%7C1%7Cdigipost-api-client-java

3. Bruk Digipost klientbiblioteket fra din kode
________________________________________________

Det er ikke mange kodelinjer som skal til for å sende brev. Koden nedenfor viser hva som skal til for å sende et brev. Det er naturlig nok mange flere muligheter i klientbibliotekene og APIene til Digipost, men dette er en enkel introduksjon som lar deg sende et standard brev til én mottaker i Digipost.

..  tabs::

    ..  group-tab:: C#

        ..  code-block:: c#

			// 1. Initialiser konfig med din Digipost avsender id
			// Avsender id finner du på digipost.no/bedrift
			var config = new ClientConfig(senderId: "xxxxx", environment: Environment.Production);

			// 2. Initialiser klient med thumbprint til sertifikat som er knyttet til din Digipost virksomhetskonto
			// Virksomhetssertifikat knyttes til din Digipost konto på digipost.no/bedrift.
			// Informasjon om bruk av thumbprints finnes på http://digipost.github.io/digipost-api-client-dotnet/#installcert.
			var client = new DigipostClient(config, thumbprint: "84e492a972b7e...");

			// 3. Lag melding med bruk av fødselsnummer som personidentifikator
			var message = new Message(
				new RecipientById(IdentificationType.PersonalIdentificationNumber, "00000000000"),
				new Document(subject: "Dokumentets emne", fileType: "pdf", path: @"c:\...\dokument.pdf")
			);

			// 4. Send melding
			var result = client.SendMessage(message);

    ..  group-tab:: Java

        ..  code-block:: java

			// 1. Les inn sertifikatet du har knyttet til din Digipost-konto (i .p12-formatet)
			InputStream sertifikatInputStream = new FileInputStream("certificate.p12");

			// 2. Opprett en DigipostClient
			DigipostClient client = new DigipostClient(DigipostClientConfigBuilder.newBuilder().build(), ATOMIC_REST, "https://api.digipost.no", AVSENDERS_KONTOID, sertifikatInputStream, SERTIFIKAT_PASSORD);

			// 3. Opprett et fødselsnummerobjekt
			PersonalIdentificationNumber pin = new PersonalIdentificationNumber("26079833787");

			// 4. Opprett hoveddokumentet
			Document primaryDocument = new Document(UUID1, "Dokumentets emne", PDF);

			// 5. Opprett en forsendelse
			Message message = newMessage(UUID2, primaryDocument)
				.personalIdentificationNumber(pin)
				.build();

			// 6. Legg til innhold og send
			client.createMessage(message)
				.addContent(primaryDocument, new FileInputStream("content.pdf"))
				.send();