# Roadmap

## Continuously

- blogging every week about the advancement of the project
- posting on LinkedIn and Twitter
- eventually doing 1 or 2 tutorial / video explainer
- trying to speak at conferences

## Deadline #1: the 1st of July 2023 (~3 months of work)

Goals:
- Convince potential users, collaborators and investors of Privency's vision & viability

Non-goals:
- good software quality (successful or not, the PoC will be thrown away)
- high number of features
- beautiful design
- production-grade security (though we must convince that Privency *can* be secure, given enough engineering time)

How?:
- create an open-source proof-of-concept featuring key aspects of the project, that is freely usable online

PoC features:
- To showcase Privency capability, we want to make an app that only allow registering people over 18 that holds a French citizenship or a French residency card
- Each unique person can only create 1 account
- The app should not be able to know the personal info of the users - only that they pass the requirements
- this app will be a basic forum with restricted write access for French citizens over 18.
- anyone can start a poll, and people can vote

Assumptions and limitations:
- zkML would be too long to implement so we assume users hold a valid Verifiable Credential containing their personal ID data
- governments often use the "NIN" (national identification number) to uniquely identify a person in a given government
- in France, the NNI is the Social Security Number. 
- we assume that the users hold a Verifiable Credential issued by the French government containing the unique Social Security Number, the nationality, the first name, the last name and the age
- for simplification, we assume both users and issuers use did:key as did:method

Auth Server for web app:
- users create a message containing:
	- a zk proof of the hash of their Social Security Number, previously salted with an application-specific nounce ==> this will be their unique identifier, meaning if the two did:key has a VC containing the same data, they will only be able to create one account
	- a zk proof that they are over 18
	- their digital signature
- if this pass and is unique, then the server will allow registering a new account
- on login, repeat the same verification
- on successful login, the server will issue a UCAN to the front as a bearer token - then same as oauth2

Ethereum account abstraction:
- we need blockchain for voting
- an unlimited amount of Eth accounts can be claimed representing the same human through the same process as described above for the auth server. We need to be able to link the Ethereum accounts with the web server account. They all should be associated with the same unique anonymous ID.

Transparency:
- no doxxing of government ID is allowed, the verifiable credential remain private on the user's device
- we use IPFS, WNFS, UCAN, content-addressing, freeing data from server's silo and minimizing trust
- we use [Snapshot](https://docs.snapshot.org/) for voting, probably using [Ethereum Attestation Service](https://attest.sh/) ==> among the votes from the Ethereum accounts associated with the same user, only the last vote will count. We will use Gnosis Chain.

Marketing:
- Launch a short website to present the PoC

Note:
- https://www.senat.fr/lc/lc181/lc181_mono.html shows that it is illegal to use the Social Security Number in France for login outside of restricted services.
- it is not clear whether using a hash of a SSN is illegal: having knowledge of the hash doesn't give you knowledge of the SSN - it is cryptographically impossible.
- there are countries where it is legal 
- there are ways to identify people without this number. We would generate locally a unique deterministic number per human based on their ID and their unique biometrics characteristics. Instead of the SSN, one could imagine for the sake of the PoC that this number represents this value.

## Deadline #2: the 1st of October 2023 (~6 months of work)

Find solution for funding:
- grants (Ethereum Foundation? Filecoin Foundation? The European Commission? Others?)
- incubators (YC? Telecom ParisTech - Station F? Open-source-centric incubators? Others?)
- find early-adopters/customers willing to pay for custom development?

Expensive things to fund:
- zkML
- battle-tested account abstraction for crypto wallets
- production-grade equivalent of the authorization server/client created for the PoC
- production-grade equivalent of the PoC forum => Privency.org - the social network
