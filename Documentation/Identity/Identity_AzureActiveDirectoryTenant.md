---
uid: identityAzureActiveDirectoryTenant
---

# AzureActiveDirectoryTenant

An Azure Active Directory Tenant is used to map an existing
            [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/)
            Tenant from Azure to OSIsoft Cloud Services. We only allow one Azure Active Directory Tenant
            per OSIsoft Cloud Services Tenant.

## Properties

For HTTP requests and responses, the AzureActiveDirectoryTenant object has the following properties and JSON-serialized body: 

Property | Type | Description
 --- | --- | ---
Id | string | Gets or sets id of an Azure Active Directory Tenant.
ConsentState | ConsentState | Gets or sets Consent State of Azure Active Directory Tenant. Can be: NotConsented (0), Consented (1).

### Serialized Model

```json
{
  "Id": "Id",
  "ConsentState": 0
}
```

***

## Base URL

All URLs referenced in this section have the following base:

`https://dat-b.osisoft.com/`

## Authentication

All endpoints referenced in this documentation require authenticated access. Authorization header must be set to the access token you retrieve after a successful authentication request.

`Authorization: Bearer <token>`

Requests made without an access token or an invalid/expired token will fail with a 401 Unauthorized response.
Requests made with an access token which does not have the correct permissions (see security subsection on every endpoint) will fail with a 403 Forbidden.
Read [here](https://github.com/osisoft/OSI-Samples-OCS/tree/master/basic_samples/Authentication) on how to authenticate against OCS with the various clients and receive an access token in response.

## Error Handling

All responses will have an error message in the body. The exceptions are 200 responses and the 401 Unauthorized response. The error message will look as follows:

```json
{
    "OperationId": "1b2af18e-8b27-4f86-93e0-6caa3e59b90c", 
    "Error": "Error message.", 
    "Reason": "Reason that caused error.", 
    "Resolution": "Possible solution for the error." 
}
```

If and when contacting OSIsoft support about this error, please provide the OperationId.

## `Add Azure Active Directory Tenant to OCS Tenant`

Add an Azure Active Directory Tenant to the OSIsoft Cloud Services Tenant.

### Request

`POST api/v1/Tenants/{tenantId}/AzureActiveDirectoryTenants/{azureActiveDirectoryTenantId}`

### Parameters

```csharp
[Required]
string tenantId
```

Id of OSIsoft Cloud Services Tenant.

```csharp
[Required]
string azureActiveDirectoryTenantId
```

Id or Domain Name of Azure Active Directory Tenant.

### Security

Allowed for these roles:

- `Account Administrator`

### Returns

#### 201

Created.

##### Type:

 `AzureActiveDirectoryTenant`

```json
{
  "Id": "Id",
  "ConsentState": 0
}
```

#### 400

Missing or invalid inputs.

#### 401

Unauthorized.

#### 403

Forbidden.

#### 404

OSIsoft Cloud Services Tenant not found.

#### 409

Id of Azure Active Directory Tenant. is already in use on the specified Tenant.

#### 500

Internal server error.
***

## `Get All Azure Active Directory Tenants from OCS Tenant`

Get all Azure Active Directory Tenants from an OSIsoft Cloud Services Tenant.

### Request

`GET api/v1/Tenants/{tenantId}/AzureActiveDirectoryTenants`

### Parameters

```csharp
[Required]
string tenantId
```

Id of OSIsoft Cloud Services Tenant.

```csharp
[FromQuery]
[Optional]
[Default = ""]
string query
```

Query to execute. Currently not supported.

```csharp
[FromQuery]
[Optional]
[Default = 0]
int32 skip
```

Number of Azure Active Directory tenants to skip.

```csharp
[FromQuery]
[Optional]
[Default = 100]
int32 count
```

Maximum number of Azure Active Directory tenants to return.

### Security

Allowed for these roles:

- `Account Administrator`

### Returns

#### 200

Success.

##### Type:

 `List`

```json
[
  {
    "Id": "Id",
    "ConsentState": 0
  },
  {
    "Id": "Id",
    "ConsentState": 0
  }
]
```

#### 401

Unauthorized.

#### 403

Forbidden.

#### 403

Forbidden.

#### 500

Internal server error.
***

## `Get AAD Tenant from OCS Tenant`

Get Azure Active Directory Tenant from an OSIsoft Cloud Services Tenant.

### Request

`GET api/v1/Tenants/{tenantId}/AzureActiveDirectoryTenants/{azureActiveDirectoryTenantId}`

### Parameters

```csharp
[Required]
string tenantId
```

Id of OSIsoft Cloud Services Tenant.

```csharp
[Required]
string azureActiveDirectoryTenantId
```

Id of Azure Active Directory Tenant.

### Security

Allowed for these roles:

- `Account Administrator`

### Returns

#### 200

Success.

##### Type:

 `AzureActiveDirectoryTenant`

```json
{
  "Id": "Id",
  "ConsentState": 0
}
```

#### 400

Missing or invalid inputs.

#### 401

Unauthorized.

#### 403

Forbidden.

#### 404

OSIsoft Cloud Services Tenant not found.

#### 500

Internal server error.
***

## `Remove AAD Tenant from OCS Tenant`

Remove Azure Active Directory Tenant from an OSIsoft Cloud Services Tenant.

### Request

`DELETE api/v1/Tenants/{tenantId}/AzureActiveDirectoryTenants/{azureActiveDirectoryTenantId}`

### Parameters

```csharp
[Required]
string tenantId
```

Id of OSIsoft Cloud Services Tenant.

```csharp
[Required]
string azureActiveDirectoryTenantId
```

Id of Azure Active Directory Tenant to remove.

### Security

Allowed for these roles:

- `Account Administrator`

### Returns

#### 204

Removed.

#### 400

Missing or invalid inputs.

#### 401

Unauthorized.

#### 403

Forbidden.

#### 404

OSIsoft Cloud Services Tenant not found.

#### 500

Internal server error.
***

## `Send Consent Email for AAD Tenant`

Send consent for an Azure Active Directory Tenant. OSIsoft Cloud Services needs to be granted
            permission to interact with the Azure Active Directory tenant. Otherwise, users from this Azure Active Directory Tenant
            cannot accept invitations from OSIsoft Cloud Services and log in. You can read more about this
            [here](https://pisquare.osisoft.com/docs/DOC-3986-msa-consent-for-azure-active-directory).

### Request

`POST api/v1/Tenants/{tenantId}/AzureActiveDirectoryTenants/{azureActiveDirectoryTenantId}/SendConsent`

### Parameters

```csharp
[Required]
string tenantId
```

Id of OSIsoft Cloud Services Tenant.

```csharp
[Required]
string azureActiveDirectoryTenantId
```

Id of Azure Active Directory Tenant.

```csharp
[FromBody]
[Required]
ConsentInformation consentInformation
```

ConsentInformation object.

Property | Type | Required | Description 
 --- | --- | --- | ---
AzureActiveDirectoryConsentEmail | string | Yes | Gets or sets address to email consent.            Only Azure Active Directory Admins have permission to consent to            being allowed to interact with the tenant. The email            does not have to be sent to an Admin.
AzureActiveDirectoryConsentGivenName | string | Yes | Gets or sets preferred name to use in the consent email.
AzureActiveDirectoryConsentSurname | string | Yes | Gets or sets preferred surname to use in the consent email.
AzureActiveDirectoryTenant | string | Yes | Gets or sets Azure Active Directory Domain Name (e.g. mydomain.onmicrosoft.com).



```json
{
  "AzureActiveDirectoryConsentEmail": "user@company.com",
  "AzureActiveDirectoryConsentGivenName": "Name",
  "AzureActiveDirectoryConsentSurname": "Surname",
  "AzureActiveDirectoryTenant": "AzureActiveDirectoryTenant"
}
```

### Security

Allowed for these roles:

- `Account Administrator`

### Returns

#### 204

Removed.

#### 400

Missing or invalid inputs.

#### 401

Unauthorized.

#### 403

Forbidden.

#### 404

OSIsoft Cloud Services Tenant not found.

#### 500

Internal server error.
***

## `Get Azure Active Directory Tenant in Tenant`

Validate that Azure Active Directory Tenant exists in this OSIsoft Cloud Services Tenant.
            This endpoint is identical to the GET one but
            it does not return any objects in the body.

### Request

`HEAD api/v1/Tenants/{tenantId}/AzureActiveDirectoryTenants/{azureActiveDirectoryTenantId}`

### Parameters

```csharp
[Required]
string tenantId
```

Id of OSIsoft Cloud Services Tenant.

```csharp
[Required]
string azureActiveDirectoryTenantId
```

Id of Azure Active Directory Tenant.

### Security

Allowed for these roles:

- `Account Administrator`

### Returns

#### 200

Success.

##### Type:

 `Void`

#### 400

Missing or invalid inputs.

#### 401

Unauthorized.

#### 403

Forbidden.

#### 404

OSIsoft Cloud Services Tenant not found.

#### 500

Internal server error.
***

## `Get Total Count of Azure Active Directory Tenant in Tenant`

Return total number of Azure Active Directory tenants in a OSIsoft Cloud Services Tenant. This endpoint
            is identical to the GET one but it does not return any objects
            in the body.

### Request

`HEAD api/v1/Tenants/{tenantId}/AzureActiveDirectoryTenants`

### Parameters

```csharp
[Required]
string tenantId
```

Id of OSIsoft Cloud Services Tenant.

### Security

Allowed for these roles:

- `Account Administrator`

### Returns

#### 200

Success.

##### Type:

 `Void`

#### 400

Missing or invalid inputs.

#### 401

Unauthorized.

#### 403

Forbidden.

#### 404

OSIsoft Cloud Services Tenant not found.

#### 500

Internal server error.
***

