# HashiCorp Vault: Development Server နှင့် Sealed Vault အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ Vault Development Server နဲ့ Sealed Vault အကြောင်းကို ဆက်လက်ရှင်းပြပေးသွားမှာဖြစ်ပါတယ်။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ development server ကို ဘယ်လိုအသုံးပြုရမလဲ၊ sealed vault ကို ဘယ်လိုတည်ဆောက်ရမလဲဆိုတာတွေကို မြန်မာဘာသာနဲ့ ရှင်းလင်းစွာ ဖော်ပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Vault Development Server ဆိုတာ ဘာလဲ?

Vault Development Server (သို့မဟုတ် dev server) ဆိုတာ Vault ရဲ့ စမ်းသပ်အသုံးပြုဖို့အတွက် အထူးပြုလုပ်ထားတဲ့ server တစ်ခုဖြစ်ပါတယ်။ ဒါကို သင့်ရဲ့ laptop ဒါမှမဟုတ် အခြား system တွေမှာ အလွယ်တကူ run လို့ရပါတယ်။ သို့သော် **testing, experimenting, နဲ့ educational purposes** အတွက်သာ အသုံးပြုသင့်ပြီး production မှာ လုံးဝမသုံးသင့်ပါဘူး။

Dev server ရဲ့ အဓိကထူးခြားချက်တွေကတော့ -

1. **အလိုအလျောက်ဖွင့်ထားသည်** - ဥပမာ သော့မပါတဲ့ safe တစ်ခုလိုမျိုး၊ dev server က automatically initialized နဲ့ unsealed ဖြစ်နေပါတယ်။ ဒါကြောင့် လုံခြုံမှုနည်းပါတယ်။
2. **Memory အပေါ်အခြေခံသည်** - Server က RAM ထဲမှာ အလုပ်လုပ်တာဖြစ်တဲ့အတွက် ယာယီသာဖြစ်ပါတယ်။ ဆိုလိုတာက server ကို ပိတ်လိုက်တာ၊ terminal ကို ပိတ်လိုက်တာ၊ ဒါမှမဟုတ် computer ကို restart လုပ်လိုက်တာနဲ့ data အားလုံး ပျောက်သွားမှာဖြစ်ပါတယ်။
3. **Local Loopback** - Default အနေနဲ့ 127.0.0.1 (localhost) မှာ port 8200 နဲ့ အလုပ်လုပ်ပါတယ်။
4. **Secrets Engine** - Dev server က default အနေနဲ့ `secret/` ဆိုတဲ့ key-value pair secrets engine တစ်ခုကို အလိုအလျောက်ဖန်တီးပေးပါတယ်။

**Exam Alert**: Vault dev server ကို production မှာ ဘယ်တော့မှမသုံးပါနဲ့။ ဘာကြောင့်လဲဆိုတော့ ဒါက insecure ဖြစ်ပြီး volatile ဖြစ်နေလို့ပါ။ TLS မပါတာကြောင့် HTTPS အစား HTTP နဲ့သာ connect လုပ်လို့ရပြီး၊ server က unsealed ဖြစ်နေတာကြောင့် sensitive data တွေကို အလွယ်တကူ ခိုးယူခံရနိုင်ပါတယ်။

### Lab: Vault Dev Server တည်ဆောက်ခြင်း

Course ထဲက Lab 2 မှာ Vault dev server ကို deploy လုပ်ပြီး basic secrets တွေကို ဖန်တီးကြည့်ရပါတယ်။ ဒီမှာ အဓိကအဆင့်တွေကို ဖော်ပြပေးလိုက်ပါတယ်။

1. **Dev Server ကို Run ပါ**  
   Terminal တစ်ခုမှာ အောက်ပါ command ကို ရိုက်ထည့်ပါ။
   ```bash
   vault server -dev
   ```
   ဒါက 127.0.0.1:8200 မှာ server ကို start လုပ်ပေးပါလိမ့်မယ်။ Terminal ကို ပိတ်မထားပါနဲ့၊ ဒါမှမဟုတ်ရင် server ရပ်သွားပါလိမ့်မယ်။ Output ထဲမှာ Unseal Key နဲ့ Root Token ကို သေချာမှတ်သားထားပါ။

2. **Vault Address ကို Export လုပ်ပါ**  
   နောက်ထပ် terminal တစ်ခုဖွင့်ပြီး အောက်ပါ command ကို ရိုက်ထည့်ပါ။
   ```bash
    export VAULT_ADDR='http://127.0.0.1:8200'
   ```
   ဒါက Vault commands တွေကို server နဲ့ ချိတ်ဆက်ဖို့ လိုအပ်ပါတယ်။ Command history ထဲ မကျန်ရှိဖို့ space တစ်ချက်ထည့်ပြီး ရိုက်တာ ပိုကောင်းပါတယ်။

3. **Vault Status ကို Check ပါ**  
   Server ရဲ့ status ကို သိရှိဖို့ အောက်ပါ command ကို ရိုက်ကြည့်ပါ။
   ```bash
   vault status
   ```
   Output မှာ `initialized: true`, `sealed: false`, `storage_type: inmem` စတဲ့ အချက်အလက်တွေကို မြင်ရပါလိမ့်မယ်။ ဒါက dev server က အလုပ်လုပ်နေပြီဆိုတာကို အတည်ပြုပေးပါတယ်။

4. **Basic Secret တစ်ခု ဖန်တီးပါ**  
   Key-value pair တစ်ခုကို ဖန်တီးဖို့ အောက်ပါ command ကို အသုံးပြုပါ။
   ```bash
   vault kv put -mount=secret color-A red=1
   ```
   ဒါက `secret/` secrets engine ထဲမှာ `color-A` ဆိုတဲ့ secret တစ်ခုကို ဖန်တီးပြီး `red=1` ဆိုတဲ့ key-value pair ကို သိမ်းဆည်းပေးပါတယ်။

5. **Secret ကို ကြည့်ပါ**  
   ဖန်တီးထားတဲ့ secret ကို ကြည့်ရှုဖို့ -
   ```bash
   vault kv get -mount=secret color-A
   ```
   Output ထဲမှာ `red=1` ဆိုတဲ့ data ကို မြင်ရပါလိမ့်မယ်။

6. **Secret ကို Delete နဲ့ Undelete လုပ်ပါ**  
   Secret တစ်ခုကို delete လုပ်ဖို့ -
   ```bash
   vault kv delete -mount=secret color-A
   ```
   ပြီးရင် ပြန်လည်ရယူဖို့ (undelete) -
   ```bash
   vault kv undelete -mount=secret -versions=1 color-A
   ```

7. **UI ကနေ Login ဝင်ပါ**  
   Browser မှာ `http://127.0.0.1:8200` ကို သွားပြီး Root Token ကို ထည့်သွင်းကာ login ဝင်ပါ။ UI ထဲမှာ `secret/` secrets engine ထဲက secrets တွေကို ကြည့်ရှုနိုင်ပါတယ်။ ဥပမာ `color-C` ဆိုတဲ့ secret တစ်ခုကို UI ကနေ ဖန်တီးကြည့်နိုင်ပါတယ်။

8. **Server ကို Stop ပါ**  
   ပထမဆုံး terminal မှာ `Ctrl+C` နှိပ်ပြီး server ကို ရပ်ပါ။ ဒါဆိုရင် server နဲ့ data အားလုံး ပျောက်သွားပါလိမ့်မယ်။

## Sealed Vault ဆိုတာ ဘာလဲ?

Sealed Vault ကတော့ production မှာ အသုံးပြုဖို့ သင့်တော်တဲ့ Vault server တစ်ခုဖြစ်ပါတယ်။ Dev server နဲ့ မတူဘဲ sealed vault က default အနေနဲ့ sealed ဖြစ်နေပြီး sensitive data တွေကို လုံခြုံစွာ သိမ်းဆည်းထားနိုင်ပါတယ်။ Sealed vault ကို အသုံးပြုဖို့ဆိုရင် initialize လုပ်ပြီး unseal လုပ်ရပါတယ်။

**Exam Alert**: Typical Vault server တစ်ခုက sealed state နဲ့ စတင်ပါတယ်။ ဒါက physical storage ကို access လုပ်နိုင်ပေမယ့် data တွေကို decrypt လုပ်ဖို့ unseal keys တွေ လိုအပ်ပါတယ်။ Default အနေနဲ့ unseal keys ၅ ခုထုတ်ပေးပြီး အနည်းဆုံး ၃ ခုက threshold အဖြစ် လိုအပ်ပါတယ်။ ဒါက Shamir’s Secret Sharing algorithm ကို အသုံးပြုထားတာပါ။

### Lab: Sealed Vault တည်ဆောက်ခြင်း

Lab 3 မှာ sealed vault တစ်ခုကို configuration file တစ်ခုနဲ့ တည်ဆောက်ရပါတယ်။ အဓိကအဆင့်တွေကတော့ -

1. **Directory ဖန်တီးပါ**  
   Data သိမ်းဆည်းဖို့ directory တစ်ခုလိုအပ်ပါတယ်။
   ```bash
   mkdir -p vault/data
   ```

2. **Configuration File ဖန်တီးပါ**  
   `config.hcl` ဆိုတဲ့ HCL file တစ်ခုကို ဖန်တီးပြီး အောက်ပါအတိုင်း configure လုပ်ပါ။
   ```hcl
   ui = true
   disable_mlock = true
   storage "raft" {
     path = "./vault/data"
     node_id = "node1"
   }
   listener "tcp" {
     address = "10.0.2.52:8200"
     cluster_address = "10.0.2.52:8201"
     tls_disable = true
   }
   api_addr = "http://10.0.2.52:8200"
   cluster_addr = "http://10.0.2.52:8201"
   ```
   ဒီမှာ IP address ကို သင့် system ရဲ့ IP (ဥပမာ 10.0.2.52) နဲ့ ပြောင်းလဲပြီး အသုံးပြုပါ။

3. **Sealed Vault ကို Deploy လုပ်ပါ**  
   Configuration file ကို အသုံးပြုပြီး server ကို run ပါ။
   ```bash
   vault server --config=config.hcl
   ```
   ဒါက sealed vault ကို စတင်ပေးပါလိမ့်မယ်။

4. **Vault Address ကို Export လုပ်ပါ**  
   နောက်ထပ် terminal တစ်ခုမှာ -
   ```bash
    export VAULT_ADDR='http://10.0.2.52:8200'
   ```

5. **Vault ကို Initialize လုပ်ပါ**  
   Vault ကို initialize လုပ်ဖို့ -
   ```bash
   vault operator init
   ```
   ဒါက unseal keys ၅ ခုနဲ့ root token တစ်ခုကို ထုတ်ပေးပါလိမ့်မယ်။ ဒါတွေကို သေချာစွာ မှတ်သားထားပါ။

6. **Vault ကို Unseal လုပ်ပါ**  
   Unseal keys ၃ ခုကို အသုံးပြုပြီး vault ကို unseal လုပ်ပါ။
   ```bash
   vault operator unseal
   ```
   Unseal key တစ်ခုစီကို ထည့်သွင်းပြီး သုံးကြိမ် run ရပါမယ်။

7. **Root Token နဲ့ Login ဝင်ပါ**  
   ```bash
   vault login
   ```
   Root token ကို ထည့်သွင်းပြီး login ဝင်ပါ။

8. **Secrets Engine ဖန်တီးပါ**  
   Sealed vault မှာ default secrets engine မပါတာကြောင့် KV secrets engine တစ်ခုကို ဖန်တီးရပါတယ်။
   ```bash
   vault secrets enable kv
   ```

9. **Secret တစ်ခု ဖန်တီးပြီး ကြည့်ပါ**  
   ```bash
   vault kv put -mount=kv solar_system planet1=mercury
   vault kv get -mount=kv solar_system
   ```

10. **Vault ကို Seal ပြန်လုပ်ပါ**  
    ```bash
    vault operator seal
    ```
    ဒါက vault ကို ပြန်လည် seal လုပ်ပေးပါတယ်။ ဒါဆိုရင် `vault status` က sealed: true ဖြစ်နေတာကို တွေ့ရပါလိမ့်မယ်။

11. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး server ကို ရပ်ပါ။ ဒါပေမယ့် data တွေက `vault/data` directory ထဲမှာ persistent ဖြစ်နေပါလိမ့်မယ်။

**Exam Alert**: Vault က sealed ဖြစ်နေတဲ့အခါ API request လုပ်ရင် HTTP status code 503 ပြန်ပေးပါတယ်။ ဒါက vault က sealed ဖြစ်နေတယ်ဆိုတာကို ညွှန်ပြပါတယ်။

## ကျွန်တော့်အတွေ့အကြုံ

Vault dev server က testing အတွက် အလွန်အဆင်ပြေပါတယ်။ Command line ကနေ secrets တွေကို အလွယ်တကူ ဖန်တီးကြည့်လို့ရပြီး UI ကနေလည်း access လုပ်လို့ရတာ အဆင်ပြေပါတယ်။ သို့သော် လုံခြုံရေးအတွက် sealed vault ကသာ production မှာ အသုံးပြုဖို့ သင့်တော်ပါတယ်။ Shamir’s Secret Sharing နဲ့ unseal keys တွေကို ဖြန့်ဝေထားတာက လုံခြုံရေးအတွက် အလွန်အသုံးဝင်တဲ့ နည်းလမ်းတစ်ခုပါ။

ဒီ course ကနေ Vault ရဲ့ authentication နဲ့ tokens တွေရဲ့ အရေးပါမှုကို ပိုမိုနားလည်လာခဲ့ပါတယ်။ အထူးသဖြင့် root token ကို သတိထားပြီး အသုံးပြုဖို့ လိုအပ်ပြီး production မှာ အနည်းဆုံး permissions ပါတဲ့ tokens တွေကို အသုံးပြုသင့်ပါတယ်။

ဒီ blog post က Vault ကို စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။