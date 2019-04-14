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
    "currency": "string",
    "issue-date": "string",
    "due-date": "string"
    }

### Data Parameter Description

    |-----------------------+-----------+-----------------------------------------|
    | Field                 | Data Type | Data Qualifications                     |
    |-----------------------+-----------+-----------------------------------------|
    | creditor-identifier   | String    | Maximum string length of 100 characters |
    | creditor-country-code | String    | Maximum string lenght of 50 characters  |
    | debtor-identifier     | String    | Maximum string lenght of 100 characters |
    | debtor-country-code   | String    | Maximum string length of 50 characters  |
    | invoice-identifier    | String    | Maximum string length of 100 characters |
    | value                 | Number    | Numeric float                           |
    | currency              | String    | Three character currency code           |
    | issue-date            | String    | Date in yyyy-mm-dd format               |
    | due-date              | String    | Date in yyyy-mm-dd format               |
    |-----------------------+-----------+-----------------------------------------|

## Success Response

### Code: 200 SUCCESS

1.  Body

        {
            "id": "065012ba-8e10-5539-8233-029499ddd93f",
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
            "The creditor country-code field is limited to 50 characters",
            "The debtor identifier field is limited to 100 characters",
            "The debtor country-code field is limited to 50 characters",
            "The identifier field is limited to 100 characters",
            "Currency code is limited to 3 characters and must be a valid currency code",
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
    "currency": "string", \ 
    "issue-date": "string", \ 
    "due-date": "string" \ 
    }' 'http://34.222.4.16:3000/invoices?access_token=ACCESSTOKEN'

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
        "id": "052ad0fa-a7b3-528b-a909-02ec4f72dbc1",
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
    'http://localhost:3000/invoices/041fa23b-6691-541f-be22-61b18171d2c2/stamp?access_token=BRADTOKEN'

### Response

    {
        "id": "041fa23b-6691-541f-be22-61b18171d2c2",
        "stamped": true
    }

## Notes

-   To obtain the appropriate invoice\_id one must submit the appropriate data to the \`Submit Invoice\` to retrieve the id.
