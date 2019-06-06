Create client configuration
****************************

A client configuration includes all organization specific configuration and all settings needed to connect to the correct environment for Posten signering.

..  tabs::

    ..  group-tab:: Java

        The first step is to load the enterprise certificate (virksomhetssertifikat) through the :code:`KeyStoreConfig`. It can be created from a Java Key Store (JKS) or directly from a PKCS12-container, which is the usual format of an enterprise certificate. The latter is the recommended way of loading it if you have the certificate stored as a simple file:

        ..  code-block:: java

            KeyStoreConfig keyStoreConfig;
            try (InputStream certificateStream = Files.newInputStream(Paths.get("/path/to/certificate.p12"))) {
                keyStoreConfig = KeyStoreConfig.fromOrganizationCertificate(
                    certificateStream, "CertificatePassword"
                );
            }

        If you have a Java Key Store file containing the organization certificate, it can be loaded in the following way:

        ..  code-block:: java

            KeyStoreConfig keyStoreConfig;
            try (InputStream certificateStream = Files.newInputStream(Paths.get("/path/to/javakeystore.jks"))) {
                keyStoreConfig = KeyStoreConfig.fromJavaKeyStore(
                        certificateStream,
                        "OrganizationCertificateAlias",
                        "KeyStorePassword",
                        "CertificatePassword"
                );
            }

        When the certificate has been loaded correctly, a :code:`ClientConfiguration` can be initialized. A trust store and service Uri needs to be set to properly connect. Please change the trust store and service Uri in the following example when connecting to our production environment.

        ..  code-block:: java

            KeyStoreConfig keyStoreConfig = null; //As initialized earlier

            ClientConfiguration clientConfiguration = ClientConfiguration.builder(keyStoreConfig)
                    .trustStore(Certificates.TEST)
                    .serviceUri(ServiceUri.DIFI_TEST)
                    .globalSender(new Sender("123456789"))
                    .build();

    ..  group-tab:: C#

        ..  code-block:: c#

            const string organizationNumber = "123456789";

            var clientConfiguration = new ClientConfiguration(
                Environment.DifiTest,
                CertificateReader.ReadCertificate(),
                new Sender(organizationNumber)
            );

        Where :code:`ReadCertificate` is:

        ..  code-block:: none

            var pathToSecrets = $"{System.Environment.GetEnvironmentVariable("HOME")}/.microsoft/usersecrets/enterprise-certificate/secrets.json";
            _logger.LogDebug($"Reading certificate details from secrets file: {pathToSecrets}");

            var fileExists = File.Exists(pathToSecrets);
            if (!fileExists)
            {
                _logger.LogDebug($"Did not find file at {pathToSecrets}");
            }

            var certificateConfig = File.ReadAllText(pathToSecrets);
            var deserializeObject = JsonConvert.DeserializeObject<Dictionary<string, string>>(certificateConfig);

            deserializeObject.TryGetValue("Certificate:Path:Absolute", out var certificatePath);
            deserializeObject.TryGetValue("Certificate:Password", out var certificatePassword);

            _logger.LogDebug("Reading certificate from path found in secrets file: " + certificatePath);

            return new X509Certificate2(certificatePath, certificatePassword, X509KeyStorageFlags.Exportable);


..  NOTE::
    For organizations acting as brokers on behalf of multiple senders, you may specify the sender’s organization number on each signature job. The sender specified for a job will always take precedence over the :code:`globalSender` in :code:`ClientConfiguration`.
