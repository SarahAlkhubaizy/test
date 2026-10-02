# 2. Components, Classes, and Database Design

## 2.1 Back-End Design (Classes & Methods)

### Class Descriptions:
- **User:** Represents system users (Hikers).
- **Admin:** Extends `User` with administrative privileges (e.g., managing trails).
- **Trail:** Represents trail routes and their attributes.
- **Review:** Manages user ratings and feedback for trails.
- **Favorite:** Manages user saved trails.
- **TrailService:** Utility class for searching, filtering, and sorting trails.

```mermaid
classDiagram
    direction TB
    class User {
        +int id
        +string name
        +string email
        +string password
        +string role
        +register()
        +login()
        +logout()
        +updateProfile()
    }
    class Admin {
        +addTrail()
        +updateTrail()
        +deactivateTrail()
    }
    class Trail {
        +int id
        +string name
        +string region
        +string difficulty
        +float distance
        +getDetails()
    }
    class Review {
        +int id
        +int rating
        +string comment
        +addReview()
    }
    class Favorite {
        +int userId
        +int trailId
    }

    User <|-- Admin
    User "1" --> "0..*" Review
    User "1" --> "0..*" Favorite
    Trail "1" --> "0..*" Review
    Trail "1" --> "0..*" Favorite
```

---

## 2.2 Database Design (Relational ERD)

```mermaid
erDiagram
    users ||--o{ reviews : "writes"
    users ||--o{ favorites : "has"
    users ||--o| admins : "is"

    trails ||--o{ reviews : "belongs to"
    trails ||--o{ favorites : "belongs to"

    users {
        int id PK
        string name
        string email
        string password
        string role
    }

    admins {
        int user_id PK, FK
    }

    trails {
        int id PK
        string name
        string description
        string region
        string difficulty
        float distance
        time estimated_duration
        string status
    }

    reviews {
        int id PK
        int user_id FK
        int trail_id FK
        int rating
        text comment
    }

    favorites {
        int user_id PK, FK
        int trail_id PK, FK
    }
```

---

## 2.3 Front-End UI Components

| Component Name | Description | Interactions |
| :--- | :--- | :--- |
| **AuthForms** | Handles user registration and login. | Submits user credentials to API to get Auth Tokens. |
| **TrailCardList** | Displays cards for available trails with search/filter controls. | Triggers `TrailService` filters (Difficulty, Region, Rating). |
| **TrailDetailsView** | Shows full information for a selected trail including route maps. | Fetches route coordinates and displays reviews. |
| **ReviewSection** | Displays user reviews and provides a form to add new reviews. | Submits new ratings and updates average rating dynamically. |
| **FavoriteButton** | Toggle button to save or remove trails from user favorites. | Sends request to `Favorite` API endpoint. |
