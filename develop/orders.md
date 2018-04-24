---
title: Order
description: A partner places an order when a customer wants to buy a subscription from a list of offers.
ms.assetid: 5CFA35FF-1C0D-461D-A942-309AFCD98395
ms.author: v-thpr
ms.date: 12/15/2017
ms.topic: article
ms.prod: partner-center
ms.technology: partner-center-sdk
---

# Order

>[!IMPORTANT]   
>This information relates to pre-released product which may be substantially modified before it's commercially released. Microsoft makes no warranties, express or implied, with respect to the information provided here. 

<span class="sidebar_heading" style="font-weight: bold;">Applies
To</span>

-   Partner Center
-   Partner Center operated by 21Vianet
-   Partner Center for Microsoft Cloud Germany
-   Partner Center for Microsoft Cloud for US Government

A partner places an order when a customer wants to buy a subscription from a list of offers.

>[!NOTE]
>The Order resource has a rate limit of 500 requests per minute per tenant identifier.


## <span id="order"></span><span id="ORDER"></span>Order


Describes a partner's order.

| Property            | Type                                                           | Description                                                 |
|---------------------|----------------------------------------------------------------|-------------------------------------------------------------|
| id                  | string                                                         | An order identifier that is supplied upon successful creation of the order.                                   |
|referenceCustomerId  | string                                                         | The customer identifier. |
| billingCycle        | string                                                         | Indicates the frequency with which the partner is billed for this order. Supported values are the member names found in [BillingCycleType](products.md#billingcycletype). The default is "Monthly" or "OneTime" at order creation. This field is applied upon successful creation of the order. |
| lineItems           | array of [OrderLineItem](#orderlineitem) resources             | An itemized list of the offers the customer is purchasing including the quantity.        |
| currencyCode        | string                                                         | Read-only. The currency used when placing the order. Applied upon successful creation of the order.           |
| creationDate        | datetime                                                       | Read-only. The date the order was created, in date-time format. Applied upon successful creation of the order.                                   |
| status              | string                                                         | Read-only. The status of the order.  Supported values are the member names found in [**OrderStatus**](#orderstatus).        |
| links               | [OrderLinks](utility-resources.md#resourcelinks)           | The resource links corresponding to the Order.            |
| attributes          | [ResourceAttributes](utility-resources.md#resourceattributes) | The metadata attributes corresponding to the Order.       |



## <span id="orderLineItem"></span><span id="orderlineitem"></span><span id="ORDERLINEITEM"></span>OrderLineItem


An order contains an itemized list of offers, and each item is represented as an OrderLineItem.

| Property             | Type                                      | Description                                                                                                                                                                                                                                |
|----------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| lineItemNumber       | int                                       | Each line item in the collection gets a unique line number, counting up from 0 to count-1.                                                                                                                                                 |
| offerId              | string                                    | The ID of the offer.                                                                                                                                                                                                                       |
| subscriptionId       | string                                    | The ID of the subscription.                                                                                                                                                                                                                |
| parentSubscriptionId | string                                    | Optional. The ID of the parent subscription in an add-on offer. Applies to PATCH only.                                                                                                                                                     |
| friendlyName         | string                                    | Optional. The friendly name for the subscription defined by the partner to help disambiguate.                                                                                                                                              |
| quantity             | int                                       | The number of licenses for a license-based subscription or instances for an Azure reservation.                                                                                                                                                                                |
| partnerIdOnRecord    | string                                    | When an indirect provider places an order on behalf of an indirect reseller, populate this field with the MPN ID of the **indirect reseller only** (never the ID of the indirect provider). This ensures proper accounting for incentives. |
| provisioningContext  | Dictionary<string, string>                | Information required for provisioning for some items in the catalog. The provisioningVariables property in a SKU indicates which properties are required for specific items in the catalog.                                                                                                                                               |
| links                | [OrderLineItemLinks](#orderlineitemlinks) | Read-only. The resource links corresponding to the order line item.                                                                                                                                                                                |

 

## <span id="orderLinks"></span><span id="orderlinks"></span><span id="ORDERLINKS"></span>OrderLinks


Represents the resource links corresponding to the order.

| Property           | Type                                         | Description                                                                   |
|--------------------|----------------------------------------------|-------------------------------------------------------------------------------|
| provisioningStatus | [Link](utility-resources.md#Link)            | When populated, the link to retrieve provisioning status for the order.       |
| self               | [Link](utility-resources.md#Link)            | The link to retrieve the order resource.                                      |



## <span id="orderLineItemLinks"></span><span id="orderlineitemlinks"></span><span id="ORDERLINEITEMLINKS"></span>OrderLineItemLinks


Represents the full subscription associated with the order.

| Property           | Type                                         | Description                                                                          |
|--------------------|----------------------------------------------|--------------------------------------------------------------------------------------|
| provisioningStatus | [Link](utility-resources.md#Link)            | When populated, the link to retrieve the provisioning status of the line item.       |
| sku                | [Link](utility-resources.md#Link)            | The link to retrieve SKU information for the catalog item bought.                    |
| subscription       | [Link](utility-resources.md#Link)            | When populated, the link to the full subscription information.                       |

 

 
## <span id="orderStatus"></span><span id="orderstatus"></span><span id="ORDERSTATUS"></span>OrderStatus


An [Enum](https://docs.microsoft.com/en-us/dotnet/api/system.enum) with values that indicate the state of the order.

| Value              | Position     | Description                                     |
|--------------------|--------------|-------------------------------------------------|
| unknown            | 0            | Enum initializer.                               |
| completed          | 1            | Indicates that the order is completed.          |
| pending            | 2            | Indicates that the order is still pending.      |
| cancelled          | 3            | Indicates that the order has been cancelled.    |

 




