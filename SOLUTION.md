# Detailed solution

On reaching ZKorum website when the MVP is ready, anyone can browse the existing public surveys and their results. Participating in surveys is restricted to users who can prove they hold the right credentials (eligibility). Eligibility is verified in a privacy-preserving way using zero-knowledge proof (ZKP). On the MVP, a survey on ZKorum is a unique multiple-choice question that allows respondents to write one accompanying comment. By default, surveys are public. The frontend explicitly displays cryptographic proof of the authenticity of the data, with external links to IPFS when necessary.

## How does ZKorum unlocks the possibility for surveys to be proposed from bottom-up?

When a social group is created, members agree on an authority (the Issuer) whose first task is to recognize its members (the Holders). Members receive credentials that they can show to anyone that desires to verify their membership (the Verifiers). This pattern is called the Issuer-Holder-Verifier pattern, and has been standardized by the W3C, especially with Verifiable Credentials (VC). This suite of technology is what enables bottom-up survey proposals to be technically possible.

In the MVP, ZKorum acts as a Verifier that allows any Holder who can prove ownership of a Verifiable Credential to create surveys that only other members of the community can reply to. Survey proposers can further restrict the participation eligibility to a subset of the members of their community, which technically translates into enforcing the voters to provide a specific type of Verifiable Presentation for their vote to count.

The exact mechanism that ensure voters anonymity is described in more details [here](https://github.com/zkorum/poc/tree/main/vc-flow#how-does-it-work).

To ensure that voters don’t inadvertently reveal their identity via their comments, ZKorum automatically anonymizes both the content and style of their speech on the client-side before sending the comment.

## How does ZKorum solves the technical trilemma?

(This is extremely work-in-progress and is not priority. The general idea is to rely on libp2p, IPLD, IPFS and federated servers.)

The usage of the decentralized identity primitives described above provides both privacy and proof of eligibility to our surveys. But how do we make sure that anyone can independently verify the authenticity of data and processing?

### Current chain of thought (not up to date)

ZKorum extensively uses IPLD to address voting data:

- 1 vote proposal = 1 CID representing the content (name of the question => 1 child CID, list of choices => N children CIDs, etc).
- the list of vote proposal CID is publicly pinned to IPFS by ZKorum
- the exact content of the vote proposal (values behind the CID of the name of the question and choices) is not pinned to IPFS
- the list of anonymized valid votes for each proposal is constantly pinned to IPFS by ZKorum (the only thing to anonymize are comments which are hashed to CIDs)
- the raw comments are not pinned to IPFS by ZKorum

ZKorum provides a libp2p node that anyone can run:

- you configure which vote proposal CID you're interested in
- it will listen to the corresponding libp2p topic
- anyone can publish valid vote in this topic directly instead of going through the UI.
- ZKorum runs a node that listen to all the vote proposal topics
- each time ZKorum receives a new valid vote, it updates its local database
- the local database containing each valid votes for each proposals is regularly pinned to IPFS (in anonymized form, see above)
- a valid vote is a vote signed by a DID that hold the eligible credentials. It is verified in a privacy-preserving way
