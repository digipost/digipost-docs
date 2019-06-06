Error handling
****************

There are differet forms of exceptions that can occur. Some are more specific than others. All exceptions related to client behavior inherits from :code:`SignatureException`.

..  tabs::

    ..  code-tab:: c#

        try
        {
            //Some signature action
        }
        catch (BrokerNotAuthorizedException notAuthorizedException)
        {
            //Not authorized to perform action. The correct access rights for organization are not set.
        }
        catch (UnexpectedResponseException unexpectedResponseException)
        {
            //UnexpectedResponseException will normally contain an `Error` object giving a more detailed error description. If this error does not exist,
            // you can still get the status code and message.
            var statusCode = unexpectedResponseException.StatusCode;
            var responseMessage = unexpectedResponseException.Message;

            if (unexpectedResponseException.Error != null)
            {
                var errorMessage = unexpectedResponseException.Error.Message;
                var errorType = unexpectedResponseException.Error.Type;
            }
        }
        catch (SignatureException exception)
        {

        }

    ..  code-tab:: java

        try {
            client.confirm(statusChange);
        } catch (BrokerNotAuthorizedException brokerNotAuthorized) {
            // Broker is not authorized to perform action. Contact Difi in order to set up access rights.
        } catch (UnexpectedResponseException unexpectedResponse) {
            // The server returned an unexpected response.
            Response.StatusType httpStatusCode = unexpectedResponse.getActualStatus();

            // errorCode and errorMesage will normally contain information returned by the server. May be null.
            String errorCode = unexpectedResponse.getErrorCode();
            String errorMessage = unexpectedResponse.getErrorMessage();
        } catch (SignatureException e) {
            // An unexpected exception was thrown, inspect e.getMessage().
        }


