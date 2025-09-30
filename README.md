MedAuth** is a **blockchain-powered medicine authentication system** designed to combat counterfeit drugs through transparent, tamper-proof verification. The system creates an immutable record of medicine lifecycle from manufacture to consumption.

---

## 🛠️ **Technology Stack**

### **Frontend Stack**
- **⚡ Vite** (v5.4.19) - Build tool and dev server
- **⚛️ React** (v18.3.1) - UI library with hooks
- **🔷 TypeScript** (v5.8.3) - Type safety and better DX
- **🎨 Tailwind CSS** (v3.4.17) - Utility-first styling
- **🎯 shadcn/ui** - Modern component library with Radix UI primitives
- **📱 React Router** (v6.30.1) - Client-side routing

### **Blockchain Stack**
- **⛓️ Ethereum/EVM Compatible** - Smart contract platform
- **🔨 Hardhat** (v2.26.3) - Development environment and testing framework
- **🛡️ OpenZeppelin** (v5.4.0) - Security-audited smart contract library
- **🔗 Ethers.js** (v6.15.0) - Ethereum JavaScript library
- **🦊 MetaMask SDK** (v0.28.4) - Wallet integration
- **⚙️ Solidity** (v0.8.27) - Smart contract programming language

### **Backend & Storage**
- **🗄️ Supabase** (v2.57.4) - PostgreSQL database and auth
- **🌐 IPFS** (v60.0.1) - Decentralized file storage for metadata
- **🔄 TanStack Query** (v5.83.0) - Data fetching and caching

### **Additional Tools**
- **📱 QR Code Libraries** - Generation and scanning
- **📊 Recharts** - Data visualization
- **🎭 Lucide React** - Icon library
- **📅 Date-fns** - Date manipulation

---

## 🌐 **Network Configuration**

### **Current Deployment**
- **🏠 Local Development**: Hardhat Network (Chain ID: 31337)
- **🌍 Contract Address**: `0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512`
- **🔗 RPC URL**: `http://127.0.0.1:8545`

### **Production Network Options**
- **🟣 Polygon Mumbai Testnet** (Chain ID: 80001)
- **🟣 Polygon Mainnet** (Chain ID: 137)
- **⚡ Optimized for low gas fees and fast transactions**

---

## 🔗 **Blockchain Architecture**

### **Smart Contract: MedicineAuth.sol**

**Core Features:**
- **🏭 Role-based Access Control** (OpenZeppelin AccessControl)
- **🔒 Reentrancy Protection** (ReentrancyGuard)
- **🎯 Three Primary Roles**: Admin, Manufacturer, Seller

**Data Structure:**
```solidity
struct Medicine {
    uint256 tokenId;           // Unique identifier
    string metadataHash;       // IPFS hash containing details
    address manufacturer;      // Manufacturer's wallet address
    uint256 registrationTime;  // Blockchain timestamp
    bool isActivated;          // Seller activation status
    address activatedBy;       // Seller who activated
    uint256 activationTime;    // Activation timestamp
}
```

**Key Functions:**
- [registerMedicine()](file:///Users/hmt/Downloads/karanprojext/src/lib/blockchain.ts#L82-L110) - Manufacturer registers new medicine
- [activateMedicine()](file:///Users/hmt/Downloads/karanprojext/src/lib/blockchain.ts#L112-L123) - Seller activates before selling
- [verifyMedicine()](file:///Users/hmt/Downloads/karanprojext/src/lib/blockchain.ts#L125-L144) - Anyone can verify authenticity
- `addManufacturer()` / `addSeller()` - Admin role management

**Events Emitted:**
- `MedicineRegistered` - New medicine added
- `MedicineActivated` - Medicine activated by seller
- `ManufacturerAdded` / `SellerAdded` - Role assignments

---

## 📊 **Data Flow Architecture**

### **1. Registration Flow (Manufacturer)**
```
Manufacturer → Frontend → MetaMask → Smart Contract → Blockchain
                    ↓
              IPFS Storage ← Medicine Metadata
                    ↓
              QR Code Generation ← Token ID
```

### **2. Activation Flow (Seller)**
```
Seller → Scan QR → Extract Token ID → Smart Contract → Update Status
              ↓
        Verify Manufacturer → Activate Medicine → Emit Event
```

### **3. Verification Flow (Customer)**
```
Customer → Scan QR → Extract Token ID → Smart Contract Query
              ↓
        Fetch IPFS Metadata → Display Results → Authenticity Status
```

### **4. Complete Supply Chain**
```
Raw Materials → Manufacturing → Quality Control → Registration
        ↓
QR Code Generation → Distribution → Seller Activation → Customer Verification
        ↓
Blockchain Record → IPFS Metadata → Immutable Proof → Anti-Counterfeiting
```

---

## 🎭 **User Roles & Permissions**

### **👨‍💼 Admin (Contract Deployer)**
- Deploy smart contract
- Add/remove manufacturers
- Add/remove sellers
- Manage system permissions

### **🏭 Manufacturer**
- Register new medicines on blockchain
- Upload metadata to IPFS
- Generate QR codes
- Access manufacturing dashboard

### **🏪 Seller**
- Activate medicines before selling
- Scan QR codes for verification
- Update medicine status
- Access seller dashboard

### **👤 Customer**
- Scan QR codes to verify authenticity
- View medicine details
- Check activation status
- Access verification interface

---

## 🗂️ **Project Structure**

```
karanprojext/
├── contracts/
│   └── MedicineAuth.sol          # Smart contract
├── scripts/
│   ├── deploy.cjs                # Deployment script
│   └── assignRoles.cjs           # Role assignment script
├── src/
│   ├── components/
│   │   ├── ui/                   # shadcn/ui components
│   │   ├── WalletConnection.tsx  # MetaMask integration
│   │   ├── ManufacturerDashboard.tsx
│   │   ├── SellerApp.tsx
│   │   ├── CustomerVerification.tsx
│   │   └── RoleSelector.tsx
│   ├── lib/
│   │   ├── blockchain.ts         # Ethers.js integration
│   │   ├── contractConfig.ts     # Contract ABI & address
│   │   ├── ipfsService.ts        # IPFS file handling
│   │   └── utils.ts              # Utility functions
│   ├── types/
│   │   └── medicine.ts           # TypeScript interfaces
│   └── pages/
│       ├── Index.tsx             # Main application
│       └── NotFound.tsx          # 404 page
├── hardhat.config.cjs            # Hardhat configuration
├── package.json                  # Dependencies & scripts
└── vite.config.ts               # Vite configuration
```

---

## 🔄 **Development Workflow**

### **Local Development Setup**
1. **Start Hardhat Node**: `npx hardhat node`
2. **Deploy Contract**: `npx hardhat run scripts/deploy.cjs --network localhost`
3. **Start Frontend**: `npm run dev`
4. **Connect MetaMask**: Add local network (Chain ID: 31337)

### **Smart Contract Development**
1. **Compile**: `npx hardhat compile`
2. **Test**: `npx hardhat test`
3. **Deploy**: `npx hardhat run scripts/deploy.cjs`
4. **Verify**: Check contract on block explorer

### **Frontend Development**
1. **TypeScript Compilation**: Real-time with Vite
2. **Hot Module Replacement**: Instant updates
3. **Component Development**: shadcn/ui + Tailwind
4. **State Management**: React hooks + TanStack Query

---

## 📡 **API & Integration Points**

### **Blockchain Interactions**
- **ethers.js Provider**: Browser wallet connection
- **Contract Calls**: Read/write blockchain operations
- **Event Listening**: Real-time blockchain updates
- **Transaction Management**: Gas estimation & confirmation

### **IPFS Integration**
- **Metadata Upload**: JSON documents with medicine details
- **Content Addressing**: Immutable file references
- **Decentralized Storage**: No single point of failure
- **Gateway Access**: HTTP access to IPFS content

### **Supabase Integration**
- **User Authentication**: (Optional) User management
- **Off-chain Data**: Additional application data
- **Real-time Updates**: Database change subscriptions

---

## 🛡️ **Security Features**

### **Smart Contract Security**
- **OpenZeppelin Libraries**: Battle-tested security patterns
- **Access Control**: Role-based permissions
- **Reentrancy Guards**: Prevent attack vectors
- **Input Validation**: Robust parameter checking

### **Frontend Security**
- **TypeScript**: Compile-time error prevention
- **Wallet Integration**: Secure MetaMask connection
- **Environment Variables**: Sensitive data protection
- **CSP Headers**: Content Security Policy (Vite)

### **Data Integrity**
- **Immutable Records**: Blockchain guarantees
- **IPFS Hashing**: Content verification
- **Digital Signatures**: Cryptographic proof
- **Timestamping**: Blockchain-based timestamps

---

## 🚀 **Deployment Strategy**

### **Current Status: Local Development**
- Hardhat local network running
- Contract deployed and verified
- Frontend connected and functional
- All roles configured and tested

### **Production Deployment Options**

**Option 1: Polygon Mumbai (Testnet)**
- Free testing environment
- Real blockchain conditions
- Public accessibility
- MetaMask compatible

**Option 2: Polygon Mainnet (Production)**
- Low transaction costs
- High throughput
- Production-ready
- Real economic value

**Option 3: Ethereum Mainnet**
- Maximum security
- Highest decentralization
- Higher gas costs
- Premium positioning

---

## 📈 **Scalability & Performance**

### **Blockchain Efficiency**
- **Gas Optimization**: Efficient Solidity code
- **Batch Operations**: Multiple medicines per transaction
- **Event Indexing**: Fast data retrieval
- **Layer 2 Solutions**: Polygon for scalability

### **Frontend Performance**
- **Code Splitting**: Lazy loading components
- **Caching**: TanStack Query optimization
- **Bundle Optimization**: Vite tree-shaking
- **Progressive Loading**: Better UX

---

## 🎯 **Business Logic Flow**

### **Medicine Lifecycle**
1. **Manufacturing** → Medicine produced with batch details
2. **Registration** → Manufacturer registers on blockchain + IPFS
3. **QR Generation** → Unique QR code with Token ID created
4. **Distribution** → Medicine moves through supply chain
5. **Activation** → Authorized seller activates before sale
6. **Purchase** → Customer receives activated medicine
7. **Verification** → Customer scans QR to verify authenticity

### **Anti-Counterfeiting Mechanism**
- **Unique Token IDs**: Each medicine has blockchain-verified ID
- **Role-based Registration**: Only authorized manufacturers
- **Activation Requirement**: Sellers must activate legitimately
- **Public Verification**: Anyone can check authenticity
- **Immutable History**: Complete audit trail on blockchain

---

This **MedAuth** system represents a comprehensive **Web3 solution** combining **modern frontend development** with **blockchain security** to solve **real-world pharmaceutical counterfeiting problems**. The architecture ensures **scalability**, **security**, and **user-friendly interaction** while maintaining **decentralized principles**.
