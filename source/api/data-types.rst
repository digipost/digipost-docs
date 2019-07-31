..  _data-types:

Data types
***********

Introduction
______________

From version 7 of the Digipost XSD the concept of data types has been introduced. The purpose of data types is to make it possible to send data alongside the document which will result in extra functionality in Digipost.

Appointment
_____________

Enriching a digital mail message with the appointment data type will result in a special appointment presentation in the recipients Digipost mailbox. For example, the recipient will be able to add the appointment to her calendar and/or see where the appointment will take place in a map. The message still requires the actual file and the actual file will still be available for the recipient.

Endpoint
^^^^^^^^

https://api.digipost.no/messages

Verb
^^^^

HTTP POST

Request
^^^^^^^

Headers and actual document content are not included below. Only XML metadata is included. See the send endpoint for examples of headers and multi-part requests.

Response
^^^^^^^^

xyz