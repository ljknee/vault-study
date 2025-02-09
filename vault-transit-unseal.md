# HashiCorp Vault Transit Auto-Unseal Configuration Guide

This guide demonstrates how to configure auto-unseal for a Vault server using another Vault server's Transit secrets engine as the auto-unseal mechanism.

## Overview

Auto-unseal was developed to reduce the operational complexity of unsealing Vault. Instead of requiring multiple operators to provide their unseal key shares, auto-unseal allows Vault to automatically unlock its master key using an external trusted source.

## Part 1: Configure the Transit Vault (Primary)

This is the Vault server that will be used to store the unseal key.

### 1. Enable Transit Secret Engine
```bash
# Enable the transit secrets engine
$ vault secrets enable transit
Success! Enabled the transit secrets engine at: transit/

# Create an encryption key for auto-unseal
$ vault write -f transit/keys/unseal-key
Success! Data written to: transit/keys/unseal-key

# Verify the key was created
$ vault list transit/keys
Keys
----
unseal-key
```

### 2. Create an Policy for Auto-Unseal

Create a policy file (policy.hcl) with the following contents:
```hcl
path "transit/encrypt/unseal-key" {
   capabilities = [ "update" ]
}

path "transit/decrypt/unseal-key" {
   capabilities = [ "update" ]
}
```

Apply the policy:
```bash
# Write the policy
$ vault policy write unseal policy.hcl
Success! Uploaded policy: unseal

# Verify the policy
$ vault policy list
default
root
unseal

# Create a token with the unseal policy
$ vault token create -policy=unseal
Key                  Value
---                  -----
token                s.v9hDN1IycSM8zL7wsFo9vD0i
token_accessor       VtKG91SPhC515hHak3BBoZga
token_duration      768h
token_renewable     true
token_policies      ["default" "unseal"]
```

## Part 2: Configure the Target Vault (Secondary)

This is the Vault server that will be auto-unsealed.

### 1. Check Initial Status
```bash
$ vault status
Key                      Value
---                      -----
Recovery Seal Type       shamir
Initialized             false
Sealed                  true
Total Recovery Shares   5
Threshold              3
```

### 2. Configure Vault for Auto-Unseal

Edit the Vault configuration file (/etc/vault.d/vault.hcl):
```hcl
storage "raft" {
  path = "/opt/vault3/data"
  node_id = "node-a-us-east-1"
  retry_join {
    auto_join = "provider=aws region=us-east-1 tag_key=vault tag_value=us-east-1"
  }
}

seal "transit" {
  address = "http://10.0.1.209:8200"
  token = "s.v9hDN1IycSM8zL7wsFo9vD0i"
  key_name = "unseal-key"
  mount_path = "transit"
}

listener "tcp" {
  address = "0.0.0.0:8200"
  cluster_address = "0.0.0.0:8200"
  tls_disable = 1
}

api_addr = "http://10.0.1.37:8200"
cluster_addr = "http://10.0.1.37:8201"
cluster_name = "vault-prod-us-east-1"
ui = true
log_level = "INFO"
```

### 3. Restart and Initialize Vault
```bash
# Restart Vault service
$ sudo systemctl restart vault

# Check status
$ vault status
Key                Value
---                -----
Recovery Seal Type shamir
Initialized        false
Sealed            true

# Initialize Vault
$ vault operator init
Recovery Key 1: 7C1UIyIkFYFQ2OvLOZ5B95y2miu+gOwfSFNkNzBBDLv8
Recovery Key 2: 9ThaOROeCD0WjJRhnYOfZ/ANbZSAmRzc45sSg88328Hn
Recovery Key 3: PtBGURNKH5Ddu7wSMHH/US2vxYDJJOqYW0n3C039vN3P
Recovery Key 4: Qw6pCQgfcjY9ymou7TOvzWJyhramvOp+YVMlvNFqgg53
Recovery Key 5: 9Gxy0cUJ5bFvV8zPI9KJ+wGnSBMPJVJmfsIyCPgYBgqJ

Initial Root Token: s.j2kLuFYRvi4RXLa3Zjr6sOpp

Success! Vault is initialized

Recovery key initialized with 5 key shares and a key threshold of 3. Please
securely distribute the key shares printed above.
```

### 4. Verify Auto-Unseal Configuration
```bash
$ vault status
Key                      Value
---                      -----
Recovery Seal Type       shamir
Initialized             true
Sealed                  false
Total Recovery Shares   5
Threshold              3
Version                1.7.1
Storage Type           raft
Cluster Name          vault-prod-us-east-1
Cluster ID            64ce9442-b78f-fad1-5ad6-f9b6a1b5b9c8
HA Enabled            true
HA Cluster            https://10.0.1.37:8201
HA Mode               active
```

## Security Considerations

1. **Token Security**: The transit unseal token should be stored securely and rotated periodically
2. **Network Security**: Ensure network connectivity between Vault servers is secure
3. **Recovery Keys**: Safely store the recovery keys as they're needed for recovery scenarios
4. **Access Control**: Limit access to the transit unseal key to only necessary services

## Recovery Procedures

In case of issues:
1. Keep recovery keys secure and distributed
2. Maintain backup of Vault configuration
3. Document the recovery procedure
4. Test recovery process periodically

## Troubleshooting

Common issues and solutions:
1. Connection issues between Vault servers
2. Token permissions problems
3. Configuration syntax errors
4. Network connectivity problems

Remember to always verify the status after configuration changes and maintain proper security practices throughout the setup process.