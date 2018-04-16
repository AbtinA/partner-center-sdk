---
title: Partner Center
description: The Partner Center SDK is designed for partners in the Cloud Solution Provider program.
ms.assetid: 0F976995-22BC-4B95-A3C7-DA5552D9BE81
ms.author: v-thpr
ms.date: 2/26/2018
ms.topic: article
ms.prod: partner-center
ms.technology: partner-center-sdk
---

# Partner Center


**Applies To**

-   Partner Center
-   Partner Center operated by 21Vianet
-   Partner Center for Microsoft Cloud Germany

The Partner Center SDK is designed for partners in the Cloud Solution Provider program. With it, you can programmatically manage your customer accounts, place orders, manage subscriptions, and find support. Indirect partners can also manage their channel. It enables you to customize your existing customer relationship management or billing tools to retrieve or submit data about subscriptions. Employees at your company can work in their typical environment rather than signing in to [Partner Center](http://go.microsoft.com/fwlink/p/?LinkId=620294) and duplicating data entry and other tasks.

The Partner Center SDK includes a managed API library, a REST API, a comprehensive sample app written in C#, and this documentation. The Partner Center SDK is a superset of the existing CREST API and includes a Managed API that features improved token management and a simple interface library for network calls with retries.

**Note**  
There are different versions of Partner Center for specific regions, countries, or government agencies. App developers need to understand the different SDK capabilities for these versions. For more information, see [Developing for Partner Center for Microsoft National Cloud](developing-for-partner-center-for-microsoft-national-cloud.md).   

 

## <span id="In_this_section"></span><span id="in_this_section"></span><span id="IN_THIS_SECTION"></span>In this section


-   [Get started](get-started.md)

    Get the SDK download and code samples. Get authenticated for both test and production accounts.

-   [Scenarios](scenarios.md)

    Get descriptions, C# code snippets, and REST request and response samples.

-   [Partner Center REST API reference](partner-center-rest-api-reference.md)

    API reference documentation for the Partner Center REST headers, resources, events, and error messages.

-   [Partner Center Managed API reference](https://docs.microsoft.com/en-us/dotnet/api/?branch=master)

    API reference documentation for the Partner Center namespaces, classes and interfaces.

## <span id="Other_resources_for_Cloud_Solution_Providers"></span><span id="other_resources_for_cloud_solution_providers"></span><span id="OTHER_RESOURCES_FOR_CLOUD_SOLUTION_PROVIDERS"></span>Other resources for Cloud Solution Providers


Depending on your existing tools and solutions, you may find the following APIs useful for managing accounts, permissions and IDs, orders, and subscriptions.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>CREST Commerce APIs (see [CREST support and deprecation](https://msdn.microsoft.com/library/partnercenter/mt844538.aspx))</p></td>
<td><p>[Documentation](http://msdn.microsoft.com/en-us/library/partnercenter/dn974944.aspx)</p>
<p>[CREST forums]( http://go.microsoft.com/fwlink/p/?LinkId=617103)</p>
<p>[Sample program - C#](http://go.microsoft.com/fwlink/p/?LinkId=620291)</p>
<p>[Sample program - Java](http://go.microsoft.com/fwlink/p/?LinkId=624059)</p>
<p>[Intro to CREST]( http://go.microsoft.com/fwlink/p/?LinkId=717361) [video]</p></td>
</tr>
<tr class="even">
<td><p>Azure AD Graph APIs</p></td>
<td><p>[Documentation]( http://go.microsoft.com/fwlink/p/?LinkId=717363)</p>
<p>[Azure AD Graph forums]( http://go.microsoft.com/fwlink/p/?LinkId=717364)</p>
<p>[Sample code]( http://go.microsoft.com/fwlink/p/?LinkId=717365)</p></td>
</tr>
<tr class="odd">
<td><p>Office 365 APIs</p></td>
<td><p>[Documentation]( http://go.microsoft.com/fwlink/p/?LinkId=717362)</p></td>
</tr>
</tbody>
</table>

 

Office 365 and Microsoft Azure each provide an API that partners can use to retrieve real-time service health, message center communications, and planned maintenance events. These APIs are publicly available, and partners can use them on behalf of their customers when they have delegated admin privileges.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Office 365 service communications API</p></td>
<td><p>[Documentation](http://go.microsoft.com/fwlink/p/?LinkId=616899)</p></td>
</tr>
<tr class="even">
<td><p>Azure Insights REST API</p></td>
<td><p>[Documentation](http://go.microsoft.com/fwlink/p/?LinkId=617300)</p>
<p>[Sample code](http://go.microsoft.com/fwlink/p/?LinkId=617299)</p></td>
</tr>
</tbody>
</table>

 

 

 




