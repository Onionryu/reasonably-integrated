---
title: Logic Apps Consumption and Storage Account with Managed Identity
date: 2025-07-23
---
# Intro
If you want to reach Storage Account from a Consumption Logic Apps with a Managed Identity it required a few steps more than what you would typically need in a Standard Logic Apps. Since Consumption Logic Apps does not have an in-app connector to Storage Account we have to use the next best thing and leverage the REST API built into Storage Account. The idea here is to use the HTTP action so we would not need a connector.

# First problem
If you just set up the Logic Apps with a System-assigned Managed Identity and give role assignments on Storage Account as a Data Contributor and try to access the Storage Account with a HTTP action, we get the following when using a Table service for this example.

> [Azure Documentation about the REST API](https://learn.microsoft.com/en-us/rest/api/storageservices/table-service-rest-api)

Request:
```HTTP
GET https://<storage account name>.table.core.windows.net/<table name>()
```
Authentication Type: Managed Identity
Managed Identity: System-assigned managed identity
Audience: https://storage.azure.com

Response:
```XML
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<error xmlns="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata">
    <code>AuthenticationFailed</code>
  <message xml:lang="en-US">Server failed to authenticate the request. Make sure the value of Authorization header is formed correctly including the signature.</message>
</error>
```

So looks like we are getting a 403 Forbidden error even though a similar kind of message would work for other endpoints. And since we cannot see the actual header of the outgoing call we cannot verify if the Authorization header is set correctly.

# Resolving first issue
After some digging around on the documentation and stackoverflow we can see that we need some additional information in the outgoing call, namely a header x-ms-version with a value of 2017-11-09.

> [Azure Documentation about the header](https://learn.microsoft.com/en-us/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens)

Adding this header to the call gives us a different response this time, more specifically a 415 Unsupported Media Type:
```XML
<?xml version="1.0" encoding="utf-8"?>
<error xmlns="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata">
    <code>AtomFormatNotSupported</code>
    <message xml:lang="en-US">Atom format is not supported.</message>
</error>
```

# Resolving second issue
After some more digging around we find out that the request is failing because the Storage account defaults to sending application/atom+xml as a media type. If we now add an Accept: application/json to the request we finally get an actual response with 200 status message!

# Alternate issues
If you later on decide to test just the Accept application/json Header, you will get the same 403 Forbidden response with XML as a error message, pretty weird.

In blob storage side, use additionally x-ms-blob-type: BlockBlob and Content-type: application/octet-stream headers.

If you are getting InvalidProtocolResponse as an error when inputting information, try turning off the HTTP Allow chunking setting.

# TLDR
- Directly use the HTTP Action on Logic Apps
- Enable System-assigned Identity to the Logic Apps
- Add the Storage Account role assignments to the Logic Apps Identity
- Add the Headers: x-ms-version: 2017-11-09 and Accept: application/json
- For blob storage also add Headers: x-ms-blob-type: BlockBlob and Content-type: application/octet-stream
- If you are getting errors while inputting data, go to HTTP action settings and turn off Allow chunking option