# Components, Classes, and Database Design

## 1. Back-end Classes

### User

**Attributes:**

* `id`
* `name`
* `email`
* `password`
* `role`

**Methods:**

* `register()`
* `login()`
* `logout()`
* `updateProfile()`
* `viewFavorites()`
* `addFavorite()`
* `removeFavorite()`
* `addReview()`
* `viewMyReviews()`

### Trail

**Attributes:**

* `id`
* `name`
* `description`
* `region`
* `difficulty`
* `distance`
* `estimatedDuration`
* `images`
* `isOfficial`
* `startPoint`
* `endPoint`
* `routeCoordinates`
* `status`
* `createdAt`

**Methods:**

* `getDetails()`
* `getLocation()`
* `getRoute()`
* `isActive()`
* `calculateAverageRating()`
* `getReviews()`

### Review

**Attributes:**

* `id`
* `userId`
* `trailId`
* `rating`
* `comment`
* `createdAt`

**Methods:**

* `addReview()`
* `updateReview()`
* `deleteReview()`
* `getReview()`
* `validateRating()`

### Favorite

**Attributes:**

* `userId`
* `trailId`
* `createdAt`

**Methods:**

* `addFavorite()`
* `removeFavorite()`
* `isFavorite()`
* `getUserFavorites()`

### TrailService

**Attributes:**

* None

**Methods:**

* `searchByName()`
* `filterByRegion()`
* `filterByDifficulty()`
* `sortByRating()`
* `sortByDate()`
* `clearFilters()`

### Admin

**Attributes:**

* `id`
* `name`
* `email`
* `password`

**Methods:**

* `addTrail()`
* `updateTrail()`
* `deactivateTrail()`
* `uploadTrailImages()`
* `updateTrailRoute()`

---

# 2. UML Class Diagram

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

class Trail {
    +int id
    +string name
    +string description
    +string region
    +string difficulty
    +float distance
    +string estimatedDuration
    +string images
    +boolean isOfficial
    +string startPoint
    +string endPoint
    +string routeCoordinates
    +string status
    +datetime createdAt
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
    +datetime createdAt
    +addReview()
    +updateReview()
    +deleteReview()
    +getReview()
    +validateRating()
}

class Favorite {
    +int userId
    +int trailId
    +datetime createdAt
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

class Admin {
    +int id
    +string name
    +string email
    +string password
    +addTrail()
    +updateTrail()
    +deactivateTrail()
    +uploadTrailImages()
    +updateTrailRoute()
}

User "1" --> "0..*" Review : writes
Trail "1" --> "0..*" Review : receives

User "1" --> "0..*" Favorite : saves
Trail "1" --> "0..*" Favorite : is saved in

TrailService ..> Trail : searches and filters
Admin --> Trail : manages
Admin --> Review : moderates
```

---

# 3. Database Design

The system uses a relational database with the following tables:

### Users

| Field    | Type    | Key    |
| -------- | ------- | ------ |
| id       | INT     | PK     |
| name     | VARCHAR |        |
| email    | VARCHAR | UNIQUE |
| password | VARCHAR |        |
| role     | VARCHAR |        |

### Trails

| Field             | Type     | Key |
| ----------------- | -------- | --- |
| id                | INT      | PK  |
| name              | VARCHAR  |     |
| description       | TEXT     |     |
| region            | VARCHAR  |     |
| difficulty        | VARCHAR  |     |
| distance          | FLOAT    |     |
| estimatedDuration | VARCHAR  |     |
| images            | TEXT     |     |
| isOfficial        | BOOLEAN  |     |
| startPoint        | VARCHAR  |     |
| endPoint          | VARCHAR  |     |
| routeCoordinates  | TEXT     |     |
| status            | VARCHAR  |     |
| createdAt         | DATETIME |     |

### Reviews

| Field     | Type     | Key            |
| --------- | -------- | -------------- |
| id        | INT      | PK             |
| userId    | INT      | FK → Users.id  |
| trailId   | INT      | FK → Trails.id |
| rating    | INT      |                |
| comment   | TEXT     |                |
| createdAt | DATETIME |                |

### Favorites

| Field     | Type     | Key                |
| --------- | -------- | ------------------ |
| userId    | INT      | PK, FK → Users.id  |
| trailId   | INT      | PK, FK → Trails.id |
| createdAt | DATETIME |                    |

---

# 4. ER Diagram

```mermaid
erDiagram

    USERS {
        INT id PK
        VARCHAR name
        VARCHAR email UK
        VARCHAR password
        VARCHAR role
    }

    TRAILS {
        INT id PK
        VARCHAR name
        TEXT description
        VARCHAR region
        VARCHAR difficulty
        FLOAT distance
        VARCHAR estimatedDuration
        TEXT images
        BOOLEAN isOfficial
        VARCHAR startPoint
        VARCHAR endPoint
        TEXT routeCoordinates
        VARCHAR status
        DATETIME createdAt
    }

    REVIEWS {
        INT id PK
        INT userId FK
        INT trailId FK
        INT rating
        TEXT comment
        DATETIME createdAt
    }

    FAVORITES {
        INT userId PK, FK
        INT trailId PK, FK
        DATETIME createdAt
    }

    USERS ||--o{ REVIEWS : writes
    TRAILS ||--o{ REVIEWS : receives

    USERS ||--o{ FAVORITES : saves
    TRAILS ||--o{ FAVORITES : has
```

---

# 5. Front-end Components

The main front-end components are:

* **Home / Trail List**

  * Displays available hiking trails.
  * Provides access to search, filters, and sorting.

* **Search Bar**

  * Searches trails by name.

* **Filter Component**

  * Filters trails by region.
  * Filters trails by difficulty.

* **Sort Component**

  * Sorts trails by rating.
  * Sorts trails by newest.

* **Saudi Map**

  * Displays hiking trails across Saudi Arabia.
  * Allows users to select a trail marker.

* **Trail Details**

  * Displays trail description, region, difficulty, distance, duration, images, and official approval status.

* **Trail Route Map**

  * Displays the trail route with start and end points.

* **Authentication**

  * Registration.
  * Login.
  * Logout.

* **Favorites**

  * Allows registered users to save and remove favorite trails.

* **Reviews & Ratings**

  * Displays reviews and average ratings.
  * Allows registered users to submit ratings and comments.

* **User Profile**

  * Displays the user's saved trails and reviews.

* **Admin Dashboard**

  * Add, edit, and deactivate trails.
  * Upload trail images.
  * Update trail routes.
  * Manage user reviews.
