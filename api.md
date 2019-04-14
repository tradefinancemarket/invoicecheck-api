# Invoice Check API

Invoice Check provides two endpoints `/invoices` and `/invoice/{id}/stamp` for submitting invoices for verification and stamping.

It is important to understand that an invoice is charaterized by nine fields as described in the `Data Parameter Description` section below. The system will use these values to build a unique identifier for the invoice which is returned from the `Submit Invoice` method. 

If the invoice has not been previously stamped and you as a firm which to stamp it you will use the identifier in the `Stamp Invoice` method.

After you have signed up to become an Invoice Check user you will receive the root URL, a link to a Swagger UI for testing and your ACCESS_TOKEN.

# Submit Invoice

## URL

    /invoices

## Method

    POST

## Query Params

    access_token=[ACCESS_TOKEN]

## Header Params

    None

## Data Params

### Example Data

     {
     "creditor": {
        "identifier": "string",
        "country-code": "string"
    },
     "debtor": {
        "identifier": "string",
        "country-code": "string"
    },
    "identifier": "string",
    "value": 0,
    "currency-code": "string",
    "issue-date": "string",
    "due-date": "string"
    }

### Data Parameter Description

    |-----------------------+-----------+-----------------------------------------|
    | Field                 | Data Type | Data Qualifications                     |
    |-----------------------+-----------+-----------------------------------------|
    | creditor-identifier   | String    | Maximum string length of 100 characters |
    | creditor-country-code | String    | Two character ISO 3166 country code     |
    | debtor-identifier     | String    | Maximum string length of 100 characters |
    | debtor-country-code   | String    | Two character ISO 3166 country code     |
    | invoice-identifier    | String    | Maximum string length of 100 characters |
    | value                 | Number    | Numeric float                           |
    | currency-code         | String    | Three character ISO 4217 currency code  |
    | issue-date            | String    | Date in yyyy-mm-dd format               |
    | due-date              | String    | Date in yyyy-mm-dd format               |
    |-----------------------+-----------+-----------------------------------------|

#### Valid Country Codes

- [country-codes](country-codes.md)

#### Valid Currency Codes

- [currency-codes](currency-codes.md)


## Success Response

### Code: 200 SUCCESS

1.  Body

        {
            "id": "041fa23b-6691-541f-be22-61b18171d2c2",
            "stamped": false
        }

## Error Responses

### Code: 400 BAD REQUEST

All fields in the Invoice data structure is required and must be of the type as described above in the 'Data Parameter Description'.

1.  Body

        {
            "errors": {
                "creditor": "missing-required-key"
            }
        }

### Code: 401 UNAUTHORIZED

Returned when the submitted \`access\_token\\\` value is not valid.

1.  Body

        {
            "errors": {
                "authorization": "Invalid access_token"
            }
        }

### Code: 422 UNPROCESSABLE ENTRY

Returned if any of the data fields do not confirm to the \`Data Qualifications\` specified above in the \`Data Parameter Description\` table.

     {
     "errors": [
        "The creditor identifier field is limited to 100 characters",
        "The creditor country code must be a two character ISO 3166 country code",
        "The debtor identifier field is limited to 100 characters",
        "The debtor country code must be a two character ISO 3166 country code",
        "The identifier field is limited to 100 characters",
        "The currency code value must be a three character ISO 4217 currency code",
        "Required date format for the issue-date is yyyy-mm-dd",
        "Required date format for the due-date is yyyy-mm-dd"
        ]
    }

## Sample Call

    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \ 
    "creditor": { \ 
      "identifier": "string", \ 
      "country-code": "string" \ 
    }, \ 
    "debtor": { \ 
      "identifier": "string", \ 
      "country-code": "string" \ 
    }, \ 
    "identifier": "string", \ 
    "value": 0, \ 
    "currency-code": "string", \ 
    "issue-date": "string", \ 
    "due-date": "string" \ 
    }' 'http://invoicecheck.app/invoices?access_token=ACCESS_TOKEN'

### Response

    {
        "id": "041fa23b-6691-541f-be22-61b18171d2c2",
        "stamped": false
    }

## Notes

-   Processing:
-   All data as strings is trimmed of leading and trailing whitespace.
-   All data as strings is considered in a case-insenstive manner.

# Stamp Invoice

## URL

    /invoices/:id/stamp

## Method

    PUT

## Query Params

    access_token=[ACCESS_TOKEN]

## Header Params

    None

## Data Params

    None

## Success Response

### Code: 200 SUCCESS

### Body

    {
        "id": "041fa23b-6691-541f-be22-61b18171d2c2",
        "stamped": true
    }

## Error Responses

### Code: 401 UNAUTHORIZED

Returned when the submitted \`access\_token\\\` value is not valid.

1.  Body

        {
            "errors": {
            "authorization": "Invalid access_token"
            }
        }

### Code: 422 UNPROCESSABLE ENTRY

Returned if the invoice id is invalid or if the requested Invoice has already been stamped.

    {
        "errors": {
            "invoice": "Invalid invoice id"
        }
    }

    {
        "errors": {
            "invoice": "Invoice has already been stamped"
        }
    }

## Sample Call

    curl -X PUT --header 'Content-Type: application/json' --header 'Accept: application/json' \
    'http://invoicecheck.app/invoices/041fa23b-6691-541f-be22-61b18171d2c2/stamp?access_token=ACCESS_TOKEN'

### Response

    {
        "id": "041fa23b-6691-541f-be22-61b18171d2c2",
        "stamped": true
    }

## Notes

-   To obtain the appropriate invoice\_id one must submit the appropriate data to the \`Submit Invoice\` to retrieve the id.
