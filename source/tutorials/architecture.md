title: Architecture of FOAM 
---

# Overview

The FOAM architecture consists of three major parts:

1. The FOAM smart contracts, written in solidity
2. The FOAM REST API, written in Haskell
3. The SpatialIndex. written in Purescript

The guiding principles are
1. `DRY` ("don't repeat yourself")
2. Strongest possible type-safety
3. Don't re-invent the wheel


The diagram below outlines the different parts, and also the direction in which parts communicate with eachother. Communication here really means everything from API calls and websockets to compile-time information flow.


<img src="../images/foam.architecture_0.4.png" width="700">
