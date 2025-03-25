# Dating Server with Information Flow Control

A secure dating application server that matches users while preserving privacy through information flow control.

## Overview
This dating server implements a secure matching system that guarantees strong privacy properties:

- Privacy-Preserving Matching: User preferences remain confidential

- Controlled Information Sharing: Users determine what profile information is shared upon matching

- No Rejection Disclosure: Users never learn who rejected them

- Mutual Consent: Connections are established only when both parties express interest

## Implementation
Core Compenents are:
- Server: Manages profiles and orchestrates the matching process

- Matching Logic: Determines matches while maintaining information flow control

- Agent Functions: Represent user preference logic and control shared information

- Dispatcher Integration: Connects with a testing service that sends sample profiles

## Security Model
The server maintains strict information flow control through:

The system manages several critical information flows:
1. Profile -> Agent: When an agent evaluates another profile, the profile data flows into the agent's decision logic. Labels ensure this information can't leak beyond the agent's computation.
2. Agent -> Match Result: Each agent's preference (boolean) flows into the match result. The combined match result inherits labels from both preferences.
3. Match Result -> Notification: The match result only flows to users after explicit declassification and only when there's mutual interest.
4. Profile -> Filtered Profile: Each agent produces a filtered version of their profile to share upon matching. This filtered profile inherits labels from the original profile.
5. Filtered Profile -> Matched User: The filtered profile only flows to the matched user after declassification.

- Protected Computation Blocks:
    - Preference decisions occur in a secure context

    - Security labels from input data propagate to computation results

    - Prevents preference data from leaking outside the protected block

```   
let pini authority
    val (pref1, maybeProfile1) = agent1 profile2
    val (pref2, maybeProfile2) = agent2 profile1
    val secureMatch = pref1 andalso pref2
in 
```
- Controlled Declassification
    - Declassification happens only with proper authority

    - Only declassifies the minimum necessary information

    - Uses empty label {} as target to allow sharing with matched parties

```  
val declassifiedMatch = declassify (secureMatch, authority, `{}`)
val profile1ForPid2 = declassify (maybeProfile1, authority, `{}`)
val profile2ForPid1 = declassify (maybeProfile2, authority, `{}`) 
```  
- Privacy-Preserving Notification
    - Only mutual matches are notified

    - Each user only receives the filtered version of the other's profile

    - No information about rejections is ever disclosed

``` 
if declassifiedMatch then (
    send (pid1, ("NEWMATCH", profile2ForPid1));
    send (pid2, ("NEWMATCH", profile1ForPid2));
    printString ("Match found between " ^ name1 ^ " and " ^ name2)
) 
``` 

## Starting the server
` make dating-server `

## Server Protocol
The server responds to the following messages:

- Connection Test: ("CONNECT_TEST", clientPid)
    - Response: ("CONNECTION_OK", "Server connected")

- New Profile: ("NEWPROFILE", (profile, agentFn, pid))

    - Adds the profile to the database

    - Tests for matches with existing profiles

    - Notifies matched parties if mutual interest is found