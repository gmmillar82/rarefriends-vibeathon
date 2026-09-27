# Rareton

Walk your Rare Friend around a cosy pixel village, chat with villagers, pick flowers and post 16 × 16 pixel-art gifts to any other Rare Friend by number.

**Builder:** [@gmmillar82](https://github.com/gmmillar82) · **Category:** Character Spotlight · **SDK:** FriendSDK v0.1.2

Your own Generations NFT is the main character. Its canonical on-chain sprite walks the village, and every gift you address shows the recipient Friend's sprite, read live from the public artwork registry. [Source code](https://github.com/gmmillar82/rareton/tree/f59b6a6a121f8d976b15bad3fa67c772c1f329af) · [Game rules](https://github.com/gmmillar82/rareton/blob/f59b6a6a121f8d976b15bad3fa67c772c1f329af/games/rareton/README.md)

## Play

**Live preview:** https://gmmillar82.github.io/rareton/

You need a browser wallet on **Robinhood mainnet (4663)** holding a hardwired Rare Friends Generations NFT (generation ≥ 1). The SDK runtime connects the wallet and verifies ownership with a read-only check. There are no signatures, transactions or RF funding. **On mobile**, open the game inside MetaMask's in-app browser ([MetaMask link](https://metamask.app.link/dapp/gmmillar82.github.io/rareton/)); regular mobile browsers have no wallet.

**Controls:** WASD / arrow keys, or tap/click where to go. Press **E**, or tap a building, flower or villager (your Friend walks over), to interact. **Satchel** shows your items and sent gifts. **Settings** has sound (off by default) and reduced motion.

**Rules:**
- Pick daisies, tulips, bluebells, poppies and sunflowers in the east meadow, by the pond and near cottages. Each regrows 25 seconds after picking.
- Bramble's Bakery gives free honey buns (you can carry up to 3). The wishing well gives one daisy.
- Four villagers chat, and two of them give you gifts.
- At the **Post Office**, make a bouquet (1–3 flowers), a letter (6 messages) or a honey bun parcel. Type any Friend number, preview their sprite and your gift art, then press **Mint & send (simulated)**.

Each gift is a 16 × 16 one-bit bitmap in the same format as Friend walking sprites, packed into a single uint256 "gift code". Letters carry a stamp derived from the recipient's number, so each is unique to its recipient.

## Costs and rewards

**Everything is simulated.** Nothing is minted or sent on-chain. There are no RF costs, rewards, redemption or fees. The "SIMULATED GIFTS" stamp, the post office's "Practice post" notice and each gift card label this. The runtime requires a chance-game definition, so `game.json` holds an unused reference definition that the game never calls.

**Economy potential (future work with the Rare Friends team):** gifts are already on-chain-shaped (a uint256 bitmap plus from/to Friend IDs). A live version could mint each gift as a small NFT delivered to the recipient Friend's canonical wallet, paid for or burned in RF.

## Run from source

Node.js 22+ on Linux or Ubuntu/WSL2:

```sh
git clone https://github.com/gmmillar82/rareton.git
cd rareton
git checkout f59b6a6a121f8d976b15bad3fa67c772c1f329af
npm ci
npm run dev
```

Open `http://localhost:4173`. The FriendSDK v0.1.2 release archive is included in the repo.

## Checks

All pass:
- TypeScript typecheck (`npx tsc -p .`)
- `friendsdk check` game validation and `friendsdk build`
- `friendsdk test` automated browser smoke checks at 960 px and 360 px
- A scripted walkthrough at 960 px and 360 px (`node scripts/smoke.mjs`): walk to the post office, address a letter (with a live artwork lookup) and send it, then check the satchel

The automated checks use the SDK's mock wallet and RPC. A real-wallet playthrough on the hosted preview passed on desktop (MetaMask extension) and mobile (MetaMask in-app browser).

## Known limitations

- Progress resets on reload, because the SDK sandbox has no storage.
- Recipient Friend numbers aren't verified: any number shows its registry artwork, even if not minted.
- Mobile requires a wallet's in-app browser.
- No live minting, trading, wearable NFTs or creator fees.

## Credits

Friend sprites are canonical Rare Friends artwork, used under the FriendSDK NOTICE. Player and recipient sprites are read live. The villagers use Friends #21, #77, #3 and #150, baked in from the public registry, with invented names and dialogue. Sounds come from the FriendSDK sound kit. The village, flowers and gift art are original pixel art drawn in code.
