# Invoice Check API

Invoice Check provides three methods for submitting invoices for verification and stamping.

To stamp an invoice is to mark that it is being funded.

It is important to understand that an invoice is charaterized by six fields as described in the `Data Parameter Description` section below.

The system will use these values to build a unique identifier for the invoice which is returned from the `Submit Invoice` method. 

If the invoice has not been previously stamped and you as a firm which to stamp it you will use the identifier in the `Stamp Invoice` method.

After you have signed up to become an Invoice Check user you will receive the root URL, a link to a Swagger UI for testing and your ACCESS_TOKEN.

    

## Methods

* [Submit](api-submit.md)
* [Check](api-check.md)
* [Stamp](api-stamp.md)