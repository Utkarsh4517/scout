# **Scout**: Plug & Play Solana Wallet Mobile Notification Service For Your DEFI App.

![Scout Banner](https://placehold.co/1200x400/1e1e2e/7f5af0?text=Scout&font=raleway)



**Scout** is a lightweight, open-source, and self-hostable service that provides Mobile developers with a simple REST API to enable real-time push notifications for **any SPL token activity** on any Solana wallet in their **Defi app**.

Super Simple APIs to add a robust notification system for any of your DEFI app.
- /api/register
- /update-fcm-token
- /api/untrack
- /api/actvities/:wallet_address

Just register your user's wallet address once, and your user will receive push notification for any wallet activty, doesnot matter the origin, for example:
- You received 540 $XYZ 
- You sent 30 $BONK
- You received $500 USDC

## ***Spend time creating and engineering your defi app, leave notifications on scout.***

## Why Scout?

-   **Real-Time:** Uses Solana's WebSockets for instant, event-driven notifications.
-   **Universal SPL Token Support:** Detects balance changes for ANY token, including new airdrops, meme coins, or custom tokens, not just major ones.
-   **Simple for Mobile Developers:** Abstracts away all push notification complexity for a wallet behind a dead-simple REST API. If you can call an endpoint, you can have Solana notifications.
-   **Self-Hostable & Control:** Run it on your own infrastructure. You control the data, the keys, and the performance.


## Architecture Overview

Scout is composed of three core components working in harmony:

1.  **The API Server:** A simple Express.js server that provides endpoints for developers to register and manage wallets.
2.  **The Database:** A high-performance SQLite database to store wallet-to-device mappings and log all transaction activity.
3.  **The WebSocket Manager:** The engine of Scout. It maintains a persistent connection to the Solana network, manages subscriptions, processes transactions, and triggers notifications.


## Getting Started

### Prerequisites

-   Node.js (v18+)
-   A Solana RPC Provider (e.g., Helius, QuickNode, Alchemy). The public RPC is fine for testing but **not for production**.
-   A Firebase Project with a Service Account key.

### Installation & Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Utkarsh4517/scout.git
    cd scout
    ```

2.  **Install dependencies:**
    ```bash
    npm install
    ```

3.  **Configure your environment:**
    -   Copy `.env.example` to a new file named `.env`.
    -   Fill in the required values: `SOLANA_WSS_ENDPOINT`, `SOLANA_RPC_ENDPOINT`, and `PORT`.
    -   Place your downloaded Firebase service account key in the project root and name it `firebase-service-account.json`. Update `FCM_SERVICE_ACCOUNT_PATH` in `.env` if you name it differently.

### Running Scout

-   **For development (with auto-reloading):**
    ```bash
    npm run dev
    ```

-   **For production:**
    ```bash
    npm run build
    npm run start
    ```

Your Scout service is now live and listening for API requests!

## API Documentation

### Register a Wallet

Subscribe a wallet to receive notifications.

-   **Endpoint:** `POST /api/register`
-   **Body:**
    ```json
    {
      "wallet_address": "YourSolanaWalletAddressHere",
      "fcm_token": "TheDeviceFCMTokenHere"
    }
    ```
-   **Example:**
    ```bash
    curl -X POST http://localhost:3000/api/register \
         -H "Content-Type: application/json" \
         -d '{"wallet_address": "9...","fcm_token": "c..."}'
    ```

### Get Wallet Activities

Retrieve the most recent logged activities for a wallet.

-   **Endpoint:** `GET /api/activities/:wallet_address`
-   **Example:**
    ```bash
    curl http://localhost:3000/api/activities/YourSolanaWalletAddressHere
    ```

### Untrack a Wallet

-   **Endpoint:** `DELETE /api/untrack`
-   **Body:**
    ```json
    { "wallet_address": "YourSolanaWalletAddressHere" }
    ```

## For App Developers: Integrating Scout

Integrating Scout into your mobile app is incredibly simple.

1.  **Get an FCM Token:** Use the Firebase SDK in your app to get a device token.
2.  **Call the API:** When a user wants notifications, call the `/api/register` endpoint with their wallet address and the FCM token.
3.  **Handle the Payload:** Configure your app to receive push notifications. The payload from Scout will look like this:
    ```json
    {
      "notification": {
        "title": "Scout: USDC Received!",
        "body": "Amount: 100.00 USDC"
      },
      "data": {
        "signature": "3...s",
        "mint": "EPj...t1v",
        "symbol": "USDC"
      }
    }
    ```


## Contributing

Contributions are welcome! Please feel free to submit a Pull Request or open an issue for bugs, features, or improvements.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.