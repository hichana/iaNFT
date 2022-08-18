# The State of Interactive NFTs on Flow

_Hi, I'm Matt Chana, a member of the Flow community more commonly known as ‘hichana | iaNFT#7569’. This report is the initial deliverable for a project I call [iaNFT](https://github.com/hichana/iaNFT-Flow) (Interactive Non Fungible Tokens) – an in-development set of tools and composable smart contracts on the Flow blockchain for creating fully [on-chain](#on-chain), infinitely compositable and mutable artwork assets for interactive NFT experiences. This project is supported by the [Flow Ecosystem Fund's](https://flow.com/ecosystemsupport) developer grant program, which I'd recommend to anyone looking to build innovative apps and tools on Flow._

As builders on Flow we all have some intuitive understanding of what [interactive NFTs](#Interactive-NFTs) are and what we can do with them. Some awesome projects might come to mind. In [Flovatar](https://flovatar.com/), you can use their [builder tool](https://flovatar.com/builder) to customize your avatar with traits, accessories (called “flobits”) and rarity boosts. Some traits are animated, or even directly interacted with by [clicking on a basketball](https://flovatar.com/builder#combination=B57H386FxE259N449M439C111&accessory=2&background=479) for instance. In a similar vein, [The Fabricant is teaming up with leading fashion designers and NFT brands like World of Women](https://www.thefabricant.studio/campaigns/worldofwomen) to let you co-create unique 3D digital fashion by combining garments, choosing materials and patterns, and applying colors.

There's a reason why Flow has become a home for interactive and [composable](#Composability) NFTs; it's the Cadence [resource-oriented programming](#Resource-Oriented-Programming) language and it lets us intuitively apply real-world ownership models to digital assets, giving us the ability to interact with and modify the digital assets that we own on the blockchain. This report aims to demonstrate why Cadence is uniquely suited for building dynamic and interactive NFT experiences. In doing so, we'll introduce a new concept called _private composability_ and show how we can do new things with an NFT, like allowing it to be used as an infinitely modifiable building block across future, **yet-to-be-released** NFT projects. We'll also review the current state of interactive NFTs across the web3 space, discuss their challenges and outline several unique opportunities. Lastly, this report introduces iaNFT, the framework to build unique, interconnected and interactive NFT experiences on Flow.

If you're new to Flow, read the [Flow Primer](https://www.onflow.org/primer) to learn about the underlying technology, check out the [glossary at the end of this article](#Glossary) with some key terms, or visit [Flowverse](https://www.flowverse.co/) to see many of the projects on Flow. You can email me at matt@floasis.fun with any questions or comments.

### STATE OF INTERACTIVE NFTS

Interactive NFT experiences have a couple key requirements:

1. An interactive user interface (e.g. a game or app users use to interact with the NFTs)
2. A way to persist state changes directly associated with the NFT, usually displaying them in the user interface

Ideally we would memorialize state changes directly to the blockchain as much as possible because that’s where our NFTs live. Yet, that’s not usually what happens. Even with tiny amounts of data, most Interactive NFT experiences store their data using decentralized approaches like IPFS naming service patterns, or even on centralized databases. In contrast to those approaches, I believe Cadence makes the Flow blockchain best-suited to solve the second requirement for persisting state changes. As I’ll demonstrate later, the on-chain storage model of Cadence gives us the opportunity to persist meaningful amounts of stateful data directly on the Flow blockchain **without compromising ownership of the data**. Data ownership is important because we often invest time and effort into building out a game character or upping the rarity of a [PFP](https://www.gemini.com/cryptopedia/what-is-a-pfp-nft-pfp-crypto-pfp-nft-profile-pic-twitter#section-what-are-pf-ps), for example, and we can often later realize that stored value by selling our assets  in a marketplace if we choose. We might want to move a record of our interactions to other experiences later on. When we don’t have custody over that data the NFT is essentially tied to the originator of the NFT experience and their good faith intentions to maintain the project and keep the data accessible. 

The ability to persist state changes for our NFTs has some cool applications, like changing the accessories on our avatars and evolving game characters. I'm pretty sure it's where most of the engagement for NFTs is going to come from for now on, as many projects like Yuga Labs' [Otherside](https://otherside.xyz/world) sell dynamic land NFTs and take [their characters and universe in that direction](https://twitter.com/danny_p3d/status/1521110733420908546). There are a number of notable interactive NFT projects out there already.

#### Existing Interactive NFT Projects

Composability of NFTs across NFT experiences is still nascent, but it’s where we’re headed. Flovatar is a great example of a project on that path. Its development team churns out fun interactive experiences and demonstrates they can consistently tick items off their project roadmap. According to Luca, the founder of Flovatar, it's not just about being a PFP project. The intention is more about making a brand, like Lego or Disney of sorts, but for Web3. The PFP is just an important building block in that journey. The story they tell plays through the lens of those characters as they evolve, connecting to other things like airdrops, 3D models of the Flovatar, and the $DUST token that owners accumulate over time. Luca intends to make Flovatar more composable, extending the Flovatar experience into other metaverses, DeFi, and so on. On top of that, Flovatar data is stored on-chain. I'll explain why I think that's awesome later.

Here are the standout projects that are, or soon will be, leveraging interactive NFTs on Flow and other blockchains.

##### Flow Projects

| Project Name | Highlights|
| --- | ----------- |
| [Enemy Metal](https://enemymetal.com/) | has a card-crafting system in which players can transform and upgrade their cards, is also building a [starship factory](https://enemy-metal.gitbook.io/info/the-future/starship-factory) to build 3D NFT fighters |
| [Dark Country](https://darkcountry.io/) | has a card-crafting system that relates to other activity such as joining a guild |
| [GoatedGoats](https://goatedgoats.com) | upgrade or swap traits and accessories to make them match your personality and interests |
| [Flovatar](https://flovatar.com) | builder experience – design exactly the Flovatar that you want, one that you can identify with from the start, upgrade or swap traits and accessories at any time, some fun interactions have been achieved like click or tap interaction to [animate a basketball in the artwork](https://flovatar.com/builder#combination=B57H386FxE259N449M439C111&accessory=2&background=479) or artwork changes upon processing a transaction ([the balloon prank](https://twitter.com/flovatar/status/1520509399466483716)), view your Flovatar in 3D, download png version
| [The Fabricant](https://www.thefabricant.studio/) | users are invited as 'co-creators' and they select a garment and material, and they can colorize some parts of the garment as well.|
| [Some.Place](https://some.place) (also building on Ethereum) | promises "novel types of NFTs, unlike anything in-market today: interactive, 3D, AR." |
| [Matrix World](https://matrixworld.org/home), [Matrix World white paper](https://d2yoccx42eml7e.cloudfront.net/website/whitepaper.pdf?date=1650867640014) (also building on Ethereum) | promises programmable NFTs and objects. "_Landowners can customize their behaviors, appearances, and transformations via Turing-complete programs._", example, a developer can build a 3D ATM that when interacted with invokes functions from a decentralized token exchange, In Q4 of 2022 is promising a object and scene editor via a low-code creation tool, users will be able to make Matrix Objects into NFTs |
| [Genies](https://twitter.com/genies/status/1527056009037901824?s=21&t=Oi-WLLuAiorktDTuzHxaIg), [how to become a seller on Genies](https://genies.com/becomeaseller) | recruiting people to create and sell digital clothing for avatars, building a tool to allow people to create different 'species', like a Zen Teacup or Psychotic Bunny |
| [Build on Flow Banner](https://www.buildonflow.art/) | The first fully customizable on-chain SVG banner NFT for Flow builders, show off your Floats and NFTs, determined to build more and more interactive elements |
| [Zeedz](https://www.zeedz.io/) | has a growing game to turn a Zeedle into other forms and shapes, growing its uniqueness and value |

##### Non-Flow Projects

On non-Flow blockchains, projects like The Sandbox will soon make it possible to integrate other project PFPs like World of Women collection into their metaverse. Here are some of the standouts:

| Project Name | Highlights|
| --- | ----------- |
| [Async.art](https://async.art/) | interactive and generative art & music NFTs |
| [ArchLoot](https://www.archloot.com/) | [Loot](https://www.lootproject.com/)-style composability with interactive game play |
| [Cybertino](https://docs.cybertino.com/introduction-to-cybertino#r5-2-interactive-nft) | claims to have made the world's first interactive NFT with a mutable layer, allowing influencers for example to add messages as a way to create an exclusive community to communicate with, made [influencer coin NFTs](https://cybertino.com/nft/b2fb4043-eb36-4966-b99e-6060947088fc)  that changed color if [influencer wins a competition](https://www.dexerto.com/entertainment/youtube-vs-tiktok-boxing-event-1534527/) |
| [Decentraland](https://decentraland.org/) | [parcels of land](https://market.decentraland.org/lands), [NFTs displayed as 3D picture frames holding a 2D image](https://docs.decentraland.org/builder/nfts/), [custom wearables as NFTs](https://docs.decentraland.org/decentraland/wearables-overview/) |
| [The Sandbox](https://www.sandbox.game) | similar to Decentraland ,collaborations, like [with Snoop Dogg](https://www.sandbox.game/en/snoopdogg/), where the avatar NFTs are playable in the Sandbox |
| [The Merge, by Pak](https://niftygateway.com/collections/pakmerge) | dynamically generated visuals, all on-chain, the interaction happens as a function of the activity of all owners |
| [The Cryptids](https://thecryptoids.io/) | nft game is [playable directly on opensea](https://opensea.io/assets/ethereum/0x1427db973f9428d8a9211e177d9e53b21f314bcd/463) |
| [Zerion DNA](https://zerion.io/dna/) | dynamic avatar based on your on-chain footprint (your wallet contents) |
| [Mechaverse](https://themechasnft.com/) | personalize, breed Mechas characters |
| [Saplings](https://www.saplings.earth/) | 3D interactive NFTs |
| [Nuvola's Cloud NFT](https://nft.nuvolaproject.cloud/) | cloud has lightning every time someone tweets with specific hashtags related to climate change |
| [Fuzzle](https://collectfuzzle.com/) | cute characters you can chat with and via AI as they learn and develop their own unique personality |
| [Aurory](https://aurory.io/) | hatch and evolve ‘Nefties’ characters |
| [Axie Infinity](https://whitepaper.axieinfinity.com/gameplay/breeding) | ‘play to earn’ game with the ability to breed Axies with new offspring and traits |
| [Baby Blobs](https://babyblobs.art/) | claims to be the first interactive NFT collection on Solana where each Baby Blob is its own custom HTML file |
| [PORTALS](https://theportal.to/) | a dense city-focused metaverse on Solana |
| [Infinity Labs](https://infinitylabsnft.com/) | collect and merge NFTs. 8,888 NFTs are eventually merged into 1,111 |
| [Solarians](https://www.solarians.click/) | on-chain robot characters, has a 'changing room' where you can customize the appearance of your Solarian |
| [NFT Worlds](https://www.nftworlds.com/) | [Land-based NFT builder](https://www.nftworlds.com/build) |
| [FLUF World](https://www.fluf.world/) | 3D-like characters with music, animation and the ability to customize things like the background of the NFT art using the [builder tool](https://under.fluf.world/) |
| [Rebels](https://rebels.art/) | A pfp collection of dynamic NFTs aimed at on-chain customization |
| [x0r](https://twitter.com/x0rart) | [basefee](https://twitter.com/x0rart/status/1545795506084818946) - fully on-chain dynamic NFT that visually represents the ETH burned in every transaction due to EIP-1559, [MEV Army](https://mevarmy.x0rart.com/), [MEV Army Banners](https://twitter.com/x0rart/status/1525161741050445824?s=20&t=T_BnMJcuIzEPEyXZtRI_6w) |


#### So, what is the state of interactive NFTs on Flow? 

It’s good, but first I want to point out that the state of interactive NFTs on non-Flow chains is looking pretty good, too! Ethereum has the advantage of having been around for a long time compared to other chains. Permissionless deployment of smart contracts on Ethereum has been available from the start, and as such there has been a lot of experimentation there ([see some of the stuff x0r is up to for inspiration](https://linktr.ee/x0r)). Ethereum also has a [massive market capitalization overall](https://www.slickcharts.com/currency) so the incentive to build there is high. Other non-Flow top blockchains, especially Solana, have a growing ecosystem of interactive NFT projects as well. 

Overall, I think we're going to see continued experimentation with interactive NFTs because that's what more engagement demands, and more engagement and utility is what we want. The problem is that most non-Flow blockchains need to solve some scalability issues before being viable for high-engagement interactive NFT experiences. Some projects like The Sandbox have started to [migrate to layer 2 solutions](https://dappradar.com/blog/the-sandbox-migrates-to-polygon-this-summer) like Polygon so they can meet the demands as users interact and do more with their NFTs. In general, blockchains are working to be faster and better. [Solana](https://solana.com/) focuses on speed, claiming to be the fastest blockchain in the world. Layer 2 solutions like [Polygon](https://polygon.technology/) aim to be a fast and cheap decentralized scaling platform for Ethereum. Ethereum itself will see a number of [scalability upgrades](https://ethereum.org/en/upgrades/) come to fruition as a change of consensus mechanism coupled with a [layer 2 scaling initiative](https://ethereum.org/en/developers/docs/gas/#initiatives-to-reduce-gas-costs) helps reduce gas costs. The data storage needs for interactive NFT experiences on these chains might be provided by solutions like airweave and IPFS as [on-chain data storage is quite expensive](https://forums.solana.com/t/storing-data-on-the-blockchain/5405/2). I don't see why excited developers won't deliver a lot of interesting projects in this space.

Ethereum, Ronin, Solana, Flow and Polygon are the leaders in all-time NFT sales volume (in that order) according to [CryptoSlam.io](https://cryptoslam.io/), with two of those being Ethereum-linked sidechains ([Ronin](https://whitepaper.axieinfinity.com/technology/ronin-ethereum-sidechain), made specifically for Axie Infinity, and Polygon as a more general purpose sidechain). The number of NFT projects on Flow that we can consider to be ‘interactive’ is likely in the tens, while adding up the interactive NFT projects on all the other chains is likely some multiple of that, with more coming out all the time. The Sandbox, NFT World and FLUF World are interactive NFT projects amongst the top ten most profitable NFT projects from Q1 of 2022 according to [NonFungible.com](https://nonfungible.com/).

The list of interactive NFT projects on Flow from above hints that in the very least, Flow developers are thinking creatively and delivering engaging projects to the community, just as the other top chains are doing. There's no evidence to suggest a winner-take-all scenario for NFTs on any given blockchain, and that doesn't seem to be what most developers I know are rooting for anyways. Most developers I know in the community are just excited about building on Flow and being there as the community and the experiences grow. One thing is clear, as interactive NFT experiences gain traction on Flow, if any of them really blow up in popularity, Flow can handle it. NBA Top Shot has almost 21 million all-time transactions in owner to owner sales alone (at the time of writing). That's almost 4 million more than the next most transacted NFT project, Axie Infinity. Imagine Flow developers doubling down this crypto winter, building partnerships like the one with the NBA, and creating the next interactive NFT version of a [multiple-million-dollar market cap projects like NBA Top Shot](https://momentranks.com/topshot/market). On top of that, the Flow team has just rolled out permissionless contract deployment which should expand the body of projects experimenting with interactive NFTs.


| Blockchain | Permissionless deployment | On-chain data (artwork assets and metadata) | Transactions Per Second | Programming language | composability |
| --- | --- | --- | --- | --- | --- |
| Flow | YES | An NFT storing an on-chain image with the size of a few megabytes is easy and cheap | [Approx. 100](https://docs.onflow.org/faq/developers/#what-is-the-expected-tps-transactions-per-second-for-the-forseeable-future) | Cadence | Public and Private |
| Other notable chains | Mostly YES (with Ronin, only through their [builders program](https://axie.substack.com/p/axie-infinity-builders-program?s=r) | Very small amounts of data are fine, ex. Enough for [ascii art](https://twitter.com/x0rart/status/1545795506084818946) or [nouns art](https://nouns.wtf/) nouns art | Ethereum: approximately 15 currently, but expected to grow with scaling efforts, Solana: [approx. 2500](https://explorer.solana.com/) | Ethereum: Solidity, Solana: Solana: Rust, Typescript | Public |


Flow has a whole different architecture than other blockchains. Its multi-node/multi-role architecture is designed for extensive scaling without [sharding](https://www.investopedia.com/terms/s/sharding.asp#:~:text=Sharding%20splits%20a%20blockchain%20company's,when%20compared%20to%20other%20shards.). The Flow blockchain architecture is paired with the [Cadence](https://docs.onflow.org/cadence/language/#:~:text=The%20Cadence%20Programming%20Language%20is,(inspired%20by%20linear%20types).) resource-oriented smart contract programming language, featuring [linear types](https://wiki.c2.com/?LinearTypes) and [capability-based access control](https://docs.onflow.org/cadence/language/capability-based-access-control/). I believe Flow is the most well-suited blockchain for interactive NFT experiences because of this architecture. If you're thinking that I'm just inserting a bunch of technical jargon to sound smart, well then you're right 🤓😈, but still, trust me there's a reason for it!!!

When you design and implement a smart contract in Cadence, you quickly learn to think in terms of resources (the linear types part), and access control to those resources (capability-based access control). You also realize that Flow and Cadence give you the ability to store meaningful amounts of data directly on-chain and in the custody of the NFT owner.


##### Public Composability

With respect to blockchain, most people know about the notion of ‘composability’. I’m appending it with ‘public’ because later I want to introduce the concept of ‘private’ composability, which is something I believe is unique to Flow. Public composability is the superpower of blockchain. Ethereum, Solana, Flow and most other chains are designed around it. It’s used with many DeFi protocols on Ethereum and other blockchains, something known as '[money legos](https://academy.shrimpy.io/post/what-is-defi-composability-an-introduction-to-money-legos)'. A great example of this on Flow is the [NFTStorefront](https://github.com/onflow/nft-storefront) contract that lets anyone create a peer-to-peer marketplace to sell NFTs without having to build and deploy their own marketplace contract. I wanted to specifically call it out as "public composability" because it mostly depends on open code and functions on the blockchain. For public composability, a smart contract essentially says:

“Hey! I’m open, poke around! Call my methods, look at my code -- go ahead and fork me if you want!!!”


##### Private Composability

Again, public composability happens on Ethereum, Solana, Flow, and so on. It’s the reason we can fork smart contract code or directly invoke functionality from other project’s smart contracts as the building blocks for new experiences. In addition to being publicly composable, NFT resources on Flow can also be privately composable. It’s ‘private’ because it’s the owner of the NFT who decides how and when to modify its internal state (metadata, artwork assets) as they utilize them as building blocks in existing and future NFT experiences. Here’s how it works:

1. original designer of the NFT can craft the internals (metadata and artwork assets) so they can be safely and infinitely modified by the owner
2. the owner maintains custody of its internals by owning the NFT
3. internals can be modified with the permission of the owner as they use use the NFT as a building block in new experiences on Flow

When you have a resource on Flow as an NFT, it's your account on the Flow blockchain that takes custody of it. If the data for an interactive NFT experience is also stored there, it’s very hard to compromise that data. In fact, the data can persist as long as the Flow blockchain itself does, or as long as you own the account. Even if the original NFT experience disappears, your NFT and its data will always be there, ready to be used as a building block for another experience. On Ethereum a smart contract essentially has a ledger that records who owns what, and the 'what' part is usually the asset (artwork, metadata, etc.) that is stored [off-chain](#off-chain), like on IPFS for example. 


![Smart Contracts](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4sr4yavqa30f09x13h3b.png)

![NFTs](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9td4enib578ny5jfbidg.png)

As builders on Flow, we have the opportunity to nest the asset directly into the NFT itself, so it's stored fully on-chain and in your custody as the owner. We use the Cadence programming language to do this, so as designers of the NFT we can apply a set of rules around the assets if we choose, like who can change it or not, what can go there, and so on. If I embed assets into the NFT with a certain Cadence structure, one that is safely enforced by the Cadence programming language, then it means that I can also allow other projects to access and modify the asset according to my design. I don't have to whitelist other NFT projects to operate on that asset as all they have to do is conform to the rules I've placed on the asset and do it all in Cadence. The owner of the asset can give permission (or not) for a totally unrelated project to add or reshape artwork or metadata. This is why it’s a ‘private’ type of composability, because it’s about a relationship between the owner of the NFT and any other NFT project out there they wish to bring it to, yet they can still use it as a building block in other experiences. The originator project that first made the NFT doesn't have to be around for this to happen, so in a way **the NFT can be composed endlessly across future, yet-to-be-released, NFT projects**. It's the owner of the NFT who has custody of it, and there is no external dependency or expectation that anyone else but the owner should ever need to make certain the assets are pinned to IPFS for example. It's a more accurate model of real-world assets. My assets are custodied in my wallet (thanks to linear types) and I’m free to bring them around the metaverse to use them as building blocks for new experiences (thanks to capability-based access control). Blockchain is all about making guarantees (like there will only ever be 21M Bitcoin). I think our digital assets as NFTs need to do something similar with their metadata and assets. For private composability, a smart contract essentially says:

“Hey! I’m going to give you access to some of my assets. I’ll let you operate on them too. With my permission, go ahead and use them for this whole new and unrelated experience. You can do this safely and never corrupt my original asset thanks to how it was designed by the NFT creator in the Cadence language!”

For me, the technical jargon about the Flow blockchain is meaningful as it's the foundation for this new modality of blockchain composability.

#### Expanding Private Composability with iaNFT

Today on Flow, we can already embed meaningful amounts of metadata and assets inside of an NFT. We can design it so that its content can be useful as a building block when taken to another NFT experience. It’s up to the owner of the NFT to allow other NFT experiences to modify the metadata and add assets. In this sense private composability patterns on Flow are just waiting to be crafted by us builders. With iaNFT, I’m creating tools that will allow us to embed Cadence analogs to the common [SVG](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics) file format inside of the NFT. The Cadence analogs are serializable to SVG and back to Cadence at any time. What this unlocks is the ability to modify the properties of an SVG like changing its colors, shape or animation. We can also composite the artwork on-chain and do so safely across unrelated NFT projects if we choose. From the [iaNFT GitHub repo](https://github.com/hichana/iaNFT-Flow):

> iaNFT is an in-development set of tools and composable smart contracts on the Flow blockchain for creating fully on-chain, infinitely compositable and mutable artwork assets for interactive NFT experiences. These assets can safely evolve as they are utilized in various NFT projects on Flow, adding new layers, recompositing/reordering layers and even changing the properties (color, shape) of the artwork itself. For a summary and proof-of-concept demonstration of iaNFt, see the [Flow Office Hours presentation from @hichana](https://youtu.be/q8jk5hGAwSs?t=1865).

I think we’re just getting started when it comes to public and private composability. Although public composability is a foundational part of blockchain, NFT projects on Flow and other blockchains are mostly isolated. They don’t really openly leverage each other’s resources and userbases to create new experiences (there have been some notable sketchy instances elsewhere, like in the [Sushiswap/Uniswap vampire attack saga](https://www.gemini.com/cryptopedia/sushiswap-uniswap-vampire-attack#section-what-is-a-vampire-attack)). For private composability, I hope iaNFT can demonstrate a robust enough set of tools to inspire NFT projects on Flow to accept each other’s NFTs and safely operate on their metadata and artwork assets. 

I’m hopeful we can expand the body of NFT projects that integrate public and private composability patterns. There is already collaboration between notable projects that seem like a precursor to this, like [Matrix World giving land owners the ability to participate exclusively in a mint from The Fabricant](https://medium.com/@matrixworld/matrix-world-partners-with-the-fabricant-c09e3fb229a5?source=rss-1749959bd98e------2). Platforms like Matrix world, although they are starting with their own custom avatars, claim to be considering supporting other projects' avatars into their platform. According to some.place, - "We see a world where you’ll be able to use your Meebit or Crypto Chick as your avatar, to truly capture that community spirit." I think Flovatar is going to succeed in this direction too. With Flovatar NFTs:

* all art is on-chain
* the owner has custody of the the metadata and artwork assets
* interactivity is embedded directly into the NFT artwork
* the artwork is composited/re-composited on-chain
* has no external dependencies (is same SVG format that has been around for decades)

Flovatar NFTs are essentially building blocks waiting to be used in clever new ways. I think it’s projects like this, public composability, and private composability that doesn’t compromise ownership of our assets, that will expand the current state of interactive NFTs on Flow and other blockchains.

If you’re interested in private composability patterns and iaNFT, or working on an interactive NFT project I’d love to hear from you. I’m also working on an NFT project for Flow called [The FLOASIS](https://www.floasis.fun/) that will [‘dogfood’](https://www.jetbrains.com/lp/dogfooding/#:~:text=Dogfooding%20is%20a%20practice%20especially,eats%20its%20own%20dog%20food%E2%80%9D.) iaNFT and show the community some of the cool things I think we can do. If you’re interested in and good at community, partnerships and marketing, I’d really love to hear from you :) 

Email: hichana@floasis.fun

iaNFT/FLOASIS discord: [https://discord.gg/RMZxSaqn](https://discord.gg/RMZxSaqn)

I added this article to the [iaNFT repo on GitHub](https://github.com/hichana/iaNFT-Flow). You’re welcome to send me a PR to help improve it. I hope you got some value here and wish everyone building an interactive NFT experience on any blockchain lots of success :)


### Key Takeaways



* Interactive NFT projects are a notable paradigm in blockchain, with some big names like Yuga Labs and awesome indie projects like Flovatar headed in that direction
* There is a wide and increasing variety of experimentation into interactive NFTs across the top blockchains
* Non-Flow blockchains will likely benefit from protocol improvements, like Ethereum's layer 2 scaling initiative, while Flow is already uniquely well-positioned to handle interactive NFT experiences
* Composability of NFTs across experiences on any blockchain is still nascent. The architecture of Flow provides us with a new dynamic of composability I'm calling 'private composability'. With it we custody resources and assets on the actual NFT and take them to new experiences that don't necessarily need the originator project to be around anymore in order to maintain their utility. This is the approach that most closely fits with the ethos of blockchain, aligning user interfaces and persisting state changes without compromising ownership.
* iaNFT aims to extend the use of private composability for builders on the Flow blockchain


### GLOSSARY

Here are some key terms related to this article. I'll mostly just try to characterize and illustrate them rather than shooting for a strict definition. 


#### On-chain

This usually refers to code or data that lives on the blockchain. It can be stateful or not. When minting an NFT, for instance, there is usually a variable on the smart contract that is incremented to record the total supply. The incrementing of the variable is happening 'on-chain'. In the case of Flow, we can easily store data in smart contracts or even within digital assets like NFTs. It works kind of like this – all NFTs on Flow implement the [Flow Non-Fungible Token standard](https://github.com/onflow/flow-nft). The NFT itself is just a block of code that conforms to that standard. In addition to the requirements of the NFT standard you can store other data inside as well. So if the NFT represents something like a ticket to an event, then that block of code is where you have an opportunity to store things like an image of the ticket or other metadata like the event name, performers, and so on. It's up to the developer to implement what else goes there in addition to the Flow NFT standard requirements. Adding these 'extra' things to the NFT or a smart contract – things that are meaningful to the experience you're trying to build – are also part of what we colloquially refer to as 'on-chain'.


#### Off-chain

With any blockchain, including Flow, there will be data that can or should live outside of the blockchain but still be referenced on the blockchain. Metadata or images for NFTs are sometimes stored on services like [IPFS](https://ipfs.io/) or Arweave, and a reference to where they are stored is added to the NFT itself.


#### Resource Oriented Programming

[The definition from Flow](https://docs.onflow.org/cadence/):

> Resource-oriented programming, a new paradigm that pairs linear types with object capabilities to create a secure and declarative model for digital ownership by ensuring that resources (which are used to represent scarce digital assets) can only exist in one location at a time, cannot be copied, and cannot be accidentally lost or deleted.

In other words, the digital assets we create can behave more like assets do in the real world. ‘Scarce digital assets’ are represented in code. With resource-oriented programming, we have custody of them in our individual Flow account. Also like many real world assets, we can choose how public we wish to make them -- i.e. inaccessible to the rest of the world, partially accessible or fully accessible.

In some non-Flow blockchains, a smart contract maintains a long list (aka a ledger) of the owners of scarce digital assets like NFTs. Most of the data for those assets is not stored on the blockchain itself. So when you buy or sell them, the address for the previous owner is simply swapped out for the new owner's address. There are some exceptions to this on-chain-ledger-off-chain-asset-data paradigm, but they mostly just store data on-chain on behalf of the owners. On Flow, we can take custody of digital assets as we would with real world assets.

It's up to us what we do with these resources. We can even get rid of them if we choose, so they are essentially deleted, or 'burned', never to be recovered again. It's this model of taking custody of a unique digital asset, deciding the access to it that we wish to grant others, and exercising self-determination over what we do with it that the resource-oriented programming centers around. As Cadence developers we design with that in mind. 

A resource-oriented model that leverages on-chain data storage like this even more closely represents the real world. If you inflate a resource with more things to store, it's up to you to pay for that capacity, but that's a good thing. If you've paid for it and you have custody of it it will be there until the end of the blockchain itself. With other approaches where the data is stored off-chain, if you're not the one paying for that storage you might want to find out who is and what their incentive to steward that data indefinitely might be...


#### Interactive NFTs

An NFT might integrate some public data to change the state of the artwork so as the data changes the artwork itself is automatically updated. In this case it's more like the interaction is between the NFT and the data, wherever it may be on the internet or elsewhere. This is an interesting approach because even after the NFT has been purchased it can conditionally display artwork based on things that are hard to predict, like the winner of a contest or price of a cryptocurrency. Direct user interaction via clicking or tapping on the artwork, processing a transaction or playing a game are the most obvious ways NFTs might be interactive. The state changes resulting from those interactions are often not stored on the NFT itself, though ideally they would be. For the sake of this article we will consider an interactive NFT to be any digital experience that has interactivity in some form, persists state changes over time and memorializes those elements as an NFT.


#### Composability

[The definition from Flow](https://docs.onflow.org/cadence/tutorial/06-marketplace-compose/#gatsby-focus-wrapper):

> the ability for developers to leverage shared resources, such as code or userbases, and use them as building blocks for new applications.

Composability is one of the superpowers of blockchain.

In fact, almost every smart contract leverages composability in some way or another. Composability is like those plants or fungi that scientists find to be the size of a city yet also a single organism with the same DNA from end to end ([World's biggest plant](https://www.bbc.com/news/world-australia-61655327), [humongous fungus](https://www.discovery.com/nature/the-largest-living-thing-on-earth-is-a-3-5-square-mile-fungus)). The Flow team's [NFTStorefront](https://github.com/onflow/nft-storefront) contract deployed to mainnet address [0x4eb8a10cb9f87357](https://flowscan.org/contract/A.4eb8a10cb9f87357.NFTStorefront/overview) illustrates this idea well. Let's say you want to host an NFT marketplace in your app. You don't even need to fork and deploy your own version of NFTStorefront to make that happen. You simply integrate into your app the transaction code that processes the buy/sell orders via NFTStorefront. All of the logic of creating and processing orders is taken care of for you via that contract as opposed to having to create your own marketplace contract. A whole ecosystem of smart contracts can use NFTStorefront in this way. 

Composability shines when contracts like NFTStorefront are imported into a transaction for instance, and the functionality is used directly from where it lives on-chain. Still, someone can just fork and deploy a new version of NFTStorefront, one that suits some other need in a better way. The more NFTStorefront-like building blocks we have, the faster we can build and the more enabled we can be to create novel experiences. Some DeFi protocol contracts, like Uniswap on Ethereum, are used as building blocks for other DeFi projects in a similar way. Some DAOs also work similarly, where a single DAO smart contract can be useful for other DAOs to carry out their on-chain governance (For example, [CAST](https://cast.fyi/#/) by Dapper Collective).

Open source software has been described as making it so "[...every problem has to be solved once](https://twitter.com/naval/status/1444366754650656770)". This goes for blockchain composability too, but in the case of blockchain composability we get the ability to use other on-chain resources dynamically instead of just having the option to fork their source code.

Another example of composability is leveraging the tokens a user has in their wallet for other purposes. We can verify the owner of the token and use that resource to do things like gatekeeping events.[Emerald city DAO](https://www.ecdao.org/#) has created [Emerald bot](https://emeralddao.notion.site/emeralddao/Emerald-bot-962a1d4c22a5417baad56ba5c79fdfcd) – "A Discord bot that allows you to distribute roles and gate access to channels based on token/NFT holdings. Also includes a lot of useful utilities for the Flow ecosystem." In this way, composability allows us to find existing audiences for a new, maybe unrelated, event.

--- 

- A special thanks to Flow discord members #alx-flw.find#6198 and #pete#0003 for helping me edit this article with great suggestions and feedback.
- Header image (in part) created with [Craiyon.com](https://www.craiyon.com/)'s Dall-e mini service

