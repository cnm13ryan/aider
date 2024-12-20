## FunctionDef scrub_sensitive_info(args, text)
### Object: CustomerFeedback

**Definition:** 
CustomerFeedback is an entity designed to capture detailed feedback from customers regarding their experiences with products or services provided by our organization.

**Fields:**

1. **feedbackId (String)**
   - **Description:** A unique identifier for each customer feedback record.
   - **Constraints:** Not null, Unique

2. **customerId (String)**
   - **Description:** The ID of the customer who provided the feedback.
   - **Constraints:** Not null

3. **productId (String)**
   - **Description:** The ID of the product or service related to this feedback.
   - **Constraints:** Not null

4. **rating (Integer)**
   - **Description:** A numerical rating given by the customer, ranging from 1 to 5, where 1 is poor and 5 is excellent.
   - **Constraints:** Not null, Range: 1-5

5. **textFeedback (String)**
   - **Description:** Free-form text provided by the customer detailing their experience or comments.
   - **Constraints:** Not null, Max Length: 2048 characters

6. **createdAt (Timestamp)**
   - **Description:** The timestamp when this feedback record was created.
   - **Constraints:** Not null

7. **updatedAt (Timestamp)**
   - **Description:** The timestamp when this feedback record was last updated.
   - **Constraints:** Not null

**Relationships:**

- **ManyToOne:** One CustomerFeedback is associated with one Customer.
- **ManyToOne:** One CustomerFeedback is associated with one Product or Service.

**Usage Examples:**

1. **Creating a New Feedback Record:**
   ```sql
   INSERT INTO CustomerFeedback (customerId, productId, rating, textFeedback)
   VALUES ('C001', 'P005', 4, 'The product works well but could use some improvements.');
   ```

2. **Updating an Existing Feedback Record:**
   ```sql
   UPDATE CustomerFeedback
   SET rating = 3, textFeedback = 'There are issues with the delivery process.'
   WHERE feedbackId = 'FB007';
   ```

**Best Practices:**

- Regularly review and analyze customer feedback to identify trends and areas for improvement.
- Ensure that all feedback is handled confidentially and securely.

By maintaining detailed records of customer feedback, our organization can continuously improve its products and services, leading to enhanced customer satisfaction.
## FunctionDef format_settings(parser, args)
### Object: CustomerServiceTicket

#### Overview
The `CustomerServiceTicket` object is designed to manage customer service requests efficiently within our support system. It serves as a structured data model that captures essential details about each support ticket, enabling seamless tracking and resolution of customer issues.

#### Fields

1. **id**  
   - Type: Integer  
   - Description: A unique identifier for the ticket generated by the system upon creation.
   - Example: `45678`

2. **customerId**  
   - Type: String  
   - Description: The unique identifier of the customer associated with this ticket.
   - Example: `CUST12345`

3. **ticketNumber**  
   - Type: String  
   - Description: A unique alphanumeric code assigned to each support ticket for easy reference.
   - Example: `TICKET-000456789`

4. **subject**  
   - Type: String  
   - Description: The subject or brief description of the issue reported by the customer.
   - Example: `Payment Issue Not Resolved`

5. **description**  
   - Type: Text  
   - Description: A detailed explanation of the problem, including any relevant information provided by the customer.
   - Example:
     ```
     I have not received my order for over a week. The tracking number shows it was delivered on 10/20/23 but there is no signature or confirmation from the delivery service. Please look into this as soon as possible.
     ```

6. **status**  
   - Type: String  
   - Description: The current status of the ticket, indicating whether it is open, in progress, resolved, or closed.
   - Example: `Resolved`

7. **priorityLevel**  
   - Type: Integer  
   - Description: An integer value representing the urgency of the issue (1 being low and 5 being high).
   - Example: `3`

8. **createdDate**  
   - Type: DateTime  
   - Description: The date and time when the ticket was created.
   - Example: `2023-10-24T14:56:32Z`

9. **lastUpdatedDate**  
   - Type: DateTime  
   - Description: The last date and time when any changes were made to the ticket.
   - Example: `2023-10-27T16:45:21Z`

10. **assignedAgentId**  
    - Type: String  
    - Description: The unique identifier of the agent currently handling the ticket.
    - Example: `AGENT98765`

11. **resolutionNotes**  
    - Type: Text  
    - Description: Notes or comments added by the support team regarding the resolution process and outcome.
    - Example:
      ```
      Customer's order was found to be mistakenly marked as shipped. A new shipment has been initiated, and the customer will receive a refund for the shipping cost. The issue will be resolved within 24 hours.
      ```

#### Methods

1. **createTicket**
   - Description: Creates a new support ticket in the system.
   - Parameters:
     - `customerId`: String
     - `subject`: String
     - `description`: Text
     - `priorityLevel`: Integer (1-5)
   - Returns: `CustomerServiceTicket` object

2. **updateTicket**
   - Description: Updates an existing support ticket with new information.
   - Parameters:
     - `ticketId`: Integer
     - `status`: String
     - `resolutionNotes`: Text
   - Returns: Boolean (true if successful, false otherwise)

3. **closeTicket**
   - Description: Closes a support ticket after resolution.
   - Parameters:
     - `ticketId`: Integer
   - Returns: Boolean (true if successful, false otherwise)

4. **getTicketById**
   - Description: Retrieves a specific support ticket by its ID.
   - Parameters:
     - `ticketId`: Integer
   - Returns: `CustomerServiceTicket` object

5. **listTickets**
   - Description: Lists all tickets based on various filters (e.g., status, priority).
   - Parameters:
     - `filterByStatus`: String
     - `filterByPriorityLevel`: Integer
   - Returns: List of `CustomerServiceTicket` objects

#### Example Usage

```python
# Create a new ticket
new_ticket = createTicket(
    customerId="CUST12345",
    subject="Payment Issue Not Resolved",
    description="I have not received my order for over a week. The tracking number shows it was delivered on 10/20/23 but there is no signature or confirmation from the delivery service.",
    priorityLevel=3
)

# Update an existing ticket
updateTicket(
    ticketId=new_ticket.id,

