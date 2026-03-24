# Subscription Manager

A decentralized subscription system that tracks user access based on time using a Stellar Soroban smart contract. The project includes a sleek, premium frontend web application with demo mode support.

## Project Structure

- `contracts/subscription/` - The Rust smart contract code (`src/lib.rs`) and tests (`src/test.rs`).
- `frontend/` - A sleek, modern HTML/CSS/JS web application to interact with the subscription system.

## Smart Contract Setup

Make sure you have Rust and the `wasm32-unknown-unknown` target installed, as well as the Soroban CLI.

```bash
rustup target add wasm32-unknown-unknown
cargo install --locked soroban-cli
```

## CLI Commands

### 1. Build Contract
Compiles the smart contract into a `.wasm` file.

```bash
cargo build --target wasm32-unknown-unknown --release
```

### 2. Deploy Contract
Deploys the compiled `.wasm` file to the Stellar Testnet. You must create an identity (e.g., `alice`) first if you haven't.

```bash
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/subscription.wasm \
  --source alice \
  --network testnet
```

*Note: The deployment command will output your Contract ID (e.g., `C...`). Substitute `<CONTRACT_ID>` below.*

### 3. Invoke `subscribe`
Extends or initiates a subscription for a given duration (e.g., 3600 seconds = 1 hour).

```bash
soroban contract invoke \
  --id <CONTRACT_ID> \
  --source alice \
  --network testnet \
  -- \
  subscribe \
  --user alice \
  --duration 3600
```

### 4. Invoke `is_active`
Checks whether the subscription is currently active (true or false).

```bash
soroban contract invoke \
  --id <CONTRACT_ID> \
  --source bob \
  --network testnet \
  -- \
  is_active \
  --user alice
```

### 5. Invoke `get_expiry`
Fetches the precise UNIX timestamp of when the subscription expires.

```bash
soroban contract invoke \
  --id <CONTRACT_ID> \
  --source bob \
  --network testnet \
  -- \
  get_expiry \
  --user alice
```

## Running the Frontend

To view the stunning frontend in demo mode simply open:
```bash
./frontend/index.html
```

Or serve it locally using Python to prevent CORS issues if modifying further:
```bash
python -m http.server -d frontend
```
