# HashiCorp Vault: နိဒါန်းနှင့် Vault Associate Certification

ဒီ blog post မှာ HashiCorp Vault ရဲ့နိဒါန်းနဲ့ HashiCorp Certified Vault Associate (002) စာမေးပွဲအကြောင်းကို ဆွေးနွေးသွားမှာဖြစ်ပါတယ်။ Vault Basics course ရဲ့ပထမဆုံး lesson အခန်းကနေ အဓိကအကြောင်းအရာတွေဖြစ်တဲ့ Vault ရဲ့အခြေခံသဘောတရား၊ secrets management ရဲ့အရေးပါမှု၊ နဲ့ certification ရဲ့အကျိုးကျေးဇူးတွေကို မြန်မာလိုရှင်းပြသွားမှာပါ။ နည်းပညာဆိုင်ရာ term တွေကိုတော့ မူရင်းအတိုင်းထားပါမယ်။

## HashiCorp Vault ဆိုတာ ဘာလဲ?

**HashiCorp Vault** ဆိုတာ HashiCorp ရဲ့ **secrets management tool** တစ်ခုပါ။ Secrets တွေကို လုံခြုံစွာသိမ်းဆည်းပြီး စီမံခန့်ခွဲဖို့ ဒီဇိုင်းထုတ်ထားတဲ့ပရိုဂရမ်တစ်ခုဖြစ်ပါတယ်။

### Secrets ဆိုတာ ဘာလဲ?
**Secrets** ဆိုတာ စနစ်တစ်ခုကို **authentication** ဒါမှမဟုတ် **authorization** ပေးနိုင်တဲ့ လျှို့ဝှက်အချက်အလက်တွေပါ။ ဥပမာ:
- Usernames နဲ့ Passwords
- API keys
- SSH keys
- RSA tokens
- One-time passwords (OTPs)
- Certificates

### Secrets Management ရဲ့ပြဿနာ
အရင်က secrets တွေဟာ ဖိုင်တွေ၊ databases၊ version control systems (ဥပမာ Git) ထဲမှာ **plain text** အနေနဲ့ ဖရိုဖရဲသိမ်းထားတတ်ပါတယ်။ ဒါက **secret sprawl** လို့ခေါ်တဲ့ပြဿနာဖြစ်ပြီး လုံခြုံရေးအတွက် အန္တရာယ်ရှိပါတယ်။ Vault က ဒီပြဿနာကို ဖြေရှင်းပေးပါတယ်။

### Vault ရဲ့အဓိကရည်ရွယ်ချက်နဲ့ ဖြေရှင်းနည်းတွေ
Vault ရဲ့အဓိကရည်ရွယ်ချက်က **plain text secrets** တွေကို ဗဟိုချုပ်ကိုင်ထားတဲ့စနစ်တစ်ခုထဲ ပြောင်းရွှေ့ပြီး တင်းကြပ်တဲ့ access control နဲ့ စီမံခန့်ခွဲဖို့ပါ။ ဖြေရှင်းနည်းတွေက:
- **Centrality**: Secrets တွေကို တစ်နေရာတည်းမှာ စုစည်းသိမ်းဆည်းပါတယ်။
- **Dynamic Ephemeral Keys**: တသမတ်တည်းမဟုတ်တဲ့ dynamic keys နဲ့ သက်တမ်းတိုတို ephemeral keys တွေကို အသုံးပြုပြီး လုံခြုံရေးကို မြှင့်တင်ပါတယ်။
- **Strong Cryptography**: AES-256 လို encryption techniques တွေနဲ့ key management ကို ခိုင်မာစွာ လုပ်ဆောင်ပါတယ်။

Vault က secrets တွေကို **secure နဲ့ organized** ဖြစ်အောင် သိမ်းဆည်းပေးပြီး secret sprawl ကို လျှော့ချပေးပါတယ်။

## HashiCorp Certified Vault Associate (002) ဆိုတာ ဘာလဲ?

**HashiCorp Certified Vault Associate (002)** ဆိုတာ Vault ပရိုဂရမ်ကို ကောင်းစွာအလုပ်လုပ်နိုင်တဲ့ အသိပညာရှိမှန်း သက်သေပြတဲ့ certification တစ်ခုပါ။ ဒီ certification ဟာ သင့်ရဲ့ resume/CV ကို မြှင့်တင်ပေးပြီး IT နယ်ပယ်မှာ အလုပ်အကိုင်အခွင့်အလမ်းတွေကို တိုးစေနိုင်ပါတယ်။

### စာမေးပွဲအချက်အလက်များ
- **Exam Name**: HashiCorp Certified Vault Associate (002)
- **Format**: Online proctored (အိမ်ကနေ ကွန်ပျူတာနဲ့ ဖြေဆိုရတယ်)
- **Question Types**: Multiple choice နဲ့ True/False (အများစုက multiple choice)
- **Number of Questions**: 50-60 ခု
- **Passing Score**: 70%
- **Duration**: 1 နာရီ
- **Cost**: $70.50 (ဗီဒီယိုရိုက်ကူးချိန်အရ)
- **Language**: အင်္ဂလိပ်
- **Validity**: 2 နှစ်
- **Vault Version**: 1.6 နဲ့ အထက်

**မှတ်ချက်**: Certification သက်တမ်း ၂ နှစ်ပြည့်ရင် အောက်က ရွေးချယ်စရာတွေ ရှိပါတယ်:
- စာမေးပွဲကို ပြန်ဖြေပြီး recertify လုပ်နိုင်တယ်။
- Higher-level Vault exam (ဥပမာ Vault Operations Professional) ကို ဖြေပြီး Vault Associate ကို အလိုအလျောက် recertify လုပ်နိုင်တယ်။
- Certification ကို lapse ဖြစ်အောင် ထားနိုင်တယ် (ဆက်မလုပ်တော့ဘူး)။

### စာမေးပွဲအတွက် Exam Alerts
စာမေးပွဲဖြေဆိုဖို့ ပြင်ဆင်တဲ့အခါ အောက်က အကြံပြုချက်တွေကို လိုက်နာပါ:
1. **Test Area ကို ကြိုတင်ပြင်ဆင်ပါ**:
   - စာမေးပွဲမဖြေခင် အနည်းဆုံး ၁ ရက် (ဒါမှမဟုတ် ၂ ရက်) အလိုမှာ စာမေးပွဲဧရိယာကို ပြင်ဆင်ထားပါ။
   - ကွန်ပျူတာ၊ webcam (external webcam က ပိုကောင်းတယ်)၊ နဲ့ internet connection ကို စစ်ဆေးပါ။
   - စာရွက်စာတမ်းတွေ ဒါမှမဟုတ် မလိုအပ်တဲ့ ပစ္စည်းတွေ မရှိအောင် သန့်ရှင်းထားပါ။

2. **စောစောရောက်ပါ**:
   - စာမေးပွဲစဖို့ ၃၀ မိနစ်အလိုမှာ log in ဝင်ပါ။
   - ID verification, webcam scanning, နဲ့ system setup အတွက် အချိန်ယူပါ။

3. **External Webcam ကို စဉ်းစားပါ**:
   - External webcam က စာမေးပွဲဧရိယာကို scan လုပ်တဲ့အခါ ပိုအဆင်ပြေစေပါတယ်။

4. **အချိန်ကို ထိရောက်စွာ အသုံးပြုပါ**:
   - စာမေးပွဲမှာ အချိန်ကျန်ရင် အားလုံးကို ပြန်စစ်ဆေးပါ။ အထူးသဖြင့် မသေချာတဲ့ မေးခွန်းတွေကို **mark/flag** လုပ်ထားပြီး ပြန်ကြည့်ပါ။
   - အားလုံး ဖြေပြီးကြောင်း သေချာအောင် စစ်ဆေးပါ။

**အကြံပြုချက်**: စာမေးပွဲအတွက် အဓိကအချက်အလက်တွေကို HashiCorp ရဲ့ [Vault Associate Certification page](https://www.hashicorp.com/certifications/vault-associate) မှာ ကြည့်ပါ။ ဒီစာမေးပွဲဟာ **PSI Online** ကတစ်ဆင့် ဖြေဆိုရတာဖြစ်လို့ PSI ရဲ့ system requirements နဲ့ exam guidelines တွေကို ကြိုတင်ဖတ်ထားပါ။

## ကျွန်တော့်အတွေ့အကြုံ

HashiCorp Vault သင်တန်းရဲ့ နိဒါန်းအပိုင်းက Vault ရဲ့အဓိကရည်ရွယ်ချက်နဲ့ secrets management ရဲ့အရေးပါမှုကို နားလည်လာစေပါတယ်။ Secret sprawl လို လုံခြုံရေးပြဿနာတွေကို Vault က centrality, dynamic keys, နဲ့ strong cryptography တွေနဲ့ ဘယ်လိုဖြေရှင်းပေးတယ်ဆိုတာကို သိခဲ့ရပါတယ်။ 

Vault Associate Certification အကြောင်း လေ့လာရတာက စာမေးပွဲရဲ့ပုံစံနဲ့ ပြင်ဆင်နည်းတွေကို နားလည်ဖို့ အထောက်အကူဖြစ်ပါတယ်။ Exam Alerts တွေက စာမေးပွဲဖြေဆိုတဲ့အခါ အချိန်ကို ထိရောက်စွာအသုံးပြုဖို့ နဲ့ စိတ်ဖိစီးမှုကို လျှော့ချဖို့ အထူးအထောက်အကူဖြစ်ပါတယ်။ အထူးသဖြင့် test area ကို ကြိုတင်ပြင်ဆင်ထားတာ၊ စောစောရောက်တာ၊ နဲ့ external webcam အသုံးပြုတာတွေက စာမေးပွဲ process ကို ချောမွေ့စေပါတယ်။

ဒီ blog post က HashiCorp Vault ကို စတင်လေ့လာနေသူတွေနဲ့ Vault Associate Certification အတွက် ပြင်ဆင်နေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာအကြောင်းအရာတွေကို နောက်ထပ်လည်းဆက်လက်မျှဝေပေးသွားပါမယ်။