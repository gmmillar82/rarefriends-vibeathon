# Rareton

Walk your Rare Friend around a cosy pixel village, chat with villagers, grow rare flowers from RF seed packets and post 16 × 16 pixel-art gifts, stamped in RF, to any other Rare Friend by number.

**Builder:** [@gmmillar82](https://github.com/gmmillar82) · **Category:** Character Spotlight (also relevant to Economy Potential) · **SDK:** FriendSDK v0.1.2

Your own Generations NFT is the main character. Its canonical on-chain sprite walks the village, and every gift you address shows the recipient Friend's sprite, read live from the public artwork registry. [Source code](https://github.com/gmmillar82/rareton/tree/c3bf477de29759720c5fbe839e36b818108d637a) · [Game rules](https://github.com/gmmillar82/rareton/blob/c3bf477de29759720c5fbe839e36b818108d637a/games/rareton/README.md)

## Play

**Live preview:** https://gmmillar82.github.io/rareton/

You need a browser wallet on **Robinhood mainnet (4663)** holding a hardwired Rare Friends Generations NFT (generation ≥ 1). The SDK runtime connects the wallet and verifies ownership with a read-only check. There are no signatures, transactions or RF funding. **On mobile**, open the game inside MetaMask's in-app browser ([MetaMask link](https://metamask.app.link/dapp/gmmillar82.github.io/rareton/)); regular mobile browsers have no wallet.

**Controls:** WASD / arrow keys, or tap/click where to go. Press **E**, or tap a building, flower or villager (your Friend walks over), to interact. **Satchel** shows your items and sent gifts. **Settings** has sound (off by default) and reduced motion.

**Rules:**
- Pick daisies, tulips, bluebells, poppies and sunflowers in the east meadow, by the pond and near cottages. Each regrows 25 seconds after picking.
- Bramble's Bakery gives free honey buns (you can carry up to 3). The wishing well gives one daisy.
- Four villagers chat, and two of them give you gifts.
- At the **Seed stall**, buy a seed packet (1 RF) and plant it in the **community garden**. It blooms into one of six garden-only flowers with fixed RF values. Keep them for bouquets or sell them back at the stall.
- At the **Post Office**, make a bouquet (1–3 flowers), a letter (6 messages) or a honey bun parcel. Type any Friend number, preview their sprite and your gift art, then press **Stamp & send**. Each gift needs a 0.1 RF stamp. Garden flowers in a bouquet carry their RF value to the recipient.

Each gift is a 16 × 16 one-bit bitmap in the same format as Friend walking sprites, packed into a single uint256 "gift code". Letters carry a stamp derived from the recipient's number, so each is unique to its recipient.

## Costs and rewards

**Everything is simulated.** The SDK preview ledger gives each Friend 20 simulated RF. Nothing is minted or sent on-chain, and there are no transactions, signatures or real fees. The in-game "SIMULATED · RF" stamp, the stall and post office notices and each gift card label this.

**Seed packets** use the SDK chance-game client (buy → plant → bloom → sell), with runtime confirmations for each action. One packet costs 1 RF and grows exactly one flower:

| Garden flower | Chance | Sell value |
|---|---:|---:|
| Clover | 40% | 0.25 RF |
| Rose | 25% | 0.75 RF |
| Lily | 20% | 1 RF |
| Orchid | 10% | 1.5 RF |
| Golden sunflower | 4.5% | 4 RF |
| Moonflower | 0.5% | 20 RF |

- Expected value is 0.9175 RF per packet.
- Each packet reserves the 20 RF maximum prize, and kept flowers stay backed with no expiry.
- Meadow flowers, honey buns and letters are free and have no RF value.

**Postage stamps:** each gift costs 0.1 RF. Half (0.05 RF) is burned and half goes to a village post fund. The notice board shows RF burned this visit. Stamps are tracked locally on top of the SDK ledger (the bridge has no burn API), so the runtime's Friend wallet panel doesn't include them; the in-game balance does.

**On-chain path (for review with the Rare Friends team):**
- A stamp contract would burn RF, which drives Token Activity.
- Gifts are already on-chain-shaped: a uint256 bitmap plus from/to Friend IDs. They could mint as small NFTs to the recipient Friend's canonical wallet, carrying any RF-backed garden flowers, so gifting moves real value between Friends (Economy Potential).
- Seed packets map directly onto the existing chance-game contract.

## Run from source

Node.js 22+ on Linux or Ubuntu/WSL2:

```sh
git clone https://github.com/gmmillar82/rareton.git
cd rareton
git checkout c3bf477de29759720c5fbe839e36b818108d637a
npm ci
npm run dev
```

Open `http://localhost:4173`. The FriendSDK v0.1.2 release archive is included in the repo.

## Checks

All pass:
- TypeScript typecheck (`npx tsc -p .`)
- `friendsdk check` game validation and `friendsdk build`
- `friendsdk test` automated browser smoke checks at 960 px and 360 px
- A scripted walkthrough at 960 px and 360 px (`node scripts/smoke.mjs`): walk to the post office, address a letter (with a live artwork lookup), stamp and send it, then walk to the seed stall, buy a packet, plant it and check the bloom and satchel

The automated checks use the SDK's mock wallet and RPC. A real-wallet playthrough of the gifting flow on the hosted preview passed on desktop (MetaMask extension) and mobile (MetaMask in-app browser). Seed packets and stamps were added afterwards; a real-wallet check of them is pending.

## Known limitations

- Progress resets on reload, because the SDK sandbox has no storage.
- Recipient Friend numbers aren't verified: any number shows its registry artwork, even if not minted.
- Mobile requires a wallet's in-app browser.
- Stamp spending isn't shown in the runtime's Friend wallet panel (see above).
- No live minting, trading, wearable NFTs or creator fees.

## Credits

Friend sprites are canonical Rare Friends artwork, used under the FriendSDK NOTICE. Player and recipient sprites are read live. The villagers use Friends #21, #77, #3 and #150, baked in from the public registry, with invented names and dialogue. Sounds come from the FriendSDK sound kit. The village, flowers and gift art are original pixel art drawn in code.
