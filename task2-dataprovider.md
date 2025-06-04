# Task 2: Data Provider - Instructions

- you are the product owner of the team shipment (SHIP)
- you own the shipments bounded context
- your internal operational database structure: add it here as a table.
- your team has a slack channel #SHIP for contact in their example.slack.com slack
- your team has an email address where they can be reached: team.ship@example.com
- you provide support for your services only during business hours (9to5) in CEST

## Shipment Data Model

This data model describes the structure of shipment data available to consumers. It contains a wide range of fields, some of which may not be directly relevant to every consumer use case. Consumers are expected to apply transformation logic to extract the information they need (e.g., filtering undelivered shipments, calculating durations, etc.).

| Field Name                | Type           | Description                                                        |
|--------------------------|----------------|--------------------------------------------------------------------|
| shipment_id               | string         | Unique identifier for the shipment                                 |
| tracking_number           | string         | Carrier-provided tracking number                                   |
| status                    | string         | Current status (e.g., 'in_transit', 'delivered', 'exception')      |
| origin                    | string         | Origin location (address or code)                                  |
| destination               | string         | Destination location (address or code)                             |
| carrier                   | string         | Name of the carrier handling the shipment                          |
| carrier_service           | string         | Service level (e.g., 'express', 'standard')                       |
| created_at                | datetime       | Timestamp when the shipment was created                            |
| shipped_at                | datetime       | Timestamp when the shipment was shipped                            |
| expected_delivery_at      | datetime       | Expected delivery timestamp                                        |
| delivered_at              | datetime/null  | Actual delivery timestamp (null if not delivered)                  |
| last_update_at            | datetime       | Timestamp of the last status/location update                       |
| current_location          | string         | Most recent known location                                         |
| location_history          | array<object>  | List of past locations and timestamps                              |
| weight_kg                 | float          | Weight of the shipment in kilograms                                |
| dimensions_cm             | object         | Dimensions (length, width, height) in centimeters                  |
| value_usd                 | float          | Declared value of the shipment in USD                              |
| contents_description      | string         | Description of the shipment contents                               |
| sender_contact            | object         | Sender's contact information                                       |
| recipient_contact         | object         | Recipient's contact information                                    |
| customs_status            | string         | Customs clearance status                                           |
| insurance                 | object         | Insurance details (provider, amount, etc.)                         |
| special_handling_required | boolean        | Whether special handling is required                               |
| notes                     | string         | Additional notes                                                   |

## Example Shipments (JSON)

Below are full examples of the shipments data model in JSON format. These examples illustrate the use of all fields, including nested objects and arrays.

### Example 1
```json
{
  "shipment_id": "SHIP123456",
  "tracking_number": "1Z999AA10123456784",
  "status": "in_transit",
  "origin": "Berlin Warehouse",
  "destination": "Munich Store",
  "carrier": "DHL",
  "carrier_service": "express",
  "created_at": "2025-06-01T08:00:00Z",
  "shipped_at": "2025-06-01T10:00:00Z",
  "expected_delivery_at": "2025-06-03T18:00:00Z",
  "delivered_at": null,
  "last_update_at": "2025-06-02T15:30:00Z",
  "current_location": "Nuremberg Hub",
  "location_history": [
    { "location": "Berlin Warehouse", "timestamp": "2025-06-01T10:00:00Z" },
    { "location": "Leipzig Depot", "timestamp": "2025-06-01T18:00:00Z" },
    { "location": "Nuremberg Hub", "timestamp": "2025-06-02T15:30:00Z" }
  ],
  "weight_kg": 12.5,
  "dimensions_cm": { "length": 80, "width": 40, "height": 30 },
  "value_usd": 250.00,
  "contents_description": "Electronics - Laptops",
  "sender_contact": {
    "name": "Alice Example",
    "phone": "+49 30 1234567",
    "email": "alice@example.com"
  },
  "recipient_contact": {
    "name": "Bob Example",
    "phone": "+49 89 7654321",
    "email": "bob@example.com"
  },
  "customs_status": "cleared",
  "insurance": {
    "provider": "Allianz",
    "amount_usd": 300.00,
    "policy_number": "INS-987654"
  },
  "special_handling_required": false,
  "notes": "Handle with care."
}
```

### Example 2
```json
{
  "shipment_id": "SHIP654321",
  "tracking_number": "1Z999BB20234567890",
  "status": "delivered",
  "origin": "Hamburg Port",
  "destination": "Frankfurt Office",
  "carrier": "FedEx",
  "carrier_service": "standard",
  "created_at": "2025-05-28T09:00:00Z",
  "shipped_at": "2025-05-28T12:00:00Z",
  "expected_delivery_at": "2025-05-30T17:00:00Z",
  "delivered_at": "2025-05-30T16:45:00Z",
  "last_update_at": "2025-05-30T16:45:00Z",
  "current_location": "Frankfurt Office",
  "location_history": [
    { "location": "Hamburg Port", "timestamp": "2025-05-28T12:00:00Z" },
    { "location": "Kassel Facility", "timestamp": "2025-05-29T08:30:00Z" },
    { "location": "Frankfurt Office", "timestamp": "2025-05-30T16:45:00Z" }
  ],
  "weight_kg": 5.0,
  "dimensions_cm": { "length": 50, "width": 30, "height": 20 },
  "value_usd": 120.00,
  "contents_description": "Office Supplies",
  "sender_contact": {
    "name": "Carol Sender",
    "phone": "+49 40 9876543",
    "email": "carol@sender.com"
  },
  "recipient_contact": {
    "name": "Dave Receiver",
    "phone": "+49 69 1239876",
    "email": "dave@receiver.com"
  },
  "customs_status": "not_applicable",
  "insurance": {
    "provider": "Zurich",
    "amount_usd": 150.00,
    "policy_number": "INS-123456"
  },
  "special_handling_required": true,
  "notes": "Deliver before 5pm."
}
```
