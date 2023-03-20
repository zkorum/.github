# Privency

Privency is both:
- Privency.tech => a set of unopinionated software tools that attempt to solve the dilemma between:
	- PRIVacy, freedom and democracy on one side (example: protecting whistleblowers, proving that we are over 18 without giving away more information)
	- transparENCY, control and justice on the other side (example: preventing cyber-bullies or money launderers to stay anonymous and go unpunished)
- Privency.org built on top of Privacy.tech => the very first privacy-preserving social network where 1 user = 1 verified human, providing bottom-up e-democracy tools such as reliable polls and voting

Privency is a work-in-progress project that is actively built in public by @nicobao.

@nicobao will seek to sustain the development of Privency by finding an ethical and profitable funding/business model route.

Privency will always remain *100% open-source*.

The [Roadmap](./ROADMAP.md) is continuously updated.

## The problem we try to solve

On the web today there is no way to prove our humanity without going through a costly KYC process that reveals way too many personal info about us.

Most web apps do not use KYC because they want to respect user's privacy.

They use plain OAuth2/OpenID Connect instead which create an anonymous ID usually out of an e-mail address, but then, they struggle to:
- avoid bots and spams ([sybil attacks](https://en.wikipedia.org/wiki/Sybil_attack))
- limit account creation per real human - or per human that hold a specific property, such as their nationality - preventing any real possible form of e-democracy to emerge
- prove properties about these humans ("are they allowed in their juridisction to access my services?")
- reveal real human identity behind a specific user's action when the said action was against the law (harassment, money laundering, pedo-pornography...etc)

This is also true on-chain, where proof of unique humanity could be used to recover account which private key has been stolen/lost.

## What do we need to solve this problem?

We need a way to:
1. prove unique personhood of each user in a privacy-preserving way
1. keep users anonymous, while enabling them to prove properties about themselves (being over 18 to access a porn website, having a legal license to buy weapons...etc)
1. in very specific cases (tbd), we need to reveal the user's identity only to specific agents (i.e: judges): every doxxing of identity should be monitored publicly, transparently and be strictly confined to a set of rules specified in advance.

\#3 is very political and controversial: users legitimally want to protect themselves against a rogue government for example.
The scientific literature has proven that there is no easy answer to that dilemma.

That is why we need an unopinionated software toolbox, that:
- allows web developers to choose the set of rules they want their users to abide to
- makes the web developers accountable for the set of rules they decided to choose. There is no turning back, and private access to user's identity must be technically impossible, every access occurence has to be public information, that can then be publicly monitored by users themselves.

Privency aims to be exactly that solution.

## How does Privency.tech solves this problem?

Privency.tech is NOT bound to a specific blockchain or token. In fact unless strictly necessary for technical reasons, Privency.tech will not use blockchain technology. It will probably be necessary for the "transparency" aspect of the project, as well as to support blockchain-specific did:method, but these features will be optional.

Privency.tech can be very useful for off-chain & on-chain applications alike.

App developers and smart-contract developers have complete freedom to choose the spectrum between transparency and privacy that they want their users to abide to. 
Users have complete freedom to accept to use an app/smart-contract or not.

### Privacy-preserving proof of humanity

#### Non government-based approach

The first step is to provide tools for a privacy-preserving [proof of personhood](https://en.wikipedia.org/wiki/Proof_of_personhood).

There are already a lot of open-source projects that attempt to prove personhood via various techniques that do not require a government ID:
- [BrightID](https://brightid.gitbook.io/brightid/)
- [Proof of Humanity](https://proofofhumanity.id/)
- [Humanode](https://blog.humanode.io/)
- [Fractal ID](https://web.fractal.id/)
- ... and more

Privency.tech does not intend to reinvent the wheel here.

#### Using government ID (KYC/AML)

##### Image analysis of government ID

There are no open-source solution of my knowledge that can be used to parse a government ID using Image Vision & ML, analyze video/picture of the user, compare the two and output a corresponding [Verifiable Credential](https://www.w3.org/TR/vc-data-model/). Essentially there is no open-source Know Your Customer (KYC) solution.
And of course there is no open-source solution to go through this process in a privacy-preserving way.

A privacy-preserving KYC library could be one of the tools included in Privency.tech.
This library could be reused in a variety of situation, from blockchain account abstraction to a traditional web auth server.

To make the verification private, the actual analysis will either be done: 
1. locally on the client side together with zk-SNARKS proof that the output has been generated from the provided software without being tampered with
1. in a third-party server, but with all inputs encrypted using [fully homomorphic encryption](https://en.wikipedia.org/wiki/Homomorphic_encryption)

\#2 is not trustless because the computation being done server-side is not verifiable. That's an issue.

I explored #1.

\#1 is a work-in-progress research-level topic, BUT there have been significant improvements in the past few years.

The [zkML Community](https://github.com/worldcoin/awesome-zkml) is a new online community that aims to gather people interested in builder and using tools for generating zero-knowledge proofs of ML/Image Vision algorithms.

The most promising library is probably [ezkl](https://github.com/zkonduit/ezkl).

##### The Issuer-Holder-Verifier pattern

Instead of parsing government credentials, we could simply assume that trusted government agencies issuing IDs hold a private key, that could be ~the same as the private key identifying their DNS domain, using the [did:web](https://w3c-ccg.github.io/did-method-web/) method, which is already widely popular in the SSI ecosystem.

Then, we assume that humans go to their government agency - the Issuer, go through a local KYC there, then on top of a traditional physical passport/ID, the agency delivers peer-to-peer a digital Verifiable Credential to the human - the Holder.
This Verifiable Credential is digitally signed by the Issuer and address the Holder via his [Decentralized Identifier](https://www.w3.org/TR/did-core/). (Here arises a complicated problem: which did:method to use for individuals? How to conveniently and securely store keys? Privency.tech would provide the toolbox to support a wide variety of did:method and leave the choice to the developers.)

Finally, when the Holder needs to prove claims in its Verifiable Credential (being French or over 18 to access certain content for example), he can present zk proof of them to the Verifier which is a third-party server application (or a smart contract).

This architecture is anyway necessary, as the output of the local zkKYC is a Verifiable Credential that one has to present. 
In both the zkML and this approach, the Issuer is the same entity: the trusted government organization. With the zkML approach, the government doesn't provide the necessary infrastructure, so to work-around this constraint, we use verifiable computation of an algorithm.

Some interesting resources:
- https://eprint.iacr.org/2021/907.pdf
- https://www.w3.org/TR/vc-data-model/

#### Proving attributes contained in the Verifiable Credential without revealing personal information

Once we get the digital ID as a Verifiable Credential - we can start proving properties about it without revealing more information, using zero-knowledge proof.

This is called "selective disclosure" and it is already wildly used in the SSI space. Privency.tech doesn't intend to reinvent the wheel here.

Some related articles/libraries:
- https://github.com/evernym/coconut-rust
- https://medium.com/51nodes/selectively-disclosed-verifiable-credentials-79a236b81ee2
- https://academy.affinidi.com/a-detailed-guide-on-selective-disclosure-87b89cea1602

#### Note on other ID documents

The 1st focus is on proving unique personhood - and then age and nationality.

However, we could imagine a zkKYC process that prove other information about the users: 
- having a license to buy weapons
- having a medical license
- having a membership to a private club
- ...etc

### An authorization/authentication server and client for web applications

Privency.tech provides an authorization/authentication server and client for web applications

We could use the standard [OpenID for Verifiable Credentials](https://oauth.net/openid-for-verifiable-credentials/#:~:text=OpenID%20for%20Verifiable%20Credentials%20is,claims%20to%20a%20relying%20party.) 

I am not entirely sure how it will work out though.

If we go this route, we would benefit from adding support for it in [Ory](https://github.com/ory/hydra), which is a mature, well-thought and fully open-source suite of tools for auth.

Ideally, I want to allow data in web applications to be seamlessly accessible by the IPFS/blockchain space via the same auth mechanisms.
I'd like to rely on [capability-based authorization](https://srl.cs.jhu.edu/pubs/SRL2003-02.pdf), instead of access control, in order to open doors for more interoperability.

In this regards, using [UCAN](https://ucan.xyz/) could be a more interesting option.

We would make SDKs, an admin frontend, and a backend, exactly like existing solution do (think Keycloak, Firebase, Ory, Auth0...etc).

In the admin frontend, the web app dev would configure which kind of conditions they want their users to abide to.

### A smart-contract for blockchain account abstraction

Same as above, but for blockchain accounts, abstracting away their auth from key pairs, for example allowing their access only to those that prove they hold a given government ID and eventually passed a local privacy-preserving KYC every now and then.

### What about transparency?

Web app devs could configure their Privency.tech auth server to:
1. only accept a proof of humanity based on certain condition (French ID and over age 18)
1. require the real government ID to be revealed *transparently* if a judge fill a formal request after the anonymous user has been sued for an action associated with his anonymous identifier. The content of the identity is only revealed to the judge, but the *access* to this identity is *public*

How to technically achieve #2?

I haven't explored it in much details yet, but my intuition is that:
- an encrypted version of the government ID should be sent to the web app server
- this encrypted government ID can only be decrypted once a certain public smart-contract has been called by an account controlled by the department of justice
- once the smart-contract is called by the designated entity, it generates the decryption key that only the department of justice can make use of to reveal the user's identity
- to call the smart-contract, the department of justice must provide a proof: for example a case number or the CID of the data representing the lawsuit
- it's up to the wider public to monitor the calls to the smart-contract, and decide, completely off-chain, whether this access was legitimate or not

This way, like today governments can still misbehave, but unlike today they technically *cannot misbehave privately*.

We could imagine other conditions to allow for revealing user's identity. For example - we could decide that on top of the department of justice requesting it, we need more than 50% of the most active users to vote for revealing the identity. How to determine the amount of users, or the most active users? I am not sure yet.

### Potential Privency.tech use cases

- social network and e-democracy
- government organization
- banks and crypto exchanges
- blockchain account abstraction
- e-commerce
- healthcare software systems

### Non-goals

- solving user data privacy. There are already projects working towards this direction, namely [WNFS](https://guide.fission.codes/developers/webnative/file-system-wnfs) for local-first edge applications, and [zama.ai](https://www.zama.ai/), through its set of fully-homomorphic encryption tools. 
- solving user metadata privacy. There are already projects working towards this direction, namely mixnets such as [Tor](https://www.torproject.org/) or [HOPR](https://hoprnet.org/).

Privency.org may have data and metadata privacy features, but Privency.tech focuses solely on identity, authentication and authorization,

## Privency.org - the e-democracy social network

Privency.org will be a social network based on Privency.tech - the auth toolbox. It is aimed to provide a safe place to discuss everything - an e-agora enhancing e-democracy.

It will be the very fist social network of its kind, that allows for:
- 1 account = 1 human
- zkKYC all users
- restricted discussions groups per nationality, profession...etc
- reliable and trustless e-voting and polling system
- users, while anonymous, will be held accountable for their words (transparency). Different groups will be subject to different juridisctions and rules

Besides its intrinsic use-case, Privency.org will serve as a pilot customer for Privency.tech.

## Resources

On the privacy-transparency dilemma:
- https://www.hbs.edu/ris/Publication%20Files/Bernstein_TransparencyParadox_ASQ_June2012_cdaaee20-3a45-4a07-8867-9761a5d4b5e8.pdf
- http://faculty.washington.edu/moore2/PTPD.pdf
