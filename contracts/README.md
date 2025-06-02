# 🗺️ Mappy Smart Contracts

Cairo smart contracts for the Mappy (MapMySTRK) platform on Starknet.

## 📋 Contracts Overview

### 1. BusinessRegistry Contract
**Purpose**: Core contract for managing business listings on-chain

**Features**:
- ✅ Register new businesses with location data
- ✅ Update business information
- ✅ Community verification system
- ✅ Moderator-based governance
- ✅ Category-based filtering
- ✅ Owner-based business tracking

**Key Functions**:
- `register_business()` - Add new business to registry
- `verify_business()` - Moderator verification
- `get_businesses_by_category()` - Filter by business type
- `get_verified_businesses()` - Get all verified listings

### 2. Rewards Contract
**Purpose**: Incentivize community participation with STRK rewards

**Features**:
- ✅ Registration rewards for new businesses
- ✅ Verification rewards for approved businesses
- ✅ Check-in rewards for user engagement
- ✅ Configurable reward amounts
- ✅ Anti-spam protection (one reward per action)

**Reward Structure**:
- **Registration**: 10 STRK (for business owners)
- **Verification**: 5 STRK (when business gets verified)
- **Check-in**: 1 STRK (per visit to businesses)

### 3. Reviews Contract
**Purpose**: Community-driven business reviews and ratings

**Features**:
- ✅ 5-star rating system
- ✅ Text comments with reviews
- ✅ One review per user per business
- ✅ Review flagging for moderation
- ✅ Automatic rating calculations

## 🚀 Deployment Guide

### Prerequisites
\`\`\`bash
# Install Scarb (Cairo package manager)
curl --proto '=https' --tlsv1.2 -sSf https://docs.swmansion.com/scarb/install.sh | sh

# Install Starkli (Starknet CLI)
curl https://get.starkli.sh | sh
starkliup
\`\`\`

### Build Contracts
\`\`\`bash
# Clone and build
git clone <repository-url>
cd mappy_contracts
scarb build
\`\`\`

### Deploy to Starknet

1. **Set up environment**:
\`\`\`bash
export STARKNET_ACCOUNT=~/.starkli-wallets/deployer/account.json
export STARKNET_KEYSTORE=~/.starkli-wallets/deployer/keystore.json
export STARKNET_RPC=https://starknet-mainnet.public.blastapi.io
\`\`\`

2. **Deploy BusinessRegistry**:
\`\`\`bash
starkli deploy target/dev/mappy_contracts_BusinessRegistry.contract_class.json \
    <OWNER_ADDRESS>
\`\`\`

3. **Deploy Rewards Contract**:
\`\`\`bash
starkli deploy target/dev/mappy_contracts_Rewards.contract_class.json \
    <STRK_TOKEN_ADDRESS> \
    <BUSINESS_REGISTRY_ADDRESS> \
    <OWNER_ADDRESS>
\`\`\`

4. **Deploy Reviews Contract**:
\`\`\`bash
starkli deploy target/dev/mappy_contracts_Reviews.contract_class.json \
    <OWNER_ADDRESS>
\`\`\`

## 🔧 Contract Interactions

### Register a Business
\`\`\`bash
starkli invoke <BUSINESS_REGISTRY_ADDRESS> register_business \
    str:"Crypto Cafe" \
    str:"restaurant" \
    u64:30044400 \
    u64:31235700 \
    str:"Best coffee in Cairo" \
    str:"contact@cryptocafe.com"
\`\`\`

### Verify a Business (Moderator only)
\`\`\`bash
starkli invoke <BUSINESS_REGISTRY_ADDRESS> verify_business \
    u256:1
\`\`\`

### Add a Review
\`\`\`bash
starkli invoke <REVIEWS_ADDRESS> add_review \
    u256:1 \
    u8:5 \
    str:"Amazing coffee and great service!"
\`\`\`

### Claim Registration Reward
\`\`\`bash
starkli invoke <REWARDS_ADDRESS> claim_registration_reward \
    u256:1
\`\`\`

## 📊 Contract Architecture

\`\`\`
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ BusinessRegistry│    │     Rewards     │    │     Reviews     │
│                 │    │                 │    │                 │
│ • Businesses    │◄──►│ • STRK Rewards  │    │ • Ratings       │
│ • Verification  │    │ • Check-ins     │    │ • Comments      │
│ • Moderation    │    │ • Anti-spam     │    │ • Flagging      │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────┐
                    │   Frontend      │
                    │   (Next.js)     │
                    │                 │
                    │ • starknet.js   │
                    │ • Wallet Connect│
                    │ • Map Interface │
                    └─────────────────┘
\`\`\`

## 🔐 Security Features

### Access Control
- **Owner**: Can add/remove moderators, transfer ownership
- **Moderators**: Can verify/unverify businesses, moderate reviews
- **Users**: Can register businesses, add reviews, claim rewards

### Anti-Spam Protection
- One review per user per business
- One-time reward claims per action
- Moderator verification required for business approval

### Data Integrity
- Immutable business registration records
- Transparent verification process
- Public review system with flagging

## 🧪 Testing

\`\`\`bash
# Run contract tests
scarb test

# Test specific contract
scarb test business_registry

# Run with verbose output
scarb test -v
\`\`\`

## 📈 Gas Optimization

The contracts are optimized for:
- **Efficient storage**: Using Maps for O(1) lookups
- **Batch operations**: Multiple businesses can be queried efficiently
- **Event indexing**: Key fields are indexed for fast filtering

## 🔮 Future Enhancements

### Phase 2 Features
- [ ] **NFT Badges**: Issue NFTs for verified businesses
- [ ] **Staking Mechanism**: Stake STRK for business verification
- [ ] **Governance Token**: Community voting on platform changes
- [ ] **Oracle Integration**: Real-world data verification

### Phase 3 Features
- [ ] **Cross-chain Bridge**: Support for other networks
- [ ] **DeFi Integration**: Yield farming for staked STRK
- [ ] **Mobile SDK**: Native mobile app integration
- [ ] **Analytics Dashboard**: On-chain business analytics

## 📄 License

MIT License - see [LICENSE](../LICENSE) file for details.

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/new-feature`
3. Write tests for new functionality
4. Ensure all tests pass: `scarb test`
5. Submit pull request

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/your-username/mappy/issues)
- **Cairo Docs**: [Cairo Book](https://book.cairo-lang.org/)
- **Starknet Docs**: [Starknet Documentation](https://docs.starknet.io/)

---

**Built with ❤️ for the Starknet ecosystem**
