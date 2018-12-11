```
shortname: 2/COSS
name: Consensus-Oriented Specification System
type: Meta
status: Draft
```

This document describes a consensus-oriented specification system (COSS) for building interoperable technical specifications. COSS is based on a lightweight editorial process that seeks to engage the widest possible range of interested parties and move rapidly to consensus through working code.

This specification is based on [unprotocols.org 2/COSS](https://rfc.unprotocols.org/spec:2/COSS/), on [BEP2/COSS](https://github.com/bigchaindb/BEPs/tree/master/2) and on [EIP1 - EIP Purpose and Guidelines](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md).

## Change Process
This document is governed by the [2/COSS](../2/README.md) (COSS).

## Language
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.

## Goals
The primary goal of COSS is to facilitate the process of writing, proving, and improving new technical specifications. A "technical specification" defines a protocol, a process, an API, a use of language, a methodology, or any other aspect of a technical environment that can usefully be documented for the purposes of technical or social interoperability.

A Marketplace specification is called **M**arketplace **E**nhancement **P**roposal, **MEP** henceforth.

COSS is intended to above all be economical and rapid, so that it is useful to small teams with little time to spend on more formal processes.

Principles:

* We aim for rough consensus and running code.
* MEPs are small pieces, made by small teams.
* MEPs should have a clearly responsible editor.
* The process should be visible, objective, and accessible to anyone.
* The process should clearly separate experiments from solutions.
* The process should allow deprecation of old MEPs.

MEPs should take minutes to explain, hours to design, days to write, weeks to prove, months to become mature, and years to replace.

MEPs have no special status except that accorded by the community.

The author of the MEP is responsible for building consensus within the community and documenting dissenting opinions.

## Architecture

### Types of MEP
There are three types of MEPs:
* A **Standard Track MEP** describes any change to network protocols, transaction validity rules, proposed application standards/conventions, or any change or addition that affects the interoperability of applications using OceanProtocol products.
* An **Informational MEP** describes a OceanProtocol design issue, or provides general guidelines or information to the OceanProtocol community, but does not propose a new feature. Informational MEPs do not necessarily represent OceanProtocol community consensus or a recommendation, so users and implementers are free to ignore Informational MEPs or follow their advice.
* A **Meta MEP** describes a process surrounding OceanProtocol or proposes a change to a process.

### MEP Format
A MEP is a set of Markdown documents (the main file SHOULD be called `README.md`), together with comments, attached files, and other resources. A MEP is identified by its number and short name (e.g. this MEP is **2/COSS**). The number of the MEP is also the name of the directory where its files are stored.

Every MEP (including branches) carries a different number. New versions of the same MEP have new numbers.

### MEP template
Each MEP MUST customize and include this header:
````
```
shortname: [number/shortname]
name: [Full name of the MEP]
type: [standard | informational | meta ]
status: [raw | draft | stable | deprecated | retired | deleted]
editor: [Editor Name <email address>]
contributors: [Optional Contributor 1 <email address>, ..., Optional Contributor N <email address>]
```
````
_Note: the `number` is assigned after a MEP has been submitted._

Each MEP SHOULD include the following sections:

1. **Abstract**. The abstract is a short (~200 word) description of the technical issue being addressed.

1. **Motivation**. The motivation is critical for MEPs that want to change the OceanProtocol protocol. It should clearly explain why the existing protocol MEP is inadequate to address the problem that the MEP solves. MEP submissions without sufficient motivation may be rejected outright.

1. **Specification**: The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations. It MAY describe the impact on data models, API endpoints, security, performance, end users, deployment, documentation, and testing.

1. **Rationale**. The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.

1. **Backwards Compatibility**. All MEPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The MEP must explain how the author proposes to deal with these incompatibilities. MEP submissions without a sufficient backwards compatibility treatise may be rejected outright.

1. **Implementation**. The implementations must be completed before any MEP is given status "stable", but it need not be completed before the MEP is accepted. While there is merit to the approach of reaching consensus on the MEP and rationale before writing code, the principle of "rough consensus and running code" is still useful when it comes to resolving many discussions of API details.

1. **Copyright Waiver**. Except for 1/C4 and 2/COSS, all MEPs MUST be released to the public domain. The following waiver SHOULD be used: _To the extent possible under law, the person who associated CC0 with this work has waived all copyright and related or neighboring rights to this work._


## COSS Lifecycle
Every MEP has an independent lifecycle that documents clearly its current status.

A MEP has six possible states that reflect its maturity and contractual weight:

![Lifecycle diagram](lifecycle.png)

### Raw MEPs
All new MEPs are **raw** MEPs. Changes to raw MEPs can be unilateral and arbitrary. Those seeking to implement a raw MEP should ask for it to be made a draft MEP. Raw MEPs have no contractual weight.

### Draft MEPs
When raw MEPs can be demonstrated, they become **draft** MEPs. Changes to draft MEPs should be done in consultation with users. Draft MEPs are contracts between the editors and implementers.

### Stable MEPs
When draft MEPs are used by third parties, they become **stable** MEPs. Changes to stable MEPs should be restricted to cosmetic ones, errata and clarifications. Stable MEPs are contracts between editors, implementers, and end-users.

### Deprecated MEPs
When stable MEPs are replaced by newer draft MEPs, they become **deprecated** MEPs. Deprecated MEPs should not be changed except to indicate their replacements, if any. Deprecated MEPs are contracts between editors, implementers and end-users.

### Retired MEPs
When deprecated MEPs are no longer used in products, they become **retired** MEPs. Retired MEPs are part of the historical record. They should not be changed except to indicate their replacements, if any. Retired MEPs have no contractual weight.

### Deleted MEPs
Deleted MEPs are those that have not reached maturity (stable) and were discarded. They should not be used and are only kept for their historical
value. Only Raw and Draft MEPs can be deleted.

## Editorial control
A MEP MUST have a single responsible editor, the only person
who SHALL change the status of the MEP through the lifecycle stages.

A MEP MAY also have additional contributors who contribute changes to it. It is RECOMMENDED to use the [C4 process](../1/README.md) to maximize the scale and diversity of contributions.

The editor is responsible for accurately maintaining the state of MEPs and for handling all comments on the MEP.

## Branching and Merging
Any member of the domain MAY branch a MEP at any point. This is done by copying the existing text, and creating a new MEP with the same name and content, but a new number. The ability to branch a MEP is necessary in these circumstances:

* To change the responsible editor for a MEP, with or without the cooperation of the current responsible editor.
* To rejuvenate a MEP that is stable but needs functional changes. This is the proper way to make a new version of a MEP that is in stable or deprecated status.
* To resolve disputes between different technical opinions.

The responsible editor of a branched MEP is the person who makes the branch.

Branches, including added contributions, SHOULD be dedicated to the public domain using CC0 (just like the original MEP). This means that contributors are guaranteed the right to merge changes made in branches back into their original MEPs.

Technically speaking, a branch is a *different* MEP, even if it carries the same name. Branches have no special status except that accorded by the community.

## Conflict resolution
COSS resolves natural conflicts between teams and vendors by allowing anyone to define a new MEP. There is no editorial control process except that practised by the editor of a new MEP. The administrators of a domain (moderators) may choose to interfere in editorial conflicts, and may suspend or ban individuals for behaviour they consider inappropriate.

## Conventions
Where possible editors and contributors are encouraged to:

* Refer to and build on existing work when possible, especially IETF specifications.
* Contribute to existing MEPs rather than reinvent their own.
* Use collaborative branching and merging as a tool for experimentation.

## License
Copyright (c) 2008-16 Yurii Rashkovskii <yrashk@gmail.com>, Pieter Hintjens <ph@imatix.com>, André Rebentisch <andre@openstandards.de>, Alberto Barrionuevo <abarrio@opentia.es>, Chris Puttick <chris.puttick@thehumanjourney.net>
Copyright (c) 2018 BigchainDB GmbH
Copyright (c) 2018 OceanProtocol Foundation

This MEP is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

This MEP is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, see http://www.gnu.org/licenses.
