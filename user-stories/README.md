# AirBnB Clone User Stories

## Overview
This directory (`user-stories/`) contains documentation for the user stories of the AirBnB Clone project backend. The user stories are derived from the use case diagram, capturing interactions between actors (Guest, Host, Admin) and the system for core functionalities such as user registration, property booking, and payments.

## Files
- **user-stories.md**: A Markdown file listing the user stories, grouped by actor (Guest, Host, Admin).
- **README.md**: This file, describing the purpose and contents of the directory.

## User Stories Details
The user stories translate each use case from the use case diagram into the format: **"As a [role], I want to [action] so that [benefit]."** They cover:
- **Guest**: Register, login, search properties, check availability, book property, make payment, submit review, send/receive message.
- **Host**: Register, login, create/update/delete property, manage availability, send/receive message.
- **Admin**: Login, manage users, moderate properties, handle disputes, handle refunds, view metrics.

### Creation Process
The user stories were created by:
1. Reviewing the use case diagram from the `use-case-diagram/` directory.
2. Translating each use case into a user story, ensuring coverage of core interactions.
3. Grouping stories by actor (Guest, Host, Admin) for clarity.
4. Documenting in `user-stories.md` with a clear structure.

## Usage
- **View the User Stories**: Open `user-stories.md` to review the documented user stories.
- **Edit the User Stories**: Modify `user-stories.md` to add or update stories as needed.
- **Repository**: This documentation is part of the `alx-airbnb-project-documentation` repository.

## Notes
- The user stories align with the normalized database schema (tables: `users`, `locations`, `properties`, `bookings`, `payments`, `reviews`, `messages`) and AirBnB-like requirements.
- The stories cover all use cases from the diagram, exceeding the minimum requirement of five.
- For the use case diagram, refer to the `use-case-diagram/` directory.
- For schema details, refer to the `alx-airbnb-database` repository.