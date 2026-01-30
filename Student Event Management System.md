## Project Overview

**Project Title**: Student Event Management System (Back-End Java Application)

This project is designed to demonstrate core Java programming concepts typically used in back-end application development. The goal of this project is to build a console-based Student Event Management System that allows students to register, authenticate, create events, and browse campus activities. The system focuses on object-oriented design, input validation, file persistence, and application flow control. All data is stored locally using text files, simulating basic database functionality.

## Skills Used

- Object-Oriented Programming (Classes, Encapsulation, Constructors)
- ArrayLists for In-Memory Data Storage
- File I/O (Reading and Writing Text Files)
- Input Validation & Error Handling
- Authentication Logic
- Java Date/Time API (LocalDate, LocalTime)
- Search and Filtering Logic
- Menu-Driven Console Applications

## Objectives

1. **Design Core Models**
2. **Implement Authentication and Persistence**
3. **Build Event Management Features**

## Project Structure

### 1. Design Core Models

- Before building the application logic, I started by designing the core data models: `Student` and `Event`.
- Each class encapsulates its own fields and behavior using private variables and public getter/setter methods.
- Unique IDs are generated automatically using static counters, ensuring every student and event receives a distinct identifier.
- Both classes include custom `toString()` methods to format data cleanly for console output.

The `Student` class stores user credentials and profile information such as name, email, major, and academic year.  
The `Event` class manages event-related data including title, description, date, time, venue, capacity, fee, and status.

This separation of responsibilities made it easier to manage data consistently across the application.

---

### 2. Implement Authentication and Persistence

- After defining the core models, I created an `AuthenticationManager` class to handle registration, login, and session tracking.
- Student accounts are stored in an `ArrayList`, which acts as an in-memory database while the program is running.

During registration, several validations are enforced:

- Email must end with `@asu.edu`
- Password must be at least 8 characters
- Password must contain both a number and a symbol
- Duplicate email addresses are rejected

Once a student registers successfully, their information is immediately saved to a `student.txt` file. When the application launches, existing students are loaded from this file so accounts persist between sessions.

The login system verifies credentials against stored records and assigns a `currentUser`, allowing access to authenticated features such as viewing profiles and creating events.

A logout method clears the session and returns the user to the main menu.

---

### 3. Build Event Management Features

- Event functionality is handled through the `EventManager` class, which mirrors the structure of the authentication system.
- Events are stored in an ArrayList and persisted to `event.txt`.

When creating an event, the system validates:

- Title and venue are not empty  
- Date is not in the past  
- Capacity is greater than zero  
- Fee is not negative  
- Status is provided  

Valid events are saved to file and displayed back to the user for confirmation.

Additional features include:

- Browsing all available events
- Keyword-based searching across title, description, venue, and status
- Automatic loading of saved events when the application starts

The main application (`SEMSApplication`) ties everything together with a menu-driven interface that changes based on login state. Users can register, authenticate, view profiles, create events, browse events, search events, and log out.

Error handling is implemented for invalid date formats, time formats, and numeric input to prevent application crashes and improve usability.

---

## Conclusion

Throughout this project, I gained hands-on experience designing a complete back-end Java application from scratch. I implemented authentication, file persistence, object-oriented models, and user-driven workflows while maintaining clean separation between responsibilities. Building this system helped reinforce fundamental Java concepts such as encapsulation, collections, file I/O, and application state management.

Although this version runs entirely in the console and uses text files instead of a database, it provides a strong foundation for future expansion into a full-stack application using frameworks such as Spring Boot and a relational database. Overall, this project strengthened my understanding of back-end architecture and real-world application
