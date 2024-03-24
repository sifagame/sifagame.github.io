---
title: "Whitepaper"
draft: true
---

## Concept

The project operates 2 smart contracts:
1. **SIFA Coin** (SIFA) is a ERC20, used as the internal game currency to start the game, join the game and get rewards.
2. **SIFA Game** (Game) is a ERC1175, contain game rounds and results for each round as NFT.

## Tokenomics

|Total supply|100%|1,000,000,000|Lock|
|-|-|-|-|
|DEX|10%|100,000,000|1Y|
|Developers|5%|50,000,000|2Y|
|Vault rewards|85%|850,000,000|5Y|

- Lquidity Pool LP is locked for 3 years.
- Presale tokens are locked for 1 year, released by 0.28% daily.
- Developers tokens are locked for 5 years, released by 27.4K daily.

### The Vault

85% of the total supply are locked in the vault, released by 20,000 tokens each hour and shared across all vault participants according to their share. This will make the emission evenly distributed for approximately 5 years (42,500 hours, 1770 days and 20 hours).

The emission only happens if there are any SIFA tokens locked in the vault.

## Game Rules

Minimum players in a round: 2, maximum players in a round: 4.

- `create` transfers the desired amount (ex. 100 SIFA) from the caller to **Game** and creates the Round. The Round state is `waiting`.
- `join` accepts the ID of the round. Requires the round to have less than 4 players in the round already. Requires caller is not a player of the round already. Transfers the same amount (ex. 100 SIFA) from the caller to **Game** and adds the caller to the list of players in the round. If the last (4th) player has joined, calls `start`.
- `start` accepts the ID of the round. Requires the round to have at least 2 players, or 86,400 seconds (1 day) passed since the Round was created. The new NFT (SIFAK) is minted to the random player in this round. The SIFAK has a number of available throws (random from 6 to 12). The Round state change to `started`.
- `cancel` requires the total number of players in the Round is less than 4 or 86,400 seconds (1 day) passed since the Round was created. Requires the Round state is `waiting`. Make full refund: transfer the initial amount (ex. 100 SIFA) to each player in the Round. The Round state change to `end`.
- `throw` accepts the ID of the SIFAK. Requires the number of SIFAK's available throws is greater than 0. Reduce the number of available throws. Transfer ownership of the SIFAK to the other random player in a round. If the reduced number of available throws is 0, calls `finish`.
- `finish` requires the SIFAK available throws to be 0 or 604,800 seconds (7 days) passed since the round was created. The looser of the round is the player owning the SIFAK. All other players of the round are winners. Transfer 125% of amount (ex. 125 SIFA) to each player. Transfer the remaining 25% of initial amount (6.25% of the total round funds, ex. 25 SIFA) to the Vault rewards. The looser keeps the SIFAK for 1 year as the sign of the shame. After 1 year the SIFAK is unlocked to transfer.
