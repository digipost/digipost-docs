..  _introduction:

Introduction
************

If you wish to send digital mail in smaller volumes, but more often, then our API is a sensible solution. Our API is based on the REST architecture.

Client libraries for the Digipost API are available in both `C# <http://digipost.github.io/digipost-api-client-dotnet/>`_ and `Java <http://digipost.github.io/digipost-api-client-java/>`_. We encourage you to use these libraries to reduce development time, ensure correct security implementation and generate valid XML.

.. tabs::

  .. tab:: C#

    * `Nuget package <https://www.nuget.org/packages?q=digipost-api-client>`_
    * `Source code on GitHub <https://github.com/digipost/digipost-api-client-dotnet>`_
    * `Documentation <http://digipost.github.io/digipost-api-client-dotnet>`_

  .. tab:: Java
    
    * `Maven Central <http://search.maven.org/#search%7Cga%7C1%7Cdigipost-api-client-java>`_
    * `Source code on GitHub <https://github.com/digipost/digipost-api-client-java>`_
    * `Documentation <http://digipost.github.io/digipost-api-client-java>`_

The Digipost API requires the use of :ref:`Digiposts own security implementation <security>` and uploading your organisation's :ref:`enterprise certificate <certificates>` to your Digipost `organisation account <https://www.digipost.no/bedrift>`_.

If you wish to implement sending of digital mail from a web application, we have created `an example .NET web application <https://github.com/digipost/digipost-client-lib-webapp-dotnet>`_ which shows how the .NET client library can be used from both a functional and technical perspective.

This section of the technical documentation also includes a description of all the :ref:`available endpoints <api-reference>` in the Digipost API and information on which HTTP headers should be used.