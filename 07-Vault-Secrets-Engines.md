# HashiCorp Vault: Secrets Engines အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ secrets engines တွေအကြောင်းကို အသေးစိတ်ရှင်းပြပေးသွားမှာဖြစ်ပါတယ်။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ secrets engines တွေရဲ့ အခြေခံသဘောတရား၊ built-in နဲ့ add-on engines တွေရဲ့ လုပ်ဆောင်ပုံ၊ နဲ့ built-in secrets engines တွေနဲ့ လုပ်ဆောင်ရတဲ့ lab အကြောင်းကို မြန်မာဘာသာနဲ့ ရှင်းလင်းစွာ ဖော်ပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Secrets Engines ဆိုတာ ဘာလဲ?

Vault ရဲ့ secrets engines တွေက Vault ထဲမှာ secrets တွေကို စီမံခန့်ခွဲဖို့အတွက် အဓိက အစိတ်အပိုင်းတွေဖြစ်ပါတယ်။ Secrets engines တွေက plugins တွေလို့လည်း ခေါ်ပါတယ်။ ၎င်းတို့ဟာ secrets တွေကို သိမ်းဆည်းတာ၊ dynamic credentials ဖန်တီးတာ၊ ဒါမှမဟုတ် encryption as a service (EAAS) ပေးတာစတဲ့ လုပ်ဆောင်ချက်တွေကို လုပ်ဆောင်ပေးပါတယ်။ Secrets engine မရှိရင် Vault ထဲမှာ ဘာမှ လုပ်လို့မရပါဘူး။

Vault မှာ secrets engines တွေက path-based ဖြစ်ပြီး သတ်မှတ်ထားတဲ့ path (ဥပမာ `secret/`, `aws/`) မှာ အလုပ်လုပ်ပါတယ်။ Secrets engines တွေက case-sensitive ဖြစ်လို့ `kv` နဲ့ `KV` ဆိုရင် သီးခြား engines နှစ်ခုအဖြစ် သတ်မှတ်ပါတယ်။

### Built-in Secrets Engines

Vault မှာ default အနေနဲ့ ပါဝင်တဲ့ built-in secrets engines လေးခုရှိပါတယ်။

1. **Cubbyhole Secrets Engine**:

   - Path: `cubbyhole/`
   - Per-token private secret storage အတွက် အသုံးပြုပါတယ်။ ဆိုလိုတာက တစ်ခုချင်းစီ token မှာ ၎င်းရဲ့ သီးသန့် cubbyhole ရှိပြီး တခြား tokens တွေက access လုပ်လို့မရပါဘူး။
   - Sharing လုပ်လို့မရပါဘူး။
   - Token ကို revoke လုပ်လိုက်ရင် ၎င်းနဲ့ ဆက်စပ်တဲ့ cubbyhole ထဲက secrets တွေလည်း ဖျက်သိမ်းခံရပါတယ်။
   - Key တစ်ခုကို rewrite လုပ်ရင် အဟောင်း value ကို ဖျက်ပစ်ပြီး အသစ်နဲ့ အစားထိုးပါတယ်။

2. **Identity Secrets Engine**:

   - Path: `identity/`
   - Vault ရဲ့ identity management solution ဖြစ်ပါတယ်။
   - Client တစ်ခုက backend (ဥပမာ AWS, GitHub) ကတစ်ဆင့် authenticate လုပ်တဲ့အခါ entity တစ်ခုကို ဖန်တီးပြီး ID number နဲ့ track လုပ်ပါတယ်။
   - Entities တွေကို multiple authentication backends တွေနဲ့ ချိတ်ဆက်ဖို့ aliases တွေကို အသုံးပြုပါတယ်။
   - OpenID Connect (OIDC) compliant ဖြစ်ပြီး MFA (multi-factor authentication) ကို support လုပ်ပါတယ်။
   - UI ထဲမှာ မပြပါဘူး၊ CLI ဒါမှမဟုတ် API ကတစ်ဆင့်ပဲ access လုပ်လို့ရပါတယ်။

3. **KV (Key/Value) Secrets Engine**:

   - Path: Dev server မှာ default အနေနဲ့ `secret/`
   - Generic key/value storage အတွက် အသုံးပြုပါတယ်။ Password manager လိုမျိုး arbitrary secrets တွေကို သိမ်းဆည်းနိုင်ပါတယ်။
   - Version 1: Value တစ်ခုကို rewrite လုပ်ရင် အဟောင်းကို ဖျက်ပစ်ပါတယ်။
   - Version 2: Multiple versions တွေကို သိမ်းဆည်းနိုင်ပြီး default အနေနဲ့ 10 versions ထိ သိမ်းထားပါတယ်။ Deleted values တွေကို undelete လုပ်လို့ရပါတယ်။
   - Dev server မှာ default အနေနဲ့ `secret/` path နဲ့ KV engine ပါဝင်ပေမယ့် sealed vault မှာဆိုရင် ကိုယ်တိုင် ဖန်တီးရပါတယ်။

4. **System (Sys) Secrets Engine**:

   - Path: `sys/`
   - Vault ရဲ့ core system နဲ့ တိုက်ရိုက်ဆက်စပ်နေပြီး configuration information တွေကို သိမ်းဆည်းထားပါတယ်။
   - Modify လုပ်လို့မရပါဘူး၊ view information အတွက်ပဲ အသုံးပြုပါတယ်။
   - ဥပမာ `sys/leases/lookup` path ကို အသုံးပြုပြီး leases တွေကို ကြည့်ခဲ့ဖူးပါတယ်။

### Add-on Secrets Engines

Built-in engines တွေအပြင် Vault မှာ external providers တွေနဲ့ ချိတ်ဆက်ဖို့ add-on secrets engines (plugins) တွေလည်း ရှိပါတယ်။ ဥပမာ:

- **AWS**: IAM users နဲ့ credentials တွေကို dynamically ဖန်တီးဖို့ အသုံးပြုပါတယ် (ယခင် lab မှာ လေ့လာခဲ့ပြီးပါပြီ)။
- **Databases**: Postgres, MySQL, MongoDB, Cassandra စတဲ့ database credentials တွေကို manage လုပ်ပါတယ်။
- **Consul**: HashiCorp ရဲ့ Consul tool နဲ့ ချိတ်ဆက်ပြီး secrets တွေကို manage လုပ်ပါတယ်။
- **LDAP**: Lightweight Directory Access Protocol ကို အသုံးပြုပြီး accounts တွေကို Vault ထဲ manage လုပ်ပါတယ်။

### Secrets Engine Lifecycle

Secrets engines တွေကို စီမံခန့်ခွဲဖို့ `vault secrets` command ကို အသုံးပြုပြီး အောက်က subcommands တွေရှိပါတယ်:

- **Enable**: Secrets engine တစ်ခုကို သတ်မှတ်ထားတဲ့ path မှာ ဖွင့်ဖို့ (ဥပမာ `vault secrets enable -path=my-kv kv`)။
- **Disable**: Secrets engine ကို ပိတ်ဖို့။ ဒါလုပ်ရင် engine ထဲက secrets အားလုံး revoke ဖြစ်သွားပြီး physical storage ထဲက data တွေ ဖျက်သိမ်းခံရပါတယ်။
- **Move**: Engine ရဲ့ path ကို ပြောင်းဖို့။ ဒါလုပ်ရင်လည်း secrets အားလုံး revoke ဖြစ်သွားပေမယ့် configuration data တွေကတော့ ဆက်လက်ရှိနေပါတယ်။
- **Tune**: Engine ရဲ့ global configuration (ဥပမာ TTLs) တွေကို ပြင်ဆင်ဖို့။

**Exam Alert**: `vault secrets list` command က enabled secrets engines အားလုံးကို ပြပေးပါတယ်။ `-detailed` option ထည့်ရင် accessor ID, max TTL, options စတဲ့ အချက်အလက်တွေကို ထပ်ပြပါတယ်။

**Exam Alert**: **Barrier View** ဆိုတာ secrets engines တွေကို သီးခြားခွဲထားတဲ့ security feature တစ်ခုပါ။ Engine တစ်ခုက တခြား engine ရဲ့ data ကို access လုပ်လို့မရပါဘူး။ ဖွင့်လိုက်တဲ့ engine တိုင်းမှာ universally unique identifier (UUID) တစ်ခု ထုတ်ပေးပြီး Vault core မှာ သီးခြား data route အဖြစ် အလုပ်လုပ်ပါတယ်။

## Lab: Vault Secrets Engines နဲ့ လုပ်ဆောင်ခြင်း

Course ထဲက Lab 8 မှာ Vault development server တစ်ခုကို ဖန်တီးပြီး built-in secrets engines လေးခုဖြစ်တဲ့ cubbyhole, identity, KV, နဲ့ system engines တွေကို စူးစမ်းကြည့်ရပါတယ်။ CLI နဲ့ UI ကနေ ဒီ engines တွေနဲ့ ဘယ်လို အလုပ်လုပ်ရမလဲဆိုတာကို လက်တွေ့လုပ်ဆောင်ကြည့်ရပါတယ်။ အဓိကအဆင့်တွေကို အောက်မှာ ဖော်ပြပေးလိုက်ပါတယ်။

 1. **Vault Development Server ကို Run ပါ**\
    Terminal တစ်ခုမှာ -

    ```bash
    vault server -dev -dev-root-token-id=root
    ```

    Root token ID ကို `root` လို့ သတ်မှတ်ထားတာပါ။ ဒါက testing အတွက်ပဲ သင့်တော်ပြီး production မှာ မသုံးသင့်ပါဘူး။

 2. **Vault Address နဲ့ Token ကို Export လုပ်ပါ**\
    နောက်ထပ် terminal တစ်ခုမှာ -

    ```bash
     export VAULT_ADDR='http://127.0.0.1:8200'
     export VAULT_TOKEN='root'
    ```

    Space တစ်ချက်ထည့်ပြီး command တွေကို ရိုက်ပါ၊ ဒါမှ history ထဲမှာ မကျန်ရှိမှာပါ။

 3. **Vault Secrets Help File ကို ကြည့်ရှုပါ**

    ```bash
    vault secrets -h
    ```

    Subcommands တွေဖြစ်တဲ့ `enable`, `disable`, `list`, `move`, `tune` တွေအကြောင်း လေ့လာပါ။ Specific subcommand တစ်ခုအကြောင်း သိချင်ရင် ဥပမာ `vault secrets enable -h` လို့ ရိုက်ကြည့်ပါ။

 4. **Enabled Secrets Engines တွေကို List လုပ်ပါ**

    ```bash
    vault secrets list
    ```

    Output မှာ built-in secrets engines လေးခုကို တွေ့ရပါလိမ့်မယ်:

    - `cubbyhole/`
    - `identity/`
    - `secret/` (KV engine, dev server မှာ default ပါဝင်)
    - `sys/`

 5. **Secrets Engines တွေရဲ့ Help Files ကို ကြည့်ရှုပါ**\
    Engine တစ်ခုချင်းစီရဲ့ အချက်အလက်ကို ကြည့်ဖို့ `vault path-help` command ကို အသုံးပြုပါ။

    - Cubbyhole:

      ```bash
      vault path-help cubbyhole
      ```

      Output: Cubbyhole က token-specific private storage အတွက်ပဲ အသုံးပြုပြီး တခြား tokens က access လုပ်လို့မရဘူးဆိုတာကို ဖော်ပြပါတယ်။
    - KV:

      ```bash
      vault path-help secret
      ```

      Output: KV engine ရဲ့ key/value pair storage အကြောင်း ဖော်ပြပါတယ်။
    - Identity:

      ```bash
      vault path-help identity
      ```

      Output: Entities, aliases, OIDC, MFA စတဲ့ features တွေအကြောင်း ဖော်ပြပါတယ်။
    - System:

      ```bash
      vault path-help sys
      ```

      Output: System configuration နဲ့ ဆက်စပ်တဲ့ paths တွေအကြောင်း ဖော်ပြပါတယ်။

 6. **New KV Secrets Engine ဖန်တီးပါ**

    ```bash
    vault secrets enable -path=test_path1 kv
    ```

    Enabled engines တွေကို ပြန်ကြည့်ဖို့ -

    ```bash
    vault secrets list
    ```

    Output မှာ `test_path1/` ဆိုတဲ့ KV engine အသစ်ကို တွေ့ရပါလိမ့်မယ်။

 7. **New Secret ဖန်တီးပါ**\
    `test_path1` engine ထဲမှာ secret တစ်ခုဖန်တီးဖို့ -

    ```bash
    vault kv put -mount=test_path1 abco-password passwd=test123
    ```

 8. **Child Root Token ဖန်တီးပြီး UI ကနေ ကြည့်ရှုပါ**\
    Child root token တစ်ခုဖန်တီးဖို့ -

    ```bash
    vault token create
    ```

    Token ID ကို copy လုပ်ပြီး browser မှာ `http://127.0.0.1:8200` ကို သွားပါ။ Token method ကို ရွေးပြီး child root token ID ကို ထည့်ပြီး login ဝင်ပါ။

    UI ထဲမှာ secrets engines တွေကို ကြည့်ရင်:

    - `cubbyhole/`
    - `secret/` (default KV engine)
    - `test_path1/` (အသစ်ဖန်တီးထားတဲ့ KV engine) တွေကို တွေ့ရပါလိမ့်မယ်။ `identity/` နဲ့ `sys/` engines တွေက UI ထဲမှာ မပြပါဘူး။

    `test_path1` engine ထဲဝင်ကြည့်ရင် `abco-password` secret ကို တွေ့ရပါလိမ့်မယ်။

 9. **Cubbyhole Secrets Engine နဲ့ လုပ်ဆောင်ပါ**\
    Cubbyhole ထဲမှာ secret တစ်ခုဖန်တီးဖို့ (main root token နဲ့) -

    ```bash
    vault kv put -mount=cubbyhole secret1 test="this is not a secret"
    ```

    UI ထဲမှာ (child root token နဲ့ login ဝင်ထားတဲ့) cubbyhole engine ကို ကြည့်ရင် `secret1` ကို မတွေ့ရပါဘူး၊ ဘာကြောင့်လဲဆိုတော့ cubbyhole က token-specific ဖြစ်လို့ပါ။

    UI ထဲက ထွက်ပြီး main root token (`root`) နဲ့ ပြန်ဝင်ကြည့်ရင် `cubbyhole` engine ထဲမှာ `secret1` ကို တွေ့ရပါလိမ့်မယ်။

10. **Identity Secrets Engine နဲ့ လုပ်ဆောင်ပါ**\
    Entity တစ်ခုဖန်တီးဖို့ -

    ```bash
    vault write identity/entity name=Bob
    ```

    ဒါက `Bob` ဆိုတဲ့ entity တစ်ခုကို identity secrets engine ထဲမှာ ဖန်တီးပေးပါတယ်။ ဒီ entity ကို AWS, GitHub စတဲ့ authentication methods တွေနဲ့ aliases အသုံးပြုပြီး ချိတ်ဆက်နိုင်ပါတယ်။

11. **System Secrets Engine ကို စူးစမ်းပါ**\
    System engine ထဲက information တွေကို ကြည့်ဖို့ -

    ```bash
    vault list sys/leases/lookup/auth/token/create
    ```

    Output မှာ main root token နဲ့ child root token ရဲ့ lease IDs တွေကို ပြပေးပါတယ်။

12. **Secrets Engine ကို Disable လုပ်ပါ**\
    `test_path1` engine ကို disable လုပ်ဖို့ -

    ```bash
    vault secrets disable test_path1
    ```

    Secrets engines တွေကို ပြန်ကြည့်ရင် `test_path1/` ပျောက်သွားတာကို တွေ့ရပါလိမ့်မယ်။

13. **Server ကို Stop ပါ**\
    `Ctrl+C` နှိပ်ပြီး dev server ကို ရပ်ပါ။

## ကျွန်တော့်အတွေ့အကြုံ

Vault secrets engines တွေကို လေ့လာရတာ အလွန်အသုံးဝင်ပါတယ်။ Built-in engines လေးခုရဲ့ ထူးခြားချက်တွေကို နားလည်လာခဲ့ပြီး အထူးသဖြင့် cubbyhole engine ရဲ့ token-specific nature က security အတွက် ဘယ်လောက်အရေးပါတယ်ဆိုတာကို သဘောပေါက်ခဲ့ပါတယ်။ KV engine ကို အသုံးပြုပြီး secrets တွေကို စီမံခန့်ခွဲတာ၊ identity engine မှာ entities ဖန်တီးတာ၊ နဲ့ system engine ထဲက configuration data တွေကို ကြည့်တာတွေက Vault ရဲ့ versatility ကို ပြသပေးပါတယ်။ Barrier view လို security feature တွေက secrets engines တွေကို ဘယ်လို သီးခြားခွဲထားတယ်ဆိုတာကို နားလည်လာခဲ့ပါတယ်။

Production မှာ secrets engines တွေကို အသုံးပြုတဲ့အခါ engine-specific paths တွေကို သေချာသတ်မှတ်ပြီး principle of least privilege ကို လိုက်နာဖို့ အရေးကြီးပါတယ်။ Add-on engines တွေဖြစ်တဲ့ AWS ဒါမှမဟုတ် database engines တွေကို လိုအပ်ချက်အလိုက် ထပ်လောင်းအသုံးပြုနိုင်တာကလည်း Vault ရဲ့ အားသာချက်တစ်ခုပါ။

ဒီ blog post က Vault secrets engines တွေအကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။