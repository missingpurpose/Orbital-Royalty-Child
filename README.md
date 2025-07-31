# 🎭 **ALKANE ROYALTY NFT CHILD CONTRACT**

**Individual NFT Instance & Royalty Enforcer**

[![Production Ready](https://img.shields.io/badge/Status-Production%20Ready-green)](https://github.com/missingpurpose/Orbital-Royalty-Child)
[![WASM](https://img.shields.io/badge/WASM-Optimized-blue)](target/wasm32-unknown-unknown/release/)
[![Royalties](https://img.shields.io/badge/Royalties-100%25%20Unavoidable-red)](#revolutionary-royalty-enforcement)
[![Child Contract](https://img.shields.io/badge/Type-NFT%20Instance-purple)](src/lib.rs)

---

## 🔒 **Revolutionary Royalty Enforcement**

This is the **child contract** of the world's first truly unavoidable Bitcoin NFT royalty system. Each NFT is an instance of this contract, making royalties **100% unavoidable**.

### **🎯 Core Innovation**
- **🔒 TransferWithRoyalty (Opcode 88)**: The ONLY way to transfer NFTs
- **💰 5% Guaranteed**: Every secondary sale automatically pays 5% royalty
- **🛡️ Safe Failures**: Insufficient royalty = transaction fails safely (no asset loss)
- **🚫 No Bypass Methods**: PSBTs, direct transfers, marketplace tricks all blocked

---

## 🎨 **Algorithmic Art Integration**

### **Dynamic Metadata System**
Each NFT generates unique art by calling back to the Collection Contract:

```rust
fn get_data(&self) -> Result<CallResponse> {
    let collection_id = self.collection_ref();
    let cellpack = Cellpack {
        target: collection_id, 
        inputs: vec![1000, self.index()], // Collection.GetData + NFT index
    };
    // Returns algorithmic SVG based on NFT's unique index
}
```

### **Art Features**
- **🎨 6 Art Styles**: Geometric Fractal, Flow Field, Circle Packing, Mandala, Wave Interference, Crystalline
- **🌈 12 Color Palettes**: Sunset, Ocean, Cosmic, Neon, Aurora, Forest, Volcanic, Arctic, Desert, Tropical, Cyberpunk, Ethereal
- **📊 Built-in Rarity**: Automatic scoring system (90-180 points per NFT)
- **🔍 Mathematical Uniqueness**: Every NFT provably unique through index-based generation

### **Collection Contract Integration**
- **🏭 Parent Contract**: Each child references its collection contract
- **🎨 Art Delegation**: Child calls collection for dynamic SVG generation
- **📊 Metadata Delegation**: Child calls collection for algorithmic attributes
- **💰 Royalty Forwarding**: Child sends royalties to collection contract

---

## 📊 **Contract Opcodes**

| Opcode | Function | Parameters | Returns | Purpose |
|--------|----------|------------|---------|---------|
| **0** | **Initialize** | `index: u128` | NFT token (1 unit) | Create NFT instance with unique index |
| **🔒 88** | **TransferWithRoyalty** | `sale_price: u128` | Transfer to buyer | **ONLY transfer method - enforces 5% royalty** |
| **89** | **GetRoyaltyInfo** | `none` | `Vec<u8>` | Returns [500, collection_block, collection_tx] |
| **99** | **GetName** | `none` | `String` | Returns "Alkane RoyaltyNFT #[index]" |
| **100** | **GetSymbol** | `none` | `String` | Returns "RoyaltyNFT" |
| **101** | **GetTotalSupply** | `none` | `u128` | Returns 1 (unique NFT) |
| **998** | **GetCollectionIdentifier** | `none` | `String` | Returns collection AlkaneId as string |
| **999** | **GetCollectionAlkaneId** | `none` | `Vec<u8>` | Returns collection AlkaneId bytes |
| **🎨 1000** | **GetData** | `none` | `Vec<u8>` | **Delegates to collection** - Gets algorithmic SVG |
| **1001** | **GetContentType** | `none` | `String` | Returns "image/svg+xml" |
| **🎨 1002** | **GetAttributes** | `none` | `String` | **Delegates to collection** - Gets algorithmic attributes |

---

## 🔒 **The Revolutionary TransferWithRoyalty (Opcode 88)**

```rust
// The ONLY way to transfer NFTs - makes royalties 100% unavoidable
// Parameters: sale_price (u128)
// Required incoming alkanes:
[
  AlkaneTransfer { 
    id: nft_alkane_id,     // The NFT being sold (1 unit)
    value: 1 
  },
  AlkaneTransfer { 
    id: PAYMENT_TOKEN_ID,  // BTC payment for royalty
    value: royalty_amount  // max(sale_price * 5% / 100, 1000)
  }
]
```

### **Step-by-Step Process**
1. **🔍 Ownership Verification**: Seller must own exactly 1 NFT unit
2. **💰 Royalty Calculation**: `max(sale_price * 5% / 100, 1000 sats)`
3. **💳 Payment Verification**: Buyer must send sufficient royalty payment
4. **📤 NFT Transfer**: NFT transferred to buyer
5. **💸 Royalty Forward**: Royalty automatically sent to Collection Contract
6. **✅ Success**: Transaction completes successfully

---

## 🧪 **Usage Examples**

### **NFT Transfer with Royalty**
```bash
# Transfer NFT with 5% royalty (sale price: 100,000 sats)
# Required royalty: max(100,000 * 5% / 100, 1000) = 5,000 sats
oyl provider alkanes --method call \
  --calldata "88:100000" \
  --alkane-id "NFT_ALKANE_ID" \
  --incoming-alkanes "NFT_ALKANE_ID:1,BTC_ID:5000"
```

### **Query NFT Metadata**
```bash
# Get algorithmic SVG art
oyl provider alkanes --method call \
  --calldata "1000" \
  --alkane-id "NFT_ALKANE_ID"

# Get algorithmic attributes  
oyl provider alkanes --method call \
  --calldata "1002" \
  --alkane-id "NFT_ALKANE_ID"
```

### **Get Royalty Information**
```bash
# Returns: [500, collection_block, collection_tx] (5% royalty)
oyl provider alkanes --method call \
  --calldata "89" \
  --alkane-id "NFT_ALKANE_ID"
```

---

## 🔧 **Configuration Constants**

```rust
/// Royalty configuration
const ROYALTY_PERCENTAGE: u128 = 500;         // 5% in basis points (500/10000)
const MIN_ROYALTY_AMOUNT: u128 = 1000;        // Minimum 1000 sats royalty
const PAYMENT_TOKEN_ID: AlkaneId = AlkaneId { block: 0, tx: 0 }; // BTC token for royalties

/// Update PAYMENT_TOKEN_ID for mainnet:
/// Regtest: AlkaneId { block: 0, tx: 0 }
/// Mainnet: AlkaneId { block: 2, tx: 9000 } // frBTC example
```

---

## 🏭 **Build Information**

### **WASM Compilation**
```bash
# Build optimized WASM
cargo build --target wasm32-unknown-unknown --release

# Compress for deployment
gzip -9 -k target/wasm32-unknown-unknown/release/alkane_pandas_child.wasm
```

### **Build Results**
- **📦 Original**: `195KB` WASM file
- **🗜️ Compressed**: `69KB` (.gz) for deployment
- **🎯 Target**: `wasm32-unknown-unknown`
- **⚡ Optimization**: Release mode with maximum compression

---

## 🔗 **System Integration**

### **Collection Contract** (Factory)
- **Repository**: [missingpurpose/Royalty-Orbital-Collection](https://github.com/missingpurpose/Royalty-Orbital-Collection)
- **Role**: NFT factory, revenue manager, algorithmic art generator
- **Creates**: Instances of this child contract for each NFT

### **Deployment Sequence**
1. **🪙 Deploy Tokens**: frBTC and BUSD test tokens first
2. **🎭 Deploy Child**: This contract as template (record template ID)
3. **🏭 Deploy Collection**: Collection contract with template ID
4. **🧪 Test System**: Comprehensive testing of royalty enforcement

---

## 📞 **For Marketplace Developers**

### **🔴 Critical Integration Requirements**
```javascript
// ❌ WRONG: Direct transfers will fail
await transferNFT(nftId, newOwner);

// ✅ CORRECT: Must use royalty transfer
await nftContract.call({
    opcode: 88, // TransferWithRoyalty
    data: salePrice,
    incoming_alkanes: [
        { id: nftId, value: 1 },           // NFT being sold
        { id: btcTokenId, value: royalty }  // 5% royalty payment
    ]
});
```

### **Integration Checklist**
- ✅ **Use Opcode 88**: Only TransferWithRoyalty works
- ✅ **Calculate Royalty**: `max(sale_price * 5% / 100, 1000)` sats
- ✅ **Include Payment**: BTC token with sufficient royalty amount
- ✅ **Handle Errors**: Graceful failure for insufficient payments
- ✅ **Test Thoroughly**: Verify no bypass methods work

---

## 🎊 **Revolutionary Achievement**

This child contract makes history as the **first NFT contract where royalties are truly unavoidable**:

✅ **100% Enforcement**: No marketplace can bypass royalties  
✅ **Creator Protection**: Guaranteed 5% revenue on every sale  
✅ **Buyer Safety**: Failed transactions protect buyer assets  
✅ **Technical Innovation**: Breakthrough in blockchain royalty systems  

---

## 📄 **License**

This project is licensed under the [LICENSE](LICENSE) file in this repository.

---

**🎭 Part of the Revolutionary Royalty NFT System**

- **🏭 Collection Contract**: [Royalty-Orbital-Collection](https://github.com/missingpurpose/Royalty-Orbital-Collection)
- **🎭 Child Contract**: [Orbital-Royalty-Child](https://github.com/missingpurpose/Orbital-Royalty-Child) *(You are here)*

*Built with ❤️ for creator empowerment on Bitcoin | Contract Status: Production Ready*
