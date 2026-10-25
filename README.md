# Digital Invoicing API — PHP SOAP/XML Request and Response Examples

This repository documents digital invoicing integration patterns for Pakistan, Spain, Egypt, Saudi Arabia, Italy, Poland, European Union / Peppol, France, and Germany.

> **Important:** These countries do not all use SOAP/XML for digital invoicing. Depending on the country and platform, integrations may use REST/JSON APIs, SOAP web services, structured XML files, or a service-provider network. The XML examples below are illustrative templates, not guaranteed production-ready payloads. Always use the current official endpoint, schema, authentication method, and technical specification.

## Supported Countries and Platforms

| Country / Region | Authority / Platform | Common format or integration pattern |
|---|---|---|
| Pakistan | FBR / PRAL Digital Invoicing | Commonly REST/JSON API |
| Spain | AEAT VERI*FACTU | Structured invoice records; follow the applicable AEAT interface and specification |
| Egypt | Egyptian Tax Authority (ETA) | Structured e-invoice data and API/SDK integration |
| Saudi Arabia | ZATCA FATOORA | UBL-based XML and country-specific clearance/reporting APIs |
| Italy | Sistema di Interscambio (SdI) | FatturaPA XML and approved submission channels |
| Poland | KSeF | Structured e-invoice schema and KSeF APIs |
| European Union / Peppol | Peppol eDelivery network | UBL invoice documents via a Peppol access point |
| France | French electronic invoicing framework | Structured formats and platform-based exchange |
| Germany | XRechnung / ZUGFeRD | Structured e-invoice formats and recipient-specific delivery channels |

---

## 1. Pakistan — FBR Digital Invoicing

**Authority:** Federal Board of Revenue (FBR) / PRAL

Pakistan's digital invoicing integration commonly uses REST APIs with JSON and Bearer-token authentication. The XML below is a generic illustration only; it is **not** an official FBR request format.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceRequest>
    <InvoiceNumber>PK-INV-1001</InvoiceNumber>
    <InvoiceDate>2026-10-26</InvoiceDate>
    <SellerNTN>YOUR_SELLER_NTN</SellerNTN>
    <BuyerNTN>BUYER_NTN</BuyerNTN>
    <TaxableAmount>10000.00</TaxableAmount>
    <SalesTax>1800.00</SalesTax>
    <TotalAmount>11800.00</TotalAmount>
</InvoiceRequest>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceResponse>
    <Status>Valid</Status>
    <InvoiceReference>DEMO-PK-1001</InvoiceReference>
    <Message>Illustrative response only</Message>
</InvoiceResponse>
```

**Production note:** Use the official FBR/PRAL REST/JSON request format, authentication, and authorized validation/submission endpoints. Do not send this illustrative XML to a standard FBR REST endpoint.

---

## 2. Spain — AEAT VERI*FACTU

**Authority:** Agencia Estatal de Administración Tributaria (AEAT)

VERI*FACTU concerns compliant invoicing software and structured invoice-record reporting. The actual interface and payload must follow the current AEAT technical specifications. The following XML is a placeholder, not an official AEAT payload.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceRecordRequest>
    <InvoiceNumber>ES-INV-1001</InvoiceNumber>
    <InvoiceDate>26-10-2026</InvoiceDate>
    <IssuerName>Example Company</IssuerName>
    <TaxBase>10000.00</TaxBase>
    <VATAmount>2100.00</VATAmount>
</InvoiceRecordRequest>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceRecordResponse>
    <Status>Accepted</Status>
    <InvoiceNumber>ES-INV-1001</InvoiceNumber>
    <Message>Illustrative response only</Message>
</InvoiceRecordResponse>
```

**Production note:** Use the current WSDL/XSD or other applicable official interface, required namespaces, invoice-record fields, hash-chain rules, authentication, and response schema.

---

## 3. Egypt — ETA eInvoicing

**Authority:** Egyptian Tax Authority (ETA)

Egypt's eInvoicing system uses structured invoice data and API-based integration. Confirm the current API version, document format, and authentication requirements with ETA documentation.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmission>
    <InternalID>EG-INV-1001</InternalID>
    <IssueDate>2026-10-26</IssueDate>
    <IssuerTaxID>YOUR_TAX_REGISTRATION_NUMBER</IssuerTaxID>
    <ReceiverTaxID>BUYER_TAX_REGISTRATION_NUMBER</ReceiverTaxID>
    <NetAmount>10000.00</NetAmount>
    <TaxAmount>1400.00</TaxAmount>
    <TotalAmount>11400.00</TotalAmount>
</InvoiceSubmission>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmissionResponse>
    <Status>Submitted</Status>
    <SubmissionReference>DEMO-EG-1001</SubmissionReference>
    <Message>Illustrative response only</Message>
</InvoiceSubmissionResponse>
```

**Production note:** ETA integrations may use REST/JSON and platform-specific document formats rather than SOAP. Follow the official API and SDK documentation.

---

## 4. Saudi Arabia — ZATCA FATOORA

**Authority:** Zakat, Tax and Customs Authority (ZATCA)

Saudi Arabia's e-invoicing requirements use structured XML invoices and defined cryptographic and compliance controls. Clearance and reporting workflows vary by invoice type.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Invoice>
    <InvoiceNumber>SA-INV-1001</InvoiceNumber>
    <IssueDate>2026-10-26</IssueDate>
    <SellerTaxNumber>YOUR_VAT_NUMBER</SellerTaxNumber>
    <BuyerName>Example Customer</BuyerName>
    <TaxableAmount>10000.00</TaxableAmount>
    <VATAmount>1500.00</VATAmount>
    <PayableAmount>11500.00</PayableAmount>
</Invoice>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceProcessingResponse>
    <Status>Accepted</Status>
    <InvoiceReference>DEMO-SA-1001</InvoiceReference>
    <Message>Illustrative response only</Message>
</InvoiceProcessingResponse>
```

**Production note:** This is not a ZATCA-compliant invoice document. Production implementations must follow the applicable UBL XML specification, security requirements, cryptographic controls, and clearance/reporting APIs.

---

## 5. Italy — Sistema di Interscambio (SdI)

**Authority:** Agenzia delle Entrate

Italian electronic invoices generally use the FatturaPA XML format and are exchanged through the Sistema di Interscambio (SdI). The example below is generic and is not a FatturaPA schema or an official SOAP request.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmission>
    <InvoiceNumber>IT-INV-1001</InvoiceNumber>
    <IssueDate>2026-10-26</IssueDate>
    <SupplierVAT>YOUR_VAT_NUMBER</SupplierVAT>
    <CustomerVAT>BUYER_VAT_NUMBER</CustomerVAT>
    <TaxableAmount>10000.00</TaxableAmount>
    <VATAmount>2200.00</VATAmount>
    <TotalAmount>12200.00</TotalAmount>
</InvoiceSubmission>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmissionResponse>
    <Status>Received</Status>
    <InvoiceNumber>IT-INV-1001</InvoiceNumber>
    <Message>Illustrative response only</Message>
</InvoiceSubmissionResponse>
```

**Production note:** Use the official FatturaPA schema and the applicable SdI submission channel or authorized intermediary.

---

## 6. Poland — KSeF

**Authority:** Polish Ministry of Finance

KSeF is Poland's national e-invoicing system. Integration requirements, authentication, structured invoice schema, and API versions must be checked against the current official specification.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmission>
    <InvoiceNumber>PL-INV-1001</InvoiceNumber>
    <IssueDate>2026-10-26</IssueDate>
    <SellerTaxID>YOUR_NIP</SellerTaxID>
    <BuyerTaxID>BUYER_NIP</BuyerTaxID>
    <NetAmount>10000.00</NetAmount>
    <VATAmount>2300.00</VATAmount>
    <GrossAmount>12300.00</GrossAmount>
</InvoiceSubmission>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmissionResponse>
    <Status>Accepted</Status>
    <InvoiceReference>DEMO-PL-1001</InvoiceReference>
    <Message>Illustrative response only</Message>
</InvoiceSubmissionResponse>
```

**Production note:** The sample is not an official KSeF invoice schema. Use the current FA structure, API version, authentication method, and processing-response format specified by the Polish Ministry of Finance.

---

## 7. European Union — Peppol

**Network:** Peppol eDelivery network

Peppol supports the exchange of structured business documents, including electronic invoices. Businesses typically connect through a Peppol service provider or access point. Peppol is not a single EU tax-authority SOAP endpoint.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SubmitInvoiceRequest>
    <ParticipantID>YOUR_PEPPOL_PARTICIPANT_ID</ParticipantID>
    <DocumentType>Invoice</DocumentType>
    <InvoiceNumber>EU-INV-1001</InvoiceNumber>
    <IssueDate>2026-10-26</IssueDate>
    <Currency>EUR</Currency>
    <TaxExclusiveAmount>10000.00</TaxExclusiveAmount>
    <TaxAmount>2100.00</TaxAmount>
    <PayableAmount>12100.00</PayableAmount>
</SubmitInvoiceRequest>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SubmitInvoiceResponse>
    <Status>Accepted</Status>
    <DocumentID>DEMO-EU-1001</DocumentID>
    <Message>Illustrative response only</Message>
</SubmitInvoiceResponse>
```

**Production note:** This is a conceptual example, not a standardized Peppol SOAP operation. Peppol invoices commonly use UBL XML and are exchanged through an access point using the applicable network protocols and specifications.

---

## 8. France — Electronic Invoicing

**Authority:** French tax administration and authorized electronic invoicing platforms

France's electronic invoicing framework uses structured electronic documents and platform-based exchange. The required format and submission workflow depend on applicable regulations and the authorized platform.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmission>
    <InvoiceNumber>FR-INV-1001</InvoiceNumber>
    <IssueDate>2026-10-26</IssueDate>
    <SupplierVAT>YOUR_VAT_NUMBER</SupplierVAT>
    <CustomerVAT>BUYER_VAT_NUMBER</CustomerVAT>
    <TaxExclusiveAmount>10000.00</TaxExclusiveAmount>
    <VATAmount>2000.00</VATAmount>
    <TotalAmount>12000.00</TotalAmount>
</InvoiceSubmission>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmissionResponse>
    <Status>Received</Status>
    <InvoiceNumber>FR-INV-1001</InvoiceNumber>
    <Message>Illustrative response only</Message>
</InvoiceSubmissionResponse>
```

**Production note:** This is not a French tax-authority SOAP request. Implement the format, transport, and platform APIs required by the applicable French electronic invoicing framework.

---

## 9. Germany — XRechnung / ZUGFeRD

**Standards:** XRechnung and ZUGFeRD

Germany supports structured electronic invoice formats including XRechnung and ZUGFeRD. The appropriate exchange channel depends on the recipient and use case. Generating a valid invoice file does not itself submit it to a government API.

### Illustrative XML Request

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceSubmission>
    <InvoiceNumber>DE-INV-1001</InvoiceNumber>
    <IssueDate>2026-10-26</IssueDate>
    <SellerTaxID>YOUR_TAX_ID</SellerTaxID>
    <BuyerReference>BUYER_REFERENCE</BuyerReference>
    <NetAmount>10000.00</NetAmount>
    <VATAmount>1900.00</VATAmount>
    <GrossAmount>11900.00</GrossAmount>
</InvoiceSubmission>
```

### Illustrative XML Response

```xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceProcessingResponse>
    <Status>Received</Status>
    <InvoiceNumber>DE-INV-1001</InvoiceNumber>
    <Message>Illustrative response only</Message>
</InvoiceProcessingResponse>
```

**Production note:** The sample is not a valid XRechnung or ZUGFeRD invoice. Use the applicable official syntax and business rules, and the recipient's required delivery channel.

---

## Shared PHP SOAP/XML Client Pattern

The following PHP cURL example demonstrates sending XML to a SOAP endpoint. Configure it with the real endpoint, SOAP action, credentials, certificates, and XML body required by the target service.

```php
<?php

$endpoint = 'YOUR_OFFICIAL_SOAP_ENDPOINT';
$soapAction = 'YOUR_DOCUMENTED_SOAP_ACTION';
$xmlRequest = '<ReplaceWithOfficialSchemaCompliantRequest/>';

$ch = curl_init($endpoint);

curl_setopt_array($ch, [
    CURLOPT_POST => true,
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_HTTPHEADER => [
        'Content-Type: text/xml; charset=UTF-8',
        'SOAPAction: "' . $soapAction . '"'
    ],
    CURLOPT_POSTFIELDS => $xmlRequest,
    CURLOPT_CONNECTTIMEOUT => 10,
    CURLOPT_TIMEOUT => 30
]);

$response = curl_exec($ch);
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);

if ($response === false) {
    error_log('Request failed: ' . curl_error($ch));
} else {
    echo 'HTTP Status: ' . $httpCode . PHP_EOL;
    echo $response;
}

curl_close($ch);
```

### Integration Checklist

- Use the correct country-specific API or document-exchange protocol.
- Validate XML against the official schema before sending.
- Implement the required authentication and certificate handling.
- Keep credentials and private keys outside source control.
- Handle HTTP errors, SOAP faults, and business-validation errors separately.
- Store submission references and invoice status securely.
- Test against the official sandbox or test environment before production.
- Label demonstration requests and responses as illustrative.
- Do not treat an HTTP success alone as proof that a tax invoice was accepted.

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

## GEROW TECH

Digital invoicing API integration, PHP development, and multi-country software integration.

- Website: https://gerowtech.com/
- Email: contact@gerowtech.com
- WhatsApp: https://wa.me/923215348101
