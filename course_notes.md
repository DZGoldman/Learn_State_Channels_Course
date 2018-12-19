# State Channel Course Syllabus

### Goals

### What the Course Isn't
- Important disclaimer: not about producting production-ready code
- P2p network artitecture here (peer discover, routing, etc.)


### Prerequisites
- Basic understanding of Ethereum / blockchain tech
- Some programming experience (solidity & javascript/react, though prior experience isn't necessary)

### Requirements:
- nom/yarn, react, truffle, ganache-cli, ethers.js, python3 (_for server/ communication layer (websockets, webRTC), though undecided_)

## Course Structure:

#### Part 1: Fundamentals

##### Theory
- Motivation/the scaling problem: i.e., why layer 1 throughput is necessarily bounded
- Brief overview of other scaling approaches (block size increase, sharding, sidechains, plasma, etc.)  
- Define payment channels and state chanels, give semi-technical overview
    - Possible topics / terminology to cover:
         - hash time locked contracts, uni-directional vs. bidirectional, "eltoo" style vs. punative style channels, conditional mutli-hop payments, generalized state transitions, "terminal states", stale-state griefing / timeout necessity 

##### Setup 
- Go through instalation of software requirements, setting up roptsein connection
- Brief into to ethers.js (familiarity can't be assumed). _Necessities are just basic read/write syntax with contract instance, nd signing/verifying transactions 
- Maybe: also some discussion more esoteric solidity needs, like ecrecover,  enums (though doing these in real time may be best)

#### Part 2: Payment Channels
- Implement and dissect contract for "minimum viable payment channel"
    - Aiming for literally simplest app possible at this stage, so simply unidirectional channel where payor locks eth into contract with dispute interval for payer exits  
    - Probably: only contract itself at this stage, not full application
- Implement lightning style payment channel contract
    - multisig entrance, bidirectional, mutual chanel closing option, punishment for stale state
- Build fully functional payment channel application
    - Off chain txns are sent via websocket and stored client side
    - Emit Events for unilateral channel closing attempts
    - React app... reacts to events, and responds appropriately
    - Very simple UI
    - host on ropstein

#### Part 3: State Channels
- Implement contract for some application-specific state channel; should include deposit, timeouts, some notion of terminality, nonce based turn taking, etc
    - Can start with boilerplate commit from part 2
    - App itself can be a Guess Number app [2].
        - Maybe a more interseting game? Like Ghost [3] (_are Merkle proofs an unnecessary complication, or a good thing to cover?_)
- Start implementing client side app and "disover" risks/challenges here, i.e., duplicative work in recreating contract logic client-side, complexity in managing various different types of state transitions, etc.
    - This motivates refactoring contract
- Refactor contract to manage off chain as generalized, flux-style state transition machine
    - Finish client side implementation; client side code should basically end up being contract agnostic
- Maybe: discuss and/or use developer APIs for state channel apps if they are available and ready at time of course (Celer? Counterfactual? etc.)

#### Part 4: Advanced Topics
_most up-in-the-air section at this stage_
- Possile topics to cover:
    -  splicing, conditional multihop payments ,perun/set style virtual channels, counterfactual instantiation, sprites, channel factories, watchtowers, cross chain atomic swaps _  
- Covering a topic will include some mix of the following
    - High level conceptual explanation
    - Contract construction and analysis
    - Full app implementation / integration as added feature to previous app
        - Most may be too complex and hard to demo for a full, working implementation, especially those that necessitate > 2 parties. Need to think on it more (channel factories could be a fun one!)
#### 


#### Part 4: Conclusion

- Discussion of downsides/ risks to using state channels:
    - liveness requirements
    - stale state griefing 
    - limited to channel counterparties / complexity of routing mechanisms
    - requirements for capital lock up
    - _Note: not sure this should go here — maybe instead it goes as a lead-in to the advanced topics section? But definitely should be included somewhere_
- Overview of actual implentations/ projects in the ecosystems and how they employ the principles covered in the course (_will be outdated quickly!_)
    - Celer, Counterfactual, Set, Piza, Magmo, Connext, Raiden, etc.
- Links / recommendations to resources for further exploration (TODO)

#### Links: 
1. Fairify
2. Guess Num
3. Ghost


messaging protocol