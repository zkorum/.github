# Roadmap

This roadmap is continuously updated.

## Note

The deadlines highly depend on whether or not this project remains a side-project or becomes a full-time occupation.

## Continuously

- blogging every week about the advancement of the project on [GitHub](https://github.com/privency/weekly-updates)
- posting short updates few times a week on LinkedIn and Twitter and post links to the blog posts
- eventually doing 1 or 2 tutorial / video explainer
- trying to speak at conferences

## Objective #1: proof-of-concept

Deadline
- if @nicobao works full-time on it ==> the 1st of July 2023 (~3 months of work)
- else, the 1st of October 2023 (~6 months of work)

Goals:
- Convince potential users, collaborators and investors of Privency's vision & technical viability

Non-goals:
- good software quality (successful or not, the PoC will be thrown away)
- high number of features
- beautiful design
- production-grade security (though we must convince that Privency *can* be secure, given enough engineering time)

How?:
- create an open-source proof-of-concept featuring key aspects of the project, that is freely usable online
- reach out to lawyers to understand whether the legal concerns can be addressed or not, in particular in France: the expected deliverables are documentation and blog posts summarizing the findings
PoC features:
- to showcase Privency capability, we want to make an app that only allow registering people over 18 that holds a French citizenship or a French residency card
- each unique person can only create 1 account
- the app should not be able to know the personal info of the users - only that they pass the requirements
- this app will be a basic forum with restricted write access for French citizens over 18
- anyone can start a poll, and people can vote
- this app will be the basis for the _Privency.org_ social network

Assumptions and limitations:
- we will use zkML to:
	- scan the French national ID or the passport locally and generate a Verifiable Credential. Only 1 document will be supported for now. Which one? TBD
	- confirm via video that the user is who he says he is
- for simplification, we assume both users and issuers use did:key as did:method
- there will be limitations and workarounds whenever we can. Which one? TBD

Auth for web app:
- use DID, WNFS, IPFS, UCAN
- some anonymous ID should probably be generated from the zkML algorithm, that will be used as unique identifier
- how exactly? TBD
- the goal is to try to extract an equivalent of an oauth2 auth server but UCAN-based => this server will be the basis for the _Privency.tech_ offering

Ethereum account abstraction:
- we need blockchain for trustless voting
- an unlimited amount of Eth accounts can be claimed representing the same human through the same process as described above for the auth server. We need to be able to link the Ethereum accounts with the web server account. They all should be associated with the same unique anonymous ID.

Transparency:
- no doxxing of government ID is allowed, the verifiable credential remain private on the user's device
- we use IPFS, WNFS, UCAN, content-addressing, so we're freeing data from server's silo and minimizing trust
- we use [Snapshot](https://docs.snapshot.org/) for voting, probably using [Ethereum Attestation Service](https://attest.sh/) ==> among the votes from the Ethereum accounts associated with the same user, only the last vote will count. We will use Gnosis Chain.

Deliverables:
- Launch the PoC at https://poc.privency.org and https://poc.privency.tech
- Have insights about the legal viability from reputable sources in the blog posts and documentation
- MPLv2-licensed (in tribute to [Pieter Hintjens](http://hintjens.com/)) source code of the poc at https://github.com/privency/poc

## Objective #2: find funding

Deadline:
- if @nicobao goes full-time on it ==> the 1st of October 2023 (~6 months of work).
- else if Privency remains a side project ==> unknown deadline as funding might be less necessary

If #1 is validated, @nicobao needs to find solution for funding:
- grants (Ethereum Foundation? Filecoin Foundation? The European Commission? Others?)
- incubators (YC? Telecom ParisTech - Station F? Open-source-centric incubators? Others?)
- find early-adopters/customers willing to pay for custom development?
- donation?
- keep/start working on it as a side-project?

Expensive things to fund:
- zkML
- battle-tested account abstraction for crypto wallets
- production-grade equivalent of the authorization server/client created for the PoC
- production-grade equivalent of the PoC forum => Privency.org - the social network

## Objective #3: MVP

If #1 is validated and #2 is secured, we need to validate who our users are and what they want in order to build a useful product. 

No deadline, as it is too far-stretched for now.
