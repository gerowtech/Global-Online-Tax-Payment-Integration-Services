# Pakistan FBR Digital Invoicing API — PHP Example

A PHP cURL example for integrating with Pakistan's Federal Board of Revenue (FBR) Digital Invoicing API. This README demonstrates the request structure, Bearer-token authentication, invoice validation, invoice submission, and response handling.

> **Important:** API endpoints, payload schemas, reference values, credentials, and integration requirements can change. Confirm the current specification and your authorized integration configuration with FBR/PRAL before testing or deploying. The payload below is an illustrative template, not a guaranteed production-ready invoice.

## Top Countries — Digital Invoicing API Examples (PHP)

This project is intended as a country-by-country reference for integrating PHP applications with tax authority platforms and electronic invoicing networks. **There is no single universal government API:** each jurisdiction has its own protocol, schema, authentication method, and onboarding rules. Confirm the current official technical documentation before implementing an adapter.

### Country and API Overview

| Country / Region | Platform / Authority | Typical Integration Style | Official Documentation |
|---|---|---|---|
| Pakistan | FBR Digital Invoicing / PRAL | JSON REST API with Bearer token | [FBR Digital Invoicing API](https://www.fbr.gov.pk/technical-documentation-di/163085/173959) |
| Spain | AEAT VERI*FACTU | SOAP/XML web services; schema and certificate requirements apply | [AEAT VERI*FACTU Technical Information](https://sede.agenciatributaria.gob.es/Sede/iva/sistemas-informaticos-facturacion-verifactu/informacion-tecnica.html) |
| India | GST e-Invoice / Invoice Registration Portal (IRP) | JSON APIs; authorized registration and authentication required | [GST e-Invoice API Integration](https://einvoice6.gst.gov.in/content/api-integration/) |
| Egypt | Egyptian Tax Authority (ETA) eInvoicing/eReceipt | API/SDK integration using ETA-defined schemas and authentication | [ETA eInvoicing SDK](https://sdk.invoicing.eta.gov.eg/) |
| Saudi Arabia | ZATCA e-invoicing (FATOORA) | XML-based invoice compliance and clearance/reporting APIs; cryptographic onboarding requirements apply | [ZATCA Official Portal](https://zatca.gov.sa/en/E-Invoicing/Pages/default.aspx) |
| Italy | Agenzia delle Entrate — Sistema di Interscambio (SdI) | FatturaPA XML exchanged through supported channels; channel and authentication requirements vary | [Agenzia delle Entrate](https://www.agenziaentrate.gov.it/portale/) |
| Poland | National e-Invoicing System (KSeF) | Official KSeF API; use current API version and authentication rules | [Polish Ministry of Finance](https://www.podatki.gov.pl/ksef/) |
| European Union / Peppol countries | Peppol eDelivery network and national requirements | UBL XML exchanged through an accredited/access-point provider; country rules differ | [OpenPeppol](https://peppol.org/) |
| France | Public invoicing portal / approved platforms | Structured e-invoice formats and platform-based exchange; follow current national rollout requirements | [French Public Finance](https://www.impots.gouv.fr/) |
| Germany | E-invoicing / national and EU formats | Structured formats such as XRechnung or ZUGFeRD; transmission method depends on transaction and recipient | [German Federal Ministry of Finance](https://www.bundesfinanzministerium.de/) |

**Coverage note:** This table is a practical starting shortlist, not a ranking of market size or a claim that every authority offers a public API to every taxpayer. Some platforms require an approved provider, certificate, registration, or accredited integration channel.

### Suggested PHP Adapter Structure

Keep the application's business logic separate from each country's protocol. For example:

```text
src/
└── Invoicing/
    ├── Contracts/
    │   └── DigitalInvoicingProvider.php
    ├── Countries/
    │   ├── Pakistan/
    │   │   └── FbrDigitalInvoicing.php
    │   ├── Spain/
    │   │   └── AeatVerifactuClient.php
    │   ├── India/
    │   │   └── GstEInvoiceClient.php
    │   ├── Egypt/
    │   │   └── EtaEInvoiceClient.php
    │   └── SaudiArabia/
    │       └── ZatcaEInvoiceClient.php
    └── DTO/
        └── InvoiceSubmissionResult.php
```

A provider interface can normalize your application's workflow without pretending that each government API works the same way:

```php
<?php

declare(strict_types=1);

interface DigitalInvoicingProvider
{
    /**
     * Validate invoice data locally and/or through the authority,
     * according to the country's official workflow.
     */
    public function validate(array $invoice): array;

    /**
     * Submit an invoice using the country's official protocol.
     */
    public function submit(array $invoice): array;

    /**
     * Return a normalized status from stored data or an official
     * status endpoint, where the authority supports one.
     */
    public function getStatus(string $reference): array;
}
```

Not every jurisdiction offers a separate validation or status endpoint. Each adapter should implement only operations supported by that country's current documentation.

### PHP Integration Checklist for Every Country

1. Read the authority's current developer guide, API specification, WSDL, or XSD.
2. Confirm whether the connection is REST/JSON, SOAP/XML, Peppol, file exchange, or a provider-mediated integration.
3. Complete taxpayer registration, integrator onboarding, certificate setup, and sandbox access where required.
4. Obtain current reference data and validate the invoice against the country's required schema.
5. Implement authentication, TLS verification, timeouts, response parsing, and safe retry handling.
6. Save authority reference numbers, submission state, timestamps, and required audit information.
7. Prevent duplicate submissions; after a timeout, check the status before retrying when the platform supports it.
8. Test accepted, rejected, partially accepted, and malformed invoice cases in the official test environment.
9. Keep credentials and private keys server-side and redact sensitive information from logs.
10. Recheck the official rules before production releases because schemas, endpoints, and mandates can change.


## Official Documentation

- [FBR Digital Invoicing Technical Documentation](https://www.fbr.gov.pk/technical-documentation-di/163085/173959)
- [FBR Digital Invoicing Technical Assistance](https://fbr.gov.pk/di-technical-assistance/173967/173970)
- [FBR Digital Invoicing FAQs](https://www.fbr.gov.pk/faqs/173967/173969)

## Requirements

- PHP 8.0 or later
- PHP cURL extension enabled
- An FBR/PRAL-authorized integration and valid Bearer token
- Access to the relevant sandbox or production environment

Check PHP and cURL:

```bash
php -v
php -m
```

Ensure `curl` appears in the PHP module list.

## API Endpoints

The following endpoint patterns are commonly documented for FBR Digital Invoicing. Confirm them against the current official documentation and your integration onboarding details.

| Operation | Environment | Method | Endpoint |
|---|---|---|---|
| Validate invoice | Sandbox | POST | `https://gw.fbr.gov.pk/di_data/v1/di/validateinvoicedata_sb` |
| Submit invoice | Sandbox | POST | `https://gw.fbr.gov.pk/di_data/v1/di/postinvoicedata_sb` |
| Validate invoice | Production | POST | `https://gw.fbr.gov.pk/di_data/v1/di/validateinvoicedata` |
| Submit invoice | Production | POST | `https://gw.fbr.gov.pk/di_data/v1/di/postinvoicedata` |

## Authentication

FBR API requests use a Bearer token in the `Authorization` header.

Set the token in your server environment rather than hard-coding it:

```bash
# Linux/macOS example for the current shell
export FBR_API_TOKEN="YOUR_AUTHORIZED_FBR_TOKEN"
export FBR_ENVIRONMENT="sandbox"
```

On Windows PowerShell:

```powershell
$env:FBR_API_TOKEN = "YOUR_AUTHORIZED_FBR_TOKEN"
$env:FBR_ENVIRONMENT = "sandbox"
```

Never commit credentials to Git or expose them in frontend JavaScript.

## Project Structure

```text
fbr-php-example/
├── FbrDigitalInvoicing.php
├── example.php
├── .gitignore
└── README.md
```

Suggested `.gitignore`:

```gitignore
.env
.env.*
!.env.example
vendor/
```

## 1. PHP API Client

Create `FbrDigitalInvoicing.php`:

```php
<?php

declare(strict_types=1);

final class FbrDigitalInvoicing
{
    private string $token;
    private string $environment;

    private const BASE_URL = 'https://gw.fbr.gov.pk/di_data/v1/di/';

    public function __construct(string $token, string $environment = 'sandbox')
    {
        if (trim($token) === '') {
            throw new InvalidArgumentException('FBR API token is required.');
        }

        if (!in_array($environment, ['sandbox', 'production'], true)) {
            throw new InvalidArgumentException(
                'Environment must be sandbox or production.'
            );
        }

        $this->token = $token;
        $this->environment = $environment;
    }

    private function request(string $operation, array $invoice): array
    {
        $allowedOperations = [
            'validateinvoicedata',
            'postinvoicedata',
        ];

        if (!in_array($operation, $allowedOperations, true)) {
            throw new InvalidArgumentException('Invalid FBR API operation.');
        }

        $suffix = $this->environment === 'sandbox' ? '_sb' : '';
        $url = self::BASE_URL . $operation . $suffix;
        $payload = json_encode($invoice, JSON_THROW_ON_ERROR);

        $ch = curl_init($url);

        if ($ch === false) {
            throw new RuntimeException('Unable to initialize cURL.');
        }

        curl_setopt_array($ch, [
            CURLOPT_POST => true,
            CURLOPT_POSTFIELDS => $payload,
            CURLOPT_HTTPHEADER => [
                'Authorization: Bearer ' . $this->token,
                'Content-Type: application/json',
                'Accept: application/json',
            ],
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_CONNECTTIMEOUT => 10,
            CURLOPT_TIMEOUT => 30,
            CURLOPT_SSL_VERIFYPEER => true,
            CURLOPT_SSL_VERIFYHOST => 2,
        ]);

        $body = curl_exec($ch);
        $httpStatus = (int) curl_getinfo($ch, CURLINFO_HTTP_CODE);
        $curlError = curl_error($ch);
        curl_close($ch);

        if ($body === false) {
            throw new RuntimeException('FBR connection failed: ' . $curlError);
        }

        $decoded = json_decode($body, true);

        if (!is_array($decoded)) {
            throw new RuntimeException(
                'FBR returned a non-JSON response. HTTP status: ' . $httpStatus
            );
        }

        if ($httpStatus < 200 || $httpStatus >= 300) {
            // In production, log a redacted response and request/correlation ID.
            throw new RuntimeException(
                'FBR API returned HTTP ' . $httpStatus . ': ' . $body
            );
        }

        return $decoded;
    }

    public function validateInvoice(array $invoice): array
    {
        return $this->request('validateinvoicedata', $invoice);
    }

    public function submitInvoice(array $invoice): array
    {
        return $this->request('postinvoicedata', $invoice);
    }
}
```

## 2. Invoice Request Example

Create `example.php`:

```php
<?php

require_once __DIR__ . '/FbrDigitalInvoicing.php';

$token = getenv('FBR_API_TOKEN') ?: '';
$environment = getenv('FBR_ENVIRONMENT') ?: 'sandbox';

$fbr = new FbrDigitalInvoicing($token, $environment);

// Illustrative request only. Replace values and verify every field
// against the current FBR/PRAL invoice schema and reference data.
$invoice = [
    'invoiceType' => 'Sale Invoice',
    'invoiceDate' => date('Y-m-d'),
    'sellerNTNCNIC' => 'YOUR_SELLER_NTN_OR_CNIC',
    'sellerBusinessName' => 'Your Registered Business Name',
    'sellerProvince' => 'Punjab',
    'sellerAddress' => 'Lahore, Pakistan',
    'buyerNTNCNIC' => 'BUYER_NTN_OR_CNIC',
    'buyerBusinessName' => 'Buyer Business Name',
    'buyerProvince' => 'Punjab',
    'buyerAddress' => 'Lahore, Pakistan',
    'buyerRegistrationType' => 'Registered',
    'invoiceRefNo' => '',
    'scenarioId' => 'SN001',
    'items' => [
        [
            'hsCode' => '0101.2100',
            'productDescription' => 'Example product',
            'rate' => '18%',
            'uoM' => 'Numbers, pieces, units',
            'quantity' => 1,
            'totalValues' => 1180.00,
            'valueSalesExcludingST' => 1000.00,
            'fixedNotifiedValueOrRetailPrice' => 0.00,
            'salesTaxApplicable' => 180.00,
            'salesTaxWithheldAtSource' => 0.00,
            'extraTax' => 0.00,
            'furtherTax' => 0.00,
            'sroScheduleNo' => '',
            'fedPayable' => 0.00,
            'discount' => 0.00,
            'saleType' => 'Goods at standard rate (default)',
            'sroItemSerialNo' => '',
        ],
    ],
];

try {
    // First validate the invoice.
    $validation = $fbr->validateInvoice($invoice);

    echo "Validation response:\n";
    echo json_encode(
        $validation,
        JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES
    ) . PHP_EOL;

    // Check the real response fields according to the current API schema.
    $validationResponse = $validation['validationResponse'] ?? [];
    $status = strtolower((string) ($validationResponse['status'] ?? ''));
    $statusCode = (string) ($validationResponse['statusCode'] ?? '');

    if ($status === 'valid' && $statusCode === '00') {
        echo "Validation succeeded.\n";

        // Submit only when the validation response confirms acceptance.
        // Uncomment after confirming the payload and integration settings:
        //
        // $submission = $fbr->submitInvoice($invoice);
        // echo json_encode(
        //     $submission,
        //     JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES
        // ) . PHP_EOL;
    } else {
        echo "Invoice validation did not return a confirmed valid status.\n";
    }
} catch (Throwable $e) {
    // Do not print secrets or sensitive taxpayer/customer data to users.
    error_log($e->getMessage());
    http_response_code(500);
    echo "FBR request failed. Check the server logs.";
}
```

### Before using the sample payload

- Replace sample taxpayer information with valid registered information.
- Use the current official schema for every required field.
- Get valid HS codes, units of measurement, sale types, tax rates, and scenario IDs from the appropriate official reference data.
- Do not assume the illustrative tax rate or product code is appropriate for your transaction.
- Do not submit an invoice to production until it has been tested and approved for your integration.

## 3. Example Validation Response

A successful validation response may have a structure similar to this. Exact fields and values depend on the API operation and current specification.

```json
{
  "validationResponse": {
    "statusCode": "00",
    "status": "Valid",
    "error": "",
    "invoiceStatuses": [
      {
        "itemSNo": "1",
        "statusCode": "00",
        "status": "Valid",
        "invoiceNo": "",
        "errorCode": "",
        "error": ""
      }
    ]
  }
}
```

## 4. Example Submission Response

A successful invoice-submission response may include an FBR-assigned invoice number and validation information. This is an illustrative response, not a real invoice.

```json
{
  "invoiceNumber": "FBR_ASSIGNED_INVOICE_NUMBER",
  "dated": "2026-10-26 12:30:00",
  "validationResponse": {
    "statusCode": "00",
    "status": "Valid",
    "error": "",
    "invoiceStatuses": [
      {
        "itemSNo": "1",
        "statusCode": "00",
        "status": "Valid",
        "invoiceNo": "FBR_ASSIGNED_INVOICE_NUMBER-1",
        "errorCode": "",
        "error": ""
      }
    ]
  }
}
```

## 5. Example Error Response

An invalid invoice may return field or item-level validation errors. The following is only an illustrative shape; use the current official schema to parse real responses.

```json
{
  "validationResponse": {
    "statusCode": "01",
    "status": "Invalid",
    "error": "Invoice validation failed",
    "invoiceStatuses": [
      {
        "itemSNo": "1",
        "statusCode": "01",
        "status": "Invalid",
        "invoiceNo": "",
        "errorCode": "INVALID_FIELD",
        "error": "Check the submitted invoice fields."
      }
    ]
  }
}
```

## 6. Run the Example

Set `FBR_API_TOKEN` and `FBR_ENVIRONMENT` in the shell, then run:

```bash
php example.php
```

The script validates the sample invoice and prints the returned JSON. Submission is intentionally commented out until you confirm the validation response and payload are correct.

## Production Checklist

- Confirm taxpayer eligibility, onboarding, and any applicable licensed-integrator requirements.
- Use the official current API specification and authorized credentials.
- Keep tokens and certificates secret and server-side.
- Validate invoice data locally before sending it.
- Store submission status, FBR invoice number, timestamps, and necessary audit metadata.
- Redact personal/tax data and credentials from logs.
- Implement idempotency and duplicate-submission protection where supported.
- If a request times out, check the submission status before retrying to avoid duplicate invoices.
- Use TLS certificate verification and bounded timeouts.
- Test with authorized sandbox credentials before production deployment.

## License and Disclaimer

This repository example is provided for development and integration guidance. It is not an official FBR library and does not constitute tax or legal advice. You are responsible for verifying the current FBR/PRAL requirements and obtaining the required integration authorization.

---

Maintained by **GEROW TECH**.

- Website: https://gerowtech.com/
- WhatsApp: https://wa.me/923215348101
- Email: contact@gerowtech.com

## Spain — VERI*FACTU Digital Invoicing API (PHP cURL)

Spain's Agencia Estatal de Administración Tributaria (AEAT) VERI*FACTU service uses SOAP/XML rather than a typical JSON REST API. Requests must follow the current AEAT WSDL/XSD schemas and integration requirements.

### Official Documentation

- [AEAT VERI*FACTU Technical Information](https://sede.agenciatributaria.gob.es/Sede/iva/sistemas-informaticos-facturacion-verifactu/informacion-tecnica.html)
- [AEAT Web Services / WSDL Information](https://www3.agenciatributaria.gob.es/Sede/iva/sistemas-informaticos-facturacion-verifactu/informacion-tecnica/wsdl-servicios-web.html)
- [VERI*FACTU Web Service Description (PDF)](https://sede.agenciatributaria.gob.es/static_files/AEAT_Desarrolladores/EEDD/IVA/VERI-FACTU/Veri-Factu_Descripcion_SWeb.pdf)

### Endpoint Configuration

Use the service address specified by the current AEAT technical documentation for your environment. Do not assume a URL copied from an old example is still valid.

### PHP cURL SOAP Request Template

This example demonstrates the transport layer only. The SOAP body below is a placeholder: replace it with a complete, schema-valid request generated from the current AEAT WSDL/XSD and VERI*FACTU specification. Do not send the placeholder to production.

```php
<?php

declare(strict_types=1);

$url = getenv('AEAT_VERIFACTU_URL') ?: '';
$certificate = getenv('AEAT_CLIENT_CERT') ?: '';
$privateKey = getenv('AEAT_CLIENT_KEY') ?: '';

if ($url === '' || $certificate === '' || $privateKey === '') {
    throw new RuntimeException(
        'Configure AEAT_VERIFACTU_URL, AEAT_CLIENT_CERT and AEAT_CLIENT_KEY.'
    );
}

// Replace with a complete request generated according to the official AEAT XSD.
// The empty body element below is NOT a valid invoice submission.
$soapXml = <<<'XML'
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Header/>
    <soapenv:Body>
        <!-- Insert schema-valid VERI*FACTU request XML here. -->
    </soapenv:Body>
</soapenv:Envelope>
XML;

$ch = curl_init($url);

curl_setopt_array($ch, [
    CURLOPT_POST => true,
    CURLOPT_POSTFIELDS => $soapXml,
    CURLOPT_HTTPHEADER => [
        'Content-Type: text/xml; charset=UTF-8',
        'Accept: text/xml',
    ],
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_CONNECTTIMEOUT => 15,
    CURLOPT_TIMEOUT => 60,
    CURLOPT_SSLCERT => $certificate,
    CURLOPT_SSLKEY => $privateKey,
    CURLOPT_SSL_VERIFYPEER => true,
    CURLOPT_SSL_VERIFYHOST => 2,
]);

$response = curl_exec($ch);
$httpStatus = (int) curl_getinfo($ch, CURLINFO_HTTP_CODE);

if ($response === false) {
    $error = curl_error($ch);
    curl_close($ch);
    throw new RuntimeException('AEAT connection failed: ' . $error);
}

curl_close($ch);

echo "HTTP status: {$httpStatus}\n";
echo $response . PHP_EOL;
```

Configure the endpoint and certificate paths in the server environment. Never commit private keys, certificates, or passwords to source control. Depending on the certificate format and AEAT requirements, additional client-certificate configuration may be necessary.

### VERI*FACTU Implementation Requirements

- Build the SOAP envelope and invoice records from the current official WSDL/XSD schemas.
- Implement the required invoice identification, tax details, timestamps, software-system information, and hash-chain fields.
- Validate XML locally before sending.
- Parse the complete SOAP response, including submission-level and individual-record results.
- Handle acceptance, partial acceptance, rejection, timeouts, and duplicate-submission scenarios.
- Test in the authorized AEAT test environment before production.
- Follow the applicable Spanish invoicing rules and AEAT integration requirements.

**Important:** This is a PHP cURL transport template, not a complete working VERI*FACTU implementation. The request body, certificate setup, endpoint, hashing, and response parsing must be completed and verified against the current official AEAT specification.

