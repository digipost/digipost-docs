..  _get-started:

..  DANGER::
    Dette nettstedet er under arbeid. Vennligst se https://www.digipost.no/plattform for oppdatert Digipost teknisk dokumentasjon.

Get started with Digipost in 1-2-3
**********************************

It is really easy to integrate with Digipost and send your first digital mail. Vi suggest you use one of our client libraries to get started. Follow these three simple steps:

1. Get registered as a Digipost sender
______________________________________

All organisations that wish to send digital mail must have a Digipost organisation (sender) account. To get a test account, send an email to support@digipost.no with the following information:

- Organisastion name
- Organisation number
- Telephone number
- Email address
- Email address

2. Choose between the Java or .NET Digipost client libraries
____________________________________________________________

Use the Digipost client library in either Java or .NET to send requests to the Digipost API. You can also integrate directly with the API, but we recommend using a client library.

.. tabs::

   .. tab:: C#

      https://www.nuget.org/packages?q=digipost-api-client

   .. tab:: Java

      http://search.maven.org/#search%7Cga%7C1%7Cdigipost-api-client-java

3. Call Digipost client library code from your code
___________________________________________________

All it takes is a few lines of code to send digital mail. The following code shows a bare bones example of how to do this. There are many other options and possibilities in the client libraries and Digipost API, but the below is quick introduction to how sending a letter to a Digipost recipient works.

..  tabs::

    ..  group-tab:: C#

        ..  code-block:: c#

			// 1. Initialise config with your Digipost sender id
			// You can find your sender id at digipost.no/bedrift
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