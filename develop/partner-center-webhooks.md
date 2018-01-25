---
title: Partner Center webhooks
description: Webhooks allow partners to register for resource change events. 
ms.assetid: 
ms.author: v-thpr
ms.date: 01/22/2018
ms.topic: article
ms.prod: partner-center
ms.technology: partner-center-sdk
---

# Partner Center webhooks


**Applies To**

-   Partner Center

The Partner Center Webhook APIs allow partners to register for resource change events. These events are delivered in the form of HTTP POSTs to the partner’s registered URL. To receive an event from Partner Center, partners will host a callback where Partner Center can POST the resource change event. The event will be digitally signed so that the partner can verify that itt was sent from Partner Center. 

Partners can select from the following Webhook events supported by Partner Center. 

-   **Test Event ("test-created")**

    This event allows you to self-onboard and test your registration by requesting a test event and then tracking its progress. You will be able to see the failure messages that are being received from Microsoft while trying to deliver the event. This will only apply to “test-created” events and data older than 7 days will be purged.

-   **Subscription Updated Event ("subscription-updated")**

    This event is raised when the subscription changes. These events will be generated when there is an internal change in addition to when changes are made through the Partner Center API. 

Future Webhook events will be added for resources that change in the system that the partner is not in control of, and further updates will be made to get those events as close to “real time” as possible. Feedback from Partners on which events add value to their business will be extremely useful in determing which new events to add. 

## <span id="receivingEvents"></span><span id="RECEIVINGEVENTS"></span>Receiving events from Partner Center


**Receiving events from Partner Center**

To receive events from Partner Center, you must expose a publicly accessible endpoint; and because this endpoint is exposed, you must validate that the communication is from Partner Center. All Webhook events that you receive are digitally signed with a certificate that chains to the Microsoft Root. A link to the certificate used to sign the event will also be provided. This will allow the certificate to be renewed without you having to re-deploy or re-configure your service. Partner Center will make 10 attempts to deliver the event. If the event is still not delivered after 10 attempts, it will me moved into an offline queue and no further attempts will be made at delivery. 

The following sample shows an event posted from Partner Center.

```
POST /webhooks/callback
content-type: application/json
authorization: Signature VOhcjRqA4f7u/4R29ohEzwRZibZdzfgG5/w4fHUnu8FHauBEVch8m2+5OgjLZRL33CIQpmqr2t0FsGF0UdmCR2OdY7rrAh/6QUW+u+jRUCV1s62M76jbVpTTGShmrANxnl8gz4LsbY260LAsDHufd6ab4oejerx1Ey9sFC+xwVTa+J4qGgeyIepeu4YCM0oB2RFS9rRB2F1s1OeAAPEhG7olp8B00Jss3PQrpLGOoAr5+fnQp8GOK8IdKF1/abUIyyvHxEjL76l7DVQN58pIJg4YC+pLs8pi6sTKvOdSVyCnjf+uYQWwmmWujSHfyU37j2Fzz16PJyWH41K8ZXJJkw==
x-ms-certificate-url: https://3psostorageacctppe.blob.core.windows.net/3pso/pcnotifications.microsoft-ppe.com.cer
x-ms-signature-algorithm: rsa-sha256
host: api.partnercenter.microsoft.com
accept-encoding: gzip, deflate
content-length: 195

{
    "EventName": "test-created",
    "ResourceUri": "http://localhost:16722/v1/webhooks/registration/test",
    "ResourceName": "test",
    "AuditUri": null,
    "ResourceChangeUtcDate": "2017-11-16T16:19:06.3520276+00:00"
} 
```

**Note** the Authorization header has a scheme of “Signature”. This is a base64 encoded signature of the content.

## <span id="AuthenticateCallback"></span><span id="authenticatecallback"></span><span id="AUTHENTICATECALLBACK"></span>How to authenticate the callback


To authenticate the callback event received from Partner Center, do the following:

1.	Verify the required headers are present (Authorization, x-ms-certificate-url, x-ms-signature-algorithm).
2.	Download the certificate used to sign the content (x-ms-certificate-url).
3.	Verify the Certificate Chain.
4.	Verify the “Organization” of the certificate.
5.	Read the content with UTF8 encoding into a buffer.
6.	Create an RSA Crypto Provider.
7.	Verify the data matches what was signed with the specified hash algorithm (e.g. SHA256).
8.	If the verification succeeds, process the message.

## <span id="EventModel"></span><span id="eventmodel"></span><span id="EVENTMODEL"></span>Event model


The following table describes the properties of a Partner Center event.

**Properties**

| Name                      | Description                                                                           |
|---------------------------|---------------------------------------------------------------------------------------|
| **EventName**             | The name of the event. In the form {resource}-{action}. For example, "test-created".  |
| **ResourceUri**           | The URI of the resource that changed.                                                 |
| **ResourceName**          | The name of the resource that changed.                                                |
| **AuditUrl**              | Optional. The URI of the Audit record.                                                |
| **ResourceChangeUtcDate** | The date and time, in UTC format, when the resource change occurred.                  |


**Sample**

The following sample shows the structure of a Partner Center event.

```
{
    "EventName": "test-created",
    "ResourceUri": "http://api.partnercenter.microsoft.com/webhooks/v1/registration/validationEvents/c0bfd694-3075-4ec5-9a3c-733d3a890a1f",
    "ResourceName": "test",
    "AuditUri": null,
    "ResourceChangeUtcDate": "2017-11-16T16:19:06.3520276+00:00"
}
```


## <span id="WebhookAPI"></span><span id="webhookapi"></span><span id="WEBHOOKAPI"></span>Webhook APIs   


### <span id="Authentication"></span><span id="authentication"></span><span id="AUTHENTICATION"></span>Authentication   

All calls to the Webhook API are authenticated using the Bearer token in the Authorization Header. You must acquire an access token to access “https://api.partnercenter.microsoft.com”. This is the same token that is used to access the rest of the Partner Center APIs.

**Get a list of events**   
Returns a list of the events that are currently supported by the Webhook API.

**Resource URL**   
https://api.partnercenter.microsoft.com/webhooks/v1/registration/events

**Request example**   

```
GET /webhooks/v1/registration/events
content-type: application/json
authorization: Bearer eyJ0e…….
accept: */*
host: api.partnercenter.microsoft.com
```

**Response example**   

```
HTTP/1.1 200
status: 200
content-length: 183
content-type: application/json; charset=utf-8
content-encoding: gzip
vary: Accept-Encoding
ms-correlationid: c0bcf3a3-46e9-48fd-8e05-f674b8fd5d66
ms-requestid: 79419bbb-06ee-48da-8221-e09480537dfc
x-locale: en-US
 
[ "subscription-updated", "test-created" ]
```



### <span id="Registration"></span><span id="registration"></span><span id="REGISTRATION"></span>Register to receive events      

Registers a tenant to receive the specified events.

**Resource URL**   
https://api.partnercenter.microsoft.com/webhooks/v1/registration


**Request example**   

```
POST /webhooks/v1/registration
content-type: application/json
ms-debug: true
authorization: Bearer eyJ0e…..
accept: */*
host: api.partnercenter.microsoft.com
accept-encoding: gzip, deflate
content-length: 219
 
{
    "WebhookUrl": "{{YourCallbackUrl}}",
    "WebhookEvents": ["subscription-updated", "test-created"]
}
```

**Response example**   

```
HTTP/1.1 200
status: 200
content-length: 183
content-type: application/json; charset=utf-8
content-encoding: gzip
vary: Accept-Encoding
ms-correlationid: c0bcf3a3-46e9-48fd-8e05-f674b8fd5d66
ms-requestid: 79419bbb-06ee-48da-8221-e09480537dfc
x-locale: en-US
 
[ "subscription-updated", "test-created" ]
```



### <span id="ViewRegistration"></span><span id="viewregistration"></span><span id="VIEWREGISTRATION"></span>View a registration        

Returns the Webhooks event registration for a tenant.

**Resource URL**   
https://api.partnercenter.microsoft.com/webhooks/v1/registration


**Request example**   

```
GET /webhooks/v1/registration
content-type: application/json
authorization: Bearer …
accept: */*
host: api.partnercenter.microsoft.com
accept-encoding: gzip, deflate
```

**Response example**   

```
HTTP/1.1 200
status: 200
content-length: 341
content-type: application/json; charset=utf-8
content-encoding: gzip
vary: Accept-Encoding
ms-correlationid: c3b88ab0-b7bc-48d6-8c55-4ae6200f490a
ms-requestid: ca30367d-4b24-4516-af08-74bba6dc6657
x-locale: en-US

{
    "WebhookUrl": "{{YourCallbackUrl}}",
    "WebhookEvents": ["subscription-updated", "test-created"]
}
```



### <span id="ValidationEvents"></span><span id="validationevents"></span><span id="VALIDATIONEVENTS"></span>Send a test event to validate your registration   

Generates a test event to validate the Webhooks registration. This test is intended to validate that you can receive events from Partner Center. Data for these events will be deleted 7 days after the initial event is created. You must be registered for the “test-created” event, using the registration API, before sending a validation event.

**Resource URL**   
https://api.partnercenter.microsoft.com/webhooks/v1/registration/validationEvents

**Request example**   

```
POST /webhooks/v1/registration/validationEvents
ms-correlationid: 3ef0202b-9d00-4f75-9cff-15420f7612b3
authorization: Bearer …
accept: */*
host: api.partnercenter.microsoft.com
accept-encoding: gzip, deflate
content-length:
```

**Response example**   

```
HTTP/1.1 200
status: 200
content-length: 181
content-type: application/json; charset=utf-8
content-encoding: gzip
vary: Accept-Encoding
ms-correlationid: 04af2aea-d413-42db-824e-f328001484d1
ms-requestid: 2f498d5a-a6ab-468f-98d8-93c96da09051
x-locale: en-US

{ "correlationId": "04af2aea-d413-42db-824e-f328001484d1" }
```



### <span id="GetValidation"></span><span id="getvalidation"></span><span id="GETVALIDATION"></span>Verify that the event was delivered   

Returns the current state of the validation event. This can be helpful for trouble shooting event delivery issues. The Response contains a result for each attempt that is made to deliver the event.

**Resource URL**   
https://api.partnercenter.microsoft.com/webhooks/v1/registration/validationEvents/{correlationId}

**Request example**   

```
GET /webhooks/v1/registration/validationEvents/04af2aea-d413-42db-824e-f328001484d1
ms-correlationid: 3ef0202b-9d00-4f75-9cff-15420f7612b3
authorization: Bearer …
accept: */*
host: api.partnercenter.microsoft.com
accept-encoding: gzip, deflate
```

**Response example**     

```
HTTP/1.1 200
status: 200
content-length: 469
content-type: application/json; charset=utf-8
content-encoding: gzip
vary: Accept-Encoding
ms-correlationid: 497e0a23-9498-4d6c-bd6a-bc4d6d0054e7
ms-requestid: 0843bdb2-113a-4926-a51c-284aa01d722e
x-locale: en-US

{
    "correlationId": "04af2aea-d413-42db-824e-f328001484d1",
    "partnerId": "00234d9d-8c2d-4ff5-8c18-39f8afc6f7f3",
    "status": "completed",
    "callbackUrl": "{{YourCallbackUrl}}",
    "results": [{
        "responseCode": "OK",
        "responseMessage": "",
        "systemError": false,
        "dateTimeUtc": "2017-12-08T21:39:48.2386997"
    }]
}
```


## <span id="SignatureValidationExample"></span><span id="signaturevalidationexample"></span><span id="SIGNATUREVALIDATIONEXAMPLE"></span>Example for Signature Validation


**Sample Callback Controller signature (ASP.NET)**     

```csharp
[AuthorizeSignature]
[Route("webhooks/callback")]
public IHttpActionResult Post(PartnerResourceChangeCallBack callback)
```

**Signature Validation**      
The following example shows how to add an Authorization Attribute to the controller that is receiving callbacks from Webhook events.

```csharp
namespace Webhooks.Security
{
    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.IO;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Security.Cryptography;
    using System.Security.Cryptography.X509Certificates;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Web.Http;
    using System.Web.Http.Controllers;
    using Microsoft.Partner.Logging;

    /// <summary>
    /// Signature based Authorization
    /// </summary>
    public class AuthorizeSignatureAttribute : AuthorizeAttribute
    {
        private const string CertificateUrlHeader = "x-ms-certificate-url";
        private const string SignatureAlgorithmHeader = "x-ms-signature-algorithm";

        /// <inheritdoc/>
        public override async Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            ValidateAuthorizationHeaders(actionContext.Request);

            await VerifySignature(actionContext.Request);
        }

        private static async Task<string> GetContentAsync(HttpRequestMessage request)
        {
            // By default the stream can only be read once and we need to read it here so that we can hash the body to validate the signature from microsoft.
            // Load into a buffer, so that the stream can be accessed here and in the api when it binds the content to the expected model type.
            await request.Content.LoadIntoBufferAsync();

            var s = await request.Content.ReadAsStreamAsync();
            var reader = new StreamReader(s);
            var body = await reader.ReadToEndAsync();

            // set the stream position back to the beginning
            if (s.CanSeek)
            {
                s.Seek(0, SeekOrigin.Begin);
            }

            return body;
        }

        private static void ValidateAuthorizationHeaders(HttpRequestMessage request)
        {
            var authHeader = request.Headers.Authorization;
            if (authHeader == null || string.IsNullOrEmpty(authHeader.Parameter))
            {
                throw new HttpResponseException(request.CreateErrorResponse(HttpStatusCode.Unauthorized, "Authorization Header missing"));
            }

            if (authHeader.Scheme != "Signature")
            {
                throw new HttpResponseException(request.CreateErrorResponse(HttpStatusCode.Unauthorized, "Authorization Scheme needs to be 'Signature'"));
            }

            if (string.IsNullOrEmpty(GetHeaderValue(request.Headers, CertificateUrlHeader)))
            {
                throw new HttpResponseException(request.CreateErrorResponse(HttpStatusCode.BadRequest, $"Request header {CertificateUrlHeader} missing"));
            }

            if (string.IsNullOrEmpty(GetHeaderValue(request.Headers, SignatureAlgorithmHeader)))
            {
                throw new HttpResponseException(request.CreateErrorResponse(HttpStatusCode.BadRequest, $"Request header {SignatureAlgorithmHeader} missing"));
            }
        }

        private static string GetHeaderValue(HttpRequestHeaders headers, string key)
        {
            headers.TryGetValues(key, out IEnumerable<string> headerValues);

            return headerValues?.FirstOrDefault();
        }

        private static async Task VerifySignature(HttpRequestMessage request)
        {
            var base64Signture = request.Headers.Authorization?.Parameter;
            var signatureAlgorithm = GetHeaderValue(request.Headers, SignatureAlgorithmHeader);
            var certificateUrl = GetHeaderValue(request.Headers, CertificateUrlHeader);
            var certificate = await GetCertificate(certificateUrl);
            var content = await GetContentAsync(request);
            var alg = signatureAlgorithm.Split('-');    // e.g. RSA-SHA1
            var isValid = false;

            // Validate the certificate
            if (VerifyCertificate(certificate))
            {
                if (alg.Length == 2 && alg[0].Equals("RSA", StringComparison.OrdinalIgnoreCase))
                {
                    byte[] signature = Convert.FromBase64String(base64Signture);

                    RSACryptoServiceProvider csp = (RSACryptoServiceProvider)certificate.PublicKey.Key;

                    var encoding = new UTF8Encoding();
                    byte[] data = encoding.GetBytes(content);

                    string hashAlgorithm = alg[1].ToUpper();

                    isValid = csp.VerifyData(data, CryptoConfig.MapNameToOID(hashAlgorithm), signature);
                }

                if (!isValid)
                {
                    // log that we were not able to validate the signature
                    Trace.WriteLine($"Failed to validate signature for webhook callback. base64Signature: {base64Signture}, certificateUrl: {certificateUrl}, signatureAlgorithm: {signatureAlgorithm}");

                    var logger = GetLoggerIfAvailable(request);
                    logger?.TrackTrace("Failed to validate Signature for webhook callback", new Dictionary<string, string> { { "base64Signature", base64Signture }, { "certificateUrl", certificateUrl }, { "signatureAlgorithm", signatureAlgorithm }, { "content", content } });

                    throw new HttpResponseException(request.CreateErrorResponse(HttpStatusCode.Unauthorized, "Signature verification failed"));
                }
            }
        }

        private static ILogger GetLoggerIfAvailable(HttpRequestMessage request)
        {
            return request.GetDependencyScope().GetService(typeof(ILogger)) as ILogger;
        }

        private static async Task<X509Certificate2> GetCertificate(string certificateUrl)
        {
            byte[] certBytes;
            using (var webClient = new WebClient())
            {
                certBytes = await webClient.DownloadDataTaskAsync(certificateUrl);
            }

            return new X509Certificate2(certBytes);
        }

        private static bool VerifyCertificate(X509Certificate2 certificate)
        {
            return certificate.Verify() && certificate.Issuer.Contains("O=Microsoft Corporation");
        }
    }
}
```
