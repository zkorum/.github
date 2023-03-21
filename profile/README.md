# Privency

_Privency_ is both:
- _Privency.tech_ => a set of unopinionated digital identity tools that attempt to solve the dilemma between:
	- **PRIV**acy, freedom and democracy on one side (example: protecting whistleblowers, proving that we are over 18 without giving away more information)
	- transpar**ENCY**, control and justice on the other side (example: minimizing the trust needed in a third-party service, preventing cyber-bullies or money launderers to stay anonymous and go unpunished)
- _Privency.org_ built on top of _Privency.tech_ => the very first privacy-preserving social network where 1 user = 1 verified human, providing bottom-up e-democracy tools such as reliable polls and voting

_Privency_ is a work-in-progress project that is actively built in public by @nicobao.

@nicobao will seek to sustain the development of _Privency_ by finding an ethical and profitable funding/business model route.

_Privency_ will always remain *100% open-source*.

The [Roadmap](./ROADMAP.md) as well as this document are continuously updated.

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
1. in very specific cases (tbd), we may want to reveal the user's identity only to specific agents (i.e: judges): every doxxing of identity should be monitored publicly, transparently and be strictly confined to a set of rules specified in advance.

\#3 is very political and controversial: users legitimally want to protect themselves against a rogue government for example.
The scientific literature has proven that there is no easy answer to that dilemma.

That is why we need an unopinionated software toolbox, that:
- allows web developers to choose the set of rules they want their users to abide to
- makes the web developers accountable for the set of rules they decided to choose. There is no turning back, and private access to user's identity must be technically impossible, every access occurence has to be public information, that can then be publicly monitored by users themselves.

_Privency.tech_ aims to be exactly that solution.

## How does _Privency.tech_ solve this problem?

_Privency.tech_ is NOT bound to a specific blockchain or token. In fact unless strictly necessary for technical reasons, _Privency.tech_ will not use blockchain technology. It will probably be necessary for the "transparency" aspect of the project, to support voting, as well as to support blockchain-specific did:method, but these features will be optional.

_Privency.tech_ can be very useful for off-chain & on-chain applications alike.

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

_Privency.tech_ does not intend to reinvent the wheel here.

#### Using government ID (KYC/AML)

##### Image analysis of government ID

There are no open-source solution of my knowledge that can be used to parse a government ID using Image Vision & ML, analyze video/picture of the user, compare the two and output a corresponding [Verifiable Credential](https://www.w3.org/TR/vc-data-model/). Essentially there is no open-source Know Your Customer (KYC) solution.
And of course there is no open-source solution to go through this process in a privacy-preserving way.

A privacy-preserving KYC library could be one of the tools included in _Privency.tech_.
This library could be reused in a variety of situation, from blockchain account abstraction to a traditional web auth server.

To make the verification private, the actual analysis will either be done: 
1. locally on the client side together with zk-SNARKS proof that the output has been generated from the provided software without being tampered with
1. in a third-party server, but with all inputs encrypted using [fully homomorphic encryption](https://en.wikipedia.org/wiki/Homomorphic_encryption)

\#2 is not trustless because the computation being done server-side is not verifiable. That's an issue.

I explored #1.

\#1 is a work-in-progress research-level topic, BUT there have been significant improvements in the past few years.

The [zkML Community](https://github.com/worldcoin/awesome-zkml) is a new online community that aims to gather people interested in building tools for generating zero-knowledge proofs of ML/Image Vision algorithms.

The most promising library is probably [ezkl](https://github.com/zkonduit/ezkl).

##### The Issuer-Holder-Verifier pattern

Instead of parsing government credentials, we could simply assume that trusted government agencies issuing IDs hold a private key, that could be ~the same as the private key identifying their DNS domain, using the [did:web](https://w3c-ccg.github.io/did-method-web/) method, which is already widely popular in the SSI ecosystem.

Then, we assume that humans go to their government agency - the Issuer, go through a local KYC there, then on top of a traditional physical passport/ID, the agency delivers peer-to-peer a digital Verifiable Credential to the human - the Holder.
This Verifiable Credential is digitally signed by the Issuer and address the Holder via his [Decentralized Identifier](https://www.w3.org/TR/did-core/). (Here arises a complicated problem: which did:method to use for individuals? How to conveniently and securely store keys? _Privency.tech_ would provide the toolbox to support a wide variety of did:method and leave the choice to the developers.)

Finally, when the Holder needs to prove claims in its Verifiable Credential (being French or over 18 to access certain content for example), he can present zk proof of them to the Verifier which is a third-party server application (or a smart contract).

This architecture is anyway necessary, as the output of the local zkKYC is a Verifiable Credential that one has to present. 
In both the zkML and this approach, the Issuer is the same entity: the trusted government organization. With the zkML approach, the government doesn't provide the necessary infrastructure, so to work-around this constraint, we use verifiable computation of an algorithm.

Some interesting resources:
- https://eprint.iacr.org/2021/907.pdf
- https://www.w3.org/TR/vc-data-model/

#### Proving attributes contained in the Verifiable Credential without revealing personal information

Once we get the digital ID as a Verifiable Credential - we can start proving properties about it without revealing more information, using zero-knowledge proof.

This is called "selective disclosure" and it is already wildly used in the SSI space. _Privency.tech_ doesn't intend to reinvent the wheel here.

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

_Privency.tech_ provides an authorization/authentication server and client for web applications

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

Cryptographic verifiability is already a form of transparency. It helps minimize trust, spam and sybil attacks.

If you want to go further, Web app devs could configure their _Privency.tech_ auth server to:
1. only accept a proof of humanity based on certain condition (French ID and over age 18)
1. require the real government ID to be revealed *transparently* if a judge fill a formal request after the anonymous user has been sued for an action associated with his anonymous identifier. The content of the identity is only revealed to the judge, but the *access* to this identity is *public*

How to technically achieve #2?

I haven't explored it in much details yet, but my intuition is that:
- this a typical use-case where smart-contract and general-purpose blockchains shine. I don't see any credible alternative.
- an encrypted version of the government ID should be sent to the web app server. (I am uncertain that holding people's government IDs on a server is legal though, even in an encrypted form.)
- this encrypted government ID can only be decrypted once a certain public smart-contract has been called by an account controlled by the department of justice
- once the smart-contract is called by the designated entity, it generates the decryption key that only the department of justice can make use of to reveal the user's identity
- to call the smart-contract, the department of justice must provide a proof: for example a case number or the CID of the data representing the lawsuit
- it's up to the wider public to monitor the calls to the smart-contract, and decide, completely off-chain, whether this access was legitimate or not

This way, like today governments can still misbehave, but unlike today they technically *cannot misbehave privately*.

We could imagine other conditions to allow for revealing user's identity. For example - we could decide that on top of the department of justice requesting it, we need more than 50% of the most active users to vote for revealing the identity. How to determine the amount of users, or the most active users? I am not sure yet.

This is a totally optional feature because it is highly controversial. 

This feature is NOT used in _Privency.org_, because:
1. this feature is impossible to implement without getting the ministry of justice involved
1. this feature is technically very challenging
1. the legality of this feature is uncertain
1. this feature is likely to scare people away
1. this feature is too prone to be hijacked by powerful people and a corrupted government

### Potential _Privency.tech_ use cases

- social network and e-democracy
- government organization
- banks and crypto exchanges
- blockchain account abstraction
- e-commerce
- healthcare software systems

### Non-goals

- solving user data privacy. There are already projects working towards this direction, namely [WNFS](https://guide.fission.codes/developers/webnative/file-system-wnfs) for local-first edge applications, and [zama.ai](https://www.zama.ai/), through its set of fully-homomorphic encryption tools. 
- solving user metadata privacy. There are already projects working towards this direction, namely mixnets such as [Tor](https://www.torproject.org/) or [HOPR](https://hoprnet.org/).

_Privency.org_ will have data and metadata privacy features, but _Privency.tech_ focuses solely on identity, authentication and authorization,

### Business Model

#### off-chain solution: SaaS and consulting

Like Keycloak, the authentication server could be either self-hosted or hosted in the cloud.

We could provide services to companies looking for help and assurance to self-host our product.

We could host the authentication server on behalf of customers in our cloud.

#### on-chain solution: Smart-contract fee

We could configure a small fee that is sent to our account whenever someone call a smart-contract that we have written.

The fee could be turned off, but the very existence of this switch could convince investors of the potential income (Uniswap does exactly that).

## _Privency.org_ - the e-democracy social network

_Privency.org_ will be a social network based on _Privency.tech_ - the auth toolbox. It is aimed to provide a safe place to discuss everything - an e-agora enabling e-democracy to emerge.

It will be the very first social network of its kind, featuring:
- zkKYC all users: preserving users anonymity while verifying their credentials
- restricted discussions groups per nationality, profession, age, club membership...etc
- reliable and trustless e-voting and polling system
- each human has unique characteristics represented by the verifiable credentials they hold. They have a specific account (or "identifier") for each designated social groups, and by design it will be technically impossible to link the identifiers together. Ex: there could be a discussion group for parents in France, and another for Indians immigrants in France. One human could have a "parent in France" account and an "Indian with legal residency in France" account. This way, the human can speak as a parent in the restricted group of parents with legal residence in France without revealing he is an Indian immigrant, and vice-versa. That would allow for each parent voice to be heard independently of other unrelated aspects, that could otherwise trigger subconscious cognitive bias and result in discrimination. It would also allow for under-represented groups to speak up without endangering their personal interest.

Besides its intrinsic use-case, _Privency.org_ will serve as a pilot customer for _Privency.tech_.

### What about transparency? 

_Privency.org_ minimizes trust by using content-addressing. Data isn't siloed in _Privency.org_ server - it's free to live its own life, thanks to IPFS. That also helps community to form around their data, that is transparently accessible to them.

### What about data privacy? 

In first versions of _Privency.org_, everyone will be able to read data from every groups (public read access). Only write access will be limited. However in the long-term, we can imagine to implement restricted read access, by using fully-homomorphic encryption (FHE).

Whenever data leaves the user's device, it would be encrypted using FHE, allowing for _Privency.org_ 's server to provide search capabilities, without having access to the actual data. 

The reason why this feature is postponed is: 
- it's isn't obvious that private-read-access group will be interesting for users
- FHE comes with a series of technological challenges, as it is known for its performance issues. 

### What about deleting data? 

Users can request to delete data. The list of deleted CID will be maintained by _Privency.org_, so that everyone holding this data can stop pinning them and stop displaying them in their frontend if they have them.

### What about metadata privacy? 

Without metadata privacy, it would be easy to associate all the "anonymous identifiers" together. If the requests from 2 different identifiers come from the same IP address, then it is easy to deduce that it is likely to be the same person. How to solve this issue? We could either encourage people to use Tor/HOPR or directly enforce its usage in the frontend (the latter is preferred).

### What about users revealing personal info themselves? 

This is mostly out of our control, and it's okay. We could provide educational content. By running natural language processing tool locally on the frontend, we could warn the users that the content they are about to send is about to reveal personal information and it is not recommended for anonymous identifiers. 

Besides the anonymous identifiers, it will be possible to create a public identifier on _Privency.org_ that will be associated with as many verified information as users want. This is particularly suitable for public figures, public organizations, or anyone willing to reveal their identity in public. It will be like the Twitter blue tick - but that actually works, because it will be based on real cryptographic verification.

### What about the way people write? Can it be used to reveal their identity? 

Yes, it can. To mitigate this, we can provide educational content and eventually leverage AI tools to provide the option for users to automatically "rewrite" their content in a random style before posting.
### How to punish bad actors? (harassment, etc...)

Like existing social networks: 
- filtering out bad data
- report and suspend/ban users
- use natural language processing as much as possible to detect bad behavior. 

Because users are verified and can only have two accounts per discussion group (one anonymous + eventually the public identifier), getting banned is bigger of a deal than with existing social medias. I presume that together with having 0% bots, it is enough to drastically diminish the amount of bad actors compared to existing social networks. 

No doxxing of government ID is enabled: private information never leaves the user's personal devices.

### Non-goals

_Privency.org_ is neither TikTok, Instagram nor Facebook. There is no social network of this kind today, but if we were to compare, we could say that _Privency.org_ is most akin to a combination of Twitter, Reddit and LinkedIn. It is aimed primarily for long-form deep conversations about serious topics, potentially controversial and political.

### Business model

Besides its intrinsic use-case, _Privency.org_ represents a marketing product that, if successful, will popularize and showcase _Privency.tech_ capabilities, boosting _Privency.tech_ revenue.

Other than that, there are two possible routes to fund _Privency.org_.

#### Advertisement

Like most social networks, _Privency.org_ can rely on advertisements. Responsible and ethical advertisements are a good way to fund expensive, yet useful public goods. Like Netflix does, I would love to provide an option to turn off advertisement, in exchange for payment. The challenge here, is that payment would require _Privency.org_ to KYC our users by law. zkKYC doesn't exist in the eye of the law - there is no alternative. We could use cryptocurrencies, but regulation on crypto payment is even stricter than for fiat payment. That is why there will be no option to turn off advertisements at launch.

Because we have 0% bots, 100% real humans, and 100% verified attributes about these human, we can offer a unique solution to advertisers.

On Twitter, ads target 90% bots, and a complicated algorithm is used to suggest products that have little to do with the real human behind the users. 

For example, if you're an advertiser that have identified that your typical potential customers are Korean profesors under the age of 40, _Privency.org_ can certify you that there are 500 000 distinct verified Korean profesors, and 30M people under the age of 40 in the different discussion groups of _Privency.org_, then help you target them with your advertisement.

To make money, _Privency.org_ will be incentivized to:
- keep user's privacy 100% safe, otherwise users won't come
- verify as many credentials about the users as possible in order to provide the most tailored service to advertisers

Value proposition for each stakeholder:
- the value for users is to be able to distinguish different aspect of them: they can speak in a group restricted to people under 40 years old, and in another group for Korean profesors, without that anyone can technically link the two identifiers together and know that in fact, it's the same person speaking.
- the value for advertisers is to link both information, in order to target their specific audience: Korean profesor under the age of 40.
- to provide value to both stakeholders, we need to send the potential ads client side, and let the client software decide what to actually present, depending on what's valuable for them according to the user's credentials - that is only available client side.
- both users and advertisers benefit from knowing that users are verified

This business model would allow to make money without compromising on the core values of _Privency_.

#### Donation 

_Privency.org_ can rely on donation - a la Wikipedia. After all, this product is a public good, so it is easy to justify.

## Resources

On the privacy-transparency dilemma:
- https://www.hbs.edu/ris/Publication%20Files/Bernstein_TransparencyParadox_ASQ_June2012_cdaaee20-3a45-4a07-8867-9761a5d4b5e8.pdf
- http://faculty.washington.edu/moore2/PTPD.pdf
