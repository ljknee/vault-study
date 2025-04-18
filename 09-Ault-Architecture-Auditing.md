# HashiCorp Vault: Architecture နှင့် Auditing အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ architecture နဲ့ auditing အကြောင်းကို ဆွေးနွေးသွားမှာပါ။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ Vault ရဲ့ high-level architecture၊ audit devices တွေရဲ့ အလုပ်လုပ်ပုံ၊ နဲ့ disk-based storage ကို အသုံးပြုပြီး audit logs တွေနဲ့ လုပ်ဆောင်ရတဲ့ lab အကြောင်းကို မြန်မာဘာသာနဲ့ သဘာဝကျကျ ရှင်းပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Vault Architecture ဆိုတာ ဘာလဲ?

Vault ရဲ့ architecture ကို high-level မှာ နားလည်ဖို့အတွက် Vault Server ထဲက components တွေကို အကျဉ်းချုပ် ကြည့်ကြည့်ရအောင်။

1. **Core**:
   - Vault ရဲ့ အဓိက ပရိုဂရမ်ဖြစ်ပြီး CLI, UI, နဲ့ API ကတစ်ဆင့် အသုံးပြုလို့ရပါတယ်။
   - CLI commands တွေက core နဲ့ တိုက်ရိုက်ဆက်သွယ်ပြီး API နဲ့ UI ကတော့ core ရဲ့ အပြင်မှာ ရှိတာကြောင့် လုံခြုံရေးအတွက် သေချာစီမံထားဖို့ လိုပါတယ်။
   - Go programming language နဲ့ ရေးထားပြီး HashiCorp Configuration Language (HCL) နဲ့ JSON ကို အသုံးပြုပါတယ်။

2. **TokenStore**:
   - Tokens တွေကို ဖန်တီးပြီး သိမ်းဆည်းတဲ့ special backend ဖြစ်ပါတယ်။
   - Token Authentication backend လို့လည်း ခေါ်ပြီး login capability မရှိတဲ့ တစ်ခုတည်းသော authentication method ပါ။
   - Vault ထဲမှာ လုပ်ဆောင်မှုအားလုံးအတွက် existing tokens တွေ လိုအပ်ပါတယ်။

3. **PolicyStore**:
   - Policies တွေနဲ့ ၎င်းတို့ရဲ့ capabilities (permissions) တွေကို ဖန်တီးပြီး သိမ်းဆည်းတဲ့ backend ဖြစ်ပါတယ်။
   - Tokens နဲ့ policies တွေက အနီးကပ်ဆက်စပ်နေလို့ core နဲ့ နီးနီးကပ်ကပ် ရှိပါတယ်။

4. **Path Routing**:
   - Core ကို ပေးပို့တဲ့ commands တွေကို appropriate backend (system backend, secrets engines, auth methods, audit devices, external storage/providers) တွေဆီ ညွှန်ကြားပေးပါတယ်။
   - Vault ထဲက အရာအားလုံးက path-based ဖြစ်ပါတယ်။

5. **Auth Methods**:
   - Internal methods (ဥပမာ Userpass) နဲ့ external methods (ဥပမာ AWS) စတဲ့ authentication methods တွေပါဝင်ပါတယ်။

6. **Secrets Engines**:
   - Secrets တွေကို create, lookup, နဲ့ store လုပ်ဖို့ အသုံးပြုတဲ့ engines တွေပါ။
   - Key/value pairs, dynamic credentials (ဥပမာ AWS IAM users), encryption as a service (EaaS) စတဲ့ functionalities တွေကို ပေးပါတယ်။
   - External providers တွေနဲ့ ဆက်သွယ်တာကြောင့် TLS နဲ့ encryption ကို သေချာအသုံးပြုဖို့ လိုပါတယ်။

7. **System Backend (Sys)**:
   - `sys/` path မှာ ရှိပြီး Vault ရဲ့ configuration data နဲ့ internal importance ရှိတဲ့ အရာတွေကို သိမ်းဆည်းထားပါတယ်။
   - Modify လုပ်လို့မရပါဘူး၊ information ကြည့်ဖို့ပဲ အသုံးပြုပါတယ်။

8. **Storage Backend**:
   - Dev server မှာ in-memory (`inmem`) storage ကို အသုံးပြုပြီး proper Vault server မှာဆိုရင် disk-based (raft), Consul, ဒါမှမဟုတ် cloud providers တွေကို အသုံးပြုနိုင်ပါတယ်။
   - Data တွေကို လုံခြုံစွာ သိမ်းဆည်းဖို့ **security barrier** ကို အသုံးပြုပါတယ်။ Barrier က data တွေကို encrypt/decrypt လုပ်ပြီး storage backend ကို လုံခြုံစွာ ပို့ပေးပါတယ်။

9. **Audit Broker နှင့် Audit Devices**:
   - Auditing က logging, security, နဲ့ troubleshooting အတွက် အရေးကြီးပါတယ်။
   - **Audit Devices** တွေက audit logs (files ဒါမှမဟုတ် databases) တွေဖြစ်ပြီး Vault ထဲမှာ ဖြစ်ပျက်တဲ့ အရာအားလုံးကို record လုပ်ပါတယ်။
   - Multiple audit devices တွေကို အသုံးပြုပြီး fault tolerance နဲ့ data tampering ကို စစ်ဆေးနိုင်ပါတယ်။
   - **Audit Broker** က audit devices တွေကို control လုပ်ပြီး load balancing လုပ်ပေးပါတယ်။

**Exam Alert**: Auditing က default အနေနဲ့ disabled ဖြစ်နေပြီး enable လုပ်ဖို့ audit device (file ဒါမှမဟုတ် database) တစ်ခုကို သတ်မှတ်ပေးရပါတယ်။

## Lab: Auditing နှင့် Troubleshooting

Course ထဲက Lab 11 မှာ disk-based storage (raft) ကို အသုံးပြုတဲ့ Vault server တစ်ခုကို ဖန်တီးပြီး audit devices တွေကို enable လုပ်ကာ audit logs တွေကို စစ်ဆေးကြည့်ရပါတယ်။ Configuration file ကို diagnose လုပ်ပြီး troubleshooting အဆင့်တွေကိုလည်း လေ့လာရပါတယ်။ အဓိကအဆင့်တွေကို အောက်မှာ ဖော်ပြပေးလိုက်ပါတယ်။

1. **Directory Structure ဖန်တီးပါ**  
   Terminal တစ်ခုမှာ `lab-10` directory ထဲမှာ -
   ```bash
   mkdir -p vault/data
   ```
   Directory structure ကို စစ်ဆေးဖို့ -
   ```bash
   tree
   ```
   Output: `vault/data` directory ကို တွေ့ရပါမယ်။

2. **Configuration File ကို Diagnose လုပ်ပါ**  
   `config.hcl` file ကို စစ်ဆေးဖို့ -
   ```bash
   vault operator diagnose -config=config.hcl
   ```
   Output မှာ success messages တွေကို အဓိကတွေ့ရပြီး warnings (ဥပမာ TLS disabled, permissions) တွေကို သတိထားပါ။ Errors တွေ့ရင် configuration file ထဲက IP address, hostname, ဒါမှမဟုတ် storage path တွေကို ပြန်စစ်ဆေးပါ။

3. **Vault Server ကို Run ပါ**  
   ```bash
   vault server -config=config.hcl
   ```
   Server က local loopback IP (127.0.0.1) နဲ့ raft storage ကို အသုံးပြုပြီး run ပါလိမ့်မယ်။

4. **Vault Address ကို Export လုပ်ပါ**  
   နောက်ထပ် terminal တစ်ခုမှာ -
   ```bash
    export VAULT_ADDR='http://127.0.0.1:8200'
   ```

5. **Vault ကို Initialize နဲ့ Unseal လုပ်ပါ**  
   Initialize လုပ်ဖို့ -
   ```bash
   vault operator init
   ```
   Unseal keys ၅ ခုနဲ့ root token ကို သိမ်းထားပါ။ Unseal လုပ်ဖို့ key ၃ ခုကို အသုံးပြုပါ -
   ```bash
   vault operator unseal
   ```
   Unseal key တစ်ခုစီကို ထည့်ပြီး သုံးကြိမ် run ရပါမယ်။

6. **Root Token နဲ့ Login ဝင်ပါ**  
   ```bash
   vault login
   ```
   Root token ကို ထည့်ပါ။ Output မှာ root policy နဲ့ infinite duration ရှိတာကို တွေ့ရပါမယ်။

7. **Vault Audit Help File ကို ကြည့်ပါ**  
   ```bash
   vault audit -h
   ```
   Subcommands တွေဖြစ်တဲ့ `enable`, `disable`, `list` တွေအကြောင်း လေ့လာပါ။

8. **Audit Device ကို Enable လုပ်ပါ**  
   File-based audit device တစ်ခုဖန်တီးဖို့ -
   ```bash
   vault audit enable file file_path=./vault/vault-audit.log
   ```
   Enabled audit devices တွေကို ကြည့်ဖို့ -
   ```bash
   vault audit list
   ```
   Output: `file/` path ကို တွေ့ရပါမယ်။

9. **Audit Log ကို စစ်ဆေးပါ**  
   Log file ကို ကြည့်ဖို့ -
   ```bash
   cd vault
   cat vault-audit.log | jq
   ```
   Output မှာ JSON format နဲ့ audit entries တွေကို တွေ့ရပါမယ်။ ဒါက initial setup နဲ့ configuration တွေရဲ့ မှတ်တမ်းတွေပါ။

10. **Vault ထဲမှာ Action တစ်ခုလုပ်ပါ**  
    AWS authentication method ကို enable လုပ်ဖို့ -
    ```bash
    vault auth enable aws
    ```
    Log file ကို ပြန်ကြည့်ဖို့ -
    ```bash
    cat vault-audit.log | jq
    ```
    Output မှာ `path: sys/auth/aws` ဆိုတဲ့ entry အသစ်ကို တွေ့ရပါမယ်။ Audit log ထဲမှာ request တိုင်းမှာ universally unique ID (UUID) ပါဝင်ပြီး path နဲ့ operation တွေကို record လုပ်ထားပါတယ်။

11. **Audit Device ကို Disable လုပ်ပါ**  
    ```bash
    vault audit disable file/
    ```
    Audit devices တွေကို ပြန်ကြည့်ဖို့ -
    ```bash
    vault audit list
    ```
    Output: No audit devices (disabled ဖြစ်သွားလို့)။

    Log file ကို ပြန်ကြည့်ရင် disable operation (`path: sys/audit/file`) ကို မှတ်တမ်းတင်ထားပြီး နောက်ထပ် actions တွေကို မမှတ်တော့ပါဘူး။

12. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး Vault server ကို ရပ်ပါ။

**Exam Alert**: Audit device တစ်ခုကို disable လုပ်ဖို့ `vault audit disable <path>` command ကို အသုံးပြုပါ။ Path က audit device ရဲ့ Vault path (ဥပမာ `file/` ဒါမှမဟုတ် `audit2/`) ဖြစ်ရပါမယ်။ Operating system ထဲက file path (ဥပမာ `./vault/audit.log`) ကို သုံးလို့မရပါဘူး။

## ကျွန်တော့်အတွေ့အကြုံ

Vault architecture နဲ့ auditing တွေကို လေ့လာရတာ Vault ရဲ့ အလုပ်လုပ်ပုံကို ပိုမိုနားလည်လာစေပါတယ်။ Core, TokenStore, PolicyStore, နဲ့ path routing တွေက Vault ရဲ့ အဓိက အစိတ်အပိုင်းတွေဖြစ်ပြီး secrets engines နဲ့ auth methods တွေက external providers တွေနဲ့ ဘယ်လို ချိတ်ဆက်လည်ပတ်တယ်ဆိုတာကို သဘောပေါက်ခဲ့ပါတယ်။ Security barrier က data encryption/decryption အတွက် ဘယ်လောက်အရေးကြီးတယ်ဆိုတာကိုလည်း သိရှိခဲ့ပါတယ်။

Audit devices တွေကို enable/disable လုပ်ပြီး audit logs တွေကို စစ်ဆေးရတာက security နဲ့ troubleshooting အတွက် အလွန်အသုံးဝင်ပါတယ်။ JSON-based logs တွေကို `jq` နဲ့ parse လုပ်ကြည့်ရတာက logs ထဲက အချက်အလက်တွေကို ဘယ်လို အလွယ်တကူ ခွဲခြမ်းစိတ်ဖြာလို့ရတယ်ဆိုတာကို ပြသပေးပါတယ်။ Configuration file ကို `vault operator diagnose` နဲ့ စစ်ဆေးတာကလည်း server setup မှာ ဖြစ်ပေါ်လာနိုင်တဲ့ issues တွေကို ကြိုတင်ဖမ်းမိဖို့ အထောက်အကူဖြစ်ပါတယ်။

Production မှာ Vault ကို အသုံးပြုတဲ့အခါ multiple audit devices တွေကို enable လုပ်ပြီး fault tolerance နဲ့ data integrity ကို သေချာစီမံဖို့ လိုအပ်ပါတယ်။ TLS နဲ့ proper storage backends တွေကို အသုံးပြုပြီး dev servers တွေကို production မှာ လုံးဝမသုံးဖို့လည်း သတိထားရမှာပါ။

ဒီ blog post က Vault architecture နဲ့ auditing အကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။