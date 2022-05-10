# iaNFT-Flow

## iaNFT Summary
- Values:
  - open collaboration
  - supporting other builders' projects
  - composing our blockchain resources
  - designing with the resource oriented model of Flow in mind -- I think that Cadence allows us opportunities that other blockchains don't have 
  - discipline as a builder on Flow for the long term

- About iaNFT:
  - iaNFT asks of us to accept artwork as a serializable data structure analoge to an SVG. When we do this we can operate in data structure land, which means we can programmatically operate on blockchain artwork and do so collaboratively and safely.
  - will contain tools and tutorials to make the general pattern easier and to spur greater combposability on Flow
  - am consulting with the Flow community to make sure iaNFT is made in their image, satisfying their requirements

- gotchas:
  - the POC and initial library/tools will only deal with pixel grid SVG artowrk, but it is definitely expandable to any kind of SVG using any shape. There may be some other supported files later down the road but for now we want to succeed with SVGs.

Example Pattern:
1. developer designs and implements an NFT resource that utilizes iaNFT data structures representing an NFT and composite/re-composite methods. Developer crafts interfaces and leverages cadence methods to design a permission scheme for the on-chain artwork layers (immutable layer, semi-mutable, fully mutable, who can and cannot modify, when it can be modified, how many times, etc.)
2. developer and/or artist create SVG artwork
3. script serialzes the artwork to cadence data structures and adds them to the blockchain
4. app user acquires the iaNFT 
  - uses it in the core experience
  - takes it to another Flow project that implements iaNFT and caters to the original use case and design intentions of the NFT to participate in the modification of the on-chain artwork
5. app user sells the iaNFT as they would with any other NFT and new owner can do what they wish

Components of iaNFT:
- artwork prep script
  - converts pixel grid (or rectanble only) png image to SVG
  - cleans/minifies the SVG
- on-chain serializer script
  - parses SVG files and adds them to an on-chain art library
- iaNFTViews contract (note, we are forking the Flow Metadata standard, MetadataViews.cdc, and repurposing it for the following)
  - view resolvers
  - SVG cadence analog
  - composite/re-composite method
  - SVG shape color-modification method
  - art library
- tutorials 

Execution:

I gave a demo of a POC app using iaNFT in the [May 5th 2022 Office Hours](ttps://www.twitch.tv/videos/1475732343?t=00h31m04s). It works as expected, but it needs refinement and generalizations to make it useful for the community. One way to achieve this is by designing and developing a real-world (mainnet) NFT project that uses iaNFT to exercise some really cool composability. As the NFT project proves the usefulness of things like the iaNFTViews.cdc contract and the scripts, they will be released and opened for debate and contributions from the Flow developer community. Stay tuned for more details on this, but the basic idea is to co-develop the open pattern and tools with an 'applied' NFT project deployed to mainnet and offered to the community. Ultimately if the community can accept that a never-not-serializable representation of SVG artwork on-chian is robust, then I hope we can create it into a new standard for Flow.



