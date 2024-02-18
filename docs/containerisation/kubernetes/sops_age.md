# Kubernetes secrets with Mozilla SOPS and Age encryption

There are various ways to manage secrets within a Kubernetes cluster but one of the easiest and most effective ways I manage secrets on my clusters is by using [Mozilla SOPS](https://github.com/getsops/sops) along with [age](https://github.com/FiloSottile/age) encryption.

I use Flux for GitOps on my clusters and so the cluster state is controlled by a GitHub repository. This repository is public facing and I wanted to include my secrets within the repository as well.

## SOPS

First, download the SOPS binary from the repository

Next, move the binary into a location of choice and update your OS environment variables with the path

### Test CLI

Run `sops -v` to confirm it returns an output

## Age

First, download the Age binary from the repository

Next, move the binary into a location of choice and update your OS environment variables with the path

### Test CLI

Run `age -version` to confirm it returns an output

## Generate Age key pair

```
age-keygen -o sops-age.key
```

## Add age config file to repository

To make encryption of secrets easier when working within the context of your GitOps repository you can create configuration files for automatic encryption. With this you can apply rules to look for particular data types as well as automatically use the correct age public key. 

Normally you will use this for just secret data but I extend this to my ingress configuration as well to hide domain names and email addresses.

## Create .sops.yaml file

``` title="encrypt data and stringData types"
creation_rules: 
  - path_regex: .*.yaml 
  -  encrypted_regex: '^(data|stringData)$' 
  - age: <age public key> 
```

``` title="encrypt email and dnsNames types"
creation_rules: 
  - path_regex: .*.yaml 
  - encrypted_regex: '(email|dnsNames)' 
  - age: <age public key> 
```

## 

## Install Age key as secret

You will now need to install the Age generated key 

## Working with Secrets 

You are now ready to start encrypting secrets with 