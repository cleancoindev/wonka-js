# Windex

The Wonka Index API (called "Windex") is a blazing fast API on top of Solana/Metaplex NFTs, to enable quick and performant fetching of NFTs by collection, wallet, and mint.

The Wonka Index is currently in Alpha testing. Please join our [Discord](https://discord.gg/p2wXHT3vm7) to report issues or suggest new features, or open up a Github Issue.

![windex](https://user-images.githubusercontent.com/796815/157983069-284a79f0-2379-412a-a529-eaca36a2428b.jpeg)


## Javascript Library

The easiest way to get started with Windex is using the `wonka` JS library.

### Installation
```
npm install @wonka-labs/wonka-js
```

### Fetching NFTs by Candy Machine or Collection ID

To display all NFTs in a collection, you can query Windex by Candy Machine ID or Collection ID (the collection id is the first verified creator in the NFT's metadata).

```JS
import Windex from "@wonka-labs/wonka-js";
import { PublicKey } from '@solana/web3.js';

const fetchNFTsByCandyMachine = async(candyMachineId: PublicKey) => {
  const nfts = await Windex.fetchNFTsByCandyMachineID(candyMachineId, 20, Windex.DEVNET_ENDPOINT);
  console.log(`Retrieved ${nfts.length} NFTs!`);
}

const fetchNFTsByCollection = async(collectionId: PublicKey) => {
  const nfts = await Windex.fetchNFTsByCollectionID(collectionId, 20, Windex.DEVNET_ENDPOINT);
  console.log(`Retrieved ${nfts.length} NFTs!`);
}
```

### Fetching NFTs in a Wallet Address

```JS
import Windex from "@wonka-labs/wonka-js";
import { PublicKey } from '@solana/web3.js';

const fetchNFTsByWallet = async(walletAddress: PublicKey) => {
  const nfts = await Windex.fetchNFTsByWallet(walletAddress, 20, Windex.DEVNET_ENDPOINT);
  console.log(`Retrieved ${nfts.length} NFTs in ${walletAddress}'s wallet!`);
}
```

### Fetching an NFT by Mint Address

```JS
import Windex from "@wonka-labs/wonka-js";
import { PublicKey } from '@solana/web3.js';

const fetchNFTsByMintAddress = async(mintAddress: PublicKey) => {
  const nft = await Windex.fetchNFTByMintAddress(mintAddress, Windex.DEVNET_ENDPOINT);
  if (!nft) {
    console.log("nft not found!");
  } else {
    console.log(`Fetched ${nft.address}: ${nft.name}`);
  }
}
```

### Fetching Sol Domain Name from Wallet Address

```JS
import Windex from "@wonka-labs/wonka-js";
import { PublicKey } from '@solana/web3.js';

async function fetchSolDomainByWalletAddress(walletAddress: PublicKey) {
  const solDomainMetadata = await Windex.fetchSolDomainMetadataByAddress(walletAddress)
  console.log(solDomainMetadata)
}
```

### Fetching Wallet Address from Sol Domain

```JS
import Windex from "@wonka-labs/wonka-js";

async function fetchWalletAddressBySolDomain(solDomain: string) {
  const solDomainMetadata = await Windex.fetchAddressBySolDomain(
    solDomain
  );
  return solDomainMetadata?.address
}
```

### Fetching Twitter Handle from Sol Domain

```JS
import Windex from "@wonka-labs/wonka-js";

async function fetchTwitterHandleBySolDomain(solDomain: string) {
  const solDomainMetadata = await Windex.fetchAddressBySolDomain(
    solDomain
  );
  return solDomainMetadata?.twitter
}
```

## GraphQL API (Advanced)

Alternatively, you can use the GraphQL API to access the exact fields you need. The easiest way to craft queries is through the [Windex interactive query editor](https://api.wonkalabs.xyz/v0.1/solana/graphiql?cluster=devnet).

The main endpoints are `candyMachineV2(...)`, `nftsByWallet(...)`, `nftsByCollection(...)`, and `nftByMintID(...)`. Using the query editor, you can discover and query for all of the fields you need (from both off-chain and on-chain data).

The GraphQL API is a typical HTTP POST request:

```
curl 'https://api.wonkalabs.xyz/v0.1/solana/mainnet/graphql' \
  -H 'content-type: application/json' \
  --data-raw '{"query":"query { solDomainNameForAddress(address:\"BYeHCJtokQecDkN34ZE4fWgF7U4vDtwjX6bkaiaprQmt\") }\n","variables":null}'
```

(There are plenty of [GraphQL client libraries](https://graphql.org/code/) that make it a little easier to enable complex GraphQL queries and responses in your frontend and backend code as well)

