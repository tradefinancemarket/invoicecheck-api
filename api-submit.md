# Submit Invoice

Add a new invoice to the system

## URL

    /invoice

## Method

    POST

## Query Params

    token=[ACCESS_TOKEN]

## Header Params

    None

## Data Params

    {
      "creditor": {
        "identifier": "string"
     },
      "debtor": {
        "identifier": "string"
     },
     "identifier": "string",
     "date": "string",
     "value": "string",
     "currency": "string"
    }


### Field Description


    |-------------+--------------------+-----------+---------------------------------------------------------------|
    | Data Fields |                    | Data Type | Data Qualifications                                           |
    |-------------+--------------------+-----------+---------------------------------------------------------------|
    | creditor    |                    |           |                                                               |
    |             | identifier         | String    | Value descriptor [eulerid, DUN, etc]                          |
    | debtor      |                    |           |                                                               |
    |             | identifier         | String    | Value descriptor [eulerid, DUN, etc]                          |
    |             |                    |           |                                                               |
    | identifier  |                    | String    | Unique value to identify invoice. Max 100 length              |
    | date        |                    | String    | Date in yyyy-mm-dd format - ISO 8601                          |
    | value       |                    | Number    | Numeric float value                                           |
    | currency    |                    | String    | Three character - ISO 4217 currency code                      |
    |-------------+--------------------+-----------+---------------------------------------------------------------|


- Creditor and Debtor identifiers are unique to the system and can be discovered using the Invoice Check Platform. Access to this system and instructions are provided when your account is creatd
- Valid currency codes: [Currency Codes](currency-codes)



## Responses
### 201 (Created)

- Success

        {
            "id": "ID-VALUE",
            "stamped": false
        }

### 400 (Bad Request)
 
- All fields in the Invoice data structure is required and must be of the type as described above in the 'Data Parameter Description'.


### 401 (Unauthorized)

- Returned when the submitted \`token\` value is not valid.

        {
                "errors": [
                "Invalid Token"
                ]
        }


### 422 (Unprocessable Entry)

- Returned if any of the data fields do not confirm to the \`Data Qualifications\` specified above in the \`Data Parameter Description\` table.

        {
                "errors": [
                    "Error in the creditor identifier field",
                    "Error in the debtor identifier field",
                    "Required date format for the issue-date is yyyy-mm-dd",
                    "The currency code value must be a three character ISO 4217 currency code"
                ]
        }


## Example

    curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \ 
    "creditor": { \ 
      "identifier": "string" \ 
    }, \ 
    "debtor": { \ 
      "identifier": "string" \ 
    }, \ 
    "identifier": "string", \ 
    "date": "string", \ 
    "value": 0, \ 
    "currency": "string" \ 
    }' 'http://INVOICE-CHECK-API/invoices?token=TOKEN'

### Response

        {
                "id": "ID-VALUE",
                "stamped": false
        }

## Notes

-   Processing:
-   All data as strings is trimmed of leading and trailing whitespace.
-   All data as strings is considered in a case-insenstive manner.





