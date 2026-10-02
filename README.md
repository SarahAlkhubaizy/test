```mermaid
classDiagram
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
        +viewFavorites()
        +addFavorite()
        +removeFavorite()
        +addReview()
        +viewMyReviews()
    }

    class Admin {
        +id
        +name
        +email
        +password
        +addTrail()
        +updateTrail()
        +deactivateTrail()
        +uploadTrailImages()
        +updateTrailRoute()
    }

    class Trail {
        +int id
        +string name
        +string description
        +string region
        +string difficulty
        +float distance
        +time estimatedDuration
        +Array images
        +boolean isOfficial
        +Point startPoint
        +Point endPoint
        +Route routeCoordinates
        +string status
        +DateTime createdAt
        +getDetails()
        +getLocation()
        +getRoute()
        +isActive()
        +calculateAverageRating()
        +getReviews()
    }

    class Review {
        +int id
        +int userId
        +int trailId
        +int rating
        +string comment
        +DateTime createdAt
        +addReview()
        +updateReview()
        +deleteReview()
        +getReview()
        +validateRating()
    }

    class Favorite {
        +int userId
        +int trailId
        +DateTime createdAt
        +addFavorite()
        +removeFavorite()
        +isFavorite()
        +getUserFavorites()
    }

    class TrailService {
        +searchByName()
        +filterByRegion()
        +filterByDifficulty()
        +sortByRating()
        +sortByDate()
        +clearFilters()
    }





```mermaid
erDiagram
    USER ||--o{ REVIEW : "writes"
    USER ||--o{ FAVORITE : "saves"
    USER ||--o| ADMIN : "is extended as"

    TRAIL ||--o{ REVIEW : "has"
    TRAIL ||--o{ FAVORITE : "is favorited in"

    USER {
        int id PK
        string name
        string email
        string password
        string role
    }

    ADMIN {
        int id PK, FK
    }

    TRAIL {
        int id PK
        string name
        string description
        string region
        string difficulty
        float distance
        time estimatedDuration
        string images
        boolean isOfficial
        point startPoint
        point endPoint
        json routeCoordinates
        string status
        datetime createdAt
    }

    REVIEW {
        int id PK
        int userId FK
        int trailId FK
        int rating
        string comment
        datetime createdAt
    }

    FAVORITE {
        int userId PK, FK
        int trailId PK, FK
        datetime createdAt
    }
```

    %% Relationships
    User <|-- Admin : inherits
    User "1" -- "0..*" Review : writes
    User "1" -- "0..*" Favorite : saves
    Trail "1" -- "0..*" Review : receives
    Trail "1" -- "0..*" Favorite : saved_in
    TrailService ..> Trail : manages/searches
