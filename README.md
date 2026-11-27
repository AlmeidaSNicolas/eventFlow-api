# EventFlow API

Backend for managing conferences and fairs: events, sessions, registrations with QR-code check-in, personal agendas and session reviews.

> 🚧 **Work in progress.** The project is being built in sprints, starting from a domain model and business rules written before the first line of code. See [Roadmap](#roadmap).

## Why this project

EventFlow is based on a software project scope for a mobile event management app (MoSCoW prioritization, sprints). Here the same domain is rebuilt as a backend service, to practice Spring Boot, JPA/Hibernate, Spring Security with JWT, tests and Docker.

## Domains

- **User**: a participant or an organizer.
- **Event**: a conference or fair, with start and end dates, location and status.
- **Session**: a talk or workshop inside an event, with room, speaker and seat capacity.
- **Registration**: a user's registration to an event, with a unique QR token and check-in time.
- **AgendaItem**: a session a registered participant added to their personal agenda.
- **Review**: a 1 to 5 rating and an optional comment, for a session.

## Business rules

1. Check-in happens only on the day of the event, and each QR code can be used once.
2. A personal agenda cannot contain overlapping sessions.
3. A session has a seat capacity, and it cannot be overbooked, even with concurrent requests.
4. Only participants who checked in can review a session, and only after the session has ended.
5. Only organizers can create and edit events.

## Design decisions

- **Feature-based packages** (`event/`, `session/`, `registration/`, `agenda/`, `review/`, `user/`): each feature has its own controller, service, repository and DTOs. Features talk to each other through the service, never through another feature's repository.
- **Registration is an entity**, not just a join table, because it has its own status, QR token and check-in time.
- **Agenda items point to a registration**, not directly to a user, so an agenda can only contain sessions of events the user is registered for.
- **Seat booking uses an atomic conditional update** (`reserved_seats < capacity`) instead of reading and then writing, so two simultaneous requests cannot both take the last seat.
- **Timestamps are `TIMESTAMPTZ`**, so an event in Madrid and a participant in Brazil see the same instant in their own time zone.

## Stack

Java 21 · Spring Boot 3 · Spring Web · Spring Data JPA (Hibernate) · PostgreSQL · Flyway · Bean Validation · Spring Security with JWT · springdoc (OpenAPI) · JUnit 5 · Mockito · Testcontainers · Docker Compose · GitHub Actions

## Roadmap

- [ ] **Sprint 0: Setup.** Project skeleton, Docker Compose with PostgreSQL, Flyway, data model diagram.
- [ ] **Sprint 1: Events and sessions.** CRUD, DTOs, validation, global error handling.
- [ ] **Sprint 2: Users.** Registration, login with JWT, roles.
- [ ] **Sprint 3: Registration and QR check-in.** Capacity and single-use QR rules, transactions.
- [ ] **Sprint 4: Personal agenda.** Overlap rule, queries.
- [ ] **Sprint 5: Reviews.** Check-in and end-of-session rules, average rating per session.
- [ ] **Sprint 6: Quality and delivery.** Integration tests, CI, deploy.

## Running locally

Setup instructions will be added in Sprint 0.
