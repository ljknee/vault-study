# HashiCorp Vault: Leases အကြောင်း

ဒီ blog post မှာ HashiCorp Vault ရဲ့ leases တွေအကြောင်းကို အသေးစိတ်ရှင်းပြပေးသွားမှာဖြစ်ပါတယ်။ Vault Basics course ထဲက အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ leases တွေရဲ့ အခြေခံသဘောတရား၊ ၎င်းတို့ကို secrets နဲ့ tokens တွေနဲ့ ဘယ်လိုပေါင်းစပ်အသုံးပြုရမလဲ၊ နဲ့ AWS secrets engine ကိုအသုံးပြုပြီး leases တွေနဲ့ လုပ်ဆောင်ရတဲ့ lab အကြောင်းကို မြန်မာဘာသာနဲ့ ရှင်းလင်းစွာ ဖော်ပြပေးသွားမှာပါ။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## Vault Leases ဆိုတာ ဘာလဲ?

Vault leases တွေကို နားလည်ဖို့ စီးပွားရေးလောကက ဥပမာတစ်ခုနဲ့ နှိုင်းယှဉ်ကြည့်ရအောင်။ စီးပွားရေးမှာ ကားတွေ၊ ပရိဘောဂတွေ၊ ရုံးခန်းတွေကို ငှားရမ်းလို့ရသလို Vault မှာလည်း secrets နဲ့ tokens တွေကို သတ်မှတ်ထားတဲ့ အချိန်တစ်ခုအထိ အသုံးပြုဖို့ "ငှားရမ်း" ပေးပါတယ်။ ဒါကို lease လို့ ခေါ်ပါတယ်။ Lease တစ်ခုက သင့်မှာ ပိုင်ဆိုင်မှု အပြည့်အဝမရှိဘဲ သတ်မှတ်ထားတဲ့ ကာလတစ်ခုအတွက်ပဲ အသုံးပြုခွင့်ရှိတာပါ။

Vault မှာ leases တွေက **ephemeral** ဖြစ်ပြီး သက်တမ်းတိုတိုပါ။ ဥပမာ 768 hours (တစ်လ)၊ 1 hour၊ ဒါမှမဟုတ် 10 minutes စသဖြင့်ပါ။ Leases တွေက secrets (ဥပမာ AWS access keys) နဲ့ tokens တွေနဲ့ ဆက်စပ်နေပါတယ်။

- **Secrets နဲ့ Leases**: Secret တစ်ခုနဲ့ ဆက်စပ်တဲ့ lease က အဲ့ဒီ secret တစ်ခုတည်းကိုပဲ ထိခိုက်ပါတယ်။
- **Tokens နဲ့ Leases**: Token တစ်ခုနဲ့ ဆက်စပ်တဲ့ lease က အဲ့ဒီ token နဲ့ ၎င်းက ဖန်တီးထားတဲ့ secrets အားလုံးကို ထိခိုက်ပါတယ်။ ဥပမာ token ရဲ့ lease ကို revoke လုပ်လိုက်ရင် အဲ့ဒီ token နဲ့ ဖန်တီးထားတဲ့ AWS access keys အားလုံး ဖျက်သိမ်းခံရမှာပါ။

Leases တွေကို စီမံခန့်ခွဲဖို့ အဓိက commands တွေကတော့:
- `vault lease lookup`: Lease ရဲ့ အချက်အလက်ကို ကြည့်ရှုဖို့။
- `vault lease renew`: Lease ကို သက်တမ်းတိုးဖို့။
- `vault lease revoke`: Lease ကို ဖျက်သိမ်းဖို့။
- `vault token` commands တွေကလည်း tokens တွေနဲ့ ဆက်စပ်တဲ့ leases တွေကို စီမံခန့်ခွဲဖို့ အသုံးပြုပါတယ်။

**Exam Alert**: Token တွေနဲ့ ဆက်စပ်တဲ့ leases တွေကို renew ဒါမှမဟုတ် revoke လုပ်ဖို့ `vault token` command ကို အသုံးပြုပါ။ Dynamic secrets တွေနဲ့ ဆက်စပ်တဲ့ leases တွေအတွက်ကတော့ `vault lease` command ကို သုံးပါ။

## Lab: Vault Leases နဲ့ လုပ်ဆောင်ခြင်း

Course ထဲက Lab 7 မှာ Vault development server တစ်ခုကို ဖန်တီးပြီး AWS secrets engine ကို အသုံးပြုကာ dynamic secrets (AWS IAM users နဲ့ credentials) တွေကို ဖန်တီးရပါတယ်။ ပြီးတော့ leases တွေကို renew, revoke လုပ်ပြီး AWS console မှာ စစ်ဆေးကြည့်ရပါတယ်။ ဒီလိုလုပ်ဆောင်ဖို့ AWS account တစ်ခုနဲ့ IAM user (administrative privileges ပါတဲ့) လိုအပ်ပါတယ်။ အဓိကအဆင့်တွေကို အောက်မှာ ဖော်ပြပေးလိုက်ပါတယ်။

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

3. **Vault Lease Help File ကို ကြည့်ရှုပါ**  
   ```bash
   vault lease -h
   ```
   Subcommands တွေဖြစ်တဲ့ `lookup`, `renew`, `revoke` တွေအကြောင်း လေ့လာပါ။

4. **AWS Console မှာ Access Key ဖန်တီးပါ**  
   - AWS Console ထဲဝင်ပြီး IAM user (administrative privileges ပါတဲ့) ကို ရွေးပါ။
   - **Security Credentials** > **Create Access Key** ကို နှိပ်ပါ။
   - Use case အနေနဲ့ **Command Line Interface (CLI)** ကို ရွေးပြီး access key နဲ့ secret access key ကို ဖန်တီးပါ။
   - ဒီ keys တွေကို CSV file အနေနဲ့ download လုပ်ပြီး လုံခြုံစွာ သိမ်းထားပါ။

   **သတိပေးချက်**: Secret access key ကို ဘယ်သူ့ကိုမှ မပြပါနဲ့။ ဒီ lab ပြီးတာနဲ့ keys တွေကို ဖျက်ပစ်ဖို့ အကြံပြုပါတယ်။

5. **AWS Credentials ကို Environment Variables အနေနဲ့ Export လုပ်ပါ**  
   ```bash
    export AWS_ACCESS_KEY_ID='<access_key>'
    export AWS_SECRET_ACCESS_KEY='<secret_access_key>'
   ```
   Space တစ်ချက်ထည့်ပြီး ရိုက်ပါ၊ ဒါမှ history ထဲမှာ မကျန်ရှိမှာပါ။

6. **AWS Secrets Engine ကို Enable လုပ်ပါ**  
   ```bash
   vault secrets enable aws
   ```
   Secrets engines တွေကို ကြည့်ရှုဖို့ -
   ```bash
   vault secrets list
   ```
   Output မှာ `aws/` path ကို တွေ့ရပါလိမ့်မယ်။

7. **Vault ထဲ AWS Credentials ထည့်ပါ**  
   ```bash
   vault write aws/config/root \
       access_key=$AWS_ACCESS_KEY_ID \
       secret_key=$AWS_SECRET_ACCESS_KEY \
       region=us-east-2
   ```
   Region ကို သင့် AWS region နဲ့ လိုက်ပြီး ပြောင်းလဲပါ (ဥပမာ `us-east-1`)။

8. **AWS Role ဖန်တီးပါ**  
   IAM users တွေကို dynamically ဖန်တီးဖို့ role တစ်ခု ဖန်တီးပါ။
   ```bash
   vault write aws/roles/my-role \
       credential_type=iam_user \
       policy_document=-<<EOF
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "ec2:*",
         "Resource": "*"
       }
     ]
   }
   EOF
   ```
   ဒီ role က EC2 resources တွေကို access လုပ်ခွင့်ပေးတဲ့ IAM users တွေကို ဖန်တီးပေးပါတယ်။

9. **Dynamic Credentials ဖန်တီးပါ**  
   ```bash
   vault read aws/creds/my-role
   ```
   Output မှာ lease ID, access key, secret access key တွေကို ပြပေးပါလိမ့်မယ်။ Lease duration က default အနေနဲ့ 768 hours (1 month) ဖြစ်ပါတယ်။

   AWS Console ထဲမှာ IAM > Users ကို သွားကြည့်ရင် `vault-token-my-role-<ID>` ဆိုတဲ့ user အသစ်တစ်ခုကို တွေ့ရပါလိမ့်မယ်။ ဒီ user မှာ access key တစ်ခု ဖန်တီးပြီးသားဖြစ်နေပါလိမ့်မယ်။

   နောက်ထပ် users တွေ ဖန်တီးဖို့ အထက်က command ကို ထပ်ခါတလဲလဲ run ပါ။ တစ်ကြိမ် run တိုင်း user အသစ်တစ်ခုနဲ့ access key အသစ်တစ်ခု ဖန်တီးပေးပါလိမ့်မယ်။

10. **Lease Duration ကို Modify လုပ်ပါ**  
    Lease တစ်ခုရဲ့ duration ကို ပြောင်းဖို့ lease ID ကို copy လုပ်ပြီး -
    ```bash
    vault lease renew -increment=3600 aws/creds/my-role/<lease_id>
    ```
    ဒါက lease duration ကို 1 hour (3600 seconds) ဖြစ်အောင် ပြောင်းပေးပါတယ်။

    Global maximum TTL (768 hours) ထက် ကျော်ဖို့ ကြိုးစားရင် warning ပေါ်ပြီး TTL ကို 768 hours မှာ cap လုပ်ပါလိမ့်မယ်။ ဥပမာ -
    ```bash
    vault lease renew -increment=1000h aws/creds/my-role/<lease_id>
    ```
    Output: `Warning: TTL exceeds maximum, capped at 768 hours`။

11. **Leases တွေကို View လုပ်ပါ**  
    Lease တစ်ခုရဲ့ အချက်အလက်ကို ကြည့်ရှုဖို့ -
    ```bash
    vault lease lookup aws/creds/my-role/<lease_id>
    ```
    Output မှာ issue time, expiry time, renewable status စတဲ့ အချက်အလက်တွေကို ပြပေးပါတယ်။

    Active leases တွေကို list လုပ်ဖို့ -
    ```bash
    vault list sys/leases/lookup/aws/creds/my-role
    ```
    Output မှာ lease IDs တွေကို ပြပေးပါတယ်။

12. **Leases တွေကို Revoke လုပ်ပါ**  
    Lease တစ်ခုကို revoke လုပ်ဖို့ -
    ```bash
    vault lease revoke aws/creds/my-role/<lease_id>
    ```
    ဒါက သက်ဆိုင်ရာ IAM user နဲ့ access key ကို AWS ထဲက ဖျက်ပစ်ပါတယ်။ AWS Console မှာ စစ်ဆေးရင် user ပျောက်သွားတာကို တွေ့ရပါလိမ့်မယ်။

    AWS secrets engine ထဲက leases အားလုံးကို revoke လုပ်ဖို့ -
    ```bash
    vault lease revoke -prefix aws/
    ```
    ဒါက IAM users အားလုံးနဲ့ ၎င်းတို့ရဲ့ access keys တွေကို ဖျက်ပစ်ပါတယ်။ AWS Console မှာ refresh လုပ်ကြည့်ရင် users အားလုံး ပျောက်သွားတာကို တွေ့ရပါလိမ့်မယ်။

13. **Server ကို Stop ပါ**  
    `Ctrl+C` နှိပ်ပြီး dev server ကို ရပ်ပါ။

    **သတိပေးချက်**: Lab ပြီးတာနဲ့ AWS Console ထဲမှာ IAM users နဲ့ access keys တွေ အားလုံး ဖျက်ပြီးသွားကြောင်း သေချာစစ်ဆေးပါ။ မဖျက်မိရင် security risks ဖြစ်ပေါ်နိုင်ပါတယ်။

## ကျွန်တော့်အတွေ့အကြုံ

Vault leases တွေကို လေ့လာရတာ အလွန်စိတ်ဝင်စားဖို့ ကောင်းပါတယ်။ AWS secrets engine ကို အသုံးပြုပြီး dynamic IAM users နဲ့ credentials တွေ ဖန်တီးကြည့်ရတာက Vault ရဲ့ စွမ်းရည်ကို ပြသပေးပါတယ်။ Leases တွေကို renew လုပ်တာ၊ revoke လုပ်တာနဲ့ AWS Console မှာ အချိန်နဲ့တပြေးညီ ပြောင်းလဲမှုတွေကို စစ်ဆေးကြည့်ရတာက secrets management ရဲ့ အရေးပါမှုကို နားလည်လာစေပါတယ်။ အထူးသဖြင့် lease revoke လုပ်လိုက်တဲ့အခါ AWS ထဲက users တွေ ချက်ချင်းပျောက်သွားတာက security အတွက် ဘယ်လောက်ထိရောက်တယ်ဆိုတာကို သက်သေပြပါတယ်။

Production မှာ ဒီလို dynamic secrets တွေကို အသုံးပြုတဲ့အခါ principle of least privilege ကို လိုက်နာပြီး secrets တွေရဲ့ leases တွေကို သေချာစီမံခန့်ခွဲဖို့ လိုအပ်ပါတယ်။ Secret access keys တွေကို လုံခြုံစွာ ကိုင်တွယ်ဖို့လည်း အမြဲသတိထားရမှာပါ။

ဒီ blog post က Vault leases တွေအကြောင်း စတင်လေ့လာနေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။