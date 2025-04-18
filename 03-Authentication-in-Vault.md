# HashiCorp Vault: Authentication Methods အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ authentication methods တွေအကြောင်းကို အသေးစိတ်ရှင်းပြပေးသွားမှာဖြစ်ပါတယ်။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ authentication ရဲ့ လုပ်ငန်းစဉ်၊ Vault မှာ အသုံးပြုနိုင်တဲ့ authentication methods တွေနဲ့ လက်တွေ့လုပ်ဆောင်ကြည့်ရတဲ့ lab တွေကို မြန်မာဘာသာနဲ့ ရှင်းလင်းစွာ ဖော်ပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Authentication ဆိုတာ ဘာလဲ?

Authentication ဆိုတာ client (လူတစ်ဦး၊ စနစ်တစ်ခု သို့မဟုတ် process တစ်ခု) ရဲ့ အထောက်အထားကို အတည်ပြုတဲ့ လုပ်ငန်းစဉ်တစ်ခုဖြစ်ပါတယ်။ Vault မှာဆိုရင် မှန်ကန်တဲ့ credentials မရှိဘဲ ဘယ်သူမှ Vault ထဲကို access လုပ်လို့မရပါဘူး။ Authentication ကို identification, authentication နဲ့ authorization ဆိုတဲ့ အဆင့်သုံးဆင့်နဲ့ ခွဲခြားနိုင်ပါတယ်။

1. **Identification**: Client ကို ဖော်ထုတ်တဲ့ အဆင့်ဖြစ်ပါတယ်။ ဥပမာ ID card သို့မဟုတ် credentials တွေနဲ့ ဖော်ထုတ်တာမျိုးပါ။
2. **Authentication**: Client ရဲ့ အထောက်အထားကို စနစ်တစ်ခုနဲ့ အတည်ပြုတဲ့ အဆင့်ဖြစ်ပါတယ်။
3. **Authorization**: Client ကို သတ်မှတ်ထားတဲ့ resources တွေကို access လုပ်ခွင့်ပေးတဲ့ အဆင့်ဖြစ်ပါတယ်။ ဒါက authentication ပြီးမှသာ ဖြစ်ပေါ်နိုင်ပါတယ်။

Vault မှာ authentication က tokens တွေပေါ်မှာ အဓိကမူတည်ပါတယ်။ Request တိုင်းနီးပါးမှာ token တစ်ခု ပါဝင်ရပါတယ်။ Tokens တွေက ephemeral ဖြစ်ပြီး သက်တမ်းတိုတို ဖြစ်တတ်ပါတယ်။ ဒါဟာ client ရဲ့ authentication နဲ့ authorization (policies နဲ့ capabilities) ပေါ်မှာ မူတည်ပြီး dynamically ထုတ်ပေးပါတယ်။

**Exam Alert**: Vault မှာ principle of least privilege ကို အမြဲလိုက်နာပါတယ်။ ဆိုလိုတာက users နဲ့ processes တွေကို သူတို့ရဲ့ လုပ်ငန်းဆောင်တာတွေအတွက် လိုအပ်တဲ့ privileges တွေကိုသာ ပေးအပ်ရပါတယ်။ ဒါဟာ security မှာ အလွန်အရေးကြီးတဲ့ အချက်တစ်ခုဖြစ်ပါတယ်။

## Vault ရဲ့ Authentication Methods

Vault မှာ လူတွေရော၊ စက်တွေပါ authenticate လုပ်နိုင်ဖို့ နည်းလမ်းပေါင်းများစွာ ရှိပါတယ်။ ဒီ authentication methods တွေကို enable လုပ်ဖို့ဒါမှမဟုတ် disable လုပ်ဖို့ `vault auth` command ကို အသုံးပြုပါတယ်။ ဒီမှာ အဓိက authentication methods တချို့ကို ဖော်ပြပေးလိုက်ပါတယ်။

- **Userpass**: Username နဲ့ password နဲ့ authenticate လုပ်တဲ့ method ဖြစ်ပါတယ်။ Vault ထဲမှာ built-in ပါဝင်ပါတယ်။
- **AppRole**: Machines ဒါမှမဟုတ် services တွေအတွက် အသုံးပြုတဲ့ method ဖြစ်ပါတယ်။ ဥပမာ တစ်ခုခုကို role တစ်ခုချပေးပြီး authenticate လုပ်တာမျိုးပါ။
- **AWS**: AWS လို external identity providers နဲ့ authenticate လုပ်နိုင်ပါတယ်။
- **GitHub, Kubernetes, Okta, LDAP**: ဒါတွေလည်း external providers တွေနဲ့ ချိတ်ဆက်ပြီး authenticate လုပ်နိုင်ပါတယ်။

**Exam Alert**: Authentication method တစ်ခုကို enable လုပ်ဖို့ `vault auth enable <method>` command ကို အသုံးပြုပါ။ ဥပမာ `vault auth enable userpass` ဆိုရင် username/password authentication ကို enable လုပ်ပေးပါတယ်။

## Lab: Authentication Methods နဲ့ လုပ်ဆောင်ခြင်း

Course ထဲက Lab 4 မှာ sealed vault တစ်ခုကို ဖန်တီးပြီး username/password authentication method ကို enable လုပ်ကာ user တစ်ဦးကို ဖန်တီးပြီး command line နဲ့ UI ကနေ login ဝင်ကြည့်ရပါတယ်။ ဒီမှာ အဓိကအဆင့်တွေကို ဖော်ပြပေးလိုက်ပါတယ်။

1. **Directory ဖန်တီးပါ**  
   Vault data သိမ်းဆည်းဖို့ directory တစ်ခု ဖန်တီးပါ။
   ```bash
   mkdir data
   ```

2. **Configuration File ဖန်တီးပါ**  
   `vault-auth-config.hcl` file ကို အသုံးပြုပြီး configuration ပြင်ဆင်ပါ။ ဒီမှာ local loopback IP (127.0.0.1) ကို အသုံးပြုထားပါတယ်။
   ```hcl
   ui = true
   disable_mlock = true
   storage "raft" {
     path = "./data"
     node_id = "node1"
   }
   listener "tcp" {
     address = "127.0.0.1:8200"
     tls_disable = "true"
   }
   api_addr = "http://127.0.0.1:8200"
   cluster_addr = "https://127.0.0.1:8201"
   ```

3. **Sealed Vault ကို Deploy လုပ်ပါ**  
   ```bash
   vault server --config=vault-auth-config.hcl
   ```

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
   Unseal keys ၅ ခုနဲ့ root token ကို သိမ်းဆည်းထားပါ။ Unseal လုပ်ဖို့ key ၃ ခုကို အသုံးပြုပါ။
   ```bash
   vault operator unseal
   ```

6. **Root Token နဲ့ Login ဝင်ပါ**  
   ```bash
   vault login
   ```
   Root token ကို ထည့်သွင်းပါ။

7. **Userpass Authentication ကို Enable လုပ်ပါ**  
   ```bash
   vault auth enable userpass
   ```
   Authentication methods တွေကို ကြည့်ရှုဖို့ -
   ```bash
   vault auth list
   ```

8. **User တစ်ဦး ဖန်တီးပါ**  
   Test user တစ်ဦးကို ဖန်တီးဖို့ -
   ```bash
   vault write auth/userpass/users/test_user password=test123
   ```

9. **User အကြောင်း အချက်အလက်ကို ကြည့်ရှုပါ**  
   ```bash
   vault read auth/userpass/users/test_user
   ```

10. **Command Line ကနေ User နဲ့ Login ဝင်ပါ**  
    ```bash
    vault login -method=userpass username=test_user password=test123
    ```
    ဒါက token-based authentication အစား username/password နဲ့ login ဝင်ပေးပါတယ်။ Output မှာ token duration က 768 hours (တစ်လ) ဖြစ်ပြီး default policy နဲ့ လာတာကို တွေ့ရပါတယ်။ Default policy က အလွန်ကန့်သတ်ထားတာကြောင့် ဒီ user က ဘာမှ လုပ်လို့မရပါဘူး။

11. **UI ကနေ Login ဝင်ပါ**  
    Browser မှာ `http://127.0.0.1:8200` ကို သွားပြီး "Username" method ကို ရွေးပါ။ `test_user` နဲ့ `test123` ထည့်ပြီး login ဝင်ပါ။ UI ထဲမှာ cubbyhole secrets engine ကိုသာ access လုပ်လို့ရပြီး တခြား ဘာမှ လုပ်လို့မရပါဘူး။ ဥပမာ secrets engine တစ်ခု enable လုပ်ဖို့ ကြိုးစားရင် code 403 (permission denied) error ပေါ်ပါလိမ့်မယ်။

12. **API ကနေ User အချက်အလက် ကြည့်ရှုပါ**  
    CURL command နဲ့ API access လုပ်ဖို့ -
    ```bash
    curl --request POST --data '{"password":"test123"}' http://127.0.0.1:8200/v1/auth/userpass/login/test_user | jq
    ```
    ဒါက JSON format နဲ့ user ရဲ့ အချက်အလက်တွေကို ပြပေးပါတယ်။

13. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး server ကို ရပ်ပါ။

**Exam Alert**: Userpass authentication method နဲ့ user တစ်ဦးရဲ့ အချက်အလက်ကို ကြည့်ရှုဖို့ `vault read auth/userpass/users/<username>` command ကို အသုံးပြုပါ။ Authentication methods တွေရဲ့ path က `auth/` နဲ့ စတင်ပါတယ်။

**Exam Alert**: Default policy နဲ့ user တွေက permission အနည်းဆုံးပဲ ရှိပါတယ်။ ဒါကြောင့် `vault auth list` လို command တွေ run ရင် code 403 (permission denied) error ပေါ်နိုင်ပါတယ်။

## ကျွန်တော့်အတွေ့အကြုံ

Vault ရဲ့ authentication methods တွေကို လေ့လာရတာ အလွန်စိတ်ဝင်စားဖို့ ကောင်းပါတယ်။ Userpass authentication ကို enable လုပ်ပြီး user တစ်ဦးကို ဖန်တီးကြည့်ရတာ လက်တွေ့မှာ ဘယ်လို အလုပ်လုပ်တယ်ဆိုတာကို ပိုမိုနားလည်လာစေပါတယ်။ အထူးသဖြင့် default policy က permission အနည်းဆုံးပဲ ပေးထားတာကြောင့် security အတွက် ဘယ်လောက်အရေးကြီးတယ်ဆိုတာကို သဘောပေါက်ခဲ့ပါတယ်။ API access နဲ့ CURL command ကို အသုံးပြုကြည့်ရတာကလည်း programmatic access ရဲ့ အရေးပါမှုကို ပြသပေးပါတယ်။

Vault ကို production မှာ အသုံးပြုတဲ့အခါ root token ကို အနည်းဆုံးသုံးပြီး သီးခြား policies နဲ့ users တွေကို ဖန်တီးသင့်ပါတယ်။ ဒါမှ principle of least privilege ကို လိုက်နာနိုင်မှာဖြစ်ပါတယ်။

ဒီ blog post က Vault ရဲ့ authentication အကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။