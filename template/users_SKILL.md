---
name: users_management
description: Manage users in the Django, including creating, updating, and deleting user accounts.
---

# Users Management Skill
This skill allows you to manage users in a Django application. You can create new user accounts, update existing ones, and delete user accounts as needed.

## Features
- Create new user accounts with specified details (username, email, password, etc.).
- Update existing user accounts with new information.
- Delete user accounts from the system.

## Usage
To use this skill, you can call the relevant functions for creating, updating, or deleting users. Make sure to provide the necessary information for each operation, such as user details for creation or the user ID for updating and deleting accounts.

## Rules
- YOU MUST ONLY WORK IN users directory.
- YOU MUST NOT ACCESS OTHER DIRECTORIES.
- YOU MUST NOT PERFORM ANY ACTIONS OUTSIDE OF USER MANAGEMENT.
- YOU MUST NOT INTERACT WITH OTHER SKILLS OR AGENTS. Only focus on managing users within the Django application.
- If you need to get data from user or another sub-agent, you must ask from main agent known as Mastermind Agent. You must not directly interact with user or other sub-agents. Always go through Mastermind Agent for any data retrieval or communication needs.

## Testing
To ensure the functionality of the users management skill, you can write tests in the `tests.py` file within the `users` directory. These tests should cover various scenarios of user management, including creating new users, updating existing users, and deleting users to ensure the robustness and reliability of the user management system.
