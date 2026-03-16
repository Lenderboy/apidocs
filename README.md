# Cost Plus Drugs API

## About

Mark Cuban Cost Plus Drugs is committed to selling medications at transparent and affordable prices.  
Learn more about [their mission](https://costplusdrugs.com/mission/).

## Base URL

```text
https://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main
````

## Notes

* The authoritative purchase information for each medication is the returned `url`.
* When requesting `quantity_units`, the API returns a price estimate.
* Shipping charges and additional purchase terms may apply.

## Endpoints and Query Parameters

### Default behavior

Calling the base endpoint with no query parameters returns the full medication catalog.

```bash
curl "https://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main"
```

### Supported query parameters

* `ndc` — National Drug Code, no hyphens
* `brand_name` — Brand name, case insensitive
* `medication_name` — Medication name, case insensitive
* `strength` — Dosage strength including units, for example `10mg`
* `quantity_units` — Quantity to quote
* `generic_equivalent_ok` — Required for brand-name quote lookups when generic equivalents are acceptable; use `true` or `yes`

## Examples

### 1. Full listing

```bash
curl "https://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main"
```

### 2. Search by NDC

```bash
curl "http://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main?ndc=16729017117"
```

### 3. Search by brand name

```bash
curl "https://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main?brand_name=Elavil"
```

### 4. Search by brand and strength

```bash
curl "https://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main?brand_name=Elavil&strength=10mg"
```

### 5. Get a quantity-based quote by NDC

```bash
curl "http://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main?ndc=16729017117&quantity_units=30"
```

### 6. Get a quantity-based quote by medication name and strength

```bash
curl "http://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main?medication_name=amitriptyline&strength=10mg&quantity_units=30"
```

### 7. Brand-name quote request without generic approval

```bash
curl "http://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main?brand_name=elavil&strength=10mg&quantity_units=30"
```

Expected partial error:

```json
{
  "results": [
    {
      "brand_name": "Elavil",
      "error_message": "You must affirm parameter 'generic_equivalent_ok' with 'true' or 'yes' to receive quote",
      "form": "Tablet"
    }
  ]
}
```

### 8. Brand-name quote request with generic approval

```bash
curl "http://us-central1-costplusdrugs-publicapi.cloudfunctions.net/main?brand_name=elavil&strength=10mg&quantity_units=30&generic_equivalent_ok=true"
```

## Example response shape

```json
{
  "results": [
    {
      "brand_name": "Elavil",
      "form": "Tablet",
      "medication_name": "Amitriptyline",
      "ndc": "16729017117",
      "pill_nonpill": "Pill",
      "requested_quote": "$4.50",
      "requested_quote_units": 30,
      "slug": "Amitriptyline-10mg-Tablet",
      "strength": "10mg",
      "unit_price": "$0.050",
      "url": "https://costplusdrugs.com/medications/amitriptyline-10mg-tablet/"
    }
  ]
}
```

## Reference

A full example output, with offerings and prices as of 2022-03-23, is available [here](/apidocs/output-ex-001.json).



The second version is the one I would use for GitHub.
```
