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
Trail "1" --> "0..*" Favorite : has

TrailService ..> Trail : searches and filters
Admin --> Trail : manages
Admin --> Review : moderates
```

---

# 3. Database Design

The system uses a relational database with the following main entities:

* `Users`
* `Trails`
* `Reviews`
* `Favorites`

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

| Field     | Type     | Key |
| --------- | -------- | --- |
| id        | INT      | PK  |
| userId    | INT      | FK  |
| trailId   | INT      | FK  |
| rating    | INT      |     |
| comment   | TEXT     |     |
| createdAt | DATETIME |     |

### Favorites

| Field     | Type     | Key    |
| --------- | -------- | ------ |
| userId    | INT      | PK, FK |
| trailId   | INT      | PK, FK |
| createdAt | DATETIME |        |

---

# 4. ER Diagram

The following ER diagram uses the traditional notation:

* **Rectangle** = Entity
* **Oval** = Attribute
* **Diamond** = Relationship

```mermaid
flowchart LR

    %% =====================
    %% USER ENTITY
    %% =====================

    U[USER]

    Uid((id))
    Uname((name))
    Uemail((email))
    Upass((password))
    Urole((role))

    U --- Uid
    U --- Uname
    U --- Uemail
    U --- Upass
    U --- Urole


    %% =====================
    %% TRAIL ENTITY
    %% =====================

    T[TRAIL]

    Tid((id))
    Tname((name))
    Tdesc((description))
    Tregion((region))
    Tdiff((difficulty))
    Tdistance((distance))
    Tduration((estimatedDuration))
    Timages((images))
    Tofficial((isOfficial))
    Tstart((startPoint))
    Tend((endPoint))
    Troute((routeCoordinates))
    Tstatus((status))
    Tcreated((createdAt))

    T --- Tid
    T --- Tname
    T --- Tdesc
    T --- Tregion
    T --- Tdiff
    T --- Tdistance
    T --- Tduration
    T --- Timages
    T --- Tofficial
    T --- Tstart
    T --- Tend
    T --- Troute
    T --- Tstatus
    T --- Tcreated


    %% =====================
    %% REVIEW ENTITY
    %% =====================

    R[REVIEW]

    Rid((id))
    Ruser((userId))
    Rtrail((trailId))
    Rrating((rating))
    Rcomment((comment))
    Rcreated((createdAt))

    R --- Rid
    R --- Ruser
    R --- Rtrail
    R --- Rrating
    R --- Rcomment
    R --- Rcreated


    %% =====================
    %% FAVORITE ENTITY
    %% =====================

    F[FAVORITE]

    Fuser((userId))
    Ftrail((trailId))
    Fcreated((createdAt))

    F --- Fuser
    F --- Ftrail
    F --- Fcreated


    %% =====================
    %% RELATIONSHIPS
    %% =====================

    RW{WRITES}
    RC{RECEIVES}

    FS{SAVES}
    FH{HAS}

    U --- RW
    RW --- R

    T --- RC
    RC --- R

    U --- FS
    FS --- F

    T --- FH
    FH --- F
```

### Relationships

* A **User** can write many **Reviews**.
* A **Trail** can receive many **Reviews**.
* A **User** can save many **Favorites**.
* A **Trail** can be saved by many users through **Favorites**.

---

# 5. Front-end Components

The front-end consists of the following main UI components:

### Home / Trail List

* Displays available hiking trails.
* Provides access to search, filtering, and sorting.

### Search Bar

* Allows users to search for a trail by name.

### Filter Component

* Filters trails by region.
* Filters trails by difficulty.

### Sort Component

* Sorts trails by highest rating.
* Sorts trails by newest.

### Saudi Map

* Displays hiking trails on a map of Saudi Arabia.
* Allows users to select a trail marker and preview its information.

### Trail Details

Displays:

* Trail name
* Description
* Region
* Difficulty
* Distance
* Estimated duration
* Images
* Official approval status

### Trail Route Map

* Displays the hiking route.
* Shows the start point and end point of the trail.

### Authentication

* Register
* Login
* Logout

### Favorites

* Allows registered users to add trails to favorites.
* Allows users to remove trails from favorites.
* Displays saved trails.

### Reviews & Ratings

* Displays user reviews.
* Displays average trail rating.
* Allows registered users to submit a rating and comment.

### User Profile

* Displays user information.
* Displays saved favorite trails.
* Displays the user's reviews.

### Admin Dashboard

* Add new trails.
* Edit existing trails.
* Deactivate trails.
* Upload trail images.
* Update trail routes.
* Manage user reviews.

```
```
