# Metadata and Media CDN Plan

## Current Status

The files in `assets/nft-animations/` are local development assets. They are not production CDN assets and should not be treated as final artwork.

BSC testnet closed-loop testing used neutral sandbox collection names and neutral `ipfs://sandbox-*` URI values. It did not depend on final AiTraceRoot artwork, final metadata, or a production CDN.

The backend now provides testnet-compatible metadata and SVG media routes:

```text
https://api.aitraceroot.pro/metadata/pioneer/1
https://api.aitraceroot.pro/metadata/advocate/1
https://api.aitraceroot.pro/metadata/builder/1
https://api.aitraceroot.pro/metadata-assets/pioneer-badge.svg
https://api.aitraceroot.pro/metadata-assets/advocate-badge.svg
https://api.aitraceroot.pro/metadata-assets/builder-badge.svg
```

## Production Metadata Rule

The Pioneer Badge is an uncapped ERC721 free-mint collection. A finite static metadata folder is not enough unless the final max supply is fixed before launch.

The current contract returns:

```text
tokenURI = baseURI + tokenId
```

For an uncapped collection, `baseURI` should point to a service or CDN route that can serve metadata for any minted token ID, for example:

```text
https://metadata.example.com/pioneer/
https://metadata.example.com/pioneer/1
https://metadata.example.com/pioneer/2
```

For BSC testnet, use:

```text
Pioneer baseURI: https://api.aitraceroot.pro/metadata/pioneer/
Advocate baseURI: https://api.aitraceroot.pro/metadata/advocate/
Builder baseURI: https://api.aitraceroot.pro/metadata/builder/
```

The route may return the same artwork for every token and include token-specific fields such as:

```json
{
  "name": "Pioneer Badge #1",
  "description": "A free-mint badge for early AiTraceRoot visitors.",
  "image": "https://cdn.example.com/nft/pioneer/image.png",
  "animation_url": "https://cdn.example.com/nft/pioneer/animation.svg",
  "attributes": [
    { "trait_type": "Badge Type", "value": "Pioneer" },
    { "trait_type": "Distribution", "value": "Free Mint" },
    { "trait_type": "Wallet Limit", "value": "One Per Wallet" }
  ]
}
```

## Recommended Production Setup

Use immutable media storage for final artwork and a stable metadata route for token-specific JSON.

- Media: IPFS, Arweave, Cloudflare R2, or S3 behind CDN.
- Metadata: Cloudflare Worker, backend route, or static route backed by object storage if max supply is fixed.
- Contract `baseURI`: set to the metadata route prefix, not the image URL.
- Frontend NFT preview URL: set `VITE_PIONEER_NFT_MEDIA_URL` to the final CDN or gateway media URL.

## Release Checklist

- Final artwork approved for Pioneer, Advocate, and Builder badges.
- Final image and animation URLs uploaded and pinned.
- Metadata route tested for multiple token IDs.
- Contract `baseURI` set to the metadata route prefix.
- Frontend `VITE_PIONEER_NFT_MEDIA_URL` set to the final media URL.
- Marketplace preview tested on BSC testnet before mainnet launch.
