# FaceRights Vault — Base USDC + RARE Proof Ready

MVP de hackathon para registrar identidad visual/vocal, crear una bóveda local de evidencia, cobrar una licencia con USDC real en Base y preparar un NFT público de prueba para RARE Protocol / rare.xyz cuando llegue Sepolia ETH.

## Qué hace esta versión

- La app corre en `localhost`, pero la wallet firma operaciones reales desde MetaMask.
- El flujo de licencia usa **Base mainnet** y puede hacer un micropago real de **0.0001 USDC**.
- El valor comercial mostrado para pitch sigue siendo **500 USD**.
- El Visual Passport queda como demo local hasta que despliegues contratos propios.
- Agrega una pantalla **RARE Proof NFT** para preparar metadata y comando `rare mint`.
- No sube rostro, voz, CURP ni biometría a blockchain.

## Redes y contratos usados

### Base mainnet

- Red: Base mainnet
- Chain ID: `8453`
- RPC: `https://mainnet.base.org`
- Explorer: `https://basescan.org`
- USDC en Base: `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`

Base documenta su mainnet con chain ID `8453`, símbolo ETH y RPC `https://mainnet.base.org`. Circle lista la dirección nativa de USDC en Base en su documentación de direcciones de contrato.

### RARE Protocol / Sepolia

RARE Protocol recomienda usar Sepolia para pruebas y su documentación indica que los comandos de deploy y mint trabajan sobre mainnet/Sepolia. La app no pide private key: el mint de Rare se hace desde terminal con `rare-cli`.

## Instalación

En Windows PowerShell:

```powershell
npm.cmd install
Copy-Item .env.example .env
npm.cmd run dev
```

Abre:

```txt
http://localhost:3000
```

## Flujo recomendado de demo

### 1. Pago real de licencia en Base

1. Abre MetaMask en red **Base**.
2. Conecta wallet desde la app.
3. Entra a **Crear Visual Passport**.
4. Sube imagen/voz de prueba.
5. Genera la **Bóveda Local**.
6. Crea el **Visual Passport** en modo demo.
7. Entra a **Emitir licencia NFT**.
8. Pega una segunda wallet tuya como receptora.
9. Confirma el pago de **0.0001 USDC**.
10. En **Registro verificable** revisa el link a Basescan.

### 2. Preparar el NFT visible en rare.xyz

Cuando llegue tu Sepolia ETH:

```powershell
npm install -g @rareprotocol/rare-cli
rare --help
```

Configura una wallet de prueba. No uses tu wallet principal si no estás cómodo usando private key en CLI:

```powershell
rare configure --chain sepolia --private-key 0xTU_PRIVATE_KEY --rpc-url https://ethereum-sepolia-rpc.publicnode.com
rare wallet address
```

Despliega una colección en Rare Sepolia:

```powershell
rare deploy erc721 "FaceRights Vault Proofs" "FRVP" --chain sepolia
```

Copia el contrato que te regrese Rare y pégalo en la pantalla **RARE Proof NFT**.

Después copia desde la app el comando `rare mint`. Se verá parecido a:

```powershell
rare mint \
  --chain sepolia \
  --contract 0xCONTRATO_RARE_COLLECTION \
  --name "FaceRights Visual License Proof #001" \
  --description "Public proof for a FaceRights visual/vocal license..." \
  --image ./public/facerights-rare-proof.png \
  --attribute "Product=FaceRights Vault" \
  --attribute "Commercial Value=$500" \
  --attribute "Payment Network=Base" \
  --attribute "Payment Token=USDC" \
  --to 0xTU_WALLET
```

Verifica el resultado:

```powershell
rare search tokens --chain sepolia --mine
rare status --chain sepolia --contract 0xCONTRATO_RARE_COLLECTION --token-id 1
```

Cuando tengas el token ID y el link de Rare, pégalos en la pantalla **RARE Proof NFT**. En **Registro verificable** aparecerá como parte del flujo.

## Mensaje de pitch

> La app corre local para la demo, pero el pago de licencia usa una transacción real de USDC en Base. El valor comercial se modela en 500 USD y para seguridad se ejecuta un micropago real de 0.0001 USDC. El NFT de RARE funciona como prueba pública de licencia: no contiene rostro ni voz, solo metadata de consentimiento y referencia al pago.

## Seguridad

- No pegues seed phrase en ninguna parte.
- Para `rare-cli`, usa una wallet de prueba si vas a configurar private key.
- No subas `.env` a GitHub.
- El NFT público no debe contener rostro, voz, CURP, documentos ni biometría.

## Fuentes

- Base docs — Connecting to Base: `https://docs.base.org/base-chain/quickstart/connecting-to-base`
- Circle USDC contract addresses: `https://developers.circle.com/stablecoins/usdc-contract-addresses`
- RARE Protocol docs: `https://rare.xyz/docs`
