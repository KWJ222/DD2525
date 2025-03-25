# DD2525

Dating Server with Information Flow Control
A secure dating application server that matches users while preserving privacy through information flow control.

Overview
This dating server implements a secure matching system that guarantees strong privacy properties:

Privacy-Preserving Matching: User preferences remain confidential

Controlled Information Sharing: Users determine what profile information is shared upon matching

No Rejection Disclosure: Users never learn who rejected them

Mutual Consent: Connections are established only when both parties express interest

The implementation leverages Troupe's information flow control mechanisms to enforce these privacy guarantees.

Implementation
Core Components
Server: Manages profiles and orchestrates the matching process

Matching Logic: Determines matches while maintaining information flow control

Agent Functions: Represent user preference logic and control shared information

Dispatcher Integration: Connects with a testing service that sends sample profiles

Key Data Structures
Profile: (label, name, age, gender, interests) tuple

Profile Data: (profile, agent, pid) tuple containing:

The user's profile information

Their matching agent function

Their process ID for notifications

Agent Function: Takes another profile and returns (boolean, filteredProfile) indicating preference and shareable profile data

Security Model
Information Flow Control
The server maintains strict information flow control through:

Protected Computation Blocks