# SpaceWar Game Requirements

**Author:** Mark Crowley

**Date:** June 2020

**Repository:** https://github.com/rateldajer/SpaceWar



## Essentials for version 1

- move by clicking on a cell, reach the cell by beginning of the next turn
- all other players move too, but they are hardcoded
- state is fully observable by everyone
- ability to load in policies from other players to train against easily, file format for policy as a function somehow. 
- Remove Trademarks: if there are any trademark names of any existing Science Fiction property, they should be anonymized and altered. It is not essential to the game dynamics that the different ship types need to be from any particular show or movie.
- state
  - `location` (could be (x,y)coord or cellid) of `agent`
  - `location` of other players, usually 3 others (`npc1`,`npc2`,`npc3`), one at each corner
- actions
  - `(M,F)` where
  - *M*ove : a `location` to move to in the next turn. If current location of `agent` then no movement
  - *F*ire : none, beam/laser + `location`, torpedo + `location`
    - laser : fires very soon after movement starts, fires straight and instantaneously crossing the targetted `location` and ending if it hits any object 
    - torpedo : fires later after movement starts, maybe even near the end, moves across a straight path from the player's current position and off the board unles it hits another object
- dynamics
  - the game is turned based, so all actions are collected and one timestep moves forward after that
  - in terms of state and rewards, all the movements in between the start and end state are discarded, only the sum of the impacts of them is retained, ie. how long was a laser hitting a player, did a torpedo hit is while it travelled?
  - for visualization purposes, the time between steps can be visualized smoothly with nifty special effects...
  - dynamics can be stochastic or deterministic
  - possible sources of randomness
    - the delay before firing the beam or torpedo can sampled from a Guassian
  - the game ends when there is only one player left on the board 
- rewards (the exact numbers can be modified)
  - movement with no impact : 0
  - winning the game : 100*(T/20) where T is the number of steps before the game ends 
  - destroying an opponent : 50
  - being hit by a laser : -1 
  - being hit by a torpoedo : -5 

## Essentials for version 2

- playable user interface, click to choose action, buttons to switch to enable choosing of move, beam or torpedo
- current speed is determined by distance travelled last turn
- available cells to move to this turn constrained by velocity (both nearby and distant cells might be invalid for certain velocities)
- add laser sweeping
  - laser : fires very soon after movement starts, sweeps a range of space where the targetted `location` is fixed, as the `agent` moves 

## Questions

### Could we have a standard encoder to represent the policy?

- Given a state, action space definition of the game, we could train the standard network with our choices and use the encoding as the definition of the policy?
- It could be an autoencoder, then it gets trained by querying the current policy for a standard range of (S,A) pairs? Not all of them, but enough to make it a good approximation, but small enough to be a something that could be trained in 1 hr on a laptop before submission of the current version to the competition.
