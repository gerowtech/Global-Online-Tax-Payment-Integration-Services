# Digital Invoicing API — PHP Examples

A PHP integration reference for digital invoicing, electronic invoice validation, submission, and tax compliance across multiple countries.

**Developed by GEROW TECH LLC**

## Supported Countries and Platforms

| Country / Region | Authority / Platform | Integration notes |
|---|---|---|
| 🇵🇰 Pakistan | FBR / PRAL Digital Invoicing | Commonly REST/JSON; use authorized API endpoints and token authentication |
| 🇪🇸 Spain | AEAT VERI*FACTU | Follow AEAT invoice-record specifications and applicable service interface |
| 🇪🇬 Egypt | Egyptian Tax Authority (ETA) | Structured e-invoice data and API/SDK integration |
| 🇸🇦 Saudi Arabia | ZATCA FATOORA | UBL-based XML, cryptographic controls, and clearance/reporting APIs |
| 🇮🇹 Italy | Sistema di Interscambio (SdI) | FatturaPA XML and approved submission channels |
| 🇵🇱 Poland | KSeF | Structured invoice schema and KSeF APIs |
| 🇪🇺 European Union | Peppol | UBL invoice documents exchanged through a Peppol access point |
| 🇫🇷 France | Electronic invoicing platforms | Structured invoice formats and platform-based exchange |
| 🇩🇪 Germany | XRechnung / ZUGFeRD | Structured e-invoice formats and recipient-specific delivery channels |

## Digital Invoicing API PHP Examples

This project provides PHP examples for integrating with digital invoicing platforms across the countries and regions listed above. It includes illustrative API requests and responses, JSON and SOAP/XML patterns, cURL integration, and country-specific implementation notes.

**Project scope:**
- PHP API request and response examples
- JSON and XML payload examples
- REST API and SOAP/XML integration patterns where supported
- Invoice validation and submission workflow guidance
- Authentication, error handling, and security recommendations
- Links to official technical documentation

> Examples are illustrative unless explicitly stated otherwise. Always use each provider's current official API specification, schemas, endpoints, and authentication requirements in production.


> **Important:** These platforms do not all use SOAP/XML APIs. Depending on the country and platform, integrations may use REST/JSON, SOAP web services, structured XML files, or a service-provider network. Examples in this repository are illustrative unless explicitly identified as official schema-compliant payloads. Always follow current official documentation.

## Features

- PHP examples for invoice integration workflows
- Illustrative request and response structures
- XML and JSON integration patterns
- cURL-based HTTP requests
- API authentication placeholders
- Error handling and response inspection
- Country-specific implementation notes
- Official documentation links

## Requirements

- PHP 8.0 or later recommended
- PHP cURL extension enabled
- Sandbox credentials for the selected provider
- Current official API documentation and schemas
- Client certificates where required by the provider

Check PHP and cURL:

```bash
php -v
php -m
```

## Suggested Project Structure

```text
digital-invoicing-api-php/
├── README.md
├── examples/
│   ├── pakistan-fbr/
│   │   ├── validate-invoice.php
│   │   └── submit-invoice.php
│   ├── spain-verifactu/
│   │   ├── invoice-request.xml
│   │   └── submit-record.php
│   ├── egypt-eta/
│   ├── saudi-zatca/
│   ├── italy-sdi/
│   ├── poland-ksef/
│   ├── eu-peppol/
│   ├── france-einvoicing/
│   └── germany-xrechnung/
├── src/
│   ├── HttpClient.php
│   └── InvoiceProviderInterface.php
├── config/
│   └── config.example.php
└── .gitignore
```

## Shared PHP cURL Client Example

This generic helper sends a request to an HTTP API. Configure the endpoint, authentication, headers, and body according to the selected provider's official specification.

```php
<?php

declare(strict_types=1);

/**
 * Send an HTTP request to an invoicing API.
 *
 * Do not hard-code real credentials in source code.
 */
function sendApiRequest(
    string $url,
    string $method,
    array $headers = [],
    ?string $body = null
): array {
    $ch = curl_init($url);

    curl_setopt_array($ch, [
        CURLOPT_CUSTOMREQUEST => strtoupper($method),
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_HTTPHEADER => $headers,
        CURLOPT_POSTFIELDS => $body,
        CURLOPT_CONNECTTIMEOUT => 10,
        CURLOPT_TIMEOUT => 30,
    ]);

    $responseBody = curl_exec($ch);
    $httpStatus = (int) curl_getinfo($ch, CURLINFO_HTTP_CODE);

    if ($responseBody === false) {
        $error = curl_error($ch);
        curl_close($ch);

        return [
            'http_status' => 0,
            'error' => $error,
            'body' => null,
        ];
    }

    curl_close($ch);

    return [
        'http_status' => $httpStatus,
        'error' => null,
        'body' => $responseBody,
    ];
}
```

## Example: JSON API Request

This demonstrates the general pattern for a REST/JSON API. The URL, fields, token, and response schema are placeholders; this is not a universal payload for all countries.

```php
<?php

$apiUrl = 'https://api.example.com/invoices';
$token = getenv('INVOICING_API_TOKEN') ?: '';

$invoice = [
    'invoiceNumber' => 'INV-1001',
    'invoiceDate' => date('Y-m-d'),
    'currency' => 'EUR',
    'netAmount' => 100.00,
    'taxAmount' => 21.00,
    'totalAmount' => 121.00,
];

$result = sendApiRequest(
    $apiUrl,
    'POST',
    [
        'Authorization: Bearer ' . $token,
        'Content-Type: application/json',
        'Accept: application/json',
    ],
    json_encode($invoice, JSON_THROW_ON_ERROR)
);

echo 'HTTP status: ' . $result['http_status'] . PHP_EOL;
echo $result['body'] ?? ($result['error'] ?? 'No response');
```

### Illustrative JSON Request

```json
{
  "invoiceNumber": "INV-1001",
  "invoiceDate": "2026-10-26",
  "currency": "EUR",
  "netAmount": 100.00,
  "taxAmount": 21.00,
  "totalAmount": 121.00
}
```

### Illustrative JSON Response

```json
{
  "status": "accepted",
  "invoiceNumber": "INV-1001",
  "reference": "DEMO-REFERENCE-1001",
  "message": "Illustrative response only"
}
```

## Example: SOAP/XML Request Pattern

Use this pattern only for a provider that officially exposes a SOAP service. The XML body, namespaces, SOAP action, certificate requirements, and response schema must match that provider's WSDL/XSD.

```php
<?php

$endpoint = 'YOUR_OFFICIAL_SOAP_ENDPOINT';
$soapAction = 'YOUR_DOCUMENTED_SOAP_ACTION';

$xmlRequest = <<<'XML'
<?xml version="1.0" encoding="UTF-8"?>
<ReplaceWithOfficialRequestRoot>
    <!-- Insert fields defined by the provider's official XSD. -->
</ReplaceWithOfficialRequestRoot>
XML;

$ch = curl_init($endpoint);

curl_setopt_array($ch, [
    CURLOPT_POST => true,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => [
        'Content-Type: text/xml; charset=UTF-8',
        'SOAPAction: "' . $soapAction . '"',
    ],
    CURLOPT_POSTFIELDS => $xmlRequest,
    CURLOPT_CONNECTTIMEOUT => 10,
    CURLOPT_TIMEOUT => 30,
]);

$response = curl_exec($ch);
$httpStatus = (int) curl_getinfo($ch, CURLINFO_HTTP_CODE);

if ($response === false) {
    error_log('SOAP request failed: ' . curl_error($ch));
} else {
    echo 'HTTP status: ' . $httpStatus . PHP_EOL;
    echo $response;
}

curl_close($ch);
```

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceRequest>
    <InvoiceNumber>INV-1001</InvoiceNumber>
    <IssueDate>2026-10-26</IssueDate>
    <NetAmount>100.00</NetAmount>
    <TaxAmount>21.00</TaxAmount>
    <TotalAmount>121.00</TotalAmount>
</InvoiceRequest>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceResponse>
    <Status>Accepted</Status>
    <InvoiceNumber>INV-1001</InvoiceNumber>
    <Reference>DEMO-REFERENCE-1001</Reference>
    <Message>Illustrative response only</Message>
</InvoiceResponse>
```

The XML above is generic demonstration content, not an official schema for any country listed in this repository.

## Country-Specific Notes

### Pakistan — FBR / PRAL

- FBR/PRAL digital invoicing integrations commonly use REST/JSON APIs.
- Use the authorized API base URL, Bearer token, and required validation/submission endpoints supplied for your integration.
- Validate seller/buyer registration details, HS codes, tax rates, invoice type, and required scenario values.
- Do not send generic SOAP/XML examples to a REST/JSON endpoint.

Official website: https://www.fbr.gov.pk/

### Spain — AEAT VERI*FACTU

- Follow the current AEAT VERI*FACTU technical specifications.
- Invoice-record reporting requirements may include specific fields, hashes, and record-linking rules.
- Use the applicable official service interface, namespaces, schemas, and authentication.
- A generic XML envelope is not a valid VERI*FACTU submission.

Official website: https://sede.agenciatributaria.gob.es/

### Egypt — ETA eInvoicing

- Check the current ETA API and SDK documentation.
- Follow the required document structure, authentication, signing, and submission workflow.
- Do not assume the integration uses SOAP/XML.

Official website: https://www.eta.gov.eg/

### Saudi Arabia — ZATCA FATOORA

- Follow the applicable ZATCA e-invoicing XML/UBL specifications.
- Implement the required cryptographic controls and clearance/reporting workflow for the invoice type.
- A generic XML invoice is not a compliant ZATCA invoice.

Official website: https://zatca.gov.sa/

### Italy — SdI

- Use the official FatturaPA XML format and applicable SdI submission channel or authorized intermediary.
- A generic SOAP request is not a FatturaPA invoice.

Official website: https://www.agenziaentrate.gov.it/

### Poland — KSeF

- Follow the current KSeF API version, structured invoice schema, authentication requirements, and processing response format.

Official website: https://ksef.podatki.gov.pl/

### European Union — Peppol

- Peppol is a document-exchange network, not one EU tax-authority SOAP endpoint.
- Invoices commonly use UBL and are exchanged through a Peppol access point.
- Use the access point provider's documented API and the applicable Peppol specifications.

Official website: https://peppol.org/

### France — Electronic Invoicing

- Follow current French electronic invoicing requirements and the selected authorized platform's technical interface.
- Formats, routing, and API details depend on the platform and applicable regulations.

Official website: https://www.impots.gouv.fr/

### Germany — XRechnung / ZUGFeRD

- Follow the applicable XRechnung or ZUGFeRD specification and recipient delivery requirements.
- Generating an invoice file does not itself submit it to a government API.

Official website: https://www.xrechnung.de/

## Recommended Error Handling

1. Check the HTTP status code and response body.
2. Parse JSON responses as JSON and XML responses as XML.
3. Handle authentication, schema-validation, business-rule, and network errors separately.
4. Save the provider's submission reference and invoice status securely.
5. Retry only when permitted by the provider; avoid duplicate submissions.
6. Do not treat HTTP success alone as proof that an invoice has been accepted by the tax authority.

## Security

- Store tokens, passwords, and private keys in environment variables or a secret manager.
- Never commit live credentials or private certificates to Git.
- Use HTTPS and verify TLS certificates.
- Redact tax identifiers and sensitive invoice data from logs.
- Use sandbox/test credentials before production.
- Follow each authority's official documentation and legal requirements.

## Official Documentation

- Pakistan — FBR: https://www.fbr.gov.pk/
- Spain — AEAT: https://sede.agenciatributaria.gob.es/
- Egypt — ETA: https://www.eta.gov.eg/
- Saudi Arabia — ZATCA: https://zatca.gov.sa/
- Italy — Agenzia delle Entrate: https://www.agenziaentrate.gov.it/
- Poland — KSeF: https://ksef.podatki.gov.pl/
- European Union / Peppol: https://peppol.org/
- France — Impots.gouv.fr: https://www.impots.gouv.fr/
- Germany — XRechnung: https://www.xrechnung.de/

## GEROW TECH LLC

Digital invoicing API integration, PHP development, and multi-country software integration.

- Website: https://gerowtech.com/
- Email: contact@gerowtech.com
- WhatsApp: https://wa.me/923215348101

---

**Disclaimer:** This repository is an integration reference, not tax or legal advice. Examples are illustrative unless explicitly documented as conforming to an official specification. Verify the current country-specific technical requirements before using any example in production.
