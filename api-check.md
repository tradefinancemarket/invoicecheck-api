# Check Invoice
Check if an invoice is in the system

## URL

    /invoice/check/ID

    Pass the ID of the invoice returned from a succesful submit
    
## Method

    GET

## Query Params

    token=[ACCESS_TOKEN]

## Header Params

    None

## Data Params

    None

## Respnses
### 200 (OK)

- Success

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

### 404 (Not Found)

- Not found. ID is not found


        {
            "errors": {
                "Invalid invoice id"
            }
        }


## Example

    curl -X GET --header 'Accept: application/json' 'http://INVOICE-CHECK-API/invoice/check/ID-VALUE?token=TOKEN'

### Response

    {
        "id": "ID-VALUE",
        "stamped": false
    }



