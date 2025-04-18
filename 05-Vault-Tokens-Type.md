# HashiCorp Vault: Tokens အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ tokens တွေအကြောင်းကို အသေးစိတ်ရှင်းပြပေးသွားမှာဖြစ်ပါတယ်။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ tokens တွေရဲ့ အခြေခံသဘောတရား၊ ၎င်းတို့ကို ဘယ်လိုဖန်တီးပြီး စီမံခန့်ခွဲရမလဲ၊ နဲ့ tokens တွေနဲ့ လုပ်ဆောင်ရတဲ့ lab အကြောင်းကို မြန်မာဘာသာနဲ့ ရှင်းလင်းစွာ ဖော်ပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Vault Tokens ဆိုတာ ဘာလဲ?

Tokens တွေက Vault မှာ authentication ရဲ့ အဓိကနည်းလမ်းဖြစ်ပါတယ်။ Vault ကို request လုပ်တဲ့ လိုလိုလားလား အားလုံးနီးပါးမှာ token တစ်ခု ပါဝင်ရပါတယ်။ Authentication က Vault တစ်ခုတည်းနဲ့ လုပ်တာပဲဖြစ်ဖြစ်၊ AWS ဒါမှမဟုတ် GitHub လို external ID providers တွေနဲ့ ပေါင်းပြီး လုပ်တာပဲဖြစ်ဖြစ်၊ client (လူတစ်ဦးရော စက်တစ်ခုရော) က properly authenticated ဖြစ်နေရင် token တစ်ခုကို statically ဒါမှမဟုတ် dynamically ထုတ်ပေးပါတယ်။

Vault မှာ token အမျိုးအစား နှစ်ခုရှိပါတယ်။

1. **Service Tokens**: 
   - Prefix က `hvs.` (HashiCorp Vault Service) နဲ့ စတင်ပါတယ်။
   - Renewal, revocation, child tokens ဖန်တီးနိုင်မှု စတဲ့ features အားလုံးကို support လုပ်ပါတယ်။
   - Root tokens လည်း service tokens တွေပါပဲ။
   - Leases တွေကို အသုံးပြုပြီး token သက်တမ်းကုန်တဲ့အခါ revoke လုပ်ပါတယ်။
   - Storage အတွက် heavy weight ဖြစ်ပါတယ်။

2. **Batch Tokens**: 
   - Prefix က `hvb.` (HashiCorp Vault Batch) နဲ့ စတင်ပါတယ်။
   - Service tokens လောက် features မရှိပါဘူး၊ ဒါပေမယ့် internal Vault actions နဲ့ Vault clusters တွေမှာ အသုံးပြုနိုင်ပါတယ်။
   - Lightweight ဖြစ်ပြီး tracking မလုပ်ပါဘူး။

**Token Hierarchy**:
- Tokens တွေကို လက်ရှိ token holder က ဖန်တီးပါတယ်။
- Default အနေနဲ့ new tokens တွေက parent token ရဲ့ **child tokens** တွေဖြစ်လာပြီး parent ရဲ့ policies တွေကို inherit လုပ်ပါတယ်။ Parent token ကို revoke လုပ်ရင် child tokens တွေလည်း အလိုအလျောက် revoke ဖြစ်သွားပါတယ်။
- **Orphan Tokens**: Parent မရှိတဲ့ tokens တွေဖြစ်ပြီး သီးခြား token tree တစ်ခုရဲ့ root ဖြစ်လာပါတယ်။
- **Periodic Tokens**: Long-running systems ဒါမှမဟုတ် services တွေအတွက် အသုံးပြုပြီး သက်တမ်းမကုန်တဲ့ tokens တွေပါ။

**Exam Alert**: Root tokens တွေက service tokens ဖြစ်ပြီး `hvs.` prefix နဲ့ စတင်ပါတယ်။ Tokens တွေကို create, renew, revoke လုပ်ဖို့ `vault token` command ကို အသုံးပြုပါ။

## Lab: Vault Tokens နဲ့ လုပ်ဆောင်ခြင်း

Course ထဲက Lab 6 မှာ Vault development server တစ်ခုကို ဖန်တီးပြီး tokens တွေကို create, view, revoke လုပ်ကာ use limits နဲ့ time to live (TTL) တွေကို စမ်းသပ်ကြည့်ရပါတယ်။ အဓိကအဆင့်တွေကို အောက်မှာ ဖော်ပြပေးလိုက်ပါတယ်။

1. **Vault Development Server ကို Run ပါ**  
   Terminal တစ်ခုမှာ -
   ```bash
   vault server -dev
   ```
   Root token နဲ့ unseal key ကို သိမ်းဆည်းထားပါ။

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

4. **Vault Token Help File ကို ကြည့်ရှုပါ**  
   ```bash
   vault token -h
   ```
   Subcommands တွေဖြစ်တဲ့ `create`, `revoke`, `renew`, `lookup`, `capabilities` တွေအကြောင်း လေ့လာပါ။ Specific subcommand တစ်ခုအကြောင်း သိချင်ရင် ဥပမာ `vault token create -h` လို့ ရိုက်ကြည့်ပါ။

5. **Basic Token ဖန်တီးပြီး Revoke လုပ်ပါ**  
   Token တစ်ခုဖန်တီးဖို့ -
   ```bash
   vault token create
   ```
   ဒါက root policy နဲ့ child root token တစ်ခုကို ဖန်တီးပေးပါတယ်။ Token ID ကို copy လုပ်ပြီး revoke လုပ်ဖို့ -
   ```bash
   vault token revoke <token_id>
   ```

6. **Use Limit နဲ့ TTL ပါတဲ့ Token ဖန်တီးပါ**  
   Default policy နဲ့ token တစ်ခုဖန်တီးဖို့ -
   ```bash
   vault token create -ttl=1h -use-limit=3 -policy=default
   ```
   Token ID ကို environment variable အနေနဲ့ သိမ်းဆည်းဖို့ -
   ```bash
   export LIMITED_TOKEN=<token_id>
   ```
   Token အချက်အလက်ကို ကြည့်ရှုဖို့ -
   ```bash
   vault token lookup $LIMITED_TOKEN
   ```
   Output မှာ `num_uses: 3` နဲ့ `ttl: 3600s` (1 hour) ကို တွေ့ရပါလိမ့်မယ်။

7. **Token ကို Use လုပ်ပြီး Limit ကို စမ်းသပ်ပါ**  
   Limited token အနေနဲ့ အလုပ်လုပ်ဖို့ environment variable ကို ပြောင်းပါ -
   ```bash
   export VAULT_TOKEN=$LIMITED_TOKEN
   ```
   Token အချက်အလက်ကို lookup လုပ်ဖို့ -
   ```bash
   vault token lookup
   ```
   ဒါက token ရဲ့ use တစ်ခါ အသုံးပြုလိုက်တာပါ။ Output မှာ `num_uses: 2` ဖြစ်သွားပါလိမ့်မယ်။ နောက်ထပ် နှစ်ကြိမ် run လိုက်ရင် `num_uses: 0` ဖြစ်သွားပြီး token က automatically revoked ဖြစ်သွားပါလိမ့်မယ်။ ဒါကို အတည်ပြုဖို့ -
   ```bash
   vault token lookup $LIMITED_TOKEN
   ```
   Output: `Error: code 403, bad token` (token မရှိတော့လို့)။

8. **TTL နဲ့ Token Renew လုပ်ပါ**  
   နောက်ထပ် token တစ်ခုဖန်တီးဖို့ -
   ```bash
   vault token create -ttl=1h -policy=default
   ```
   Token ID ကို copy လုပ်ပြီး lookup လုပ်ဖို့ -
   ```bash
   vault token lookup <token_id>
   ```
   Token ကို renew လုပ်ဖို့ -
   ```bash
   vault token renew <token_id>
   ```
   ပြီးရင် lookup ပြန်လုပ်ကြည့်ရင် TTL က 1 hour ပြန်စတင်တာကို တွေ့ရပါလိမ့်မယ်။

9. **Token Accessor နဲ့ အချက်အလက်ကြည့်ရှုပါ**  
   Accessors တွေကို list လုပ်ဖို့ -
   ```bash
   vault list auth/token/accessors
   ```
   ဒါက current tokens တွေရဲ့ accessor IDs တွေကို ပြပေးပါတယ်။ Specific accessor နဲ့ token အချက်အလက်ကို ကြည့်ရှုဖို့ accessor ID ကို သုံးနိုင်ပါတယ်။

10. **Root Token ကို Revoke လုပ်ပါ**  
    Root token ကို revoke လုပ်ဖို့ -
    ```bash
    vault token revoke $VAULT_TOKEN
    ```
    ဒါဆိုရင် Vault ထဲ access လုပ်လို့ မရတော့ပါဘူး။ ဥပမာ -
    ```bash
    vault token create
    ```
    Output: `Error: code 403, permission denied`။

    **သတိပေးချက်**: Root token ကို revoke လုပ်လိုက်ရင် Vault ထဲ လုံးဝ access မရတော့ပါဘူး။ Snapshot မရှိရင် ပြန်လည်ရယူလို့ မရတော့ပါဘူး။

11. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး dev server ကို ရပ်ပါ။

**Exam Alert**: Token တွေကို create, renew, revoke လုပ်ဖို့ `vault token create`, `vault token renew`, `vault token revoke` commands တွေကို အသုံးပြုပါ။ Root token ကို revoke လုပ်လိုက်ရင် Vault ထဲ access လုံးဝဆုံးရှုံးနိုင်လို့ သတိထားပါ။

## ကျွန်တော့်အတွေ့အကြုံ

Vault tokens တွေကို လေ့လာရတာ အလွန်အသုံးဝင်ပါတယ်။ Token တွေကို create, revoke, renew လုပ်ပြီး use limits နဲ့ TTL တွေကို စမ်းသပ်ကြည့်ရတာက authentication နဲ့ authorization ရဲ့ လုပ်ဆောင်ပုံကို ပိုနားလည်လာစေပါတယ်။ အထူးသဖြင့် root token ကို revoke လုပ်လိုက်တဲ့အခါ access လုံးဝဆုံးရှုံးသွားတာက security အတွက် ဘယ်လောက်သတိထားရမလဲဆိုတာကို သင်ခန်းစာပေးပါတယ်။ Principle of least privilege ကို လိုက်နာဖို့အတွက် limited tokens တွေနဲ့ policies တွေကို သေချာစီမံခန့်ခွဲဖို့ လိုအပ်တာကို သဘောပေါက်ခဲ့ပါတယ်။

ဒီ blog post က Vault tokens တွေအကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။