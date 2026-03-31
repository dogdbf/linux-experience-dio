# 👤 Gerenciamento de Usuários no Ubuntu

## 🔧 Criação de usuário

```bash
useradd douglas -m -c "Douglas Ferreira" -s /bin/bash  
```

### Parâmetros:

- `-m` → cria o diretório `/home/douglas`  
- `-c` → adiciona um comentário (nome completo, descrição, etc.)  
- `-s` → define o shell padrão  
- `-e` → define data de expiração da conta  
  ⚠️ *Não define obrigatoriedade de troca de senha automaticamente*  

---  

## 🔄 Modificação de usuário

```bash
usermod douglas -c "dog"  
```

- Usado para alterar características do usuário  

---  

## 🔐 Criação de usuário com senha (NÃO recomendado)

```bash
useradd convidado1 -c "convidado especial" -s /bin/bash -m -p $(openssl passwd -6 Senha123)  
```

### Problemas:

- Senha exposta no histórico (`history`)  
- Risco de segurança  
- Não recomendado em ambientes reais  

---  

## ✅ 🔐 Forma recomendada (segura)

### Método 1 — Automação com `chpasswd`

```bash
useradd douglas -m -c "Douglas Ferreira" -s /bin/bash  
echo "douglas:Senha123" | chpasswd  
```

---  

### Método 2 — Interativo (mais seguro)

```bash
useradd douglas -m -c "Douglas Ferreira" -s /bin/bash  
passwd douglas  
```

---  

### Método 3 — Script com senha oculta

```bash
read -s -p "Digite a senha: " senha  
echo  
useradd douglas -m -s /bin/bash  
echo "douglas:$senha" | chpasswd  
```

---  

## 👥 Gerenciamento de grupos

### Criar um grupo

```bash
groupadd GRP_ADM  
```

---  

### Adicionar usuário a grupos

```bash
usermod -aG adm,sudo douglas  
```

⚠️ Sempre use `-a` junto com `-G`  

---  

### Remover usuário de um grupo

```bash
gpasswd -d douglas sudo  
```

---  

## 🔐 Boas práticas de segurança

- ❌ Evite senha em comando direto  
- ❌ Evite usar `-p` com senha  
- ✅ Use `passwd` ou `chpasswd`  
- ✅ Proteja arquivos sensíveis:  

```bash
chmod 600 usuarios.txt  
```

- ✅ Use senhas fortes:  

Exemplo:  

```
T3l3com@2026!  
```

---  

# 🚀 Cenários reais (uso em servidor)

## 🔐 1. Criar usuário para acesso SSH

```bash
useradd devops -m -s /bin/bash  
passwd devops  
usermod -aG sudo devops  
```

👉 Esse usuário poderá:  

- Logar via SSH  
- Usar sudo  

---  

## 🔒 2. Criar usuário SEM acesso a shell (ex: serviço)

```bash
useradd backup -m -s /usr/sbin/nologin  
```

👉 Ideal para:  

- serviços  
- automações  
- contas que NÃO devem logar  

---  

## 🔑 3. Forçar troca de senha no primeiro login

```bash
passwd -e douglas  
```

👉 Muito usado para:  

- novos usuários  
- acessos temporários  

---  

## ⏳ 4. Definir expiração de conta

```bash
useradd temporario -m -e 2026-12-31  
```

👉 Conta expira automaticamente  

---  

## 🔍 5. Verificar informações do usuário

```bash
id douglas  
```

```bash
groups douglas  
```

---  

## 🔐 6. Bloquear e desbloquear usuário

### Bloquear:

```bash
passwd -l douglas  
```

### Desbloquear:

```bash
passwd -u douglas  
```

---  

## 🧹 7. Remover usuário

```bash
userdel douglas  
```

👉 Remover com diretório:  

```bash
userdel -r douglas  
```

---  

## 🧠 Resumo rápido

| Ação             | Comando principal                      |
| ---------------- | -------------------------------------- |
| Criar usuário    | `useradd`                              |
| Alterar usuário  | `usermod`                              |
| Definir senha    | `passwd` / `chpasswd`                  |
| Gerenciar grupos | `usermod -aG` / `groupadd` / `gpasswd` |
| Remover usuário  | `userdel -r`                           |

---  

#### 📋 Listar todos os usuários do sistema

```bash
cat /etc/passwd  
```

---  

### 🧠 O que esse comando faz?

Exibe o conteúdo do arquivo:  

```
/etc/passwd  
```

👉 Esse arquivo contém **todos os usuários do sistema**, incluindo:  

- usuários reais (ex: douglas, flavia)  
- usuários de serviço (ex: www-data, nobody)  

---  

### 📌 Estrutura de cada linha

Exemplo:  

```
douglas:x:1000:1000:Douglas Ferreira:/home/douglas:/bin/bash  
```

| Campo      | Descrição                           |
| ---------- | ----------------------------------- |
| douglas    | nome do usuário                     |
| x          | senha (armazenada em outro arquivo) |
| 1000       | UID (ID do usuário)                 |
| 1000       | GID (ID do grupo)                   |
| comentário | descrição do usuário                |
| /home/...  | diretório home                      |
| /bin/bash  | shell padrão                        |

---  

### 🎯 Dica prática

Para listar apenas usuários "reais" (UID ≥ 1000):  

```bash
awk -F: '$3 >= 1000 {print $1}' /etc/passwd  
```

---  

### ⚠️ Observação

- Senhas **não ficam aqui**  
- Elas ficam no arquivo:  

```bash
/etc/shadow  
```

(acesso restrito ao root)  

--- 

## 🎯 Dica final (nível profissional)

Em ambientes reais:  

- Prefira autenticação por **chave SSH** ao invés de senha  
- Evite acesso direto como `root`  
- Use controle de acesso por grupos (`sudo`, `adm`, etc.)  

---

✍️ Autor

**Douglas Bezerra Ferreira**  
📅 30 de Março de 2026

---

## ⭐ Observação

Se esse guia te ajudou, salva aí no repositório ou usa como base para seus labs 😄
