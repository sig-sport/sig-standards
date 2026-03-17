# SSR-1 — Club & Person Registry

**Status:** Draft  
**Version:** 0.1  
**Date:** March 2026  
**Editor:** DHB Digital Core Programme (DDC), SIG-DE  
**Sponsors:** DHB — Deutscher Hockey-Bund  
**Chapter:** SIG-DE (founding chapter)  
**License:** CC0 1.0 Universal — no rights reserved  
**Repository:** github.com/sig-sport/ssr (proposed)

---

> **This document is a DRAFT recommendation published for community review. It has not been formally adopted by the SIG Steering Council. Feedback and trial implementations are explicitly invited. To comment, open an issue at the repository above.**

---

## 1. Introduction

SSR-1 is the first Sports Standard Recommendation produced by the Sports Interoperability Group (SIG). It defines a minimal, transport-agnostic data model and a GraphQL API binding for two foundational entities in any sports federation system: **Clubs** and **Persons**, and the **Membership** relationship between them.

The goal is not to model everything a federation system does. It is to define the smallest possible shared vocabulary that allows independent systems to refer to the same club, the same person, and the same membership without ambiguity.

### 1.1 Motivation

Every sports federation system in use today represents clubs and persons differently. National federations, regional sports confederations, and Landessportbünde each maintain separate registries with incompatible identifiers, field names, and API shapes. The cost of this fragmentation falls on:

- Federations, who must maintain bespoke integrations with each vendor and each umbrella body
- Regional associations, who cannot efficiently synchronise membership data across federation boundaries
- Vendors, who must re-implement the same domain model for every customer
- Athletes and clubs, who must re-register in multiple systems with no portability of their own records

SSR-1 addresses one layer of this problem: it gives the ecosystem a shared vocabulary for what a Club and a Person are, and how Membership connects them.

### 1.2 Philosophy

- **Minimal by design.** SSR-1 specifies only the fields that every system must agree on. All other fields are extensions and outside this specification's scope.
- **Non-breaking evolution.** Once a field appears in an Accepted SSR, it is never removed — only deprecated with a migration path to a successor SSR.
- **Two bindings.** A canonical JSON field model and a GraphQL schema. REST implementations SHOULD follow the JSON model. GraphQL implementations SHOULD follow the schema in section 5.
- **Voluntary adoption.** SSR-1 is a recommendation, not a mandate. Its value comes from adoption, not enforcement.

---

## 2. Scope

SSR-1 covers:

- The **Club** entity — an organised sports club affiliated with one or more federations or regional sports bodies
- The **Person** entity — a natural person in the context of a sports federation
- The **Membership** entity — a time-bounded link between a Person and a Club, with role and status

SSR-1 explicitly does **not** cover:

- Competition structures, fixtures, or results (reserved for SSR-2)
- Licenses and certifications (reserved for SSR-3)
- Performance, biometric, or medical data
- Financial or payment data
- Authentication and authorisation mechanisms beyond identifier conventions
- Venue or facility data

---

## 3. Identifier Convention

All SSR-1 entities use **UUIDv7** as their primary identifier. UUIDv7 is defined in RFC 9562 (2024). It embeds a millisecond-precision Unix timestamp in the most-significant bits, making identifiers:

- Time-sortable without additional metadata
- Database-index-friendly (sequential inserts, reduced fragmentation)
- Globally unique without coordination

### 3.1 Format

Identifiers MUST be stored and transmitted as lowercase hyphenated strings:

```
019587a2-1c3f-7e4b-9d2a-4f8c1b3e6a7d
```

Systems MUST NOT derive semantic meaning from the identifier string itself.

### 3.2 Namespace metadata

Because SSR-1 identifiers are globally opaque, origin context is carried as separate metadata fields — not encoded into the ID. Every entity MUST include:

| Field | Type | Description |
|---|---|---|
| `namespace` | String | Reverse-DNS federation identifier, e.g. `de.dhb` |
| `localRef` | String (optional) | Legacy or internal identifier in the issuing system |

The namespace follows reverse-DNS convention: ISO 3166-1 alpha-2 country code, then federation abbreviation. Examples:

| Namespace | Organisation |
|---|---|
| `de.dhb` | Deutscher Hockey-Bund |
| `de.dfb` | Deutscher Fußball-Bund |
| `de.dosb` | DOSB umbrella registry |
| `at.oehv` | Österreichischer Hockey Verband |
| `gb.englandhockey` | England Hockey |

> **Note:** The `localRef` field exists to preserve continuity with legacy systems during migration. It is informational only and carries no semantic guarantee across systems. Two records with the same `localRef` in different namespaces are not assumed to refer to the same real-world entity.

---

## 4. Entity Definitions

### 4.1 Club

A Club is an organised sports entity — typically a registered association — that is affiliated with one or more federations or regional sports bodies.

| Field | Type | Required? | Description |
|---|---|---|---|
| `id` | UUIDv7 | Required | Globally unique identifier |
| `namespace` | String | Required | Reverse-DNS federation namespace |
| `localRef` | String | Optional | Legacy identifier in the issuing system |
| `name` | String | Required | Official registered name of the club |
| `shortName` | String | Optional | Abbreviated name for display (max 32 characters) |
| `foundedYear` | Int | Optional | Year of founding (YYYY) |
| `status` | ClubStatus | Required | See enum below |
| `address` | Address | Optional | Postal address of club headquarters |
| `federationRefs` | [FederationRef] | Required | One or more federation affiliations — see section 4.4 |
| `createdAt` | DateTime | Required | ISO 8601 timestamp of record creation |
| `updatedAt` | DateTime | Required | ISO 8601 timestamp of last modification |

**ClubStatus enum:**

```
ACTIVE | SUSPENDED | DISSOLVED
```

### 4.2 Person

A Person is a natural individual recorded in the federation system. The same physical person may have Person records in multiple federation namespaces. SSR-1 does not mandate deduplication across namespaces, but provides the fields necessary to attempt it.

| Field | Type | Required? | Description |
|---|---|---|---|
| `id` | UUIDv7 | Required | Globally unique identifier |
| `namespace` | String | Required | Reverse-DNS federation namespace |
| `localRef` | String | Optional | Legacy identifier in the issuing system |
| `givenName` | String | Required | Legal given name(s) |
| `familyName` | String | Required | Legal family name |
| `dateOfBirth` | Date | Conditional | ISO 8601 date (YYYY-MM-DD). Required for licensed athletes; optional otherwise |
| `gender` | GenderCode | Optional | See enum below |
| `nationality` | [String] | Optional | ISO 3166-1 alpha-2 country codes |
| `email` | String | Optional | Primary contact email |
| `status` | PersonStatus | Required | See enum below |
| `createdAt` | DateTime | Required | ISO 8601 timestamp of record creation |
| `updatedAt` | DateTime | Required | ISO 8601 timestamp of last modification |

**PersonStatus enum:**

```
ACTIVE | SUSPENDED | DECEASED | ANONYMISED
```

**GenderCode enum:**

```
MALE | FEMALE | DIVERSE | NOT_STATED
```

> **GDPR notice:** Personal data fields — `dateOfBirth`, `email`, `nationality` — are subject to data minimisation obligations under the EU General Data Protection Regulation and equivalent national legislation. Implementations MUST NOT expose these fields without an appropriate legal basis. Implementations SHOULD support field-level access scoping via the authorisation layer. Special care is required when the Person is a minor.

### 4.3 Membership

A Membership represents a time-bounded affiliation of a Person to a Club within a specific federation namespace. A person may hold multiple concurrent Memberships — for example, a club membership and a direct national federation membership simultaneously.

| Field | Type | Required? | Description |
|---|---|---|---|
| `id` | UUIDv7 | Required | Globally unique identifier |
| `namespace` | String | Required | Issuing federation namespace |
| `personId` | UUIDv7 | Required | Reference to Person.id |
| `clubId` | UUIDv7 | Required | Reference to Club.id |
| `role` | MembershipRole | Required | See enum below |
| `status` | MembershipStatus | Required | See enum below |
| `startDate` | Date | Required | ISO 8601 date — when membership became effective |
| `endDate` | Date | Optional | ISO 8601 date — null if current or open-ended |
| `seasonRef` | String | Optional | Human-readable season label, e.g. `2025/26` |
| `createdAt` | DateTime | Required | ISO 8601 timestamp of record creation |
| `updatedAt` | DateTime | Required | ISO 8601 timestamp of last modification |

**MembershipRole enum:**

```
ATHLETE | COACH | OFFICIAL | ADMINISTRATOR | VOLUNTEER | OTHER
```

**MembershipStatus enum:**

```
ACTIVE | INACTIVE | SUSPENDED | PENDING
```

### 4.4 Supporting Types

#### Address

| Field | Type | Required? | Description |
|---|---|---|---|
| `street` | String | Optional | Street name and number |
| `postalCode` | String | Optional | Postal code |
| `city` | String | Optional | City or municipality |
| `countryCode` | String | Optional | ISO 3166-1 alpha-2, e.g. `DE` |

#### FederationRef

A Club may be affiliated with multiple bodies simultaneously — for example, a club that is a member of both a national federation and a regional Landessportbund. Each affiliation is a FederationRef.

| Field | Type | Required? | Description |
|---|---|---|---|
| `namespace` | String | Required | Namespace of the affiliated federation |
| `localRef` | String | Optional | Club number assigned by that federation |
| `since` | Date | Optional | Date of affiliation |

---

## 5. GraphQL Binding

The following GraphQL schema is the normative binding for SSR-1 compliant GraphQL APIs. Implementations MUST expose all Required fields. Optional fields SHOULD be included where the data exists in the underlying system.

### 5.1 Scalars and enums

```graphql
scalar UUID       # UUIDv7 string, lowercase hyphenated
scalar DateTime   # ISO 8601 with timezone offset
scalar Date       # ISO 8601 date only (YYYY-MM-DD)

enum ClubStatus        { ACTIVE  SUSPENDED  DISSOLVED }
enum PersonStatus      { ACTIVE  SUSPENDED  DECEASED  ANONYMISED }
enum GenderCode        { MALE  FEMALE  DIVERSE  NOT_STATED }
enum MembershipRole    { ATHLETE  COACH  OFFICIAL  ADMINISTRATOR  VOLUNTEER  OTHER }
enum MembershipStatus  { ACTIVE  INACTIVE  SUSPENDED  PENDING }
```

### 5.2 Types

```graphql
type Address {
  street:      String
  postalCode:  String
  city:        String
  countryCode: String
}

type FederationRef {
  namespace: String!
  localRef:  String
  since:     Date
}

type Club {
  id:             UUID!
  namespace:      String!
  localRef:       String
  name:           String!
  shortName:      String
  foundedYear:    Int
  status:         ClubStatus!
  address:        Address
  federationRefs: [FederationRef!]!
  memberships:    MembershipConnection!
  createdAt:      DateTime!
  updatedAt:      DateTime!
}

type Person {
  id:          UUID!
  namespace:   String!
  localRef:    String
  givenName:   String!
  familyName:  String!
  dateOfBirth: Date
  gender:      GenderCode
  nationality: [String!]
  email:       String
  status:      PersonStatus!
  memberships: MembershipConnection!
  createdAt:   DateTime!
  updatedAt:   DateTime!
}

type Membership {
  id:        UUID!
  namespace: String!
  person:    Person!
  club:      Club!
  role:      MembershipRole!
  status:    MembershipStatus!
  startDate: Date!
  endDate:   Date
  seasonRef: String
  createdAt: DateTime!
  updatedAt: DateTime!
}

type MembershipEdge {
  node:   Membership!
  cursor: String!
}

type MembershipConnection {
  edges:    [MembershipEdge!]!
  pageInfo: PageInfo!
  total:    Int!
}

type PageInfo {
  hasNextPage:     Boolean!
  hasPreviousPage: Boolean!
  startCursor:     String
  endCursor:       String
}
```

### 5.3 Queries

```graphql
type Query {
  club(id: UUID!): Club
  clubs(
    namespace:    String
    status:       ClubStatus
    after:        String
    before:       String
    first:        Int
    last:         Int
    updatedSince: DateTime
  ): ClubConnection!

  person(id: UUID!): Person
  persons(
    namespace:    String
    status:       PersonStatus
    after:        String
    before:       String
    first:        Int
    last:         Int
    updatedSince: DateTime
  ): PersonConnection!

  membership(id: UUID!): Membership
}
```

> **Note on `updatedSince`:** This filter is required for efficient incremental synchronisation. Consuming systems MUST be able to poll for changes since their last sync timestamp without fetching the entire dataset. Implementations MUST support this parameter on all list queries.

### 5.4 Mutations

```graphql
type Mutation {
  createClub(input: CreateClubInput!):             ClubPayload!
  updateClub(id: UUID!, input: UpdateClubInput!):  ClubPayload!

  createPerson(input: CreatePersonInput!):               PersonPayload!
  updatePerson(id: UUID!, input: UpdatePersonInput!):    PersonPayload!

  createMembership(input: CreateMembershipInput!):                   MembershipPayload!
  updateMembership(id: UUID!, input: UpdateMembershipInput!):        MembershipPayload!
}

# All mutations return the entity or a structured error list — never a silent failure
type ClubPayload       { club:       Club        errors: [UserError!] }
type PersonPayload     { person:     Person      errors: [UserError!] }
type MembershipPayload { membership: Membership  errors: [UserError!] }

type UserError {
  field:   String
  message: String!
}
```

### 5.5 Subscriptions (RECOMMENDED)

Implementations SHOULD support the following subscriptions for real-time use cases such as live event check-in or membership verification at competition venues:

```graphql
type Subscription {
  clubUpdated(namespace: String):       Club!
  personUpdated(namespace: String):     Person!
  membershipUpdated(namespace: String): Membership!
}
```

---

## 6. Conformance

The key words MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY in this document are interpreted as described in RFC 2119.

| Level | Requirements |
|---|---|
| **SSR-1 Compatible** | Implements all Required fields for Club, Person, and Membership. Exposes the Query operations defined in section 5.3, including `updatedSince` on list queries. |
| **SSR-1 Complete** | SSR-1 Compatible, plus all Optional fields where the data exists, all Mutation operations from section 5.4. |
| **SSR-1 Full** | SSR-1 Complete, plus Subscription support (section 5.5) and field-level GDPR access scoping on sensitive Person fields. |

Conformance levels are self-declared. SIG does not operate a certification programme. Organisations are encouraged to publish their conformance level in their API documentation and to report confirmed implementations to the SIG repository so that the SSR can progress toward Review state.

---

## 7. Governance and Progression

SSR-1 is currently in **Draft** state. The path to Review and Acceptance is:

- **Draft → Review:** Two independent confirmed implementations, reported via the SIG repository. Only editorial changes permitted once Review is entered.
- **Review → Accepted:** Two-thirds majority vote of the SIG Steering Council.
- **Accepted:** Frozen. No further changes. Additions or breaking changes require SSR-2 or a new SSR number.

To participate:

- **Comment:** Open an issue at github.com/sig-sport/ssr
- **Implement:** Report your implementation via the repository — two confirmed implementations trigger the Review vote
- **Propose changes:** Fork the repository, submit a pull request with rationale, and post to the SIG mailing list

---

## 8. Design Notes

These notes are informational and not normative.

**Why UUIDv7 and not UUIDv4?** UUIDv4 is fully random, causing index fragmentation in relational databases at scale. UUIDv7 embeds a timestamp prefix, making inserts sequential and indexes compact, while retaining global uniqueness. UUIDv7 is standardised in RFC 9562 (2024) and is supported natively by all major database engines in current versions.

**Why namespace as a separate field?** Encoding federation identity into the ID itself (e.g. a URN scheme) makes IDs longer, harder to use as database primary keys, and requires all consumers to parse and validate the structure. Carrying namespace as a metadata field keeps IDs clean and portable while preserving full provenance information.

**Why FederationRef as an array on Club?** A sports club in Germany is almost never affiliated with only one body. It is simultaneously a national federation member, a Landessportbund member, and potentially a regional association member. Modelling affiliation as a list from the start avoids the retrofit problem that breaks single-vendor schemas when multi-affiliation requirements emerge later.

**Why `updatedSince` on list queries?** Without an incremental sync filter, any consuming system — an umbrella body import, a regional portal, a club-facing app — must fetch the entire dataset to detect changes. At federation scale this is impractical. Making `updatedSince` a named, required parameter forces implementations to support incremental synchronisation as a first-class capability.

---

## 9. Changelog

| Version | Date | Status | Notes |
|---|---|---|---|
| 0.1 | March 2026 | Draft | Initial publication for community comment. Authored by DHB DDC programme as founding contribution of SIG-DE. |

---

## Acknowledgements

SSR-1 was drafted by the DHB Digital Core (DDC) programme as part of the DHB SaaS migration initiative. It is submitted as the founding contribution of SIG-DE, the German chapter of the Sports Interoperability Group. Organisations wishing to join the SIG-DE founding circle or to establish a new chapter should contact the DHB DDC programme lead.

---

*SIG — Sports Interoperability Group · sig.sport (proposed) · github.com/sig-sport (proposed)*  
*SSR-1 is published under CC0 1.0 Universal. No rights reserved.*
