# SIG Standard Recommendations

According to the [SIG Workflow Bylaw][workflow] each SSR has a status as it is being worked on. Once a proposal has passed the Entrance Vote it will be listed here as "Draft". Unless a SSR is marked as "Accepted" it is subject to change. Draft can change drastically, but Review will only have minor changes.

As also described in the [SSR Workflow Bylaw][workflow]. The Editor, or editors, of a proposal are essentially the lead contributors and writers of the SSRs and they are supported by two voting members. Those voting members are the Coordinator who is responsible for managing the review stage and votes; and a second sponsor.

## Index by Status

### Accepted

| Num | Title                                | Maintainer                     |
|:---:|--------------------------------------|--------------------------------|
| 0   | [Placeholder][ssrX]                  |                                |


### Draft

| Num | Title                                | Editor(s)                      | Sponsor                        |
|:---:|--------------------------------------|--------------------------------|--------------------------------|
| 0   | [Placeholder][ssrX]                  |                                |                                |


### Proposed

| Num | Name                   | Scope | Depends on | Horizon |
|:---:|------------------------|-------|------------|---------|
| 1   | Club & Person Registry | Club, Person, and Membership entities. UUIDv7 identifiers. Namespace convention. | — | Now |
| 2   | License & Certification | Playing licenses, coaching licenses, referee certifications. Issue date, expiry, scope, status. Eligibility verification contract. | SSR-1 | Now |
| 3   | Competition Structure | Season, League, Division, Round, Group. The containers that fixtures live inside. Structure only — no results. | SSR-1 | Now |
| 4   | Match Event Model | MatchState, MatchEvent, EventType enum. Sport-agnostic core events (goal, card, timeout, substitution, period). Namespaced extension mechanism for sport-specific events. | SSR-3 | Now |
| 5   | Live Match Feed | Streaming contract for real-time event delivery. GraphQL subscriptions over graphql-ws (primary), SSE fallback, cursor-based poll endpoint. At-least-once delivery with resumption. | SSR-4 | Now |
| 6   | Fixture & Result | A scheduled match between two participants: venue, time, lineups, final score, outcome. The settled record after a match is complete. | SSR-3, SSR-4 | Now |
| 7   | Venue & Facility | Physical location where sport is played. Address, capacity, surface type, accessibility metadata. Alignment with OpenActive event discovery. | — | Near-term |
| 8   | Official & Role Assignment | Referees, match commissioners, technical delegates. Person-to-fixture assignment with role, scope, and status. Referee management system integration. | SSR-1, SSR-6 | Near-term |
| 9   | Disciplinary Record | Sanctions, suspensions, bans. Linked to Person and optionally Fixture. Cross-border recognition of sanctions. GDPR-sensitive: strict access scoping required. | SSR-1, SSR-6 | Near-term |
| 10  | Transfer & Registration | Player transfers between clubs, loan agreements, international transfer certificates (ITC). Cross-namespace by nature. Replaces largely manual ITC processes. | SSR-1, SSR-2 | Near-term |
| 11  | Coaching & Education Record | Coaching qualifications, completed courses, CPD history. Pathway from grassroots coach education to elite licensing. UEFA coaching licence alignment target. | SSR-1, SSR-2 | Long-term |
| 12  | Athlete Data Portability Profile | Standard export package for GDPR Article 20 data portability requests. Aggregates identity, memberships, licenses, competition history, and disciplinary record into a single structured portable file. | SSR-1, SSR-2, SSR-6, SSR-9 | Long-term |
| 13  | Event & Activity Discovery | Public-facing schema for come-and-try sessions, training camps, open events. Designed for OpenActive alignment. Grassroots participation pipeline integration. | SSR-7 | Long-term |

#### Horizon definitions

| Horizon   | Meaning                                                                               |
|:---------:|---------------------------------------------------------------------------------------|
| Now       | Draft to be published within 12 months of SIG-DE founding                             |
| Near-term | Target drafting within 1–3 years, subject to adoption and working group formation     |
| Long-term | On the roadmap; drafting begins once prerequisite SSRs reach Review or Accepted state |

#### Dependency map

```
SSR-1  Club & Person Registry
├── SSR-2  License & Certification
│   ├── SSR-10  Transfer & Registration
│   └── SSR-11  Coaching & Education Record
├── SSR-3  Competition Structure
│   ├── SSR-4  Match Event Model
│   │   ├── SSR-5  Live Match Feed
│   │   └── SSR-6  Fixture & Result
│   │       ├── SSR-8  Official & Role Assignment
│   │       └── SSR-9  Disciplinary Record
│   │           └── SSR-12  Athlete Data Portability Profile
│   └── SSR-6  (see above)
└── SSR-9  (see above)

SSR-7  Venue & Facility  (independent)
└── SSR-13  Event & Activity Discovery

SER-1  Namespace Registry              → ships with SSR-1
SER-2  GDPR Implementation Guide       → ships with SSR-1
SER-3  GraphQL Best Practices          → ships with SSR-1
SER-4  Conformance Testing Guide       → follows SSR-2 and SSR-3
SER-5  Sport-Specific Event Profiles   → follows SSR-4
SER-6  Migration & Versioning Guide    → follows SSR-2 and SSR-3
SER-7  Athlete Data Rights Playbook    → follows SSR-12
```

### Abandoned

| Num | Title                                | Editor(s)                      |
|:---:|--------------------------------------|--------------------------------|
| 0   | [Placeholder][ssrX]                  |                                |

### Deprecated

| Num | Title                                |     |
|:---:|--------------------------------------|-----|
| 0   | [Placeholder][ssrX]                  |     |

## Numerical Index

| Num | Title                                | Editor(s) / Maintainers        | Status     |
|:---:|--------------------------------------|--------------------------------|------------|
| 0   | [Placeholder][ssrX]                  |                                | Deprecated |