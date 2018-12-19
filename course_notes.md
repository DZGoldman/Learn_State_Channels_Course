# A Developer's Guide to State Channels: Rough Syllabus / Notes

### Goals
- The objective of this course is to give developers an understanding of layer 2 state channel principles and to give them tools to start building state channel applications. By the end of the course, students will have a wide and deep understanding of state channel architecture, and should have the tools to build state channel applications from the bottom up; in short, the course takes a "the best way to learn about something is to try to build it!" approach to state channels.

- Students will also come away with knowledge of technological principles and key terminology in the current ecosystem of Ethereum research and development (both layer 1 and layer 2).

- Additionally, developers could find it generally useful as a way to expand their knowledge and skillset of various tools / programming languages (or even pick them up for the first time) including Solidity, Web3.js / Ethers.js, truffle/ganache, ReactJS, etc. 


### What the Course Isn't
- Important disclaimer to give: the code provided in the course should _not_ be considered production ready; the goal is primarily to create functional applications that illustrate core principles, not provide applications ready to deploy on mainnet. Didactically, clarity and simplicity will be prioritized.

- The course will not cover any sort of P2P messaging layer or routing system; our applications will assume some centralized (though non-custodial) server for message passing (we will essentially be mimicking a more robust system with a simple web-socket/ webRTC layer).


### Recommended Prerequisites
- Basic understanding of Ethereum and blockchain technology
- Some programming experience (solidity & javascript/react)

### Installation Requirements:
- npm/yarn, react, truffle, ganache-cli, ethers.js, electron, python3 (_for backend_)

## Course Structure:

#### Part 1: Fundamentals

##### Theory
- Motivation / the scaling problem: i.e., why layer 1 throughput is necessarily bounded
- Brief overview of other scaling approaches (block size increase, sharding, sidechains, plasma, zk-snarks, etc.)  
- Define payment channels and state chanels (as distinct from other scaling approaches), give semi-technical overview
    - Possible topics / terminology to cover:
         - hash time locked contracts, uni-directional vs. bidirectional, "eltoo" style vs. punitive style channels, conditional multi-hop payments, generalized state transitions, "terminal states", stale-state griefing & necessity for timeouts 

##### Setup 
- Go through installation of software requirements, including roptsen connection via infura
- Brief intro to ethers.js (familiarity can't be assumed); necessities are just basic read/write syntax with contract instance, and client-side signing & verifying of  transactions
- Maybe: also some discussion of more esoteric solidity needs, like ecrecover, abiEncoder,  enums (_though covering these in real-time as they come up as "optional lessons" may be best_)

#### Part 2: Payment Channels
- Implement and dissect/analyze contract for "minimum viable payment channel"
    - Aiming for simplest app possible at this stage: unidirectional channel where payor locks ETH into contract with dispute interval for payer exits; no punishments (just state updates), no mutual closing, no counter-party signature verification required on-chain
    - Probably: only contract itself at this stage, not full application
- Implement lightning style payment channel contract (_Can use source from [1], with some mods/simplifications_)
    - Multisig entrance, bidirectional, mutual channel closing option, punishment for stale state
- Build fully functional payment channel application
    - Off chain transactions are sent via websocket and stored client side
    - Emit Events for unilateral channel closing attempts
    - React app... reacts to events, and responds appropriately
    - Two-step messaging protocol to achieve finality
    - Simple UI (_probably just provide .css file_)
    - host on ropsten

#### Part 3: State Channels
- Implement contract for some application-specific state channel; should include deposit, timeouts, some notion of terminality, nonce based turn taking, etc.
    - Can start with boilerplate commit from part 2
    - App itself can be something like a simple Guess Number app [2].
        - Maybe a more interesting game? Like Ghost [3] (_are Merkle proofs an unnecessary complication, or a good thing to cover?_)
- Start implementing client side app and "discover" risks/challenges here, i.e., duplicative work in recreating contract logic client-side, complexity in managing various different types of state transitions, etc.
    - This motivates refactoring contract (_the least annoying way to do this would probably be to arrive at this "realization" quickly_)
- Refactor contract for "statelessness", i.e.: 
    - Manage off-chain state as generalized, flux-style state transition machine, with all off-chain state updates managed via single "Game" struct object
    - Introduce strategy of "directly updating to state" based on mutually signed encoded state hash
    - Finish client side implementation; client side code should end up being as close to agnostic to the app-specific details as possible, i.e., should mostly utilize generalized methods like "apply action" and "close channel," with UI-triggered events simply referencing appropriate action names [2]
- Maybe: discuss and/or use developer APIs for state channel apps if they are available and ready at time of course (Celer? Counterfactual? etc.)
#### Part 4: Advanced Topics
_Note: this section is still the most "up-in-the-air"_
- Possile topics to cover:
    -  Splicing, conditional multi-hop payments, perun/set style virtual channels, counterfactual instantiation, sprites, channel factories, (trust-minimized) watchtowers, cross chain atomic swaps  
- Covering a topic will include some mix of the following
    - High level conceptual explanation
    - Contract construction and analysis
    - Full app implementation / integration as added feature to previous app
        - Most may be too complex and hard to demo for a full, working implementation, especially those that necessitate > 2 parties, though some like splicing are easily integrate-able; need to think on it more (_channel factories could be a fun one!_)


#### Part 5: Conclusion

- Discussion of downsides/ risks to using state channels (_maybe also include "potential mitigation" (PM) in here_):
    - liveness requirements (PM: unidrectional channels, watchtowers, long dispute periods, break intervals)
    - stale state griefing (PM: add in terminality "checkpoints", more robust crypto-economic incentive mechanisms) 
    - limited to channel counterparties / complexity of routing mechanisms (PM: perun, meta-channels, atomic mutlipath payments, hub and spoke topology)
    - hot wallet risk (PM: "layer 2 vaults", watchtowers, static backups)
    - _Note: not sure this section belongs here — maybe instead it functions as a lead-in to the advanced topics section (i.e., "here are some problems, and now here are some potential solutions"?) Definitely should be included somewhere regardless. Also should veer on the shorter side, since it'll be all lecture / no dev._

- Overview of actual implentations / projects in the ecosystems and how they employ the principles covered in the course (_might be outdated quickly!_)
    - Celer, Counterfactual, Set, Piza, Magmo, Connext, Raiden, etc.
- Links / recommendations to resources for further exploration (whitepapers, docs, talks, ethresearch posts, etc)

#### General Notes / Ideas / Alternative Approaches
- "Part 3" project could veer on the simpler side, with more complex / robust "final project" section at the end that combines many principles throughout the course.
- A short "Bitcoin channels vs. Ethereum channels" section (in the conclusion maybe?)
- Course hosts it's own payment/state channel hub; students' deployed projects can directly interact with it. 
- More ambitious: students connect their projects into the course's own state channel network
    - This would probably have to be "phase 2" as it would require some network layer infrastructure, but could research some simple plug-and-play options
    - (All still only on ropsten)


#### Code Links (WIP): 
1. https://github.com/DZGoldman/Fairify
2. https://github.com/DZGoldman/guess_number_demo
3. https://github.com/DZGoldman/The-Other-Ghost-Protocol

