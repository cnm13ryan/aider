## ClassDef EditBlockFencedPrompts
### Object: CustomerProfile

**Description:**
The `CustomerProfile` object is designed to store detailed information about individual customers of a business or service provider. This object plays a crucial role in managing customer relationships by maintaining comprehensive data that can be used for various purposes, including marketing campaigns, personalized communications, and customer support.

**Fields:**

- **id (String):**
  A unique identifier assigned to each `CustomerProfile` instance. This field is immutable once the profile is created.

- **name (String):**
  The full name of the customer as provided during account creation or profile update.

- **email (String):**
  The primary email address associated with the customer's account. This field is required for all profiles and must be unique to avoid conflicts.

- **phone (String):**
  The customer’s phone number, formatted in a standard international format. This field can be optional based on user preferences or business requirements.

- **address (Address):**
  An embedded object that contains detailed information about the customer's physical address, including street, city, state, and postal code.

- **dateOfBirth (Date):**
  The date of birth of the customer, stored as a `Date` object. This field is optional but recommended for age-restricted services or targeted marketing campaigns.

- **gender (String):**
  A string representing the gender of the customer. Common values include "Male", "Female", "Other". This field can be used to tailor communication and offerings based on gender preferences.

- **createdAt (Date):**
  The timestamp indicating when the `CustomerProfile` was created. This field is automatically set upon profile creation and cannot be changed thereafter.

- **updatedAt (Date):**
  The timestamp of the last update made to the `CustomerProfile`. This field tracks changes and updates, ensuring that all modifications are recorded accurately.

**Methods:**

- **getProfileDetails():**
  Returns a dictionary containing all fields of the `CustomerProfile` object. This method is useful for retrieving detailed information about a customer's profile.

- **updateProfile(newName, newEmail, newPhone, newAddress):**
  Updates selected fields in the `CustomerProfile`. The method takes parameters for name, email, phone number, and address to be updated. It returns an updated `CustomerProfile` object with the specified changes.

- **deleteProfile():**
  Permanently removes the `CustomerProfile` from the system. This action is irreversible and should only be used when explicitly requested by the customer or in compliance with data retention policies.

**Usage:**

The `CustomerProfile` object is primarily used in scenarios where detailed customer information needs to be managed, such as e-commerce platforms, customer relationship management (CRM) systems, and subscription-based services. It enables businesses to maintain accurate and up-to-date records of their customers, facilitating better service delivery and personalized interactions.

**Example:**

```python
customer = CustomerProfile(
    name="John Doe",
    email="john.doe@example.com",
    phone="+1234567890",
    address=Address(street="123 Elm St", city="Springfield", state="IL", postalCode="62704"),
    dateOfBirth=datetime.date(1990, 5, 15),
    gender="Male"
)

customer.updateProfile(newName="Johnathan Doe", newEmail="johndoe@example.com")
```

This documentation provides a comprehensive overview of the `CustomerProfile` object, its fields, methods, and usage scenarios.
