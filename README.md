# Components, Classes, and Database Design

## 1. Back-end Classes

### User

**Attributes:**

* `id`
* `name`
* `email`
* `password`
* `role`
* `language`
* `currentLocation`

**Methods:**

* `register()`
* `login()`
* `logout()`
* `updateProfile()`
* `viewFavorites()`
* `addFavorite()`
* `removeFavorite()`
* `addReview()`
* `editReview()`
* `deleteReview()`
* `viewMyReviews()`
* `markTrailAsCompleted()`
* `viewCompletedTrails()`
* `getCurrentLocation()`

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
* `startPoint`
* `endPoint`
* `routeCoordinates`
* `safetyTips`
* `status`
* `createdAt`

**Methods:**

* `getDetails()`
* `getLocation()`
* `getRoute()`
* `getSafetyTips()`
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

### CompletedTrail

**Attributes:**

* `userId`
* `trailId`
* `completedAt`

**Methods:**

* `markAsCompleted()`
* `removeCompletedTrail()`
* `getCompletedTrails()`
* `isCompleted()`

### TrailService

**Attributes:**

* None

**Methods:**

* `searchByName()`
* `filterByRegion()`
* `filterByDifficulty()`
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
* `deleteTrail()`
* `uploadTrailImages()`
* `updateTrailRoute()`
* `deleteReview()`
* `suspendUser()`

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
    +string language
    +string currentLocation
    +register()
    +login()
    +logout()
    +updateProfile()
    +viewFavorites()
    +addFavorite()
    +removeFavorite()
    +addReview()
    +editReview()
    +deleteReview()
    +viewMyReviews()
    +markTrailAsCompleted()
    +viewCompletedTrails()
    +getCurrentLocation()
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
    +string startPoint
    +string endPoint
    +string routeCoordinates
    +string safetyTips
    +string status
    +datetime createdAt
    +getDetails()
    +getLocation()
    +getRoute()
    +getSafetyTips()
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

class CompletedTrail {
    +int userId
    +int trailId
    +datetime completedAt
    +markAsCompleted()
    +removeCompletedTrail()
    +getCompletedTrails()
    +isCompleted()
}

class TrailService {
    +searchByName()
    +filterByRegion()
    +filterByDifficulty()
    +clearFilters()
}

class Admin {
    +int id
    +string name
    +string email
    +string password
    +addTrail()
    +updateTrail()
    +deleteTrail()
    +uploadTrailImages()
    +updateTrailRoute()
    +deleteReview()
    +suspendUser()
}

User "1" --> "0..*" Review : writes
Trail "1" --> "0..*" Review : receives

User "1" --> "0..*" Favorite : saves
Trail "1" --> "0..*" Favorite : is saved in

User "1" --> "0..*" CompletedTrail : completes
Trail "1" --> "0..*" CompletedTrail : is completed by

TrailService ..> Trail : searches and filters
Admin --> Trail : manages
Admin --> Review : moderates
Admin --> User : suspends
```

---

# 3. Database Design

The system uses a relational database with the following tables:

### Users

| Field           | Type    | Key    |
| --------------- | ------- | ------ |
| id              | INT     | PK     |
| name            | VARCHAR |        |
| email           | VARCHAR | UNIQUE |
| password        | VARCHAR |        |
| role            | VARCHAR |        |
| language        | VARCHAR |        |
| currentLocation | VARCHAR |        |

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
| startPoint        | VARCHAR  |     |
| endPoint          | VARCHAR  |     |
| routeCoordinates  | TEXT     |     |
| safetyTips        | TEXT     |     |
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

### CompletedTrails

| Field       | Type     | Key                |
| ----------- | -------- | ------------------ |
| userId      | INT      | PK, FK → Users.id  |
| trailId     | INT      | PK, FK → Trails.id |
| completedAt | DATETIME |                    |

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
        VARCHAR language
        VARCHAR currentLocation
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
        VARCHAR startPoint
        VARCHAR endPoint
        TEXT routeCoordinates
        TEXT safetyTips
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

    COMPLETED_TRAILS {
        INT userId PK, FK
        INT trailId PK, FK
        DATETIME completedAt
    }

    USERS ||--o{ REVIEWS : writes
    TRAILS ||--o{ REVIEWS : receives

    USERS ||--o{ FAVORITES : saves
    TRAILS ||--o{ FAVORITES : has

    USERS ||--o{ COMPLETED_TRAILS : completes
    TRAILS ||--o{ COMPLETED_TRAILS : is completed by
```

---

# 5. Front-end Components

The main front-end components are:

* **Home / Trail List**

  * Displays available hiking trails.
  * Provides access to search and filters.

* **Search Bar**

  * Searches trails by name.

* **Filter Component**

  * Filters trails by region.
  * Filters trails by difficulty.

* **Saudi Map**

  * Displays hiking trails across Saudi Arabia.
  * Allows users to select a trail marker.
  * Displays the trail starting point.
  * Displays the user's current location with periodic updates.

* **Trail Details**

  * Displays trail description, region, difficulty, distance, duration, images, and safety tips.

* **Trail Route Map**

  * Displays the trail route with start and end points.

* **Authentication**

  * Registration.
  * Login.
  * Logout.
  * Prompts guests to sign up when they try to save, rate, or review a trail.

* **Favorites**

  * Allows registered users to save and remove favorite trails.

* **Reviews & Ratings**

  * Displays reviews and average ratings.
  * Allows registered users to submit ratings and comments.
  * Allows users to edit or delete their own reviews.

* **Completed Trails**

  * Allows registered users to mark trails as completed.
  * Displays the user's completed trails.

* **User Profile**

  * Displays the user's saved trails and reviews.
  * Allows users to edit their account information.

* **Share Trail**

  * Allows users to share a trail link with friends.

* **Language Switcher**

  * Allows users to switch between Arabic and English.

* **Admin Dashboard**

  * Add, edit, and delete trails.
  * Upload trail images.
  * Update trail routes.
  * Delete inappropriate reviews.
  * Suspend users.

```
