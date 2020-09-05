# Stamp Invoice

Stamp an existing invoice as funded. Pass the ID of the invoice returned from a succesful submit.

## URL

    /api/invoices/:id


    
## Method

    PATCH

## Query Params

    token=[ACCESS_TOKEN]

## Header Params

    None

## Data Params

    {
        "stamped": true
    }

## Responses
### 200 (OK)

        {
            "id": "ID-VALUE",
            "stamped": false
        }

### 400 (Bad Request)

- Syntax errors. Invoid ID.


        {
            "errors": {
                "Invalid id parameter"
            }
        }

### 401 (Unauthorized)

- Returned when the submitted \`token\\\` value is not valid.

        {
            "errors": {
                "Invalid Token"
            }
        }

### 404

- Not found. ID is not found

        {
            "errors": {
                "Invalid invoice id"
            }
        }


### 422

- Invoice was already stamped.


        {
                "errors": {
                        "invoice": "Invoice has already been stamped"
                }
        }

## EXAMPLE

    curl -X PUT --header 'Content-Type: application/json' --header 'Accept: application/json' \
        'http://INVOICE-CHECK-API/invoice/stamp/ID-VALUE?token=TOKEN'

### Response

    {
        "id": "ID-VALUE",
        "stamped": true
    }

## Notes

-   To obtain the appropriate invoice\_id one must submit the appropriate data to the \`Submit Invoice\` to retrieve the id.
