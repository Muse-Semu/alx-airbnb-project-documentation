# AirBnB Clone Backend Features and Functionalities

## Overview
This directory (`features-and-functionalities/`) contains documentation for the key features and functionalities of the AirBnB Clone project backend. The documentation is presented as a mind map created using PlantUML, outlining the backend capabilities required to support the platform.

## Files
- **features_diagram.puml**: The PlantUML script defining the mind map of backend features.
- **features_diagram.png**: A PNG export of the rendered PlantUML diagram.
- **README.md**: This file, describing the purpose and contents of the directory.

## Diagram Details
The diagram is a hierarchical mind map that organizes the AirBnB Clone backend features into the following categories:
- **User Authentication**: Registration, login, password hashing, role-based access, profile management.
- **Property Management**: Create/update/delete properties, location management, property search, listing.
- **Booking System**: Create bookings, validate availability, calculate total price, manage status, view history.
- **Payment Processing**: Process payments, support multiple methods, store payment details, handle refunds.
- **Review System**: Submit and display reviews, restrict to completed bookings.
- **Messaging System**: Send/receive messages, store history, notify new messages.
- **Admin Features**: Manage users, moderate properties, handle disputes, view metrics.
- **Additional Features**: Search and filter, availability calendar, audit trails, data validation.

### Diagram Creation
The diagram was created using PlantUML with the following steps:
1. Wrote the `features_diagram.puml` script using `@startmindmap` syntax.
2. Defined a central node ("AirBnB Clone Backend Features") and child nodes for each feature.
3. Styled with distinct colors for each major feature (e.g., blue for User Authentication, green for Property Management).
4. Rendered the diagram using an online PlantUML editor (e.g., http://www.plantuml.com/plantuml/) or local PlantUML tool.
5. Exported as `features_diagram.png`.

## Usage
- **View the Diagram**: Open `features_diagram.png` to review the backend features.
- **Edit the Diagram**: Modify `features_diagram.puml` and re-render using a PlantUML tool.
- **Render Locally** (optional):
  ```bash
  java -jar plantuml.jar features_diagram.puml
  ```
- **Repository**: This documentation is part of the `alx-airbnb-project-documentation` repository.

## Notes
- The diagram reflects the normalized database schema (tables: `users`, `locations`, `properties`, `bookings`, `payments`, `reviews`, `messages`) and typical AirBnB-like requirements.
- The PNG file is optimized for clarity and readability.
- For detailed schema information, refer to the `alx-airbnb-database` repository.