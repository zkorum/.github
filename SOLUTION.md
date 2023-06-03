# Detailed solution

On reaching ZKorum website when the MVP is ready, anyone can browse the existing public surveys and their results. Participating in surveys is restricted to users who can prove they hold the right credentials (eligibility). Eligibility is verified in a privacy-preserving way using zero-knowledge proof (ZKP). On the MVP, a survey on ZKorum is a unique multiple-choice question that allows respondents to write one accompanying comment. By default, surveys are public. The frontend explicitly displays cryptographic proof of the authenticity of the data, with external links to IPFS and Ethereum explorers when necessary.

## How does ZKorum unlocks the possibility for surveys to be proposed from bottom-up?

When a social group is created, members agree on an authority (the Issuer) whose first task is to recognize its members (the Holders). Members receive credentials that they can show to anyone that desires to verify their membership (the Verifiers). This pattern is called the Issuer-Holder-Verifier pattern, and has been standardized by the W3C, especially with Decentralized Identifiers, Verifiable Credentials (VC) and OpenID for Verifiable Credential Issuance. This suite of technology is what enables bottom-up survey proposals to be technically possible.

In the MVP, ZKorum acts as a Verifier that allows any Holder who can prove ownership of a Verifiable Credential to create surveys that only other members of the community can reply to. To ensure Holder’s anonymity, ZKorum expects Hyperledger AnonCreds as Verifiable Presentation. Hyperledger AnonCreds is a widely used VC format that adds privacy-protecting ZKP to the core VC issuance. Survey proposers can further restrict the participation eligibility to a subset of the members of their community, which technically translates into enforcing the voters to provide a specific type of Verifiable Presentation for their vote to count.

The unicity of voters is ensured by enforcing the presentation of a survey-specific hash calculated from the unique Holder’s AnonCred `link_secret` (and the associated ZKP). This technique makes sure that Holders’ votes on multiple surveys cannot be correlated to each other, and that Holders who try to DDoS ZKorum by abusing the voting endpoint can be suspended or banned from responding to that survey.

To propose a survey, Holders are required to present a hash calculated from their unique AnonCred `link_secret` using a method that can generate up to 10 different hashes per Holder. Using this technique, bad actors who try to abuse ZKorum by publishing hateful surveys or engaging in DDoS attacks can be suspended or banned. That means Holders have at most 10 unique pseudonymous identities that they can use to propose surveys. This number is not definitive. It boils down to a trade-off between ensuring the service resistance to cyber-attacks and maximizing users' privacy.
To ensure that voters don’t inadvertently reveal their identity via their comments, ZKorum automatically anonymizes both the content and style of their speech on the client-side before sending the comment.

The exact mechanism is described in more details [here (work-in-progress)](https://github.com/zkorum/poc/blob/main/anoncreds/README.md#how-does-it-work).

## How does ZKorum solves the technical trilemma?

The usage of the decentralized identity primitives described above provides both privacy and proof of eligibility to our surveys. But how do we make sure that anyone can independently verify the authenticity of data and processing?

ZKorum extensively uses IPLD to address voting data:

- 1 vote proposal = 1 CID representing the content (name of the question => 1 child CID, list of choices => N children CIDs, etc).
- the list of vote proposal CID is publicly pinned to IPFS by ZKorum
- the exact content of the vote proposal (values behind the CID of the name of the question and choices) is not pinned to IPFS
- the list of anonymyzed valid votes for each proposal is constantly pinned to IPFS by ZKorum (the only thing to anonymize are comments which are hashed to CIDs)
- the raw comments are not pinned to IPFS by ZKorum

ZKorum provides a libp2p node that anyone can run:

- you configure which vote proposal CID you're interested in
- it will listen to the corresponding libp2p topic
- anyone can publish valid vote in this topic directly instead of going through the interface.
- ZKorum runs a node that listen to all the vote proposal topics
- each time ZKorum receives a new valid vote, it updates its local database
- the local database containing each valid votes for each proposals is regularly pinned to IPFS (in anonymized form, see above)
- a valid vote is a vote signed by a DID that hold the eligible credentials. It is verified in a privacy-preserving way via Hyperledger AnonCreds
- optionally, the signed vote can contain a timestamp signed by ZKorum Public DID. It helps for analyzing vote results.

Casting a vote go through the following steps:

- the user receives a timestamp signed by ZKorum Public DID
- the user selects a survey, casts its vote and comment, and put it all in an enveloppe
- the user signs the enveloppe and generates its eligibility credentials in the form of an Hyperledger AnonCred via its local wallet
- the user sends it to ZKorum backend
- ZKorum backend verifies that everything is correct
- ZKorum stores the vote on its own DB
- ZKorum pins the anonymyzed version of the vote on the libp2p ZKorum node
