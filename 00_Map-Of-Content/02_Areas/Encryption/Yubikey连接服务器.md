---
tags:
  - Tools/Security/YubiKey
created: 2025-08-13
author:
  - ln1
status: In Progress
---
## 1️⃣ 准备 Yubikey

* 确认你的 Yubikey 支持 **PIV / OpenPGP / FIDO2**。
* 如果只用 SSH，推荐用 **FIDO2 (WebAuthn)** 生成 SSH key，因为它可以直接做公钥认证。

---

## 2️⃣ 在本地生成 SSH Key（FIDO2）

```bash
ssh-keygen -t ecdsa-sk -f ~/.ssh/id_ecdsa_sk -C "ln1@aliyun"
```

* 系统会提示 **touch your authenticator**，触碰 Yubikey 才能生成 key。
* 可以设置 passphrase（或留空不设置）。
* 这一步生成的 key 在 **Yubikey 内部安全存储**，私钥不会离开 Yubikey。

---

## 3️⃣ 配置 SSH 使用 Yubikey（FIDO2）

编辑 `~/.ssh/config`：

```text
Host aliyun
    HostName 47.93.254.172
    User ln1
    IdentityFile ~/.ssh/id_ecdsa_sk
```

* 这样 `ssh aliyun` 时会自动使用 Yubikey 上的 FIDO2 key。
* 无需再配置 `PKCS11Provider`（PKCS11 主要用于 PIV / OpenPGP key）。

---

## 4️⃣ 把公钥上传到服务器

```bash
ssh-copy-id -i ~/.ssh/id_ecdsa_sk.pub ln1@47.93.254.172
```

或手动把 `~/.ssh/id_ecdsa_sk.pub` 的内容添加到服务器的 `~/.ssh/authorized_keys`。

---

## 5️⃣ 测试 SSH 连接

```bash
ssh aliyun
```

* 第一次可能要求触碰 Yubikey。
* 之后只要 **SSH agent** 运行，FIDO2 key 可以缓存一次验证，之后不必每次都输入 PIN。

---

## 6️⃣ (可选) 配置 SSH agent 缓存

如果想 **只验证一次就持续可用**：

```bash
# 启动 agent
eval "$(ssh-agent -s)"

# 添加 Yubikey key 到 agent
ssh-add ~/.ssh/id_ecdsa_sk
```

* 触碰一次 Yubikey 输入 PIN 后，agent 会缓存 key，后续 ssh 不再要求 PIN 直到 agent 关闭。

---

## 7️⃣ (PIV / OpenPGP 特殊情况)

* 如果你用 **PIV 或 OpenPGP key**：

  * 需要 `PKCS11Provider` 指向 `/usr/lib/x86_64-linux-gnu/opensc-pkcs11.so`。
  * ssh-add 会提示 PIN。
  * 这种方式一般不会缓存 PIN，每次登录都需要验证。

---

### ✅ 总结要点

1. **FIDO2 (WebAuthn) 更方便**：Yubikey 内部生成 key，支持缓存一次 PIN，SSH 登录体验流畅。
2. **PIV / OpenPGP key**：安全性高，但每次登录都要 PIN，适合严格安全需求。
3. **公钥上传到服务器**：`authorized_keys` 里必须有对应公钥。
4. **SSH agent** 可以缓存 FIDO2 key，减少重复输入 PIN。
5. **配置 \~/.ssh/config** 可以方便直接 `ssh aliyun`。

---


