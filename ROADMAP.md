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
- Convince potential users, collaborators, investors and myself (!!!) of Privency's vision & technical viability

Non-goals:
- good software quality (successful or not, the PoC will be thrown away)
- high number of features
- beautiful design
- production-grade security (though we must convince that Privency *can* be secure, given enough engineering time)

How?:
- create an open-source proof-of-concept featuring key aspects of the project, that is freely usable online

PoC features:
- a forum - PoC of Privency.org
- 1 section corresponds to 1 organization (an Issuer) 
- each section has restricted write access and public read access 
- only people holding a VC from the specific Issuer can post/open poll/vote in the corresponding section
- 1 VC from the Issuer of the section = one unique person from this organization. The unicity is the sole responsibility of the Issuer, though we can provide tools to make this easier.
- the app should not be able to know the personal info of the users that may be included in the VC - only that they pass the requirements

Assumptions and limitations:
- for simplification, we assume both holders and issuers use did:key as did:method

Tool for Issuer
- tool to issue VCs to Holders: TBD
- Issuers must keep track of revoked/lost VC

Auth:
- DID + holding the right VC
- both VC and the DID themselves must not be revealed - use zkp and hashes
- tool to renew lost account?

Ethereum account abstraction:
- we need blockchain for trustless voting
- an unlimited amount of Eth accounts can be claimed representing the same human through the same process as described above for the auth server. We need to be able to link the Ethereum accounts with the web server account. They all should be associated with the same unique anonymous ID.

Privacy:
- the DID itself is not revealed, otherwise it would allow issuers to know who wrote what
- the verifiable credential always remain private on the user's device. We use selective disclosure.
- we anonymize data using hashes and other cryptographic techniques
- we may showcase anonymizing speech (rewrite post in a randomly generated style) via a ChatGPT API integration
- no default metadata privacy: we will not embed a mixnet for the sake of this PoC

Transparency:
- we use IPFS, content-addressing, so we're freeing data from server's silo and minimizing trust
- we use [Snapshot](https://docs.snapshot.org/) for gasless voting, probably using [Ethereum Attestation Service](https://attest.sh/) ==> among the votes from the Ethereum accounts associated with the same user, only the last vote will count. We will use Gnosis Chain.

Deliverables:
- Launch the PoC at https://poc.privency.org and https://poc.privency.tech
- Have insights about the legal viability from reputable sources in the blog posts and documentation
- (A)GPLv3-licensed source code of the poc at https://github.com/privency/poc

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
- production-grade equivalent of the off-chain tooling
- production-grade equivalent of the PoC forum => Privency.org - the social network

## Objective #3: MVP

If #1 is validated and #2 is secured, we need to validate who our users are and what they want in order to build a useful product. 

No deadline, as it is too far-stretched for now.
