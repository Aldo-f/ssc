# Spot the Shopping Cart (SSC)

Decentralized proof-of-location protocol for spotting shopping carts on interactive maps.

## Overzicht

Bezoekers kunnen winkels en winkelkarren markeren op een kaart met foto-upload via blockchain-verificatie. Andere gebruikers kunnen deze locaties bekijken, beoordelen en claim zien als geverifieerd.

## Features

- **Kaartinteractie**: OpenStreetMap integratie met marker functionaliteit
- **Foto-upload**: Bezoeker upload foto van gevonden winkelkar
- **Blockchain claim**: Smart contract registreert locatie + timestamp + foto-hash
- **Validatie systeem**: Tokens worden uitgegeven voor geverifieerde spotting's
- **Rating/review**: Gemeenschap kan spotting's verifiëren of afvisen
- **Leaderboard**: Top spotters en meest gespotte winkels

## Stack

| Layer | Technologie |
|-------|-------------|
| Frontend | React + Leaflet + Tailwind CSS |
| Backend | Node.js/Express API |
| Blockchain | Ethereum-compatible smart contract (ERC-721 NFT voor elke spotting) |
| Database | PostgreSQL (locatie metadata, ratings) |
| Storage | IPFS voor foto's |
| Auth | WalletConnect + anonieme spotting optie |

## Architectuur

```
ssc/
├── frontend/          # React Leaflet kaartapplicatie
├── contracts/         # Solidity smart contracts
├── api/              # Express.js backend
├── prisma/           # Database schema + migraties
└── docker-compose.yml  # Ontwikkelomgeving
```

## Smart Contract Flow

1. Gebruiker upload foto → IPFS hash
2. Frontend callt `spotCart(uint256 latScaled, uint256 lonScaled, string ipfsHash)`
3. Contract mint NFT met metadata (locatie, tijdstip, spotter address)
4. API sync via events → PostgreSQL update

## API Endpoints

| Endpoint | Methode | Beschrijving |
|----------|---------|--------------|
| `/api/spots` | POST | Nieuwe spotting registreren |
| `/api/spots` | GET | Lijst alle spots (filterbaar) |
| `/api/spots/:id` | GET | Details één spot |
| `/api/spots/:id/verify` | POST | Spot verifiëren |
| `/api/maps/nearby` | GET | Spots binnen radius |

## Development Setup

```bash
# Clone & install
git clone https://github.com/Aldo-f/ssc
cd ssc
npm install

# Start met Docker
docker-compose up -d

# Contract deploy (lokaal)
npx hardhat node
npx hardhat run scripts/deploy.js --network localhost

# Frontend dev server
cd frontend && npm run dev
```

## Environment Variables

```
DATABASE_URL=postgresql://ssc:***@db:5432/ssc
RPC_URL=https://polygon-rpc.com
PRIVATE_KEY=0x... (deploy key)
IPFS_PROJECT_ID=...
IPFS_PROJECT_SECRET=*** License

MIT