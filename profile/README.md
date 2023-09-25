# ZKorum - Democratize Democracy

When addressing sensitive subjects like racism, sexism, religions, and politics, most individuals resort to self-censorship to avoid conflicts and animosities, becoming what’s known as the *Silent Majority*. Surveys reveal that only 18% of Germans believe that it’s possible to express themselves freely in public. 71% of French people engage in self-censorship in the workspace. How can we bridge the gap between the Silent Majority and an open society that values and respects the voices of all its members?

The root of these problems boils down to the *fear of judgment* and the *necessity of anonymity* for freedom of expression. Currently, there isn't any trustless social network that can simultaneously guarantee both user anonymity and data verifiability. Users are forced to either depend on third parties to verify their identities, thereby risking the loss of control over their privacy, or they remain unverifiable, resulting in a web infested with extreme opinions, hate speech, and bots.

*ZKorum* aims to give a voice to the silenced populations, especially those who are vulnerable or marginalized, by building a privacy-preserving social network where verified individuals and communities create anonymous polls, voting, and discussions. The solution can serve as a public e-Agora and as an anonymous community forum for verified organizations including companies, universities, and NGOs.

Built on the principles of data minimization, ZKorum leverages recent advancements in natural language processing, censorship-resistant infrastructure, and applied cryptography within the context of BBS+ W3C Verifiable Credentials.

Fully open source, the project aims to be compatible with European Digital Identity. Currently incubated at ESSEC Ventures, ZKorum will release its MVP in December 2023 to the entire ESSEC community including 5000 students. In the long run, ZKorum aspires to collaborate with governments to become a trustless gateway to eDemocracy.

ZKorum is currently actively [built in public](https://github.com/zkorum/zkorum). The [technical feasibility](https://github.com/zkorum/poc/tree/main/vc-flow#how-does-it-work) and [research & development](https://github.com/docknetwork/crypto-wasm-ts/pull/19) have been validated.

The [Roadmap](https://github.com/zkorum/.github/blob/main/ROADMAP.md) as well as this document are continuously updated.

## Detailed solution

The product of ZKorum is an online forum where communities and verified members can create and participate in anonymous polls, surveys, discussions, and votes. On reaching the ZKorum Progressive Web App (PWA), anyone can browse the existing public surveys and their results. For example, a public poll asking European residents from 18 to 60 years old if they will choose an Electric Vehicle over a gas one. By default, surveys are publicly visible. Participating in surveys is restricted to users who can prove they hold the right credentials (eligibility). Eligibility is verified in a privacy-preserving way using zero-knowledge proof (ZKP) of users’ Anonymous Credentials (Dock’s BBS+ W3C Verifiable Credentials).

See [Solution](https://github.com/zkorum/.github/blob/main/SOLUTION.md)
