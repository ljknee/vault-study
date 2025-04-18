# HashiCorp Vault: Transit Secrets Engine နှင့် EaaS အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ **Transit Secrets Engine** နဲ့ **Encryption as a Service (EaaS)** အကြောင်းကို ဆွေးနွေးသွားမှာပါ။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ EaaS ရဲ့ အခြေခံသဘောတရား၊ Transit Secrets Engine ကို အသုံးပြုပြီး data encryption/decryption လုပ်နည်း၊ နဲ့ လက်တွေ့လုပ်ဆောင်ကြည့်ရတဲ့ lab အကြောင်းကို မြန်မာဘာသာနဲ့ သဘာဝကျကျ ရှင်းပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Encryption as a Service (EaaS) ဆိုတာ ဘာလဲ?

**Encryption** ဆိုတာ အချက်အလက်တွေကို cryptography ဒါမှမဟုတ် keys တွေကို အသုံးပြုပြီး encode လုပ်တဲ့ လုပ်ငန်းစဉ်တစ်ခုပါ။ Plaintext (ဥပမာ `ABC`) ကို **ciphertext** (ဥပမာ `vault:v1:xyz...`) အဖြစ် ပြောင်းလဲပြီး authorized party တွေပဲ ပြန်လည်ဖတ်ရှုနိုင်အောင် လုပ်ဆောင်ပေးပါတယ်။

**Encryption as a Service (EaaS)** ဆိုတာကတော့ secrets တွေကို encrypt လုပ်ပေးတဲ့ service တစ်ခုပါ။ ဒါက data in transit (network တွေ ဒါမှမဟုတ် internet ကတစ်ဆင့် ပို့လွှတ်နေတဲ့ data) တွေကို ကာကွယ်ဖို့ ရည်ရွယ်ပါတယ်။ Data in transit ကို ကာကွယ်ရတာက applications နဲ့ services တွေရဲ့ security configurations တွေ မသိနိုင်တာကြောင့် ခက်ခဲပါတယ်။

Vault မှာ EaaS ကို **Transit Secrets Engine** ကတစ်ဆင့် ပေးပါတယ်။ Transit Secrets Engine က customer applications တွေကနေ Vault ထဲကို data ပို့ပြီး **TLS (Transport Layer Security)** နဲ့ transit မှာ ကာကွယ်ကာ **AES-256 encryption** နဲ့ သိမ်းဆည်းပေးပါတယ်။ Encrypted data ကို application ထဲ ပြန်ပို့ပြီး လိုအပ်တဲ့အခါ decrypt လုပ်နိုင်ပါတယ်။

**သတိပေးချက်**: Sensitive data တွေ (ဥပမာ credit card transactions) ကို ကိုင်တွယ်တဲ့အခါ Transit Secrets Engine ကို **on-premises** မှာ run တာက cloud-based solutions ထက် ပိုလုံခြုံနိုင်ပါတယ်။ ဒါက data တွေကို သင့်ကိုယ်ပိုင် network ထဲမှာ ထိန်းသိမ်းထားနိုင်လို့ပါ။

**Exam Alert**: Transit Secrets Engine က ciphertext ကို decrypt လုပ်တဲ့အခါ **base64 encoded plaintext** ကို ပြန်ပေးပါတယ်။ Token မှာ appropriate permissions (update capability) ရှိမှသာ decrypt လုပ်နိုင်ပါတယ်။ Sudo capability က decrypt လုပ်ဖို့ မလိုအပ်ပါဘူး။

## Lab: Transit Secrets Engine နဲ့ လုပ်ဆောင်ခြင်း

Course ထဲက Lab 12 မှာ Vault development server တစ်ခုကို ဖန်တီးပြီး **Transit Secrets Engine** ကို enable လုပ်ကာ encryption key တစ်ခု ဖန်တီးရပါတယ်။ Policy တစ်ခုကို ရေးပြီး token တစ်ခုနဲ့ ချိတ်ဆက်ကာ plaintext (email address) တစ်ခုကို encrypt/decrypt လုပ်ကြည့်ရပါတယ်။ အဓိကအဆင့်တွေကို အောက်မှာ ဖော်ပြပေးလိုက်ပါတယ်။

1. **Vault Development Server ကို Run ပါ**  
   Terminal တစ်ခုမှာ -
   ```bash
   vault server -dev
   ```
   Root token ID ကို မှတ်ထားပါ။

2. **Vault Address နဲ့ Token ကို Export လုပ်ပါ**  
   နောက်ထပ် terminal နှစ်ခုမှာ -
   ```bash
    export VAULT_ADDR='http://127.0.0.1:8200'
    export VAULT_TOKEN='<root_token>'
   ```
   Space တစ်ချက်ထည့်ပြီး command တွေကို ရိုက်ပါ၊ ဒါမှ history ထဲမှာ မကျန်ရှိမှာပါ။

3. **Vault Status ကို Check ပါ**  
   ```bash
   vault status
   ```
   Server က unsealed ဖြစ်နေတာကို အတည်ပြုပါ။

4. **Transit Secrets Engine ကို Enable လုပ်ပါ**  
   ```bash
   vault secrets enable transit
   ```
   Enabled secrets engines တွေကို ကြည့်ဖို့ -
   ```bash
   vault secrets list
   ```
   Output မှာ `transit/` path ကို တွေ့ရပါမယ်။

5. **Encryption Key ဖန်တီးပါ**  
   Encryption key ring တစ်ခုဖန်တီးဖို့ -
   ```bash
   vault write -f transit/keys/emails
   ```
   Output မှာ key type က `aes256-gcm96` ဖြစ်တာကို တွေ့ရပါမယ်။ ဒါက AES-256 encryption ကို အသုံးပြုထားတာပါ။

6. **Emails Policy ကို Upload လုပ်ပါ**  
   `emails-policy.hcl` file ထဲမှာ အောက်က paths နဲ့ capabilities တွေ ပါဝင်ပါတယ်:
   ```hcl
   path "transit/encrypt/emails" {
     capabilities = ["update"]
   }
   path "transit/decrypt/emails" {
     capabilities = ["update"]
   }
   ```
   Policy ကို Vault ထဲ upload လုပ်ဖို့ -
   ```bash
   vault policy write emails-policy emails-policy.hcl
   ```
   Policies တွေကို ကြည့်ဖို့ -
   ```bash
   vault policy list
   ```
   Output: `default`, `root`, `emails-policy`

7. **Emails Policy နဲ့ Token ဖန်တီးပါ**  
   ```bash
   vault token create -policy=emails-policy
   ```
   Token ID ကို environment variable အနေနဲ့ သိမ်းဖို့ -
   ```bash
    export EMAILS_TOKEN=<token_id>
   ```
   Token အချက်အလက်ကို ကြည့်ဖို့ -
   ```bash
   vault token lookup $EMAILS_TOKEN
   ```
   Output မှာ `default` နဲ့ `emails-policy` ပါဝင်တာကို တွေ့ရပါမယ်။

8. **Plaintext ကို Encrypt လုပ်ပါ**  
   Email address တစ်ခုကို encrypt လုပ်ဖို့ -
   ```bash
   VAULT_TOKEN=$EMAILS_TOKEN vault write transit/encrypt/emails \
       plaintext=$(base64 <<< "fake-mail@prowse.tech")
   ```
   Output မှာ `ciphertext` (ဥပမာ `vault:v1:xyz...`) နဲ့ key version (`1`) ကို ပြပါတယ်။ Plaintext ကို အရင် **base64** encode လုပ်ပြီး **AES-256** encryption နဲ့ ciphertext အဖြစ် ပြောင်းထားတာပါ။

9. **Ciphertext ကို Decrypt လုပ်ပါ**  
   Ciphertext ကို decrypt လုပ်ဖို့ (ciphertext ကို copy လုပ်ပါ) -
   ```bash
   VAULT_TOKEN=$EMAILS_TOKEN vault write transit/decrypt/emails \
       ciphertext="vault:v1:xyz..."
   ```
   Output မှာ **base64 encoded plaintext** (ဥပမာ `ZmFrZS1tYWlsQHByb3dzZS50ZWNo`) ကို ပြပါတယ်။

10. **Base64 Encoded Plaintext ကို Decode လုပ်ပါ**  
    Base64 encoded plaintext ကို original email address အဖြစ် ပြန်ရဖို့ -
    ```bash
    echo "ZmFrZS1tYWlsQHByb3dzZS50ZWNo" | base64 --decode
    ```
    Output: `fake-mail@prowse.tech`

11. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး dev server ကို ရပ်ပါ။

## ကျွန်တော့်အတွေ့အကြုံ

Transit Secrets Engine နဲ့ EaaS ကို လေ့လာရတာ Vault ရဲ့ စွမ်းရည်တွေထဲက အရေးပါတဲ့ အစိတ်အပိုင်းတစ်ခုကို နားလည်လာစေပါတယ်။ Plaintext ကို base64 encode လုပ်ပြီး AES-256 encryption နဲ့ ciphertext အဖြစ် ပြောင်းလဲတာ၊ ပြီးတော့ ciphertext ကို decrypt လုပ်ပြီး base64 decode လုပ်ရတာက data in transit ကို ဘယ်လို လုံခြုံစွာ ကာကွယ်နိုင်တယ်ဆိုတာကို ပြသပေးပါတယ်။ Emails policy နဲ့ token ကို ချိတ်ဆက်ပြီး specific permissions (update capability) တွေကို သတ်မှတ်ပေးတာက **principle of least privilege** ကို ဘယ်လို အကောင်အထည်ဖော်တယ်ဆိုတာကို သိရှိစေပါတယ်။

Production မှာ Transit Secrets Engine ကို အသုံးပြုတဲ့အခါ **TLS** ကို မဖြစ်မနေ enable လုပ်ပြီး **on-premises** deployment ကို ထည့်သွင်းစဉ်းစားဖို့ လိုပါတယ်။ Sensitive data တွေကို ကိုယ်ပိုင် network ထဲမှာ ထိန်းသိမ်းထားတာက security risks တွေကို လျှော့ချပေးနိုင်ပါတယ်။ Base64 encoding ကို encryption နဲ့ တွဲဖက်အသုံးပြုတာကလည်း security best practice တစ်ခုဖြစ်တာကို သင်ယူခဲ့ရပါတယ်။

ဒီ blog post က Transit Secrets Engine နဲ့ EaaS အကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။