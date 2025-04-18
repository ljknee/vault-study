# HashiCorp Vault: ရွေးချယ်နိုင်သော Labs များနှင့် Configurations

ဒီ blog post မှာ HashiCorp Vault Basics course ထဲက ရွေးချယ်နိုင်သော labs တွေဖြစ်တဲ့ Vim နဲ့ VSCode modifications၊ sealed Vault setup၊ နဲ့ TLS ပါဝင်တဲ့ Vault server configuration တွေအကြောင်းကို ဆွေးနွေးပေးသွားမှာပါ။ ဒီအကြောင်းအရာတွေကို မြန်မာဘာသာနဲ့ သဘာဝကျကျ ရှင်းပြပေးသွားမှာဖြစ်ပြီး နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုတော့ မူရင်းအတိုင်း ထားရှိထားပါတယ်။

## 1. Opt 01 - Vim Modifications

**Vim** က code နဲ့ configuration files တွေကို တည်းဖြတ်တဲ့အခါ အသုံးများတဲ့ text editor တစ်ခုပါ။ ဒီ lab မှာ Vim မှာ **syntax highlighting** နဲ့ အခြား features တွေကို ထည့်သွင်းပြီး ပိုမိုအဆင်ပြေအောင် လုပ်ဆောင်နည်းကို လေ့လာခဲ့ပါတယ်။

### Syntax Highlighting with Vim Polyglot
**Vim Polyglot** က Vim မှာ syntax highlighting ပေးတဲ့ plugin တစ်ခုဖြစ်ပြီး HCL, JSON စတဲ့ file formats တွေကို ဖတ်ရလွယ်ကူစေပါတယ်။

1. **Vim Plug ကို Install လုပ်ပါ**  
   Vim Plug က plugin management အတွက် အသုံးပြုတဲ့ tool ဖြစ်ပါတယ်။
   ```bash
   curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
   ```

2. **.vimrc ကို Modify လုပ်ပါ**  
   အိမ်ရှိ `.vimrc` file မရှိရင် ဖန်တီးပါ။ အောက်က configuration ကို ထည့်ပါ:
   ```vim
   set nocompatible

   call plug#begin()

   Plug 'sheerun/vim-polyglot'

   call plug#end()
   ```

3. **Vim Polyglot ကို Install လုပ်ပါ**  
   Vim ထဲမှာ အောက်က command ကို ရိုက်ပါ:
   ```vim
   :PlugInstall vim-polyglot
   ```

4. **.vimrc ကို Reload လုပ်ပါ**  
   ```vim
   :vs ~/.vimrc
   ```

### Syntax Highlighting with HashiVim
**HashiVim** plugins (ဥပမာ `vim-vaultproject`, `vim-hashicorp-tools`) တွေက HashiCorp tools တွေအတွက် သီးသန့် syntax highlighting ပေးပါတယ်။

- **Installation**: Vim packages အနေနဲ့ ဒါမှမဟုတ် **vim-pathogen** နဲ့ install လုပ်နိုင်ပါတယ်။
  - Pathogen အတွက်: [vim-pathogen GitHub](https://github.com/tpope/vim-pathogen)

### အခြား Vim Modifications
`.vimrc` ထဲမှာ အောက်က settings တွေကို ထည့်နိုင်ပါတယ်:
- **Line Numbers**: `set number` (လိုင်းနံပါတ်တွေ ပြပေးတယ်)
- **Color Scheme**: `:color <colorscheme>` (ဥပမာ `desert`, `ron`, `zellner`)  
  - Available colorschemes တွေကို ကြည့်ဖို့ Vim ထဲမှာ `:color <Space> <Ctrl+D>` ရိုက်ပါ။

## 2. Opt 02 - VSCode Modifications

**Visual Studio Code (VSCode)** က developer တွေအတွက် ရေပန်းစားတဲ့ code editor တစ်ခုပါ။ ဒီ lab မှာ VSCode ရဲ့ functionality ကို ပိုမိုကောင်းမွန်စေမယ့် modifications တွေကို လေ့လာခဲ့ပါတယ်။

### Maximize/Restore Terminal Toggle
VSCode မှာ terminal ကို maximize/restore လုပ်ဖို့ built-in keyboard shortcut မရှိပါဘူး။ ဒါကို custom shortcut နဲ့ ဖြည့်စွမ်းနိုင်ပါတယ်:

1. **Keyboard Shortcuts ကို ဖွင့်ပါ**:
   - **File > Preferences > Keyboard Shortcuts** ကို သွားပါ (ဒါမှမဟုတ် `<Ctrl+K> <Ctrl+S>`)။
2. **Search for Command**:
   - Search bar မှာ `workbench.action.toggleMaximizedPanel` လို့ ရိုက်ပါ။
3. **Assign Shortcut**:
   - Command ကို double-click လုပ်ပြီး သင်နှစ်သက်တဲ့ shortcut (ဥပမာ `<Ctrl+Shift+Q>`) ကို ထည့်ပါ။
4. **အသုံးပြုပုံ**:
   - Shortcut ကို နှိပ်ရင် terminal က maximize ဖြစ်ပြီး ထပ်နှိပ်ရင် original size ကို ပြန်ရောက်ပါတယ်။

### Peacock Extension
**Peacock** extension က multiple VSCode workspaces တွေကို ခွဲခြားဖို့ workspace colors ကို ပြောင်းလဲပေးပါတယ်။

- **Installation**:
  - Quick Open (`<Ctrl+P>`) မှာ အောက်က command ကို ရိုက်ပါ:
    ```bash
    ext install johnpapa.vscode-peacock
    ```

### အခြား Recommended Extensions
- **HashiCorp HCL**: HCL files (ဥပမာ Vault configuration files) အတွက် syntax highlighting ပေးပါတယ်။
- **GitHub Markdown Styling**: Markdown files (ဥပမာ lab instructions) တွေကို ဖတ်ရလွယ်ကူစေပါတယ်။

## 3. Opt 03 - Sealed Vault with Configuration File

ဒီ lab မှာ disk-based storage (raft) ကို အသုံးပြုတဲ့ **sealed Vault server** တစ်ခုကို configuration file နဲ့ local computer မှာ ဖန်တီးခဲ့ပါတယ်။

### Config.hcl ကို စစ်ဆေးပါ
`config.hcl` file ထဲမှာ အောက်က configuration တွေ ပါဝင်ပါတယ်:
```hcl
ui = true
disable_mlock = true

storage "raft" {
  path = "./vault/data"
  node_id = "node1"
}

listener "tcp" {
  address = "127.0.0.1:8200"
  tls_disable = "true"
}

api_addr = "http://127.0.0.1:8200"
cluster_addr = "https://127.0.0.1:8201"
```
- **UI**: Enabled (`ui = true`)
- **Storage**: Raft storage ကို `./vault/data` path မှာ အသုံးပြုတယ်။
- **TLS**: Disabled (`tls_disable = "true"`)၊ ဒါက testing အတွက်ပဲ သင့်တော်တယ်။

### Vault Server ကို Run ပါ
1. **Directory Structure ဖန်တီးပါ**:
   ```bash
   mkdir -p vault/data
   ```
2. **Server ကို Start ပါ**:
   ```bash
   vault server --config=config.hcl
   ```
3. **Vault Address ကို Export လုပ်ပါ**:
   ```bash
    export VAULT_ADDR='http://127.0.0.1:8200'
   ```
4. **Status ကို Check ပါ**:
   ```bash
   vault status
   ```
   Output: Vault က **not initialized** နဲ့ **sealed** ဖြစ်နေပါလိမ့်မယ်။

ဒီ lab က basic sealed Vault တစ်ခုကို setup လုပ်ပြီး future configurations တွေအတွက် testing ground အဖြစ် အသုံးပြုနိုင်ပါတယ်။

## 4. Opt 04 - Vault with UI, Raft, and TLS Configurations

ဒီ lab မှာ **proper Vault server** တစ်ခုကို Raft storage၊ UI၊ နဲ့ **TLS** ပါဝင်တဲ့ configuration နဲ့ ဖန်တီးခဲ့ပါတယ်။ TLS က Vault 1.13 မှာ UI modifications အတွက် လိုအပ်တာကြောင့် hostname နဲ့ self-signed TLS certificate တွေကို configure လုပ်ခဲ့ပါတယ်။

### Local System ကို Configure လုပ်ပါ
1. **Hostname ကို Modify လုပ်ပါ**:
   - Linux မှာ hostname ကို ပြောင်းဖို့:
     ```bash
     hostnamectl set-hostname <new_hostname>
     ```
     ဥပမာ: `hostnamectl set-hostname pt1.local`
2. **IP Address ကို ရှာပါ**:
   ```bash
   ip a
   ```
3. **Hosts File ကို Modify လုပ်ပါ**:
   - `/etc/hosts` file ကို sudo နဲ့ ဖွင့်ပါ:
     ```bash
     sudo vim /etc/hosts
     ```
   - IP address နဲ့ hostname ကို ထည့်ပါ:
     ```
     10.0.2.52    pt1.local    pt1
     ```

### TLS ကို Configure လုပ်ပါ
**OpenSSL** ကို အသုံးပြုပြီး self-signed TLS certificate ဖန်တီးခဲ့ပါတယ်။

1. **OpenSSL ရှိမရှိ စစ်ဆေးပါ**:
   ```bash
   openssl version
   ```
   မရှိရင် install လုပ်ပါ:
   ```bash
   sudo apt install openssl
   ```

2. **myopenssl.cnf ကို Modify လုပ်ပါ**:
   - `myopenssl.cnf` file ရဲ့ နောက်ဆုံး ၃ လိုင်းကို သင့် IP နဲ့ hostname နဲ့ ပြောင်းပါ:
     ```
     IP.2 = 10.0.2.52
     DNS.1 = ip-10-0-2-52
     DNS.2 = pt1.local
     ```

3. **TLS Certificate ဖန်တီးပါ**:
   ```bash
   openssl req -x509 -nodes -days 3 -newkey rsa:3072 -keyout key.pem -out cert.pem -config myopenssl.cnf
   ```

4. **Certificate ကို စစ်ဆေးပါ**:
   ```bash
   openssl x509 -in cert.pem -text -noout
   ```

### Vault Server ကို Build လုပ်ပါ
`server-config.hcl` file ထဲမှာ အောက်က configuration တွေ ပါဝင်ပါတယ်:
```hcl
ui = true
disable_mlock = true

storage "raft" {
  path    = "./data"
  node_id = "node1"
}

listener "tcp" {
  address     = "10.0.2.52:8200"
  tls_cert_file = "cert.pem"
  tls_key_file = "key.pem"
}

api_addr = "http://10.0.2.52:8200"
cluster_addr = "https://10.0.2.52:8201"
```
- **UI**: Enabled
- **Storage**: Raft storage ကို `./data` path မှာ သုံးတယ်။
- **TLS**: `cert.pem` နဲ့ `key.pem` ကို အသုံးပြုပြီး TLS enable လုပ်ထားတယ်။
- **Address**: Local IP address (ဥပမာ `10.0.2.52`) ကို သုံးထားတယ်။

1. **Data Directory ဖန်တီးပါ**:
   ```bash
   mkdir data
   ```

2. **Vault Server ကို Run ပါ**:
   ```bash
   vault server -config=server-config.hcl
   ```

3. **Vault Address ကို Export လုပ်ပါ**:
   ```bash
    export VAULT_ADDR='http://10.0.2.52:8200'
   ```

4. **Status ကို Check ပါ**:
   ```bash
   vault status
   ```
   Output: Vault က **sealed** နဲ့ **not initialized** ဖြစ်နေပါလိမ့်မယ်။

### UI ကနေ Vault ကို Connect/Unseal/Sign-in လုပ်ပါ
1. **Browser မှာ Connect လုပ်ပါ**:
   - `https://pt1.local:8200` ကို သွားပါ။
   - Self-signed certificate ကြောင့် security warning ပေါ်လာရင် bypass လုပ်ပါ။

2. **Raft Cluster ဖန်တီးပါ**:
   - **Create a new Raft cluster** ကို ရွေးပြီး **Next** နှိပ်ပါ။
   - **Key shares**: 5
   - **Key threshold**: 3
   - **Initialize** နှိပ်ပါ။
   - JSON file အနေနဲ့ unseal keys (5) နဲ့ root token ကို download လုပ်ပါ။

3. **Vault ကို Unseal လုပ်ပါ**:
   - JSON file ထဲက unseal keys ၃ ခုကို အသုံးပြုပြီး unseal လုပ်ပါ။
   - **Continue to Unseal** နှိပ်ပါ။

4. **Root Token နဲ့ Sign-in လုပ်ပါ**:
   - Token method ကို ရွေးပြီး JSON file ထဲက root token ကို ထည့်ပါ။
   - Sign-in ပြီးရင် `cubbyhole/` secrets engine ကို တွေ့ရပါမယ်။

### CLI ကနေ System ကို Analyze လုပ်ပါ
1. **Vault Status ကို Check ပါ**:
   ```bash
   vault status
   ```
   Output: Vault က **initialized** နဲ့ **unsealed** ဖြစ်ပြီး Raft storage နဲ့ high availability (HA) enabled ဖြစ်နေပါမယ်။

2. **Secrets Engines ကို List လုပ်ပါ**:
   ```bash
   vault secrets list
   ```
   ပထမအကြိမ်မှာ `permission denied` error ပေါ်နိုင်ပါတယ်၊ ဘာကြောင့်လဲဆိုတော့ CLI access အတွက် root token မထည့်ရသေးလို့ပါ။

3. **Root Token ကို Export လုပ်ပါ**:
   ```bash
    export VAULT_TOKEN=<root_token>
   ```

4. **Secrets Engines ကို ထပ်ကြည့်ပါ**:
   ```bash
   vault secrets list
   ```
   Output: `cubbyhole/`, `identity/`, `sys/` engines တွေကို တွေ့ရပါမယ်။ `identity/` နဲ့ `sys/` က UI ထဲမှာ မပြပါဘူး။

### Userpass Authentication နဲ့ New User ဖန်တီးပါ
1. **Userpass Authentication ကို Enable လုပ်ပါ**:
   - UI ထဲမှာ **Access > Auth Methods** ကို သွားပါ။
   - **Enable new method +** နှိပ်ပြီး **Username & Password** ကို ရွေးပါ။
   - **Enable Method** နှိပ်ပါ။

2. **New User ဖန်တီးပါ**:
   - **Access > Auth Methods > userpass** ကို သွားပါ။
   - **Create user +** နှိပ်ပါ။
   - Username: `test_user1`
   - Password: `test123`
   - **Save** နှိပ်ပါ။

3. **အခြား System ကနေ Connect ကြည့်ပါ**:
   - Browser tab အသစ်တစ်ခုမှာ `https://pt1.local:8200` ကို သွားပါ။
   - **Username** method ကို ရွေးပြီး `test_user1` နဲ့ `test123` နဲ့ login ဝင်ပါ။
   - Default policy ကြောင့် `cubbyhole/` secrets engine တစ်ခုတည်းကိုပဲ တွေ့ရပါမယ်။

### Production-Ready Vault အတွက် နောက်ထပ် လုပ်ဆောင်စရာများ
ဒီ lab က production Vault server နဲ့ နီးစပ်တဲ့ setup တစ်ခုဖြစ်ပါတယ်။ Fully production-ready ဖြစ်ဖို့ အောက်ကအဆင့်တွေကို ထည့်သွင်းစဉ်းစားပါ:
- **Real Domain**: Domain registrar ဒါမှမဟုတ် Cloudflare နဲ့ real domain တစ်ခုကို setup လုပ်ပါ။
- **Proper TLS Certificate**: **Let’s Encrypt** (certbot) နဲ့ valid TLS certificate တစ်ခုကို ဖန်တီးပါ။
- **DNS Setup**: **BIND DNS server** ကို run ပြီး real DNS server ကို forward လုပ်ပါ။
- **Cloud Deployment**: AWS ဒါမှမဟုတ် တခြား cloud provider မှာ run ပြီး DNS နဲ့ networking functionality တွေကို ပိုမိုလွယ်ကူစွာ implement လုပ်နိုင်ပါတယ်။

## ကျွန်တော့်အတွေ့အကြုံ

ဒီ optional labs တွေက Vault ရဲ့ အဆင့်မြင့် configurations တွေနဲ့ development environment ကို ပိုမိုကောင်းမွန်စေတဲ့ tools တွေကို နားလည်ဖို့ အထောက်အကူဖြစ်ပါတယ်။ **Vim Polyglot** နဲ့ **HashiVim** plugins တွေက code readability ကို မြှင့်တင်ပေးပြီး **VSCode Peacock** နဲ့ HCL extensions တွေက multiple projects တွေကို ခွဲခြားဖို့ အထောက်အကူဖြစ်ပါတယ်။

**Sealed Vault** lab က basic production-like setup ကို လက်တွေ့လုပ်ဆောင်ကြည့်ဖို့ အခွင့်အရေးပေးပြီး **TLS-enabled Vault** lab က real-world scenarios တွေမှာ လုံခြုံရေးအတွက် TLS နဲ့ hostname configurations တွေရဲ့ အရေးပါမှုကို ပြသပေးပါတယ်။ Self-signed certificate အစား **Let’s Encrypt** လို proper certificates တွေကို အသုံးပြုတာက production မှာ ပိုလုံခြုံမှုရှိစေပါတယ်။

ဒီ labs တွေကို ပြီးမြောက်အောင်လုပ်ဆောင်ခဲ့တာက HashiCorp Vault ကို production environment မှာ ဘယ်လို deploy လုပ်ရမလဲဆိုတာကို ပိုမိုနားလည်လာစေပါတယ်။ စာမေးပွဲအတွက် ပြင်ဆင်နေသူတွေအနေနဲ့ ဒီ optional labs တွေကို လေ့ကျင့်ကြည့်ဖို့ အကြံပြုပါတယ်၊ ဘာကြောင့်လဲဆိုတော့ operational knowledge ကို ပိုမိုခိုင်မာစေလို့ပါ။

ဒီ blog post က HashiCorp Vault ကို စတင်လေ့လာနေသူတွေနဲ့ Vault Associate Certification အတွက် ပြင်ဆင်နေသူတွေအတွက် အထောက်အကူဖြစ်မယ်လို့ မျှော်လင့်ပါတယ်။ မေးစရာရှိရင် comment မှာ ထားခဲ့ပါ။ Cybersecurity နဲ့ DevOps ဆိုင်ရာ အကြောင်းအရာတွေကို နောက်ထပ်လည်း ဆက်လက်မျှဝေပေးသွားပါမယ်။