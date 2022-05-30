# iaNFT-Flow

iaNFT is an in-development set of tools and composable smart contracts on the Flow blockchain for creating fully on-chain, infinitely compositable and mutable artwork assets for interactive NFT experiences. These assets can safely evolve as they are utilized in various NFT projects on Flow, adding new layers, recompositing/re-ordering layers and even changing the properties (color, shape) of the artwork iteself. For a summary and proof-of-concept demonstration of iaNFt, see the [Flow Office Hours presentation from @hichana](https://youtu.be/q8jk5hGAwSs?t=1865).

## Values of iaNFT:
- collaborating and sharing tools
- supporting our fellow builders' projects
- composing our blockchain resources

## How it works:
For now, iaNFT is only concerned with SVGs. When it is on-chain, an iaNFT SVG is a Cadence struct that is always serializable to an SVG file when utilized in a frontend application for example. Representing an SVG in Cadence as an 'analog' to the SVG has some benefits above just being stored on-chain. We can programmatically operate on these structures to do things like compositing their contents and applying requirements/boundaries to how they are modified (ex. restricting a color change, or setting an upper boundary to an elements size). These boundaries allow the designer of the iaNFT to open up an artwork resource to receive and composite new layers from any other NFT experience on Flow. Co-collaboration of artwork across projects on Flow like this is one great example of blockchain composability.

Here is an example of the SVG Cadence analog:
```
    pub struct RectAttributes {
        pub let x: String
        pub let y: String
        pub let width: String
        pub let height: String

        init(
            x: String,
            y: String,
            width: String,
            height: String,
        ) {
            self.x = x
            self.y = y
            self.width = width
            self.height = height
        }
    }

    pub struct Rect {
        pub let name: String
        pub let type: String
        pub let value: String
        pub let attributes: RectAttributes

        init(
            name: String,
            type: String,
            value: String,
            attributes: RectAttributes,
        ) {
            self.name = name
            self.type = type
            self.value = value
            self.attributes = attributes
        }
    }

    pub struct GElemAttributes {
        pub var fill: String

        init(
            fill: String,
        ) {
            self.fill = fill
        }
    }

    pub struct GElem {
        pub let name: String
        pub let type: String
        pub let value: String
        pub let attributes: GElemAttributes
        pub let children: [Rect]

        init(
            name: String,
            type: String,
            value: String,
            attributes: GElemAttributes,
            children: [Rect]
        ) {
            self.name = name
            self.type = type
            self.value = value
            self.attributes = attributes
            self.children = children
        }
    }

    pub struct SvgAttributes{
        pub let width: String
        pub let height: String
        pub let baseProfile: String
        pub let version: String
        pub let viewBox: String
        pub let xmlns: String
        pub let style: String

        init(width: String, height: String, baseProfile: String, version: String, viewBox: String, xmlns: String, style: String) {
            self.width=width
            self.height=height
            self.baseProfile=baseProfile
            self.version=version
            self.viewBox=viewBox
            self.xmlns=xmlns
            self.style=style
        }
    }

	pub struct Svg{
		pub let name: String
    pub let attributes: SvgAttributes
    pub let children: [GElem]

		init(name: String, attributes: SvgAttributes, children: [GElem]) {
			self.name=name
            self.attributes=attributes
            self.children=children
		}
	}
```

## Components of iaNFT:
The matter of getting local SVG files serialized, constructed and saved on-chain is what iaNFT as a project will be offering to the Flow community. Here are some of the components of iaNFT in development:
- artwork prep script (probably will be made into a CLI)
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

## Gotchas:
- initially iaNFT will focus only on pixel grid SVG artwork. Once that is used by the community and supported by iaNFT, we can focus more on other features of SVG like non-rectangular shapes, animation, etc., and beyond that other structured file formats like some 3D ones.


## An Example Pattern:
1. developer designs and implements an NFT resource that utilizes iaNFT data structures, composite/re-composite methods and resolver. Developer crafts interfaces and leverages cadence methods to design a permission scheme for the on-chain artwork layers (immutable layer, semi-mutable, fully mutable, who can and cannot modify, when it can be modified, how many times, etc.)
2. developer and/or artist create SVG artwork
3. script serialzes the artwork to cadence data structures and adds them to the blockchain
4. app user acquires the iaNFT 
  - uses it in the core experience
  - takes it to another Flow project that implements iaNFT and caters to the original use case and design intentions of the NFT to participate in the modification of the on-chain artwork
5. app user sells the iaNFT as they would with any other NFT and new owner can do what they wish

## How to get involved:
We're developing an NFT project, known as The FLOASIS, that should launch on mainnet by 2022 Q3. The components of iaNFT will immerge as we build that project and prove out the technology above and beyond what the POC app does. 

![iaNFT pattern](https://github.com/hichana/iaNFT-Flow/blob/main/images/project-execution.png)

Meanwhile, we're gathering requirements from some successful Flow projects and community members to integrate into iaNFT. If you'd like to discuss more about either of these please come into The FLOASIS (iaNFT) discord and say hi: https://discord.gg/c6cw43xb
