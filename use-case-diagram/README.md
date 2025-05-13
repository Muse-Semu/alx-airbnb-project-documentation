# AirBnB Clone Use Case Diagram

## Overview
This directory (`use-case-diagram/`) contains documentation for the use case diagram of the AirBnB Clone project backend. The diagram, created using PlantUML, visualizes the interactions between key actors (Guest, Host, Admin) and the system for core functionalities such as user registration, property booking, and payments.

## Files
- **use_case_diagram.puml**: The PlantUML script defining the use case diagram.
- **use_case_diagram.png**: A PNG export of the rendered PlantUML diagram.
- **README.md**: This file, describing the purpose and contents of the directory.

## Diagram Details
The use case diagram captures the interactions between actors and the AirBnB Clone System, including:
- **Actors**:
  - **Guest**: Registers, logs in, searches properties, books properties, makes payments, submits reviews, sends/receives messages.
  - **Host**: Registers, logs in, creates/updates/deletes properties, manages availability, sends/receives messages.
  - **Admin**: Logs in, manages users, moderates properties, handles disputes, views metrics.
- **Use Cases**:
  - Register, Login, Search Properties, Book Property (includes Check Availability), Make Payment, Create/Update/Delete Property, Manage Availability, Submit Review, Send/Receive Message, Manage Users, Moderate Properties, Handle Disputes (extends to Handle Refunds), View Metrics.
- **System Boundary**: AirBnB Clone System, containing all use cases.

### Diagram Creation
The diagram was created using PlantUML with the following steps:
1. Wrote the `use_case_diagram.puml` script using `@startuml` syntax for a use case diagram.
2. Defined actors (Guest, Host, Admin) and use cases within the system boundary.
3. Specified relationships (associations, includes, extends) to model interactions.
4. Styled with colors for actors (e.g., blue for Guest, green for Host) and a clear system boundary.
5. Rendered the diagram using an online PlantUML editor (e.g., http://www.plantuml.com/plantuml/) or local PlantUML tool.
6. Exported as `use_case_diagram.png`.

## Usage
- **View the Diagram**: Open `use_case_diagram.png` to review the actor-system interactions.
- **Edit the Diagram**: Modify `use_case_diagram.puml` and re-render using a PlantUML tool.
- **Render Locally** (optional):
  ```bash
  java -jar plantuml.jar use_case_diagram.puml
  ```
- **Repository**: This documentation is part of the `alx-airbnb-project-documentation` repository.

## Notes
- The diagram aligns with the normalized database schema (tables: `users`, `locations`, `properties`, `bookings`, `payments`, `reviews`, `messages`) and typical AirBnB-like requirements.
- The PlantUML script uses modern syntax (as of May 2025) to ensure compatibility and avoid errors.
- The PNG file is optimized for clarity and readability.
- For detailed feature documentation, refer to the `features-and-functionalities/` directory in the same repository.
- For schema details, refer to the `alx-airbnb-database` repository.