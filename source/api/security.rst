..  _security:

Security in the Digipost API
*****************************

Security is critical for Digipost. Therefore we have designed a custom security mechanism which guarantees the message's confidentiality, autenticity, integrity and non-repudiation.

The security mechanism is based on a combination of well-established standards. Since our approach is a custom combination, we recommend that you read the following page thoroughly so that you understand security in the Digipost API. If you are planning to integrate directly with the Digipost API, you must understand the following content.

1. Introduction
_______________

The security in the Digipost API is based on all messages between the (API) client and Digipost being signed with SHA-256 with RSA encryption (SHA256WithRSAEncryption algorithm).

SHA-256 is a simple hashing algorithm that will make the message to be signed smaller. RSA is a standard for encrypting with asymmetrical keys which is based on you using your private key (which is included in your certificate) to sign the message, while the Digipost solution uses your public key to verify the signature.

This mechanism plus use of SSL at the transport layer ensures that the Digipost api is secure:

1. **Confidentiality**: No one else can read the content of the message.
2. **Autenticity**: We know that you are the originator of the message and no one else.
3. **Integrity**: We know that the message has not been changed in transport between you and us/Digipost.
4. **Non-repudiation**: We can document after the fact that you were the originator of the message.

2. Headers
__________

Security in the Digipost API requires that you add a few custom headers in all the requests you make. The following headers are used.

- **X-Digipost-UserId** is used to identify your sender id in Digipost. You can find your sender id by logging in to digipost.no/bedrift. We can then use the correct certificate on our end.

- **Date** states when the request was sent. The date must be in RFC 1123 format. If the date/time you specify differs by more than five minutes from the Digipost server time, the request will be rejected.

- **X-Content-SHA256** (prev. Content-MD5) includes a Base64 encoded hash of the request's body. The actual request can contain different content, e.g. XML or PDF, but the X-Content-256 header value will always contain a string of the same length. This makes it much easier to use the same signing mechanism for different kinds of content. For messages without content (typically GET requests), X-Content-256 is not used.

When those three headers are added and the request is ready on your end, you can create the last header. This header is based on those three other headers and other parts of your request (HTTP method, path, parameters).

- **X-Digipost-Signature** contains the signature that verifies that you are the originator of the request.

The following information is related to this header and how to generate it.

2.1 Example of all headers
^^^^^^^^^^^^^^^^^^^^^^^^^

The following is an example with a complete set of the required headers in a POST request to the Digipost API. The HTTP request will typically also include other headers that are not relevant in this context. A GET request will include the same headers with the exception of X-Content-SHA256 which should then not be included.

..  code-block::

  Date: Wed, 29 Jun 2011 14:58:11 GMT
  X-Digipost-UserId: 9999
  X-Content-SHA256: q1MKE+RZFJgrefm34/uplM/R8/si9xzqGvvwK0YMbR0=
  X-Digipost-Signature: BHvtgDTKz490iMbYZsOf5+FvWCsWDt5oJgyTvXlLiNrWgUu/fhuY8AJYBoH8g+0t46slsmJqQxNlsa6u+cF1aE921cZy7ISSeRLl/z6WlwCtTGu9fFH9X4Kr+2ffwPqzCTRPD4D5jHrbudmSGZJIq3ImAKU250t6SCJ//aiAKMg=

3. Signing the request
______________________

You sign the request by using the SHA256WithRSAEncryption algorithm on a strongly defined string. This will result in a long string that we can verify server-side using your public key and a similar string we generate on our end.

3.1 Buypass certificate vs. Digipost certificate
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To sign your request you need a certificate. This can either be a Buypass certificate that you have purchased or a Digipost certificate that we generate for you. When you use your own Buypass certificate, Digipost will never be in posession of the private key part of your certificate, which is required for full non-repudiation. If we generate the certificate for you, we will for a very short period be in posession of the private key part of the generated certificate. The private key is never stored on disc, it is only stored in memory until it is made available for your download. It is your responsibility to decide which certificate type best meets your security requirements. After you have decided what type of certificate you wish to use, you can easily upload or download (depending on the type you use) the certificate at digipost.no/bedrift.

3.2 Generating the string to be signed
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When you have stored your certificate with Digipost and you can access the private key of your certficate from your code base, you are ready to generate the string that needs to be signed. This is a string that follows a defined format based on the content of your message.

It would be both unnecessary, timeconsuming and likely not possible to sign the entire message. Instead you will generate a shorter string which includes relevant and unchangeable parts of your request, that is then signed. It is important that the string is generated exactly as described below. If not, your request will be rejected.

The string is comprised of six lines, each line finishes with a newline (also the last line). Note that the newline consists of one byte (\n) and is not a Windows newline (it is a Unix newline).

3.2.1 Example of signed string
++++++++++++++++++++++++++++++

The following is an example of how such a generated string may look in a POST request with content:

..  code-block::

  POST
  /messages
  date: Wed, 29 Jun 2011 14:58:11 GMT
  x-content-sha256: q1MKE+RZFJgrefm34/uplM/R8/si9xzqGvvwK0YMbR0=
  x-digipost-userid: 9999
  parameter1=58&parameter2=test

Example of a GET request without content:

..  code-block::

  GET
  /
  date: Wed, 29 Jun 2011 14:58:11 GMT
  x-digipost-userid: 9999
  parameter1=58&parameter2=test

- The first line is the HTTP verb of the request. Allowed values are GET, POST, DELETE or PUT (always capitalised).

- The second line is the part of the URL that identifies the resource being requested. This means only the path should be used.

- The next three lines are the three HTTP headers Date, X-Content-SHA256 and X-Digipost-UserId, in that order (alphabetically). Every header should be written in the format «key: value», where the key must be in small-caps (value should remain as intended). There should be a space after the colon. The header X-Content-SHA256 should not be used in requests without content (typically get requests).

- The last line contains the request's parameters. The parameters are the strings that are specified after the question mark in the URL. These must be URL encoded. If there are no URL parameters in the request, the line must still be included as empty with an ending newline.

3.2.2 Pseudocode for generating the string
++++++++++++++++++++++++++++++++++++++++++

The following is pseudocode for how to generate the string to be signed:

..  code-block:: java

  String stringToSign = uppercase(verb) + "\n" +
                      lowercase(path) + "\n" +
                      "date: " + datoHeader + "\n" +
                      "x-content-sha256: " + sha256Header + "\n" +
                      "x-digipost-userid: " + virksomhetsId + "\n" +
                      lowercase(urlencode(requestparametre)) + "\n";

  String signature =    base64(sign(stringToSign));

3.2.3 Java code for generating string
+++++++++++++++++++++++++++++++++++++

The following Java code is included in the Java client library to generate the string to be signed.

If you use one of our client libraries, you will not need to implement this. If you are implementing an integration directly with the Digipost API, the following can be used as inspiration:

.. code-block:: java

  public class MessageSignatureUtil {

    private static final List<String> HEADERS_FOR_SIGNATURE = Arrays.asList(X_Content_SHA256.toLowerCase(),
            Content_MD5.toLowerCase(), Date.toLowerCase(), X_Digipost_UserId.toLowerCase());

    public static String getCanonicalRequestRepresentation(final RequestToSign request) {
        StringBuilder s = new StringBuilder();
        s.append(getCanonicalMethodRepresentation(request));
        s.append(getCanonicalUrlRepresentation(request));
        s.append(getCanonicalHeaderRepresentation(request));
        s.append(getCanonicalParameterRepresentation(request));
        return s.toString();
    }

    private static String getCanonicalMethodRepresentation(final RequestToSign request) {
        return request.getMethod().toUpperCase() + "\n";
    }

    private static String getCanonicalUrlRepresentation(final RequestToSign request) {
        return request.getPath().toLowerCase() + "\n";
    }

    private static String getCanonicalHeaderRepresentation(final RequestToSign request) {
        SortedMap<String, String> headers = request.getHeaders();
        StringBuilder headersString = new StringBuilder();
        for (Entry<String, String> entry : headers.entrySet()) {
            String key = entry.getKey();
            if (isHeaderForSignature(key)) {
                headersString.append(key.toLowerCase() + ": " + entry.getValue() + "\n");
            }
        }
        return headersString.toString();
    }

    private static String getCanonicalParameterRepresentation(final RequestToSign request) {
        return request.getParameters().toLowerCase() + "\n";
    }

    private static boolean isHeaderForSignature(final String key) {
        return HEADERS_FOR_SIGNATURE.contains(key.toLowerCase());
    }

  }

3.3 Signature
^^^^^^^^^^^^^

When you have generated the string as described, it can be signed with your private key. SHA-256 with rsa encryption is the algorithm that should be used.

Since the signed string includes a SHA256 hash of the message, it is not possible to change the message after signing with invalidating the signature. The signature also includes your sender id and the date/time the request was sent. The ensures correct authentication and prevents replay attacks. The signature can only be generated by someone in posession of your private key which ensures non-repudiation.

4. Verifying the response signature
___________________________________

The same way that you sign all requests that you send to Digipost, Digipost signs all responses that are sent to you. You can verify this signature the same way that Digipost verifies you signature.

4.1 How Digipost gererates the signature
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The signature mechanism works exactly as described in the section on signing the request. The following steps occur server-side to generate the signature.

- First we add the Date header.

- Then we add a hash of the content. This is done with the SHA256 algorithm. Then we add this as the value for the X-Content-SHA256 header. If there is no response body, this is not included.

- Finally, we sign a canonical string that represents the entire response. We do this by generating a string which consistes of the following elements:

  - Response code (200, 400, 500, etc.)

  - Path that was requested (e.g. "/messages")

  - Additional headers (Date, X-Content-SHA256) with key in small caps and value as intended (separated with colon) and sorted alphabetically.

  - Finish with newline.

The following is an example og a canonical string that represents the response:

.. code-block::

  200
  /messages
  date: Mon, 18 Nov 2013 09:06:42 GMT
  x-content-sha256: lTapuncEksiIcxVAw0ibcWzex3zoeMWmACvtov4IZJY=

The final step is to sign this string with Digiposts private key (we use the SHA256WithRSAEncryption algorithm) and populate the value for the X-Digipost-Signature header.

We end up with a response with the following headers:

.. code-block::

  Date: Mon, 18 Nov 2013 09:06:42 GMT
  X-Content-SHA256: lTapuncEksiIcxVAw0ibcWzex3zoeMWmACvtov4IZJY=
  X-Digipost-Signature: BHvtgDTKz490iMbYZsOf5+FvWCsWDt5oJgyTvDUMMYrWgUu/fhuY8AJYBoH8g+0t46slsmJqQxNlsa6u+cF1aE921cZy7ISSeRLl/z6WlwCtTGu9fFH9X4Kr+2ffwPqzCTRPD4D5jHrbudmSGZJIq3ImAKU250t6SCJ//aiAKMg=

4.2 How do you verify the signature
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To verify the signature you do the following:

1. Verify that the date/time is within the accepted time interval (you may wish to accept a few minutes of difference between your side and Digipost)

2. Verify that the actual response body matches the value of the X-Content-SHA256 header by SHA256 hashing the body content.

3. Generate a canonical string as described above and verify that the received signature matches the generated string.

To verify the signature in the response you will need Digipost's public key. You can access this key at the root resource (i.e. in the response when you send a request to https://api.digipost.no/):

.. code-block::

  GET / HTTP/1.1
  Accept: application/vnd.digipost-v7+xml

The Digipost public key is in the response:

.. code-block::

  200
  Content-Type: application/vnd.digipost-v7+xml

  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
  <entrypoint xmlns="http://api.digipost.no/schema/v7">
    <certificate>
  -----BEGIN CERTIFICATE-----
  MIIBtTCCAWOgAwIBAgIQQLU1zzCvy4ZPp4vRzp4yzjAJBgUrDgMCHQUAMBYxFDASBgNVBAMTC1Jv
  b3QgQWdlbmN5MB4XDTExMDUwMzEwMTgyOFoXDTM5MTIzMTIzNTk1OVowGDEWMBQGA1UEAxMNRGln
  aXBvc3QgVGVzdDCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAx6io8f76X+1PsL5i9kTjzSIy
  ...
  -----END CERTIFICATE-----
    </certificate>
      ...
  </entrypoint>

**Note:** The certificate above is only an example. When verifying responses from Digipost, it is important that you use the public key that is actually returned in the response when you make a request to the root resource. That way Digipost can change certificate without impact.

5. Troubleshooting
__________________

In order to make troubleshooting the Digipost api security implementation as painless as possible, we have made error handling in the api as robust as possible, and included informative error messsages.

5.1 A possible troubleshooting scenario
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following is an example that shows how a developer may interact with the Digipost api when troubleshooting.

5.1.1 First try
+++++++++++++++

A developer makes a first attempt at sending a letter and sends the following request:

.. code-block::

  POST https://api.digipost.no/messages
  Accept: application/vnd.digipost-v7+xml
  X-Digipost-UserId: 5
  Content-Type: application/vnd.digipost-v7+xml
  Date: Fri, 01 Jul 2011 09:19:20 GMT
  X-Content-SHA256: q1MKE+RZFJgrefm34/uplM/R8/si9xzqGvvwK0YMbR0=
  X-Digipost-Signature: BHvtgDTKz490iMbYZsOf5+FvWCsWDt5oJgyTvXlLiNrWgUu/fhuY8AJYBoH8g+0t46slsmJqQxNlsa6u+cF1aE921cZy7ISSeRLl/z6WlwCtTGu9fFH9X4Kr+2ffwPqzCTRPD4D5jHrbudmSGZJIq3ImAKU250t6SCJ//aiAKMg=
  User-Agent: Java/1.6.0_24
  Host: api.digipost.no
  Connection: keep-alive
  Content-Length: 389

  <?xml version="1.0" encoding="utf-8">
  <message xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://api.digipost.no/schema/v7">
    <recipient>
      <digipost-address>ola.nordmann#1234</digipost-address>
    </recipient>
    <primary-document>
      <uuid>6d99008e-2672-4b55-9b09-996b09a06e47</uuid>
      <subject>simple-document</subject>
      <file-type>txt</file-type>
      <authentication-level>PASSWORD</authentication-level>
      <sensitivity-level>NORMAL</sensitivity-level>
    </primary-document>
  </message>

The API answers with the following response:

.. code-block::

  403
  Content-Type: application/vnd.digipost-v7+xml

  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
  <error xmlns="http://api.digipost.no/schema/v7">
    <error-code>GENERAL_ERROR</error-code>
    <error-message>No certificate found. Please upload a certificate in virksomhetsadmin at digipost.no.</error-message>
  </error>

The developer then uploads the organisation's certificate to Digipost and tries again.

5.1.2 Second try
++++++++++++++++

The developer sends the exact same request as in the first try. Since she has uploaded the correct certificate, the error message has changed:

.. code-block::

  403
  Content-Type: application/vnd.digipost-v7+xml

  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
  <error xmlns="http://api.digipost.no/schema/v7">
    <error-code>GENERAL_ERROR</error-code>
    <error-message>
  The signature found in the " + X_Digipost_Signature + " header could not be verified.
  Possible reasons for this may be that the certificate used for signing is not the same that is uploaded to Digipost, or the format of the signature is invalid.

  Canonical string to sign generated by Digipost, with each line terminated with a newline character (\\\\n), was:
  ===START===
  POST
  /messages
  date: Wed, 05 Dec 2012 12:48:10 GMT
  x-content-sha256: q1MKE+RZFJgrefm34/uplM/R8/si9xzqGvvwK0YMbR0=
  x-digipost-userid: 5

  ===SLUTT===
    </error-message>
  </error>

The developer checks on her side and sees that the string she has generated is the following:

.. code-block::

  POST
  /messages
  Date: Wed, 29 Jun 2011 14:58:11 GMT
  X-Content-SHA256: q1MKE+RZFJgrefm34/uplM/R8/si9xzqGvvwK0YMbR0=
  X-Digipost-UserId: 5

After comparing the two strings, she notices two problems:

1. She has not generated the header names in small caps. This results in the signature being different.

2. There is also another small detail that is incorrect. Note that this request does not have any request parameters. The developer has not included an empty line where the request parameters would typically be included (and that the specification requires). Even a small detail like this will cause the signature to be different. The developer adds this empty line. The result is the following string:

.. code-block::

  POST
  /messages
  date: Wed, 29 Jun 2011 14:58:11 GMT
  x-content-sha256: q1MKE+RZFJgrefm34/uplM/R8/si9xzqGvvwK0YMbR0=
  x-digipost-userid: 5

5.1.3 Third try
^^^^^^^^^^^^^^^

After making the necessary changes to the string to be signed, the developer sends a new request to the Digipost API.

The Digipost API now responds with the following:

.. code-block::

  201

  Location: https://api.digipost.no/messages/1001
  Content-Type: application/vnd.digipost-v7+xml

  <!--?xml version="1.0" encoding="UTF-8" standalone="yes"?-->
  <message-delivery xmlns="http://api.digipost.no/schema/v7">
    <delivery-method>DIGIPOST</delivery-method>
    <sender-id>497013</sender-id>
    <status>DELIVERED</status>
    <delivery-time>2016-12-07T08:45:32.675+01:00</delivery-time>
    <primary-document>
      <uuid>6d99008e-2672-4b55-9b09-996b09a06e47</uuid>
      <subject>simple-document</subject>
      <file-type>txt</file-type>
      <authentication-level>PASSWORD</authentication-level>
      <sensitivity-level>NORMAL</sensitivity-level>
      <pre-encrypt>false</pre-encrypt>
      <content-hash hash-algorithm="SHA256">5o0RMsXcgSZpGsL7FAmhSQnvGkqgOcvl5JDtMhXBSlc=</content-hash>
    </primary-document>
  </message-delivery>

This time Digipost accepts the request and the developer can send the actual document content (e.g. PDF). When the developer is comfortable with the completed testing against the organisation's test account, the Digipost production account can be made accessible by sending av email to Digipost support.
