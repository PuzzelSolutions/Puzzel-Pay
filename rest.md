# InstaPay API

<!-- TOC depthFrom:2 depthTo:6 insertAnchor:false orderedList:false updateOnSave:true withLinks:true -->

- [Swagger UI](#swagger-ui)
- [Authentication](#authentication)
    - [Refresh token](#refresh-token)
        - [Obtaining a new JWT](#obtaining-a-new-jwt)
            - [Request example](#request-example)
            - [Response example](#response-example)
- [Resources](#resources)
    - [Batches](#batches)
        - [Create batch](#create-batch)
            - [Request example](#request-example)
            - [Batch Request](#batch-request)
            - [Recipient](#recipient)
            - [Payment](#payment)
            - [Message Template](#message-template)
            - [Batch Action](#batch-action)
            - [Direct Payment Settings](#direct-payment-settings)
            - [Receipt Template](#receipt-template)
                - [Predefined variables](#predefined-variables)
            - [Service Overrides](#service-overrides)
            - [Service Template](#service-template)
            - [Request Type](#request-type)
            - [Response example, ok](#response-example-ok)
            - [Response example, validation errors](#response-example-validation-errors)
            - [Response codes](#response-codes)
            - [Batch Response](#batch-response)
            - [Recipient Status](#recipient-status)
            - [Payment Link](#payment-link)
            - [Validation Error](#validation-error)
        - [Get batch status](#get-batch-status)
            - [Request example](#request-example)
            - [Response example](#response-example)
            - [Response codes](#response-codes)
            - [Batch Status](#batch-status)
            - [Sms Status](#sms-status)
            - [Payment Provider Status](#payment-provider-status)
            - [Payment Provider Payment Status](#payment-provider-payment-status)
            - [Payment Provider](#payment-provider)
            - [Recipient Status](#recipient-status)
            - [Step](#step)

<!-- /TOC -->

## Swagger UI

Swagger UI is available [here](https://instapay.intele.com/api/swagger/ui/index.html) and lists the available operations.

## Authentication

The API uses [JSON Web Tokens (JWT)](https://auth0.com/docs/jwt) for authentication.

### Refresh token

Once we've set up an account you'll receive a refresh token.

> **IMPORTANT**: Refresh tokens must be stored securely by the API client application because they essentially allow a user to remain authenticated forever. 

> **IMPORTANT**: Obtaining new tokens using the `refresh_token` should happen only if the `id_token` has expired. For example, it is a bad practice to call the endpoint to get a new token every time you do an API call. There are rate limits in Auth0 that will throttle the amount of requests that can be done using the same token from a certain IP to this endpoint.

#### Obtaining a new JWT

Done by the API client application when the API returns `401 Unauthorized`.

##### Request example

    POST https://itcauth0prod.eu.auth0.com/delegation
    Content-Type: application/json

    {
      "client_id":       "AzBkPvEzyDfzkEDcejIy50BVLXM8N4m4",
      "grant_type":      "urn:ietf:params:oauth:grant-type:jwt-bearer",
      "refresh_token":   "your_refresh_token",
      "api_type":        "app",
      "scope":           "openid app_metadata"
    }

##### Response example

    Content-Type: application/json

    {
      "token_type": "Bearer",
      "expires_in": 3600,
      "id_token": "eyJ..."
    }

> **TIP**: Check out the [libraries & SDKs](https://auth0.com/docs#toc) provided by Auth0.

## Resources

### Batches

#### Create batch

##### Request example

    POST https://instapay.intele.com/api/batches
    Content-Type: application/vnd.instapay.batchrequest.v1+json
    Accept: application/vnd.instapay.batchresponse.v1+json
    Authorization: Bearer eyJ...

    {
      "batchReference": "a_unique_reference",
      "expires": "2017-01-01T09:00:00.000Z",
      "recipients": [
        {
          "reference":"a_unique_reference",
          "msisdn": "+47XXXXXXXX",
          "variables": {
              "name":"John",
              "amount": "1"                    
          },
          "type":0,
          "payment": {
              "amount": 100,
              "currency": "NOK"
          }
        },
        {
          "reference":"a_unique_reference",
          "msisdn": "+47XXXXXXXX",
          "variables": {
              "name":"Jane",
              "amount": "2"                    
          },
          "type":0,
          "payment": {
              "amount": 200,
              "currency": "NOK"
          }
        }
      ],
      "messageTemplates":[
        {
          "type":0,
          "text":"Dear {{name}}. On behalf of company X you're receiving this message due to an unpaid invoice. Amount kr. {{amount}},-. Tap the link to pay."
        }
      ],
      "directPaymentSettings": {
        "businessModel": "STREX-PAYMENT",
        "serviceCode": "14001"
      },
      "action": 1,
      "receiptTemplates": [
        {
          "type": 0,
          "text": "Dear {{name}}, the invoice payment has been registered."
        }
      ],
      "serviceOverrides": {
        "serviceTemplates": [
          {
            "type": 0,
            "text1": "string",
            "text2": "string",
            "text3": "string",
            "text4": "string"
          }
        ]
      }
    }

##### Batch Request

| Property              | Type                                                | Description                                                        | Mandatory |
|-----------------------|-----------------------------------------------------|--------------------------------------------------------------------|:---------:|
| batchReference        | string                                              | Unique identifier for the batch                                    | Y         |
| recipients            | array of [Recipient](#recipient)                    | SMS message recipient(s)                                           | Y         |
| messageTemplates      | array of [Message Template](#message-template)      | SMS template(s)                                                    | Y         |
| action                | one of [Batch Action](#batch-action)                | Batch action                                                       | Y         |
| expires               | datetime                                            | Date/time when the batch expires                                   | N         |
| directPaymentSettings | [Direct Payment Settings](#direct-payment-settings) | Direct Payment settings, only mandatory if enabled for the service | N         |
| receiptTemplates      | array of [Receipt Template](#receipt-template)      | Receipt template(s)                                                | N         |
| serviceOverrides      | [Service Overrides](#service-overrides)             | Service overrides, will override default settings                  | N         |

##### Recipient

| Property  | Type                                 | Description                                     | Mandatory |
|-----------|--------------------------------------|-------------------------------------------------|:---------:|
| reference | string                               | Unique identifier for the recipient             | Y         |
| msisdn    | string                               | MSISDN of the recipient                         | Y         |
| type      | one of [Request Type](#request-type) | Request type                                    | Y         |
| payment   | [Payment](#payment)                  | Payment details                                 | Y         |
| variables | array of [Variable](#variable)       | Variable(s) that should be used in the template | N         |

##### Payment

| Property | Type   | Description                            | Mandatory |
|----------|--------|----------------------------------------|:---------:|
| amount   | int    | Amount to pay, in lowest monetary unit | Y         |
| currency | string | Currency                               | Y         |

##### Message Template

| Property | Type                                 | Description                                    | Mandatory |
|----------|--------------------------------------|------------------------------------------------|:---------:|
| type     | one of [Request Type](#request-type) | Request type                                   | Y         |
| text     | string                               | Template text, including variable placeholders | Y         |

##### Batch Action

| Value | Description            |
|-------|------------------------|
| 1     | Send SMS message(s)    |
| 2     | Create link(s)         |

##### Direct Payment Settings

| Property      | Type   | Description                                                                                                                | Mandatory |
|---------------|--------|----------------------------------------------------------------------------------------------------------------------------|:---------:|
| businessModel | string | Business model, valid values are listed [here](https://github.com/Intelecom/direct-payment/blob/master/business-models.md) | Y         |
| serviceCode   | string | Service code, valid values are listed [here](https://github.com/Intelecom/sms/blob/master/references/servicecodes.md)      | Y         |

##### Receipt Template

| Property | Type                                 | Description                                    | Mandatory |
|----------|--------------------------------------|------------------------------------------------|:---------:|
| type     | one of [Request Type](#request-type) | Request type                                   | Y         |
| text     | string                               | Template text, including variable placeholders | Y         |

###### Predefined variables

The following predefined variables may be used in receipt templates.

| Variable name    | Description                                          |
|------------------|------------------------------------------------------|
| paymentLink      | Payment/receipt link, e.g. https://short.url/example |
| paymentReference | Payment reference, e.g. abc123                       |

##### Service Overrides

| Property         | Type                                           | Description                                    | Mandatory |
|------------------|------------------------------------------------|------------------------------------------------|:---------:|
| serviceTemplates | array of [Service Template](#service-template) | Service template                               | Y         |

##### Service Template

If set, these will override the default values for the service. Fallback values will be used for those which are not specified.

| Property | Type                                 | Description                                    | Mandatory |
|----------|--------------------------------------|------------------------------------------------|:---------:|
| type     | one of [Request Type](#request-type) | Request type                                   | Y         |
| text1    | string                               | Template text, including variable placeholders | N         |
| text2    | string                               | Template text, including variable placeholders | N         |
| text3    | string                               | Template text, including variable placeholders | N         |
| text4    | string                               | Template text, including variable placeholders | N         |

![alt text](images/instapay-service-template-texts.png "Template texts")

##### Request Type

| Value | Description |
|-------|-------------|
| 0     | Payment     |
| 1     | Reminder    |
| 2     | Dunning     |
| 3     | Lost case   |

##### Response example, ok

    Content-Type: application/vnd.instapay.batchresponse.v1+json

    {
      "batchReference": "a_unique_reference",
      "smsStatuses": [
        {
          "reference": "a_unique_reference",
          "statusCode": 0,
          "statusMessage": "Message enqueued for sending"
        },
        {
          "reference": "a_unique_reference",
          "statusCode": 0,
          "statusMessage": "Message enqueued for sending"
        }
      ],
      "paymentLinks": null,
      "validationErrors": null
    }

##### Response example, validation errors

    Content-Type: application/vnd.instapay.batchresponse.v1+json

    {
      "batchReference": "a_unique_reference",
      "smsStatuses": null,
      "paymentLinks": null,
      "validationErrors": {
        "recipients[0].variables": [
          "The variable 'amount' is missing"
        ],
        "recipients[0].variables[0]": [
          "The variable 'name' is missing in the template"
        ]
      }
    }

##### Response codes

| Code | Description           | 
|------|-----------------------|
| 200  | Ok                    |
| 400  | Validation error(s)   |
| 401  | Unauthorized          |
| 500  | Internal server error |


##### Batch Response

| Property         | Type                                           | Description                     |
|------------------|------------------------------------------------|---------------------------------|
| batchReference   | string                                         | Unique identifier for the batch |
| smsStatuses      | array of [Recipient Status](#recipient-status) | SMS recipient status            |
| paymentLinks     | array of [Payment Link](#payment-link)         | Payment link(s)                 |
| validationErrors | array of [Validation Error](#validation-error) | Request validation error(s)     |

##### Recipient Status

| Property      | Type   | Description                        |
|---------------|--------|------------------------------------|
| reference     | string | Unique reference for the recipient |
| statusCode    | int    | Status code                        |
| statusMessage | string | Status message                     |

##### Payment Link

| Property      | Type   | Description                        |
|---------------|--------|------------------------------------|
| reference     | string | Unique reference for the recipient |
| link          | string | Payment link                       |

##### Validation Error

| Property                          | Type            | Description                 |
|-----------------------------------|-----------------|-----------------------------|
| The key path of the invalid input | array of string | Validation error message(s) |

> **IMPORTANT**: There will be one property for each key path that failed validation.

#### Get batch status

##### Request example

    GET https://instapay.intele.com/api/batches/{reference}/status
    Accept: application/vnd.instapay.batchstatus.v1+json
    Authorization: Bearer eyJ...

| Parameter | Type   | Description                    | Mandatory |
|-----------|--------|--------------------------------|:---------:|
| reference | string | Unique reference for the batch | Y         |

##### Response example

    Content-Type: application/vnd.instapay.batchstatus.v1+json

    {
      "reference": "a_unique_reference",
      "smsStatus": {
        "sent": 2,
        "delivered": 2,
        "clicked": 2
      },
      "paymentProviderStatus": {
        "averageConversionTimeInMinutes": 3,
        "paymentProviders": [
          {
            "paymentProvider": 1,
            "selected": 2,
            "paid": 2
          }
        ]
      },
      "recipientStatus": [
        {
          "reference": "a_unique_reference",
          "step": 6
        },
        {
          "reference": "a_unique_reference",
          "step": 4
        }
      ]
    }

##### Response codes

| Code | Description           | 
|------|-----------------------|
| 200  | Ok                    |
| 401  | Unauthorized          |
| 404  | Batch not found       |
| 500  | Internal server error |

##### Batch Status

| Property              | Type                                                | Description                    |
|-----------------------|-----------------------------------------------------|------------------------------- |
| reference             | string                                              | Unique reference for the batch |
| smsStatus             | [Sms Status](#sms-status)                           | SMS status                     |
| paymentProviderStatus | [Payment Provider Status](#payment-provider-status) | Payment provider status        |
| recipientStatus       | [Recipient Status](#recipient-status)               | Status of the recipient(s)     |

##### Sms Status

| Property  | Type | Description                                           |
|-----------|------|-------------------------------------------------------|
| sent      | int  | Number of sent SMS messages                           |
| delivered | int  | Number of delivered SMS messages                      |
| clicked   | int  | Number of links that were clicked in the SMS messages |

##### Payment Provider Status

| Property                       | Type                                                                         | Description                         |
|--------------------------------|------------------------------------------------------------------------------|-------------------------------------|
| averageConversionTimeInMinutes | int                                                                          | Average conversion time, in minutes |
| paymentProviders               | array of [Payment Provider Payment Status](#payment-provider-payment-status) | Payment provider status             |

##### Payment Provider Payment Status

| Property        | Type                                         | Description                                                     |
|-----------------|----------------------------------------------|-----------------------------------------------------------------|
| paymentProvider | one of [Payment Provider](#payment-provider) | Payment provider                                                |
| selected        | int                                          | Number of recipients that have selected this payment provider   |
| paid            | int                                          | Number of recipients that have paid using this payment provider |

##### Payment Provider

| Value | Description |
|-------|-------------|
| 1     | Adyen       |
| 2     | Strex       |
| 3     | Dibs        |

##### Recipient Status

| Property  | Type                 | Description                        |
|-----------|----------------------|------------------------------------|
| reference | string               | Unique reference for the recipient |
| step      | one of [Step](#step) | Current step of the recipient      |

##### Step

| Value | Description             |
|-------|-------------------------|
| 0     | SMS failed              |
| 1     | SMS enqueued            |
| 2     | SMS received            |
| 3     | SMS clicked             |
| 4     | Picked payment provider |
| 5     | Payment failed          |
| 6     | Paid                    |