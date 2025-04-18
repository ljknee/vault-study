# HashiCorp Vault သင်တန်း အကျဉ်းချုပ်

ဒီ blog post မှာ HashiCorp Vault Basics course ထဲမှာ လေ့လာခဲ့တဲ့ အဓိကအကြောင်းအရာတွေကို အကျဉ်းချုပ်ဖော်ပြပေးသွားမှာပါ။ ဒီသင်တန်းက HashiCorp Certified Vault Associate (002) စာမေးပွဲအတွက် ပြင်ဆင်ဖို့ အထောက်အကူဖြစ်ပြီး Vault ရဲ့ core concepts နဲ့ operational knowledge တွေကို လက်တွေ့လုပ်ဆောင်ကြည့်ရတဲ့ labs တွေနဲ့ ဖုံးအုပ်ထားပါတယ်။ အကြောင်းအရာတွေကို မြန်မာဘာသာနဲ့ သဘာဝကျကျ ရှင်းပြထားပြီး နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## သင်တန်းထဲမှာ ဘာတွေ လေ့လာခဲ့လဲ?

ဒီသင်တန်းမှာ Vault ရဲ့ အခြေခံကျကျ သဘောတရားတွေကနေ လက်တွေ့အသုံးချနိုင်တဲ့ အကြောင်းအရာတွေအထိ လေ့လာခဲ့ပါတယ်။ အောက်မှာ အဓိကအကြောင်းအရာတွေကို အကျဉ်းချုပ်ဖော်ပြပေးလိုက်ပါတယ်:

1. **Vault Basics**:
   - **Installation**: Vault ကို သင့်စနစ်မှာ install လုပ်နည်းကို လေ့လာခဲ့ပါတယ်။ Package managers (ဥပမာ apt, Homebrew) နဲ့ binary downloads ကို အသုံးပြုပြီး setup လုပ်နည်းတွေပါဝင်ပါတယ်။
   - **Basic Commands**: CLI မှာ အသုံးပြုတဲ့ အခြေခံ commands တွေဖြစ်တဲ့ `vault status`, `vault secrets list`, `vault kv put/get` စတဲ့ commands တွေကို လေ့ကျင့်ခဲ့ပါတယ်။

2. **Vault Servers**:
   - **Development Servers**: Testing အတွက် `vault server -dev` command နဲ့ dev servers တွေကို ဖန်တီးပြီး in-memory storage နဲ့ unsealed state မှာ အလုပ်လုပ်ပုံကို သိခဲ့ရပါတယ်။ Dev servers တွေက production မှာ မသုံးသင့်ပါဘူး။
   - **Sealed Servers**: Production အတွက် disk-based storage (raft) ကို အသုံးပြုတဲ့ sealed servers တွေကို configuration file (HCL) နဲ့ setup လုပ်နည်း၊ initialize လုပ်ပြီး unseal keys နဲ့ unseal လုပ်နည်းကို လေ့လာခဲ့ပါတယ်။

3. **Key/Value Pairs နှင့် Secrets Engines**:
   - **Key/Value (KV) Secrets Engine**: `vault kv put/get` commands နဲ့ secrets တွေကို store/retrieve လုပ်နည်းကို လေ့လာခဲ့ပါတယ်။ Version 1 နဲ့ Version 2 ရဲ့ ကွာခြားချက်တွေကိုလည်း သိခဲ့ရပါတယ် (Version 2 က multiple versions ကို သိမ်းဆည်းနိုင်တယ်)။
   - **Built-in Secrets Engines**: Cubbyhole (token-specific storage), Identity (entity/alias management), System (configuration data), နဲ့ KV engines တွေရဲ့ လုပ်ဆောင်ပုံကို လေ့လာခဲ့ပါတယ်။
   - **Add-on Secrets Engines**: AWS, Databases, Consul စတဲ့ external providers တွေနဲ့ ချိတ်ဆက်တဲ့ engines တွေကို စူးစမ်းခဲ့ပါတယ်။

4. **Authentication Methods**:
   - **Userpass**: Username/password နဲ့ authenticate လုပ်နည်းကို UI နဲ့ CLI မှာ လေ့ကျင့်ခဲ့ပါတယ်။
   - **External Providers**: AWS, GitHub, LDAP စတဲ့ external authentication methods တွေရဲ့ လုပ်ဆောင်ပုံကို သိခဲ့ရပါတယ်။
   - **Commands**: `vault auth enable`, `vault auth list`, `vault login -method=userpass` စတဲ့ commands တွေကို အသုံးပြုခဲ့ပါတယ်။

5. **Vault Policies**:
   - Policies တွေက permissions (capabilities) တွေကို သတ်မှတ်ပေးပြီး users နဲ့ tokens တွေရဲ့ access ကို ထိန်းချုပ်ပါတယ်။
   - **Built-in Policies**: `default` (အလွန်ကန့်သတ်ထားတယ်) နဲ့ `root` (full access) ကို မဖျက်နိုင်ပါဘူး။ Default policy ကို modify လုပ်လို့ရပေမယ့် root policy ကို modify လုပ်လို့မရပါဘူး။
   - **Custom Policies**: HCL files (ဥပမာ `emails-policy.hcl`) နဲ့ policies တွေကို ရေးပြီး `vault policy write` command နဲ့ upload လုပ်နည်းကို လေ့လာခဲ့ပါတယ်။

6. **Vault Tokens နှင့် Leases**:
   - **Tokens**: Service tokens (`hvs.` prefix) နဲ့ batch tokens (`hvb.` prefix) တွေရဲ့ ကွာခြားချက်ကို သိခဲ့ရပါတယ်။ Root tokens, orphan tokens, periodic tokens တွေရဲ့ အသုံးပြုပုံကိုလည်း လေ့လာခဲ့ပါတယ်။
   - **Leases**: Tokens နဲ့ secrets (ဥပမာ AWS credentials) တွေနဲ့ ဆက်စပ်တဲ့ leases တွေကို manage လုပ်နည်းကို လေ့ကျင့်ခဲ့ပါတယ်။ `vault lease renew/revoke` commands တွေကို အသုံးပြုခဲ့ပါတယ်။ Token lease ကို revoke လုပ်ရင် အဲ့ဒီ token နဲ့ ဖန်တီးထားတဲ့ secrets အားလုံး ဖျက်သိမ်းခံရပါတယ်။

7. **Vault UI, CLI, နှင့် API**:
   - **UI**: Browser-based interface ကို `http://127.0.0.1:8200` မှာ access လုပ်ပြီး users/secrets management လုပ်နည်းကို လေ့လာခဲ့ပါတယ်။ Built-in CLI console ကိုလည်း UI ထဲမှာ အသုံးပြုခဲ့ပါတယ်။
   - **CLI**: Full functionality ရှိတဲ့ CLI commands တွေကို အသုံးပြုပြီး Vault operations တွေကို လုပ်ဆောင်ခဲ့ပါတယ်။
   - **API**: `curl` commands နဲ့ API endpoints (`/v1/secret/data`, `/v1/auth/userpass`) တွေကို access လုပ်ပြီး secrets/users တွေကို programmatically manage လုပ်နည်းကို လေ့လာခဲ့ပါတယ်။

8. **Vault Architecture**:
   - High-level architecture မှာ **Core**, **TokenStore**, **PolicyStore**, **Path Routing**, **Auth Methods**, **Secrets Engines**, **System Backend**, **Storage Backend**, နဲ့ **Audit Broker/Devices** တွေရဲ့ အလုပ်လုပ်ပုံကို နားလည်ခဲ့ပါတယ်။
   - **Security Barrier** က data encryption/decryption အတွက် ဘယ်လောက်အရေးကြီးတယ်ဆိုတာကို သိခဲ့ရပါတယ်။
   - **Auditing**: Audit devices (file-based logs) ကို enable/disable လုပ်ပြီး Vault ရဲ့ activities တွေကို track လုပ်နည်းကို လေ့ကျင့်ခဲ့ပါတယ်။

9. **Encryption as a Service (EaaS)**:
   - **Transit Secrets Engine** ကို အသုံးပြုပြီး plaintext (ဥပမာ email address) ကို **base64 encode** လုပ်ပြီး **AES-256 encryption** နဲ့ ciphertext အဖြစ် ပြောင်းလဲတာ၊ ပြီးတော့ decrypt/decode လုပ်နည်းကို လေ့လာခဲ့ပါတယ်။
   - Data in transit ကို **TLS** နဲ့ ကာကွယ်ပြီး on-premises deployment ရဲ့ အကျိုးကျေးဇူးတွေကို သိခဲ့ရပါတယ်။

10. **Quiz Questions**:
    - သင်တန်းတစ်လျှောက်မှာ quiz questions တွေနဲ့ practice questions တွေကို ဖြေဆိုခဲ့ပြီး exam ပုံစံတွေကို နားလည်လာခဲ့ပါတယ်။ Bonus quiz questions တွေက secrets engine lifecycle, sealed Vault behavior, token types, token usage limits, နဲ့ sealing permissions တွေကို စစ်ဆေးပေးပါတယ်။

## Exam Alerts အကျဉ်းချုပ်

သင်တန်းထဲမှာ ဖော်ပြခဲ့တဲ့ **Exam Alerts** တွေကို အောက်မှာ အကျဉ်းချုပ်ဖော်ပြပေးလိုက်ပါတယ်။ ဒါတွေက စာမေးပွဲအတွက် အဓိကသိထားရမယ့် အချက်တွေပါ။

- **Vault Installation**: `vault -v` command နဲ့ version ကို စစ်ဆေးပြီး `vault -autocomplete-install` နဲ့ CLI autocomplete feature ကို enable လုပ်နိုင်ပါတယ်။
- **Secret Sprawl**: Vault က secret sprawl (credentials တွေကို plain text အနေနဲ့ ဖရိုဖရဲထားတာ) ကို secrets management, centralization, encryption, granular access control, နဲ့ audit trails တွေနဲ့ လျှော့ချပေးပါတယ်။
- **Dev Server**: Dev server (`vault server -dev`) က **insecure** နဲ့ **volatile** ဖြစ်ပြီး production မှာ မသုံးပါနဲ့။ HTTP နဲ့ run ပြီး automatically unsealed ဖြစ်နေပါတယ်။
- **Sealed Vault**: Typical Vault server က default အနေနဲ့ sealed ဖြစ်ပြီး unseal keys (default ၅ ခု၊ threshold ၃ ခု) နဲ့ Shamir’s Secret Sharing algorithm ကို အသုံးပြုပါတယ်။ Sealed state မှာ API requests တွေက HTTP status code 503 ပြန်ပေးပါတယ်။
- **Principle of Least Privilege**: Vault က users/processes တွေကို လိုအပ်တဲ့ permissions တွေပဲ ပေးဖို့ implicit deny by default ကို အသုံးပြုပါတယ်။
- **Authentication Methods**: `vault auth enable <method>` (ဥပမာ `userpass`) command နဲ့ auth methods တွေကို enable လုပ်ပါတယ်။ Userpass method မှာ user အချက်အလက်ကို `vault read auth/userpass/users/<username>` နဲ့ ကြည့်နိုင်ပါတယ်။ Auth paths က `auth/` နဲ့ စတင်ပါတယ်။
- **Policies**: Built-in policies (`default`, `root`) ကို remove လုပ်လို့မရပါဘူး။ Default policy က modify လုပ်လို့ရပေမယ့် root policy က modify လုပ်လို့မရပါဘူး။ Root token ကို `vault token revoke` နဲ့ ဖျက်နိုင်ပါတယ်။
- **Tokens**: Service tokens (`hvs.`) နဲ့ batch tokens (`hvb.`) တွေကို `vault token create/renew/revoke` commands နဲ့ manage လုပ်ပါတယ်။ Root token ကို revoke လုပ်ရင် Vault access လုံးဝဆုံးရှုံးနိုင်လို့ သတိထားပါ။
- **Secrets Engines**: `vault secrets list` က enabled secrets engines တွေကို ပြပြီး `-detailed` option နဲ့ accessor ID, max TTL စတဲ့ အချက်အလက်တွေကို ထပ်ပြပါတယ်။ **Barrier View** က secrets engines တွေကို သီးခြားခွဲထားတဲ့ security feature ဖြစ်ပါတယ်။
- **Auditing**: Auditing က default အနေနဲ့ disabled ဖြစ်ပြီး `vault audit enable` နဲ့ file/database-based audit devices တွေကို ဖွင့်ရပါတယ်။ Disable လုပ်ဖို့ `vault audit disable <path>` ကို Vault path (ဥပမာ `file/`) နဲ့ အသုံးပြုပါတယ်။
- **Transit Secrets Engine**: Ciphertext ကို decrypt လုပ်ရင် **base64 encoded plaintext** ကို ပြန်ပေးပြီး token မှာ `update` capability ရှိဖို့ လိုပါတယ်။ Sudo capability က decrypt အတွက် မလိုအပ်ပါဘူး။

## ကျွန်တော့်အတွေ့အကြုံ

HashiCorp Vault Basics course က Vault ရဲ့ အခြေခံကျကျ သဘောတရားတွေကနေ production-ready concepts တွေအထိ အကျုံးဝင်ပြီး HashiCorp Certified Vault Associate (002) စာမေးပွဲအတွက် ပြည့်စုံတဲ့ ပြင်ဆင်မှုတစ်ခုပေးပါတယ်။ Labs တွေက CLI, UI, နဲ့ API ကို အသုံးပြုပြီး လက်တွေ့လုပ်ဆောင်ကြည့်ရတာက Vault ရဲ့ operational aspects တွေကို နားလည်ဖို့ အထူးအထောက်အကူဖြစ်ပါတယ်။

Exam Alerts တွေက စာမေးပွဲမှာ အာရုံစိုက်ရမယ့် အဓိကအချက်တွေကို မီးမောင်းထိုးပြပေးပြီး principle of least privilege, implicit deny, နဲ့ security features (ဥပမာ Barrier View, Shamir’s Secret Sharing) တွေရဲ့ အရေးပါမှုကို နားလည်လာစေပါတယ်။ Auditing နဲ့ EaaS လို advanced topics တွေက Vault ကို production မှာ ဘယ်လို လုံခြုံစွာ အသုံးပြုနိုင်တယ်ဆိုတာကို ပြသပေးပါတယ်။