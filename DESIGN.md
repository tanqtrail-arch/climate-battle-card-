# Deck Building Game Features - Design Document

## Storage Schema (extend existing "cb2" localStorage)
```js
const INIT = {
  worldLv:1, japanLv:1, worldBest:0, japanBest:0, totalWins:0,
  coins: 0,
  worldOwned: [],    // [{id, stars}]
  japanOwned: [],    // [{id, stars}]
  worldDeck: [],     // [id, ...] max 12
  japanDeck: [],     // [id, ...] max 12
  worldLeague: 1,
  japanLeague: 1,
  worldLeagueWins: 0,
  japanLeagueWins: 0,
  initialized: false,
};
```

## League Definitions
```js
const LEAGUES = [
  { id:1, name:"ブロンズ", emoji:"🥉", color:"#CD7F32", winsNeeded:3, coinReward:30, clearBonus:100 },
  { id:2, name:"シルバー", emoji:"🥈", color:"#C0C0C0", winsNeeded:3, coinReward:40, clearBonus:150 },
  { id:3, name:"ゴールド", emoji:"🥇", color:"#FFD700", winsNeeded:3, coinReward:50, clearBonus:200 },
  { id:4, name:"プラチナ", emoji:"💎", color:"#E5E4E2", winsNeeded:4, coinReward:60, clearBonus:300 },
  { id:5, name:"チャンピオン", emoji:"👑", color:"#FF4500", winsNeeded:5, coinReward:80, clearBonus:500 },
];
```

## Rarity Calculation
- Per stat, check if card is in top/bottom 20% of pool
- 3+ extreme stats = ★3 (gold #FFD700)
- 1-2 extreme stats = ★2 (silver #C0C0C0)
- 0 extreme stats = ★1 (bronze #CD7F32)

## New Screens
- title (modified: add nav buttons, show coins)
- collection: grid of all cards, owned in color, unowned grayed
- deckbuild: select 12 from owned cards
- shop: buy Basic Pack (100 coins, 3 cards) or Premium Pack (250 coins, 5 cards)
- league: select league, show progress, start league battle
- battle (modified: use player deck, CPU deck from non-owned cards)

## Battle Changes
- Player deck = their 12 selected cards (shuffled, 6 to hand, rest draw pile)
- CPU deck = 12 cards from non-owned pool (quality scales with league)
- Win reward: coins + 1 random card from opponent
- Duplicate cards: 3 dupes = ★ upgrade (max ★3)

## First Launch
- Give 8 random starter cards per edition
- Auto-set deck = all owned cards
