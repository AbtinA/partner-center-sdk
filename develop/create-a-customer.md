---
title: Create a customer
description: This topic explains how to create a new customer. If you are an indirect provider and you want to create a customer for an indirect reseller, please see Create a customer for an indirect reseller.
ms.assetid: 7EA3E23F-0EA8-49CB-B98A-C4B74F559873
ms.author: v-thpr
ms.date: 12/15/2017
ms.topic: article
ms.prod: partner-center
ms.technology: partner-center-sdk
---

# Create a customer


**Applies To**

-   Partner Center
-   Partner Center operated by 21Vianet
-   Partner Center for Microsoft Cloud Germany
-   Partner Center for Microsoft Cloud for US Government

This topic explains how to create a new customer. If you are an indirect provider and you want to create a customer for an indirect reseller, please see [Create a customer for an indirect reseller](create-a-customer-for-an-indirect-reseller.md).

As a cloud solution provider (CSP) partner, when you create a customer you can place orders on behalf of the customer. When you create a customer, you also create:

-   An Azure Active Directory (AD) tenant object for the customer.
-   A relationship between the reseller and customer, used for delegated admin privileges.
-   A user name and password to sign in as an admin for the customer.

Once the customer is created, save the customer ID and Azure AD details for future use with the Partner Center SDK. You will need them for use with account management, for example.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Prerequisites


Credentials as described in [Partner Center authentication](partner-center-authentication.md). This scenario supports authentication with both standalone App and App+User credentials.

## <span id="C_"></span><span id="c_"></span>C#


To add a customer, instantiate a new [Customer](https://review.docs.microsoft.com/dotnet/api/microsoft.store.partnercenter.models.customers.customer) object. Be sure to fill in the [BillingProfile](https://review.docs.microsoft.com/dotnet/api/microsoft.store.partnercenter.models.customers.customerbillingprofile) and [CompanyProfile](https://review.docs.microsoft.com/dotnet/api/microsoft.store.partnercenter.models.customers.customercompanyprofile). Then, add it to your [IAggregatePartners.Customers](https://review.docs.microsoft.com/dotnet/api/microsoft.store.partnercenter.ipartner.customers) collection by calling [Create()](https://review.docs.microsoft.com/dotnet/api/microsoft.store.partnercenter.genericoperations.ientitycreateoperations.create) or [CreateAsync()](https://review.docs.microsoft.com/dotnet/api/microsoft.store.partnercenter.genericoperations.ientitycreateoperations.createasync).

```
// IAggregatePartner partnerOperations;
var partnerOperations = this.Context.UserPartnerOperations;

var customerToCreate = new Customer()
{
    CompanyProfile = new CustomerCompanyProfile()
    {
        Domain = string.Format(CultureInfo.InvariantCulture, 
            "SampleApplication{0}.{1}", 
            new Random().Next(), 
            this.Context.Configuration.Scenario.CustomerDomainSuffix)
    },
    BillingProfile = new CustomerBillingProfile()
    {
        Culture = "EN-US",
        Email = "SomeEmail@Outlook.com",
        Language = "En",
        CompanyName = "Some Company" + new Random().Next(),
        DefaultAddress = new Address()
        {
            FirstName = "Tania",
            LastName = "Carr",
            AddressLine1 = "One Microsoft Way",
            City = "Redmond",
            State = "WA",
            Country = "US",
            PostalCode = "98052",
            PhoneNumber = ""
        }
    }
};

var newCustomer = partnerOperations.Customers.Create(customerToCreate);
```

**Sample**: [Console test app](console-test-app.md). **Project**: Partner Center SDK Samples **Class**: CreateCustomer.cs

## <span id="_Request"></span><span id="_request"></span><span id="_REQUEST"></span> Request


**Request syntax**

| Method   | Request URI                                                       |
|----------|-------------------------------------------------------------------|
| **POST** | [*{baseURL}*](partner-center-rest-urls.md)/v1/customers HTTP/1.1 |

 

**Request headers**

-   This API is idempotent (it will not yield a different result if you call it multiple times).
-   A request ID and correlation ID are required.
-   See [Partner Center REST headers](headers.md) for more information.

**Request body**

This table describes the required properties in the request body.

| Name                              | Type   | Required | Description                                 |
|-----------------------------------|--------|----------|---------------------------------------------|
| [BillingProfile](#billingprofile) | object | Y        | The customer's billing profile information. |
| [CompanyProfile](#companyprofile) | object | Y        | The customer's company profile information. |

 

### <span id="billingProfile"></span><span id="billingprofile"></span><span id="BILLINGPROFILE"></span>

**Billing Profile**

This table describes the minimum required fields from the [CustomerBillingProfile](customers.md#customerbillingprofile) resource needed to create a new customer.

| Name             | Type                                     | Required | Description                                                                                                                                                                                                     |
|------------------|------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| email            | string                                   | Yes      | The customer's email address.                                                                                                                                                                                   |
| culture          | string                                   | Yes      | Their preferred culture for communication and currency, such as "en-US". See [Table of Language Culture Names](https://msdn.microsoft.com/en-us/library/ee825488%28v=cs.20%29.aspx) for the supported cultures. |
| language         | string                                   | Yes      | The default language. Two character language codes (e.g., en, fr) are supported.                                                                                                                                |
| company\_name    | string                                   | Yes      | The registered company/organization name.                                                                                                                                                                       |
| default\_address | [Address](utility-resources.md#address) | Yes      | The registered address of the customer's company/organization. See the [Address](utility-resources.md#address) resource for information on any length limitations.                                             |

 

### <span id="companyProfile"></span><span id="companyprofile"></span><span id="COMPANYPROFILE"></span>

**Company Profile**

This table describes the minimum required fields from the [CustomerCompanyProfile](customers.md#customercompanyprofile) resource needed to create a new customer.

| Name   | Type   | Required | Description                                                  |
|--------|--------|----------|--------------------------------------------------------------|
| domain | string | .Yes     | The customer's domain name, such as contoso.onmicrosoft.com. |

 

**Request example**

```
POST https://api.partnercenter.microsoft.com/v1/customers HTTP/1.1
Authorization: Bearer <token> 
Accept: application/json
MS-RequestId: 94e4e214-6b06-4fb7-96d1-94d559f9b47f
MS-CorrelationId: ab993325-1605-4cf4-bac4-fb584142a31b
X-Locale: en-US
Content-Type: application/json
Host: api.partnercenter.microsoft.com
Content-Length: 789
Expect: 100-continue
Connection: Keep-Alive

{
    "Id": null,
    "CommerceId": null,
    "CompanyProfile": {
        "TenantId": null,
        "Domain": "SampleApplication1047419322.onmicrosoft.com",
        "CompanyName": null,
        "Attributes": {
            "ObjectType": "CustomerCompanyProfile"
        }
    },
    "BillingProfile": {
        "Id": null,
        "FirstName": null,
        "LastName": null,
        "Email": "SomeEmail@Outlook.com",
        "Culture": "EN-US",
        "Language": "En",
        "CompanyName": "Some Company1817947250",
        "DefaultAddress": {
            "Country": "US",
            "Region": null,
            "City": "Redmond",
            "State": "WA",
            "AddressLine1": "One Microsoft Way",
            "AddressLine2": null,
            "PostalCode": "98052",
            "FirstName": "Tania",
            "LastName": "Carr",
            "PhoneNumber": ""
        },
        "Attributes": {
            "ObjectType": "CustomerBillingProfile"
        }
    },
    "RelationshipToPartner": "none",
    "AllowDelegatedAccess": null,
    "UserCredentials": null,
    "CustomDomains": null,
    "AssociatedPartnerId": null,
    "Attributes": {
        "ObjectType": "Customer"
    }
}
```

## <span id="Response"></span><span id="response"></span><span id="RESPONSE"></span>Response


If successful, this API returns a [Customer](customers.md) resource for the new customer. Save the customer ID and Azure AD details for future use with the Partner Center SDK. You will need them for use with account management, for example.

**Response success and error codes**

Each response comes with an HTTP status code that indicates success or failure and additional debugging information. Use a network trace tool to read this code, error type, and additional parameters. For the full list, see [Partner Center REST error codes](error-codes.md).

**Response example**

```
HTTP/1.1 201 Created
Content-Length: 834
Content-Type: application/json; charset=utf-8
MS-CorrelationId: ab993325-1605-4cf4-bac4-fb584142a31b
MS-RequestId: 94e4e214-6b06-4fb7-96d1-94d559f9b47f
MS-CV: ObwhuhD2tUKJoM+Z.0
MS-ServerId: 202010223
Date: Tue, 14 Feb 2017 20:06:02 GMT

﻿ {
    "id": "dfd8cc0a-c592-468c-8461-869a38d24738",
    "commerceId": "0a4ce58a-6f96-4273-8035-d9c7d31b9ba4",
    "companyProfile": {
        "tenantId": "dfd8cc0a-c592-468c-8461-869a38d24738",
        "domain": "SampleApplication1047419322.onmicrosoft.com",
        "attributes": {
            "objectType": "CustomerCompanyProfile"
        }
    },
    "billingProfile": {
        "id": "d17c0275-da92-5c33-9032-782ef1d0b69b",
        "email": "SomeEmail@Outlook.com",
        "culture": "en-US",
        "language": "En",
        "companyName": "Some Company1817947250",
        "defaultAddress": {
            "country": "US",
            "city": "Redmond",
            "state": "WA",
            "addressLine1": "One Microsoft Way",
            "postalCode": "98052",
            "firstName": "Tania",
            "lastName": "Carr",
            "phoneNumber": ""
        },
        "attributes": {
            "etag": "5920358838484612121",
            "objectType": "CustomerBillingProfile"
        }
    },
    "relationshipToPartner": "none",
    "userCredentials": {
        "userName": "admin",
        "password": "=;;n.=s9Z"
    },
    "attributes": {
        "objectType": "Customer"
    }
}
```

 

 




