# Callback API

We provide an option to forward the transaction statuses for each recipient in a batch to your system. The URL is configured pr service in our backend, so you need to provide the endpoint URI to our support if you wish to utilize this functionality.

Your server must be configured to accept a HTTP POST of type application/json with the following parameters:

| Property  		| Type      | Description                                     	| 
|-------------------|-----------|---------------------------------------------------|
| batchReference 	| string	| Your input from the batch request             	|
| clientReference	| string    | Your input from the batch request					|
| success      		| bool 		| If the transaction was successful or not          |
| externalReference | string    | Reference returned from the payment provider      |
| externalStatusCode| string    | Status code returned from the payment provider 	|

