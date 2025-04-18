# HashiCorp Vault: UI နှင့် API အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ user interface (UI) နဲ့ application programmer's interface (API) အကြောင်းကို ဆွေးနွေးသွားမှာပါ။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ Vault UI ရဲ့ အလုပ်လုပ်ပုံ၊ API ကို curl commands နဲ့ အသုံးပြုပုံ၊ ပြီးတော့ UI နဲ့ API ကို အသုံးပြုပြီး secrets တွေနဲ့ users တွေကို စီမံခန့်ခွဲတဲ့ lab အကြောင်းကို မြန်မာဘာသာနဲ့ ရှင်းပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Vault UI နှင့် API ဆိုတာ ဘာလဲ?

Vault ကို အဓိကအားဖြင့် command-line interface (CLI) ကနေ အသုံးပြုကြပေမယ့် CLI တစ်ခုတည်းကိုပဲ အားကိုးနေစရာ မလိုပါဘူး။ Vault ကို အသုံးပြုဖို့ နည်းလမ်းနှစ်ခု ထပ်ရှိပါတယ် - **web-based user interface (UI)** နဲ့ **application programmer's interface (API)**။

1. **Vault UI**:
   - လူသားအသုံးပြုသူတွေအတွက် ဒီဇိုင်းထုတ်ထားတဲ့ browser-based interface တစ်ခုပါ။
   - Dev server မှာဆိုရင် `http://127.0.0.1:8200` ကို browser မှာ ရိုက်ထည့်ပြီး အသုံးပြုလို့ရပါတယ်။
   - Production မှာဆိုရင် TLS (Transport Layer Security) လိုအပ်တာကြောင့် `https://` နဲ့ real IP address (ဥပမာ `10.0.2.52`) ဒါမှမဟုတ် hostname နဲ့ ချိတ်ဆက်ရပါတယ်။
   - Point-and-click GUI style နဲ့ အလုပ်လုပ်လို့ရတာကြောင့် အသုံးပြုရတာ လွယ်ကူပါတယ်။
   - ဒါပေမယ့် UI က CLI ထက် functionality နည်းပါတယ်။ ဥပမာ identity နဲ့ system secrets engines တွေကို UI ထဲမှာ မမြင်ရပါဘူး။

2. **Vault API**:
   - စက်တွေ၊ processes တွေနဲ့ systems တွေအတွက် programmatic access ပေးဖို့ ဒီဇိုင်းထုတ်ထားပါတယ်။
   - CLI ကနေ `curl` command ကို အသုံးပြုပြီး API endpoints တွေကို အသုံးပြုလို့ရပါတယ်။
   - HTTP-based verbs တွေဖြစ်တဲ့ `GET` (read), `POST`/`PUT` (create), `DELETE` (delete) စတဲ့ verbs တွေကို အသုံးပြုပါတယ်။
   - JSON format နဲ့ data တွေကို ပြန်ပေးပါတယ်။
   - `curl` အပြင် Go, Python, Ruby, Java စတဲ့ programming languages တွေနဲ့လည်း API ကို အသုံးပြုလို့ရပါတယ်။

**Exam Alert**: Root user အတွက် Vault UI ထဲမှာ **policies** section ကို အသုံးပြုလို့ရပေမယ့် typical new user (default policy နဲ့) အတွက်ကတော့ policies section ကို မမြင်ရပါဘူး။ Default policy က implicit deny ဖြစ်တာကြောင့် အလွန်ကန့်သတ်ထားပါတယ်။

## Lab: Vault UI နှင့် API နဲ့ အလုပ်လုပ်ခြင်း

Course ထဲက Lab 10 မှာ Vault development server တစ်ခုကို ဖန်တီးပြီး UI နဲ့ API ကို အသုံးပြုပြီး users တွေ၊ secrets တွေကို စီမံခန့်ခွဲတာကို လက်တွေ့လုပ်ဆောင်ကြည့်ရပါတယ်။ CLI, UI, နဲ့ API တွေရဲ့ ကွာခြားချက်တွေကိုလည်း နှိုင်းယှဉ်ကြည့်ရပါတယ်။ အဓိကအဆင့်တွေကို အောက်မှာ ဖော်ပြပေးလိုက်ပါတယ်။

1. **Vault Development Server ကို Run ပါ**  
   Terminal တစ်ခုမှာ -
   ```bash
   vault server -dev -dev-root-token-id=root
   ```
   Root token ID ကို `root` လို့ သတ်မှတ်ထားတာပါ။ ဒါက testing အတွက်ပဲ သင့်တော်ပြီး production မှာ မသုံးသင့်ပါဘူး။

2. **Vault Address နဲ့ Token ကို Export လုပ်ပါ**  
   နောက်ထပ် terminal နှစ်ခုမှာ -
   ```bash
    export VAULT_ADDR='http://127.0.0.1:8200'
    export VAULT_TOKEN='root'
   ```
   Space တစ်ချက်ထည့်ပြီး command တွေကို ရိုက်ပါ၊ ဒါမှ history ထဲမှာ မကျန်ရှိမှာပါ။

3. **UI ထဲမှာ Root Token နဲ့ Login ဝင်ပါ**  
   Browser မှာ `http://127.0.0.1:8200` ကို သွားပြီး token method ကို ရွေးပါ။ Root token (`root`) ကို ထည့်ပြီး login ဝင်ပါ။  
   UI ထဲမှာ secrets engines တွေဖြစ်တဲ့ `cubbyhole/` နဲ့ `secret/` (KV engine) တွေကို တွေ့ရပါလိမ့်မယ်။ `identity/` နဲ့ `sys/` engines တွေက UI ထဲမှာ မပြပါဘူး။

4. **UI ထဲမှာ Userpass Authentication Enable လုပ်ပါ**  
   - **Access** tab ကို သွားပြီး **Authentication Methods** ကို နှိပ်ပါ။
   - **Enable new method** ကို နှိပ်ပြီး **Username & Password (userpass)** ကို ရွေးပါ။
   - Default path က `userpass/` ဖြစ်ပြီး options တွေကို ပြင်ဆင်စရာမလိုပါဘူး။ **Enable Method** ကို နှိပ်ပါ။
   - Authentication methods ထဲမှာ `token` နဲ့အတူ `userpass` ကို တွေ့ရပါလိမ့်မယ်။

5. **UI ထဲမှာ New User ဖန်တီးပါ**  
   - `userpass` method ထဲဝင်ပြီး **Create user** ကို နှိပ်ပါ။
   - Username: `test_user1`
   - Password: `test123`
   - **Save** ကို နှိပ်ပါ။

6. **UI ထဲမှာ New User နဲ့ Login ဝင်ပါ**  
   - UI ထဲက user icon ကို နှိပ်ပြီး **Log out** လုပ်ပါ။
   - **Username** method ကို ရွေးပြီး `test_user1` နဲ့ `test123` ထည့်ပြီး login ဝင်ပါ။
   - UI ထဲမှာ `cubbyhole/` secrets engine တစ်ခုတည်းကိုပဲ တွေ့ရပါလိမ့်မယ်။ Default policy က အလွန်ကန့်သတ်ထားတာကြောင့် `secret/` engine တို့ policies section တို့ကို မမြင်ရပါဘူး။

7. **UI ထဲမှာ Cubbyhole Secret ဖန်တီးပါ**  
   - `cubbyhole/` engine ထဲဝင်ပြီး **Create secret** ကို နှိပ်ပါ။
   - Secret path: `test-secret`
   - Key/Value: `1=1`
   - **Save** ကို နှိပ်ပါ။
   - Cubbyhole က token-specific ဖြစ်တာကြောင့် ဒီ secret ကို ဒီ user ရဲ့ cubbyhole ထဲမှာပဲ သိမ်းထားပါတယ်။

8. **UI ထဲက Built-in CLI Console ကို အသုံးပြုပါ**  
   - UI ထဲမှာ terminal icon ကို နှိပ်ပြီး built-in CLI console ကို ဖွင့်ပါ။
   - Command တွေကို `vault` prefix မထည့်ပဲ ရိုက်လို့ရပါတယ်။ ဥပမာ -
     ```bash
     kv get cubbyhole/test-secret
     ```
     Output: `1=1` (key/value pair ကို ပြပါတယ်)။
   - `clear` command နဲ့ console ကို ရှင်းလို့ရပါတယ်။
   - Console ကလည်း user ရဲ့ permissions (default policy) အတိုင်း ကန့်သတ်ထားပါတယ်။

9. **CLI ထဲမှာ Test User နဲ့ Login ဝင်ပါ**  
   နောက်ထပ် terminal တစ်ခုမှာ -
   ```bash
   vault login -method=userpass username=test_user1
   ```
   Password အတွက် `test123` ထည့်ပါ။  
   Output မှာ new token ID နဲ့ default policy နဲ့ ချိတ်ဆက်ထားတာကို ပြပါတယ်။ Token duration က 768 hours (1 month) ဖြစ်ပါတယ်။

   Token ID ကို environment variable အနေနဲ့ သိမ်းဖို့ -
   ```bash
    export USER_TOKEN=<token_id>
   ```

10. **CLI ထဲမှာ User Permissions ကို စစ်ဆေးပါ**  
    Secrets engines တွေကို list လုပ်ဖို့ -
    ```bash
    VAULT_TOKEN=$USER_TOKEN vault secrets list
    ```
    Output: `Error: code 403, permission denied` (default policy က ကန့်သတ်ထားတာကြောင့်)။

    Vault status ကို ကြည့်ဖို့ -
    ```bash
    VAULT_TOKEN=$USER_TOKEN vault status
    ```
    Output: Status information (typical user တွေ status ကို ကြည့်လို့ရပါတယ်)။

    Cubbyhole secret ကို ကြည့်ဖို့ -
    ```bash
    VAULT_TOKEN=$USER_TOKEN vault kv get -mount=cubbyhole test-secret
    ```
    Output: `No value found` (UI ထဲမှာ ဖန်တီးထားတဲ့ cubbyhole secret က CLI ရဲ့ token နဲ့ မတူတာကြောင့် မမြင်ရပါဘူး)။

11. **API နဲ့ Secret ဖန်တီးပြီး ကြည့်ပါ**  
    Secret တစ်ခုဖန်တီးဖို့ curl command -
    ```bash
    curl -H "X-Vault-Token: $VAULT_TOKEN" \
          -H "Content-Type: application/json" \
          -X POST \
          -d '{"data":{"passwd":"secret"}}' \
          http://127.0.0.1:8200/v1/secret/data/secret1 | jq
    ```
    Output: JSON format နဲ့ lease ID, creation time စတဲ့ အချက်အလက်တွေကို ပြပါတယ်။

    Secret ကို ကြည့်ဖို့ -
    ```bash
    curl -H "X-Vault-Token: $VAULT_TOKEN" \
          -X GET \
          http://127.0.0.1:8200/v1/secret/data/secret1 | jq
    ```
    Output: `passwd: secret` (key/value pair ကို JSON format နဲ့ ပြပါတယ်)။

12. **API နဲ့ User ဖန်တီးပြီး Login ဝင်ပါ**  
    Payload JSON file တစ်ခုဖန်တီးဖို့ `payload.json` ဖိုင်ထဲမှာ -
    ```json
    {"password": "bad-password"}
    ```

    New user ဖန်တီးဖို့ -
    ```bash
    curl -H "X-Vault-Token: $VAULT_TOKEN" \
          -H "Content-Type: application/json" \
          -X POST \
          -d @payload.json \
          http://127.0.0.1:8200/v1/auth/userpass/users/test_user2
    ```

    User အချက်အလက်ကို ကြည့်ဖို့ -
    ```bash
    curl -H "X-Vault-Token: $VAULT_TOKEN" \
          -X GET \
          http://127.0.0.1:8200/v1/auth/userpass/users/test_user2 | jq
    ```
    Output: User `test_user2` ရဲ့ အချက်အလက်တွေကို JSON format နဲ့ ပြပါတယ်။ Default policy နဲ့ ချိတ်ဆက်ထားတာကို တွေ့ရပါတယ်။

    User နဲ့ login ဝင်ဖို့ -
    ```bash
    curl -H "Content-Type: application/json" \
          -X POST \
          -d @payload.json \
          http://127.0.0.1:8200/v1/auth/userpass/login/test_user2 | jq
    ```
    Output: New client token နဲ့ `test_user2` ရဲ့ login အချက်အလက်တွေကို ပြပါတယ်။

    Client token ကို environment variable အနေနဲ့ သိမ်းဖို့ -
    ```bash
    export USER_TOKEN=<client_token>
    ```

13. **API User ရဲ့ Permissions ကို စစ်ဆေးပါ**  
    Secrets engines ကို list လုပ်ဖို့ -
    ```bash
    VAULT_TOKEN=$USER_TOKEN vault secrets list
    ```
    Output: `Error: code 403, permission denied`။

    Cubbyhole secret ဖန်တီးဖို့ -
    ```bash
    VAULT_TOKEN=$USER_TOKEN vault kv put -mount=cubbyhole test-secret key=value
    ```

    Secret ကို ကြည့်ဖို့ -
    ```bash
    VAULT_TOKEN=$USER_TOKEN vault kv get -mount=cubbyhole test-secret
    ```
    Output: `key=value`။

    UI ထဲမှာ `test_user2` နဲ့ login ဝင်ပြီး cubbyhole ကို ကြည့်ရင် `test-secret` ကို မတွေ့ရပါဘူး၊ ဘာကြောင့်လဲဆိုတော့ CLI နဲ့ UI ရဲ့ tokens တွေက မတူတာကြောင့် cubbyhole တွေက သီးခြားဖြစ်နေလို့ပါ။

14. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး dev server ကို ရပ်ပါ။

## ကျွန်တော့်အတွေ့အကြုံ

Vault UI နဲ့ API တွေကို လေ့လာရတာ အလွန်အသုံးဝင်ပါတယ်။ UI က point-and-click နဲ့ အလုပ်လုပ်ရတာ လွယ်ကူပေမယ့် CLI ရဲ့ full functionality ကို မယှဉ်နိုင်တာကို တွေ့ရပါတယ်။ API ကို curl commands နဲ့ secrets တွေ၊ users တွေကို programmatically manage လုပ်တာက systems နဲ့ processes တွေအတွက် ဘယ်လို အဆင်ပြေတယ်ဆိုတာကို နားလည်လာခဲ့ပါတယ်။ Cubbyhole secrets engine ရဲ့ token-specific nature ကို ထပ်မံသိရှိခဲ့ပြီး ဒါက security အတွက် ဘယ်လို compartmentalized ဖြစ်တယ်ဆိုတာကို ပြသပေးပါတယ်။

Production မှာ UI ကို junior admins တွေအတွက် user management လိုမျိုးတွေမှာ အသုံးဝင်ပေမယ့် complex operations တွေအတွက် CLI နဲ့ API ကို ပိုပြီး အားကိုးရပါတယ်။ Principle of least privilege ကို လိုက်နာဖို့အတွက် users တွေကို default policy နဲ့ စတင်ပြီး လိုအပ်တဲ့ permissions တွေကို policies ထဲမှာ သတ်မှတ်ပေးဖို့ လိုအပ်ပါတယ်။

ဒီ blog post က Vault UI နဲ့ API အကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။