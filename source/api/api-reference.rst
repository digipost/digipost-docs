..  _api-reference:

API Reference
**************

HTTP headers
______________

The following http headers are used in the Digipost API.

Search
________

Search should be used when you wish to find a Digipost user based on a query string. The search endpoint will return up to 10 search results.

Identify recipient
____________________

Identify recipient should be used if you need to just check if one specific person is a Digipost user. This can be useful both prior to sending digital mail and as part of a process where (for example) a customer database is being updated with Digipost-addresses. You can identify a person by personal identification number (fødselsnummer), Digipost-address or name and street address.

Send
______

This endpoint is used to actually send digital mail to Digipost. From v6 of Digipost's REST API it became possible to send digital mail with one single request. Previously a messages needed to be created (request #1), documents added to the message (request #2) and finally a request is sent to send the message.

Attachments
_____________

To send digital mail with attachments you send as you normally would, but in addition to specifying a primary-document you must also list the attachments you wish to send.

Invoice
_________

..  DANGER::
    Invoice payment functionality should only be used when the invoice sent is to be actually paid by the recipient. Other documents similar to an invoice such as an AvtaleGiro copy must be sent as a regular document.

To send an invoice for payment you send as you normally would, but in addition you define a document as an invoice and include four extra invoice elements (kid, amount, account, due-date).

Debt collection
_________________

A debt collection document is defined by including the string "[INKASSO] " at the start of the defined subject. The dokument must also be defined as an invoice.

Physical mail
_______________

If the recipient is not a Digipost user yet, you can choose to send your letter as physical mail. The following shows how this is done.

Get inbox
___________

Get inbox provides a list of all documents that are presently in the organisation's inbox. If you would would like to interact with a person's (not an organisation's) inbox, Digipost provides another service for that. Interacting with an organisation's inbox normally also includes downloading the actual content of the documents and/or deleting the documents after they have been downloaded (not mandatory).

Get document
______________

The get document endpoint will return the actual content of the document. The URI that shoud be used to retrieve the document is included in the response when the get inbox endpoint is called.

Delete document
_________________

The delete document endpoint will delete the document from the organisation's inbox. The URI that should be used to delete the document is included in the response when the get inbox endpoint is called.

Response codes
________________

The Digipost api can return the following http response codes: