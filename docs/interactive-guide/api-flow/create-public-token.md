# Step 2: Create a Public Token

## Description
This request will generate a public key to be used in encrypting PCI data as well as a tokenId to be passed to subsequent calls through the header. Also, the passed in “fdCustomerId” is how the customer is identified in the ucom app. Best practice would be to use the same value as merchantCustomerId.

## Endpoint URL
Method: POST
URL: https://int.api.firstdata.com/ucom/v1/tokens

## Sample Request