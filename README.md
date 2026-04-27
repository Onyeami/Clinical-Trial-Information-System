# **Clinical Trial Participant Tracker**

## Statement of the Problem
Clinical trials at hospitals involve managing large volumes of sensitive participant data across multiple trial phases. Currently, many smaller research departments rely on spreadsheets or paper-based records to track participant enrolment, phase progression, follow-up schedules, and adverse event reporting. This approach is error-prone, difficult to query, and makes it hard to monitor participant safety in real time. There is a clear need for a dedicated information system that centralises trial and participant data, enforces data integrity, and supports the full lifecycle of a clinical trial from enrolment through to completion or withdrawal.

## Aim
- To design and implement a proof-of-concept web-based Clinical Trial Participant Tracking System
- enabling research staff to manage trials
- enrol participants
- record phase progressions
- schedule check-ins through a structured React frontend communicating with an Express REST API backed by a SQLite database.

## Objectives
To achieve the aim, the system will accomplish the following: 
- design a normalised relational database schema capturing trials, participants, phases, enrolments, and check-ins
- implement a full REST API with CRUD endpoints for each entity
- build a React frontend allowing staff to interact with all data without page refreshes
- implement search and filter functionality across participants and trials
- validate all inputs on both frontend and backend
- write unit tests for key CRUD functions plus one integration test covering the full frontend-to-database flow.

## Functional Requirements
1. __Trial management__: staff must be able to create a new clinical trial record with a title, description, lead researcher, start date, end date, and current status (recruiting, active, completed, suspended). Trials must be readable individually and as a full list. Trial details must be updatable, and trials must be deletable when no participants are enrolled.

2. __Participant management__: staff must be able to register a new participant with first name, last name, date of birth, PPS number, contact phone, email, and consent status. Participants must be searchable by name or PPS number. Participant records must be updatable and soft-deletable (flagged as withdrawn rather than hard deleted, to preserve audit history).

3. __Phase management__: Each trial is divided into phases (e.g. Phase I, Phase II, Phase III). Staff must be able to create, read, update and delete phases belonging to a specific trial, including a phase name, description, and expected duration in weeks.

4. __Enrolment management__: staff must be able to enrol a participant into a specific trial phase, recording the enrolment date and enrolment status (enrolled, completed, withdrawn, adverse event). Staff must be able to update enrolment status and view all enrolments per trial or per participant. 

5. __Check-in scheduling and recording__: staff must be able to schedule check-ins for an enrolled participant, record a scheduled date, check-in type (in-person, remote), and notes. When a check-in occurs, staff must be able to update it with an actual date, outcome (attended, missed, rescheduled), and clinical notes.

6. __Reporting and filtering__: staff must be able to filter appointments by trial, by phase, by date range, and by enrolment status. A summary view per trial must show total enrolled, total completed, total withdrawn, and upcoming check-ins.

## Non-Functional Requirements
1. __Performance__: all API responses must return within 500ms for standard CRUD operations under normal load conditions.

2. __Usability__: the interface must be operable without technical training; all forms must include clear labels, validation feedback, and confirmation messages on successful actions.

3. __Security__: PPS numbers must never be exposed in API list responses; they are only returned when a single participant record is explicitly requested. All inputs must be sanitised on the backend to prevent SQL injection.

4. __Data integrity__: the database must enforce foreign key constraints. A participant cannot be enrolled in the same trial twice simultaneously. A trial cannot be deleted while active enrolments exist.
Reliability: the system must handle invalid API requests gracefully, returning appropriate HTTP status codes (400 for bad input, 404 for not found, 409 for conflicts such as duplicate enrolment) with descriptive error messages.

5. __Maintainability__: all backend routes must be separated by entity into individual Express router files. All frontend API calls must be centralised in a single api.js module so that the base URL can be changed in one place. 

