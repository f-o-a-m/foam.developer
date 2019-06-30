---
FIP: 1
title: FIP Purpose and Guidelines
status: draft
type: Meta
author: open_positions :)
        https://github.com/f-o-a-m/foam.developer/fips/fip-1.md
created: 2019-05-09
---

## What is an FIP?

FIP stands for FOAM Improvement Proposal. A FIP is a design document providing information to the FOAM community, or describing a new feature for FOAM or its processes or environment. The FIP should provide a concise technical and functional specification of the feature and a rationale for the feature. The FIP author is responsible for building consensus within the community and documenting dissenting opinions.

## FIP Rationale

We intend FIPs to be the primary mechanisms for proposing new features, for collecting community technical input on an issue, and for documenting the design decisions that have gone into FOAM. Because the FIPs are maintained as text files in a versioned repository, their revision history is the historical record of the feature proposal.

For FOAM implementers, FIPs are a convenient way to track the progress of their implementation. Ideally each implementation maintainer would list the FIPs that they have implemented. This will give end users a convenient way to know the current status of a given implementation or library.

## FIP Types

There are three types of FIP:

**Standard Track** describes any change that affects FOAM implementations, such as a change to an interface or protocol-level entity like a smart contract. Standard Track FIPs may also propose application standards or conventions, or include any change or addition that affects the interoperability of applications using FOAM. Furthermore Standard FIPs can be broken down into the following categories. Standards Track FIPs consist of three parts, a design document, implementation, and finally if warranted an update to the [formal specification].
- **TCR Parameter Adjustment** - describes a proposal to initiate a vote on one or more of the five community-governed TCR parameters. These proposals inform FOAM stakeholders of a perceived flaw in a TCR parameter, and provide data-driven arguments for specific quantitative increases or decreases in the parameter(s). Note that TCR parameter adjustment votes can be initiated by anyone at anytime regardless of FIP status, but may be considered more valid by the community if first distributed as an FIP.
- **Smart Contract Upgrade** - describes a change to a map (sPoL) or location (dPoL) smart contract. These will require new contract(s) to be written, tested and deployed to mainnet.

**Informational** FIP describes a FOAM design issue, or provides general guidelines or information to the FOAM community, but does not propose a new feature. Informational FIPs do not necessarily represent FOAM community consensus or a recommendation, so users and implementers are free to ignore Informational FIPs or follow their advice.
**Meta** FIP describes a process surrounding FOAM or proposes a change to (or an event in) a process. Process FIPs are like Standard Track FIPs, but apply to areas other than the FOAM protocol itself. They may propose an implementation, but not to FOAM's codebase. They often require community consensus. Unlike Informational FIPs, they are more than recommendations, and users are typically not free to ignore them. Examples include procedures, guidelines, changes to the decision-making process, and changes to the tools or environment used in FOAM development. Any meta-FIP is also considered a process FIP.

It is highly recommended that a single FIP contain a single key proposal or new idea. The more focused the FIP, the more successful it tends to be. A change to one client doesn't require an FIP; a change that affects multiple clients, or defines a standard for multiple apps to use, does.

A FIP must meet certain minimum criteria. It must be a clear and complete description of the proposed enhancement. The enhancement must represent a net improvement. FIPs should describe a change to the FOAM protocol in terms of specific goals (ends), and specifications (means) aimed at achieving those goals. Proposals should, at a minimum, describe the functional changes proposed in their entierety. They may also detail technical revisions to the contract code. 

## FIP Process Flow

Parties involved in the process are you, the champion or *FIP author*, the [*FIP editors*](#FIP-editors), the [*FOAM Core Developers*] or other *independent developers*.

:warning: Before you begin, vet your idea. First, ask the FOAM community (via Discourse or another channel) if an idea is original and sound. This will allow you to avoid wasting time on something that may be rejected based on prior research or discussion. Doing this also helps to make sure the idea is applicable to the entire community instead of just the author. Just because an idea sounds good to the author does not mean it will work for most people in most areas where FOAM is used. [FOAM Discourse](https://discourse.foam.space/) or a FOAM Community Workshop Call are currently the best places to discuss your proposal with the community and begin creating more formalized language around your FIP. [The Issues section of this repository] is the appropriate place to post the FIP specification, once you have drafted it.

Your role as the champion is to write the FIP using the style and format described below, shepherd the discussions in the appropriate forums, and build community consensus around the idea. Following is the process that a successful FIP will move along:

```
[ WIP ] -> [ DRAFT ] -> [ LAST CALL ] -> [ ACCEPTED ] -> [ COMPLETED ]
```

**WORK IN PROGRESS (WIP)**- champion drafts the specification before releasing it to the community for review. May be proposed and discussed on FOAM Discourse and/or on FOAM Community Workshop Calls before being drafted into a specification.
  * :arrow_right: -- *DRAFT* -- FIP editor will assign the FIP a number (generally the issue or PR number related to the FIP) and merge your pull request. The FIP editor will not deny an FIP at this stage.
  
**DRAFT**- champion releases draft-version of FIP specification to the [the Issues section of this repository] as a  [pull request]. Others have the opportunity to review and propose any changes to the specification. May again be discussed by the community on FOAM Discourse or in FOAM Community Workshop Calls.
  * :x: -- A request for Last Call status will be denied if material changes are still expected to be made to the draft. FIPs should only enter *Last Call* once.
  * :arrow_right: -- *LAST CALL* -- FIP editor will assign *Last Call* status and set a review end date (`review-period-end`), normally the next FOAM Community Workshop Call. 
  
**LAST CALL**- finalized version is brought to a FOAM Community Call and argued for by the champion. Vote takes place on the call to assess community support for the FIP. These FIPs will be listed prominently on the https://fips.foam.space/ website.
  * :x: -- A request which results in material changes or substantial unaddressed technical complaints will cause the FIP to revert to *Draft*. A *Last Call* which does not pass the formal community vote with X% of FOAM Community Call participants in favor of the FIP will revert to *Draft* or be denied.
  * :arrow_right: -- *ACCEPTED* (Core FIPs only) -- A successful *Last Call* without material changes or unaddressed technical complaints that also passes the formal community vote with X% of FOAM Community Call participants in favor of the FIP will become *Accepted*.
  * :arrow_right: -- *COMPLETED* -- (Non-core FIPs: Informational or Meta) -- A successful *Last Call* without material changes or unaddressed technical complaints will become *Completed*.
  
**ACCEPTED**- an FIP is regarded as *Accepted* by the community if X% of FOAM Community Call participants agree that the FIP should be implemented. At this point, the relevant stakeholders in the FOAM ecosystem should move forward with making any accomodations to their systems or processes as they relate to this FIP. If the FIP is for a community governance vote on a change to a TCR parameter, then a vote may be initiated at this stage.
  * :arrow_right: -- *COMPLETED* -- (Non-core FIPs: Informational or Meta) -- A successful *Accepted* that has been fully implemented to mainnet will become *Completed*.
  
**COMPLETED**- FIP has been developed into code, tested, and implemented to production.

Each status change is requested by the FIP author and reviewed by the FIP editors. (Note that this begins with the change from *Draft* to *Last Call*, since *WIP* to *Draft* represents the initial creation of the FIP and post to the Issues tab on Github). Use a pull request to update the status. You may include a link to where people should continue discussing your FIP online. The FIP editors will process these requests as per the conditions above.

Other exceptional statuses include:

* **DEFERRED** -- This is for core FIPs that have been put off for the future.
* **REJECTED** -- An FIP that does not receive the required percentage of votes in favor via formal vote in a FOAM Community Call is considered *Rejected*
* **ACTIVE** --  Some Informational and Process FIPs may have a status of *Active* if they are never meant to be *Completed*. E.g. FIP 1 (this FIP). This is similar to *Completed*, but denotes an FIP which may be updated without changing its FIP number.

## What belongs in a successful FIP?

Each FIP should have the following parts:

- **Preamble** - header containing metadata about the FIP, including the FIP number (ex. #0000), a short descriptive title (limited to a maximum of 44 characters), the author name or alias, and other details (See [below](#FIP-header-preamble) for details).

- **Summary** - “If you can’t explain it simply, you don’t understand it well enough.” Provide a simplified and layman-accessible explanation of the FIP, and the issue the FIP is meant to address.

- **Motivation** - should clearly explain the problem, and why the existing protocol specification is inadequate to address the problem that the FIP is seeking to solve. FIP submissions without sufficient motivation may be rejected outright.

- **Proposal Specification** - The specification should at least functionally describe the fix for the issue outlined in the *Motivation* above. Technical syntax should be provided, if possible, by the FIP author and/or contributors, but should not be reqiured for an FIP to enter the FIP process or even become *Accepted*. (A status of *Completed*, however, requires all technical details to be documented, developed and deployed to production mainnet.) The specification should be detailed enough to allow for competing fixes to the issue described under *Motivation*.

- **Backwards Compatibility** - All FIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The FIP must explain how these incompatibilities can be dealt with. If backwards incompatibilities exist, FIP submissions without a sufficient backwards compatibility treatise may be rejected outright.

- **Implementations** - The implementations must be completed before any FIP is given status *Completed*, but it need not be completed before the FIP is merged as draft. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of “rough consensus and running code” is still useful when it comes to resolving many discussions of API details.

- **Appendix** - may include research, report, data, test cases, community discussions, and/or other supporting documentation for the FIP.

- **Copyright Waiver** - All FIPs must be in the public domain. See the bottom of this FIP for an example copyright waiver.

## FIP Formats and Templates

FIPs should be written in [markdown] format.
Image files should be included in a subdirectory of the `assets` folder for that FIP as follow: `assets/FIP-X` (for FIP **X**). 

Note: When linking to an image in the FIP, use relative links such as `../assets/FIP-X/image.png`. It is preferable that diagrams be generated as ascii such as [svgbob](https://github.com/ivanceras/svgbob). This allows git versioning and collaboration with a renderable high-resolution image.


## FIP Header Preamble

Each FIP must begin with a header preamble, preceded and followed by three hyphens (`---`). The headers must appear in the following order. Headers marked with "*" are optional and are described below. All other headers are required.

` FIP:` <FIP number> (this is determined by the FIP editor)

` title:` <FIP title>

` author:` <a list of the author's or authors' name(s) and/or username(s), or name(s) and email(s). Details are below.>

` * discussions-to:` \<a url pointing to the official discussion thread\>

` status:` <Work in Progress | Draft | Last Call | Accepted | Completed | Active | Deferred | Rejected>

`* review-period-end:` <date review period ends>

` type:` <Standard Track (TCR Parameter Adjustment, Contract Upgrade)  | Informational | Meta>

` * category:` <TCR Parameter Adjustment | Contract Upgrade>

` created:` <date created on>

` * updated:` <comma separated list of dates>

` * requires:` <FIP number(s)>

` * replaces:` <FIP number(s)>

` * superseded-by:` <FIP number(s)>

` * resolution:` \<a url pointing to the resolution of this FIP\>

Headers that permit lists must separate elements with commas.

Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

#### `author` header

The `author` header optionally lists the names, email addresses or usernames of the authors/owners of the FIP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the author header value must be:

> Random J. User &lt;address@dom.ain&gt;

or

> Random J. User (@username)

if the email address or GitHub username is included, and

> Random J. User

if the email address is not given.

#### `resolution` header

The `resolution` header is required for Standard Track FIPs only. It contains a URL that should point to an email message or other web resource where the pronouncement about the FIP is made.

#### `discussions-to` header

While an FIP is a draft, a `discussions-to` header will indicate the mailing list or URL where the FIP is being discussed. As mentioned above, examples for places to discuss your FIP include [FOAM topics on Gitter](https://gitter.im/FOAM/), an issue in this repo or in a fork of this repo.

No `discussions-to` header is necessary if the FIP is being discussed privately with the author.

As a single exception, `discussions-to` cannot point to GitHub pull requests.

#### `type` header

The `type` header specifies the type of FIP: Standards Track, Meta, or Informational. If the track is Standards please include the subcategory (core, networking, interface, or ERC).

#### `category` header

The `category` header specifies the FIP's category. This is required for standards-track FIPs only.

#### `created` header

The `created` header records the date that the FIP was assigned a number. Both headers should be in yyyy-mm-dd format, e.g. 2001-08-14.

#### `updated` header

The `updated` header records the date(s) when the FIP was updated with "substantional" changes. This header is only valid for FIPs of Draft and Active status.

#### `requires` header

FIPs may have a `requires` header, indicating the FIP numbers that this FIP depends on.

#### `superseded-by` and `replaces` headers

FIPs may also have a `superseded-by` header indicating that an FIP has been rendered obsolete by a later document; the value is the number of the FIP that replaces the current document. The newer FIP must have a `replaces` header containing the number of the FIP that it rendered obsolete.

## Appendix

FIPs may include auxiliary files such as diagrams, research, report, data, test cases, community discussions, and/or other supporting documentation for the FIP. Such files must be named FIP-XXXX-Y.ext, where “XXXX” is the FIP number, “Y” is a serial number (starting at 1), and “ext” is replaced by the actual file extension (e.g. “png”). 

Note: It is preferable that diagrams be generated as ascii such as [svgbob](https://github.com/ivanceras/svgbob). This allows git versioning and collaboration.

## Transferring FIP Ownership

It occasionally becomes necessary to transfer ownership of FIPs to a new champion. In general, retention of the original author as a co-author of the transferred FIP is preferable. Good reasons to transfer FIP ownership include the original author no longer having the time, interest, or ability to drive the FIP to conclusion.

If you are interested in assuming ownership of an FIP, send a message to the author asking to take over, addressed to both the original author and the FIP editor. If the original author doesn't respond to email in a timely manner, the FIP editor will make a unilateral decision (it's not like such decisions can't be reversed :)).

## FIP Editors

The current FIP editors are

` * John Smith (@twitter_address_here)`

## FIP Editor Responsibilities

FIP editors do the following:

- Review the FIP for soundness and completeness
- Provide editorial or supplementary inputs to enhance the FIP
- Verify that the FIP adheres to the formal FIP specifications, as outlined in this document, and has correct spelling, grammar, sentence structure, markup (Github flavored Markdown), code style, etc.

If the FIP isn't ready, the editor will send it back to the author for revision, with specific instructions. Once the FIP is ready for the repository, the FIP editor will:

- Assign an FIP number (generally the PR number or, if preferred by the author, the Issue # if there was discussion in the Issues section of this repository about this FIP)
- Merge the corresponding pull request
- Send a message back to the FIP author with the next steps

The ultimate decision as to whether or not an FIP should be included into FOAM as *Accepted* is decided by community vote. FIP editors merely provide administrative & editorial support (but are not excluded from casting votes as community members during FIP votes).

## History

This document was derived heavily from [EIP](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md), and [Bitcoin's BIP-0001] written by Amir Taaki which in turn was derived from [Python's PEP-0001]. In many places text was simply copied and modified. Although the PEP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David Goodger, they are not responsible for its use in the FOAM Improvement Process, and should not be bothered with technical questions specific to FOAM or the FIP. Please direct all comments to the FIP editors.

Enter history here...

See [the revision history for further details](https://github.com/f-o-a-m/foam.developer/master/FIPS/FIP-1.md), which is also available by clicking on the History button in the top right of the FIP.

### Bibliography

[one of the FOAM Gitter chat rooms]: https://gitter.im/FOAM/
[pull request]: https://github.com/f-o-a-m/foam.developer/pulls
[formal specification]: linked needed to technical whitepaper?
[the Issues section of this repository]: https://github.com/f-o-a-m/foam.developer/FIPs/issues
[markdown]: https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet


## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).
