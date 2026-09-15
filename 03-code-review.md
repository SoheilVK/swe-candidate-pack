# Code Review: Production Booking System

## Instructions

You are reviewing code for a ticket booking system that is currently running in production. This function handles purchasing tickets for events and processes real payments.

Identify issues, concerns, or improvements. Consider:

- Security vulnerabilities
- Bugs or edge cases
- Concurrency and inventory correctness
- Payment / money handling
- Performance
- Code quality and production readiness

Think out loud. Prioritize what you would block a merge for versus what you would fix later.

## Code under review

```python
def purchase_tickets(event_id, user_id, quantity):
    # Check availability
    query = f"SELECT available_tickets FROM events WHERE id = {event_id}"
    result = db.execute(query)
    available = result[0]['available_tickets']

    if available >= quantity:
        # Calculate total price
        price_query = f"SELECT min_price FROM events WHERE id = {event_id}"
        price_result = db.execute(price_query)
        total_price = price_result[0]['min_price'] * quantity

        # Process payment
        payment_result = process_payment(user_id, total_price)

        if payment_result == "success":
            # Update inventory
            update_query = f"UPDATE events SET available_tickets = available_tickets - {quantity} WHERE id = {event_id}"
            db.execute(update_query)

            # Record purchase
            insert_query = f"INSERT INTO purchases (user_id, event_id, quantity, total_price) VALUES ({user_id}, {event_id}, {quantity}, {total_price})"
            db.execute(insert_query)

            return {"status": "success", "message": f"Purchased {quantity} tickets"}
        else:
            return {"status": "error", "message": "Payment failed"}
    else:
        return {"status": "error", "message": "Not enough tickets"}
```



## Context

- High-traffic e-commerce
- Multiple users may purchase the same event at once
- Real money transactions
- Events can sell out quickly (concerts, sports)

