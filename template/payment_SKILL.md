---
name: payment_processing
description: Handle payment processing in the Django application, including integrating with payment gateways and managing transactions.
---

# Payment Processing Skill
This skill is responsible for handling payment processing in a Django application. It includes functionalities for integrating with payment gateways, managing transactions, and ensuring secure payment processing.

## Features
- Integrate with popular payment gateways (e.g., Stripe, PayPal) to process payments.
- Manage transactions, including creating, updating, and retrieving transaction records.
- Ensure secure handling of payment information and compliance with relevant regulations (e.g., PCI DSS).


# Usage
To use this skill, you can call the relevant functions for processing payments, managing transactions, and integrating with payment gateways. Make sure to provide the necessary information for each operation, such as payment details for processing payments or transaction IDs for managing transactions.


## Rules
- YOU MUST ONLY WORK IN payment directory.
- YOU MUST NOT ACCESS OTHER DIRECTORIES.
- YOU MUST NOT PERFORM ANY ACTIONS OUTSIDE OF payment management.
- YOU MUST NOT INTERACT WITH OTHER SKILLS OR AGENTS. Only focus on payments within the Django application.
- If you need to get data from user or another sub-agent, you must ask from main agent known as Mastermind Agent. You must not directly interact with user or other sub-agents. Always go through Mastermind Agent for any data retrieval or communication needs.

## Testing
To ensure the functionality of the payment processing skill, you can write tests in the `tests.py` file within the `payment` directory. These tests should cover various scenarios of payment processing, including successful payments, failed payments, and edge cases to ensure robustness and reliability of the payment processing system.
