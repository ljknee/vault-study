# HashiCorp Vault: Policies အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ policies တွေအကြောင်းကို အသေးစိတ်ရှင်းပြပေးသွားမှာဖြစ်ပါတယ်။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ policies တွေရဲ့ အခြေခံသဘောတရား၊ ၎င်းတို့ကို ဘယ်လိုဖန်တီးပြီး Vault server ထဲ ထည့်သွင်းရမလဲ၊ နဲ့ policies တွေနဲ့ လုပ်ဆောင်ရတဲ့ lab အကြောင်းကို မြန်မာဘာသာနဲ့ ရှင်းလင်းစွာ ဖော်ပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Vault Policies ဆိုတာ ဘာလဲ?

Vault မှာ policies တွေက operations နဲ့ paths တွေကို access လုပ်ခွင့်ပေးတာ ဒါမှမဟုတ် ပိတ်ပင်တာကို ထိန်းချုပ်ပေးပါတယ်။ Policies ထဲမှာ permissions (Vault မှာ capabilities လို့ခေါ်တယ်) တွေပါဝင်ပြီး ဒါတွေက path-based ဖြစ်ပါတယ်။ ဥပမာ `secret/*` ဆိုတဲ့ path မှာ `create`, `read`, `delete` စတဲ့ capabilities တွေကို သတ်မှတ်ပေးနိုင်ပါတယ်။ Asterisk (*) က wildcard ဖြစ်ပြီး secrets engine ထဲက secrets အားလုံးကို ရည်ညွှန်းပါတယ်။

Vault မှာ policies တွေက **implicit deny by default** ဖြစ်ပါတယ်။ ဆိုလိုတာက policy တစ်ခုထဲမှာ ပြတ်သားစွာ ခွင့်ပြုမထားတဲ့ ဘာမဆို access လုပ်လို့မရပါဘူး။ ဒါကြောင့် new user တစ်ယောက်က default policy နဲ့ဆိုရင် ဘာမှလုပ်လို့မရပါဘူး။ Policies တွေကို အသုံးပြုဖို့ဆိုရင် Vault server က running ဖြစ်နေရမယ်၊ authentication method တစ်ခုခု enable လုပ်ထားရမယ် (ဥပမာ userpass, AWS, GitHub)။

**Exam Alert**: Vault မှာ built-in policies နှစ်ခုရှိပါတယ် - **default** နဲ့ **root**။ ဒီနှစ်ခုစလုံးကို remove လုပ်လို့မရပါဘူး။ Default policy ကို modify လုပ်လို့ရပေမယ့် root policy ကို modify လုပ်လို့မရပါဘူး။ Root token ကို `vault token revoke` command နဲ့ remove လုပ်နိုင်ပါတယ်။

### Policy Workflow

1. **Vault Server ဖန်တီးပါ**: Vault server တစ်ခုကို အရင်ဖန်တီးရပါတယ်။
2. **Authentication Backend နဲ့ ချိတ်ဆက်ပါ**: Administrator ဒါမှမဟုတ် security team က authentication backend (ဥပမာ LDAP, AWS, GitHub) ကို ချိတ်ဆက်ပေးရပါတယ်။
3. **Policy ကို Author လုပ်ပါ**: Policy ကို ရေးဆွဲပြီး development server မှာ စမ်းသပ်ပါ။
4. **Policy ကို Vault ထဲ Upload လုပ်ပါ**: `vault policy write` command နဲ့ policy ကို Vault server ထဲ ထည့်သွင်းပါ။
5. **Authentication Data ကို Policy နဲ့ Map လုပ်ပါ**: Users, groups ဒါမှမဟုတ် organizational units တွေကို policy နဲ့ ချိတ်ဆက်ပေးပါ။

### Built-in Policies

- **Default Policy**: ဒါက tokens အားလုံးမှာ default အနေနဲ့ ပါဝင်ပါတယ်။ Cubbyhole secrets engine ကို access လုပ်ခွင့်ပေးပြီး token အကြောင်း data ကို lookup လုပ်ခွင့်ပေးပါတယ်။ ဒါပေမယ့် တခြား ဘာမှ လုပ်လို့မရပါဘူး။ Default policy ကို modify လုပ်လို့ရပေမယ့် remove လုပ်လို့မရပါဘူး။
- **Root Policy**: Root user နဲ့ ဆက်စပ်နေပြီး ဘာမဆို လုပ်ခွင့်ပေးပါတယ်။ ဒါကို modify လုပ်လို့မရသလို remove လည်း လုပ်လို့မရပါဘူး။ Production မှာ root token ကို setup ပြီးတာနဲ့ revoke လုပ်သင့်ပါတယ်။

## Lab: Vault Policies နဲ့ လုပ်ဆောင်ခြင်း

Course ထဲက Lab 5 မှာ Vault development server တစ်ခုကို ဖန်တီးပြီး policy တစ်ခုကို author လုပ်ကာ Vault server ထဲ upload လုပ်ရပါတယ်။ ပြီးတော့ policy နဲ့ ချိတ်ဆက်ထားတဲ့ token တစ်ခုကို ဖန်တီးပြီး ၎င်း token ရဲ့ capabilities တွေကို စစ်ဆေးကြည့်ရပါတယ်။ အဓိကအဆင့်တွေကို အောက်မှာ ဖော်ပြပေးလိုက်ပါတယ်။

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

4. **Policy ကို Review လုပ်ပါ**  
   `admin-policy.hcl` file ထဲမှာ အောက်ပါ paths နဲ့ capabilities တွေ ပါဝင်ပါတယ်။
   ```hcl
   path "sys/policies/acl/*" {
     capabilities = ["create", "read", "update", "delete", "list", "sudo"]
   }
   path "auth/*" {
     capabilities = ["create", "read", "update", "delete", "list", "sudo"]
   }
   path "sys/auth/*" {
     capabilities = ["create", "update", "delete", "sudo"]
   }
   path "sys/auth" {
     capabilities = ["read"]
   }
   path "secret/*" {
     capabilities = ["create", "read", "update", "delete", "list"]
   }
   path "sys/mounts/*" {
     capabilities = ["create", "read", "update", "delete", "list"]
   }
   ```
   ဒီ policy က authentication methods တွေကို စီမံခန့်ခွဲခွင့်၊ secrets တွေကို စီမံခွဲခွင့်ပေးပြီး `sys/auth` path မှာတော့ read-only access ပဲ ပေးထားပါတယ်။

5. **Policy ကို Vault ထဲ Upload လုပ်ပါ**  
   ```bash
   vault policy write admin admin-policy.hcl
   ```
   Policies တွေကို ကြည့်ရှုဖို့ -
   ```bash
   vault policy list
   ```
   Output မှာ `default`, `root`, နဲ့ `admin` ဆိုတဲ့ policies သုံးခုကို တွေ့ရပါလိမ့်မယ်။

6. **Policy ကို Read လုပ်ပါ**  
   Policy ရဲ့ အကြောင်းအရာကို ကြည့်ရှုဖို့ -
   ```bash
   vault policy read admin
   ```
   ဒါမှမဟုတ် path-based နည်းနဲ့ -
   ```bash
   vault read sys/policy/admin
   ```

7. **API ကနေ Policy ကို Access လုပ်ပါ**  
   ```bash
   curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/sys/policies/acl/admin | jq
   ```
   ဒါက JSON format နဲ့ policy ရဲ့ အချက်အလက်တွေကို ပြပေးပါတယ်။

8. **Policy နဲ့ Token ဖန်တီးပါ**  
   Admin policy နဲ့ token တစ်ခုဖန်တီးဖို့ -
   ```bash
   vault token create -policy=admin
   ```
   Token ကို environment variable အနေနဲ့ သိမ်းဆည်းဖို့ -
   ```bash
   export ADMIN_TOKEN=$(vault token create -policy="admin" -format=json | jq -r .auth.client_token)
   ```
   Token ရဲ့ အချက်အလက်ကို ကြည့်ရှုဖို့ -
   ```bash
   vault token lookup $ADMIN_TOKEN
   ```

9. **Token Capabilities ကို Check ပါ**  
   Token ရဲ့ permissions တွေကို စစ်ဆေးဖို့ -
   ```bash
   vault token capabilities $ADMIN_TOKEN sys/auth/apprule
   ```
   Output: `create, delete, sudo, update`  
   နောက်ထပ် path တစ်ခုအတွက် -
   ```bash
   vault token capabilities $ADMIN_TOKEN sys/auth
   ```
   Output: `read`  
   Authentication path အတွက် -
   ```bash
   vault token capabilities $ADMIN_TOKEN auth/*
   ```
   Output: `create, delete, list, read, sudo, update`  
   Identity path အတွက် -
   ```bash
   vault token capabilities $ADMIN_TOKEN identity/*
   ```
   Output: `deny` (policy ထဲမှာ identity path မပါဝင်လို့ implicit deny ဖြစ်တယ်)

10. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး dev server ကို ရပ်ပါ။

## ကျွန်တော့်အတွေ့အကြုံ

Vault policies တွေကို လေ့လာရတာ အလွန်အသုံးဝင်ပါတယ်။ Policy တစ်ခုကို author လုပ်ပြီး Vault server ထဲ upload လုပ်ကာ token နဲ့ ချိတ်ဆက်ကြည့်ရတာ permissions တွေကို ဘယ်လို granularly ထိန်းချုပ်လို့ရတယ်ဆိုတာကို ပြသပေးပါတယ်။ အထူးသဖြင့် implicit deny သဘောတရားက security အတွက် ဘယ်လောက်အရေးကြီးတယ်ဆိုတာကို နားလည်လာခဲ့ပါတယ်။ API access နဲ့ policy တွေကို ကြည့်ရတာကလည်း programmatic နည်းနဲ့ Vault ကို ဘယ်လို အလုပ်လုပ်စေလို့ရတယ်ဆိုတာကို သိရှိစေပါတယ်။

Vault ကို production မှာ အသုံးပြုတဲ့အခါ root token ကို တတ်နိုင်သမျှ မြန်မြန် revoke လုပ်ပြီး custom policies တွေနဲ့ users တွေကို စီမံခန့်ခွဲတာက အကောင်းဆုံးပါ။ ဒါမှ principle of least privilege ကို လိုက်နာနိုင်မှာဖြစ်ပါတယ်။

ဒီ blog post က Vault policies တွေအကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။