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

## ⭐ Observação# 👤 Gerenciamento de Usuários no Ubuntu

Guia prático sobre criação, modificação e gerenciamento de usuários e grupos no Ubuntu/Linux.

---

## 📚 Sumário

1. [Criação de Usuário](#-cria%C3%A7%C3%A3o-de-usu%C3%A1rio)
2. [Modificação de Usuário](#-modifica%C3%A7%C3%A3o-de-usu%C3%A1rio)
3. [Definição de Senha](#-defini%C3%A7%C3%A3o-de-senha)
4. [Gerenciamento de Grupos](#-gerenciamento-de-grupos)
5. [Boas Práticas de Segurança](#-boas-pr%C3%A1ticas-de-seguran%C3%A7a)
6. [Cenários Reais](#-cen%C3%A1rios-reais-uso-em-servidor)
7. [Operações Básicas](#-opera%C3%A7%C3%B5es-b%C3%A1sicas)

---

## 🔧 Criação de Usuário

### Comando básico

bash

```bash
useradd douglas -m -c "Douglas Ferreira" -s /bin/bash
```

### Parâmetros principais

| Parâmetro | Descrição                                               |
| --------- | ------------------------------------------------------- |
| `-m`      | Cria o diretório `/home/douglas`                        |
| `-c`      | Adiciona comentário (nome completo, descrição, etc.)    |
| `-s`      | Define o shell padrão                                   |
| `-e`      | Define data de expiração da conta (formato: YYYY-MM-DD) |
| `-u`      | Define um UID específico                                |
| `-g`      | Adiciona grupo primário                                 |

### Exemplo com mais opções

bash

```bash
useradd -m -c "Douglas Ferreira" -s /bin/bash -u 1050 douglas
```

---

## 🔄 Modificação de Usuário

bash

```bash
usermod douglas -c "Douglas DBF"
```

### Usos comuns

Alterar shell:

bash

```bash
usermod -s /bin/zsh douglas
```

Adicionar a grupos (com `-a`):

bash

```bash
usermod -aG adm,sudo douglas
```

Bloquear/desbloquear:

bash

```bash
usermod -L douglas  # bloqueia
usermod -U douglas  # desbloqueia
```

---

## 🔐 Definição de Senha

### ⚠️ MÉTODO NÃO RECOMENDADO (expõe senha)

bash

```bash
useradd convidado1 -c "Convidado" -s /bin/bash -m -p $(openssl passwd -6 Senha123)
```

**Problemas:**

- Senha exposta no histórico (`history`)
- Risco de segurança alta
- Não use em ambientes reais

---

### ✅ MÉTODO 1 — Interativo (mais seguro)

bash

```bash
useradd douglas -m -c "Douglas Ferreira" -s /bin/bash
passwd douglas
```

💡 O sistema pedirá a senha sem exibir na tela.

---

### ✅ MÉTODO 2 — Com `chpasswd` (automação segura)

bash

```bash
useradd douglas -m -c "Douglas Ferreira" -s /bin/bash
echo "douglas:Senha123" | chpasswd
```

---

### ✅ MÉTODO 3 — Script com senha oculta

bash

```bash
read -s -p "Digite a senha: " senha
echo
useradd douglas -m -s /bin/bash
echo "douglas:$senha" | chpasswd
```

---

## 👥 Gerenciamento de Grupos

### Criar um grupo

bash

```bash
groupadd GRP_ADM
```

### Adicionar usuário a grupos

bash

```bash
usermod -aG adm,sudo douglas
```

⚠️ **Importante:** Sempre use `-a` junto com `-G` para adicionar (não remover) grupos.

### Remover usuário de um grupo

bash

```bash
gpasswd -d douglas sudo
```

### Listar grupos de um usuário

bash

```bash
groups douglas
```

---

## 🔐 Boas Práticas de Segurança

### ✅ O que fazer

- Use `passwd` ou `chpasswd` para definir senhas
- Proteja arquivos sensíveis com permissões restritas:

bash

```bash
  chmod 600 usuarios.txt
```

- Senhas fortes (maiúsculas, minúsculas, números, caracteres especiais):

```
  T3l3com@2026!
  MinkSQL#789@
```

- Use chaves SSH ao invés de senhas quando possível

### ❌ O que evitar

- ❌ Senha em comando direto (visível no histórico)
- ❌ Usar `-p` com senha em plaintext
- ❌ Compartilhar senhas via mensagens/email
- ❌ Dar acesso sudo indiscriminadamente

---

## 🚀 Cenários Reais (uso em servidor)

### 1️⃣ Criar usuário para acesso SSH

bash

```bash
useradd devops -m -s /bin/bash
passwd devops
usermod -aG sudo devops
```

✅ Esse usuário:

- Pode logar via SSH
- Pode usar `sudo` para comandos administrativos

---

### 2️⃣ Criar usuário SEM acesso a shell (serviço)

bash

```bash
useradd backup -m -s /usr/sbin/nologin
```

✅ Ideal para:

- Serviços do sistema
- Automações/daemons
- Contas que NÃO devem fazer login interativo

---

### 3️⃣ Forçar troca de senha no primeiro login

bash

```bash
useradd temporal -m -s /bin/bash
passwd temporal
passwd -e temporal
```

✅ Muito usado para:

- Novos usuários
- Acessos temporários/convidados

---

### 4️⃣ Definir expiração de conta

bash

```bash
useradd temporario -m -e 2026-12-31
```

✅ A conta expira automaticamente em 31/12/2026

---

### 5️⃣ Criar grupo de acesso e adicionar usuários

bash

```bash
groupadd GRP_DEVOPS
useradd -m -G GRP_DEVOPS dev1
useradd -m -G GRP_DEVOPS dev2
```

---

## 🧹 Operações Básicas

### Verificar informações do usuário

bash

```bash
id douglas
```

Saída exemplo:

```
uid=1000(douglas) gid=1000(douglas) groups=1000(douglas),4(adm),27(sudo)
```

---

### Verificar grupos de um usuário

bash

```bash
groups douglas
```

---

### Bloquear usuário (sem deletar)

bash

```bash
passwd -l douglas
```

### Desbloquear usuário

bash

```bash
passwd -u douglas
```

---

### Remover usuário (mantém diretório)

bash

```bash
userdel douglas
```

### Remover usuário com diretório home

bash

```bash
userdel -r douglas
```

---

### Listar todos os usuários do sistema

bash

```bash
cat /etc/passwd
```

---

## 📋 Entendendo o `/etc/passwd`

### Estrutura de cada linha

Exemplo:

```
douglas:x:1000:1000:Douglas Ferreira:/home/douglas:/bin/bash
```

| Campo | Valor            | Descrição                                     |
| ----- | ---------------- | --------------------------------------------- |
| 1     | douglas          | Nome do usuário                               |
| 2     | x                | Senha (armazenada em `/etc/shadow`, não aqui) |
| 3     | 1000             | UID (ID único do usuário)                     |
| 4     | 1000             | GID (ID do grupo primário)                    |
| 5     | Douglas Ferreira | Comentário/descrição                          |
| 6     | /home/douglas    | Diretório home                                |
| 7     | /bin/bash        | Shell padrão                                  |

---

### Dica: Listar apenas usuários reais (UID ≥ 1000)

bash

```bash
awk -F: '$3 >= 1000 {print $1}' /etc/passwd
```

---

## 📊 Resumo Rápido

| Ação              | Comando                                     |
| ----------------- | ------------------------------------------- |
| Criar usuário     | `useradd -m -c "Nome" -s /bin/bash usuario` |
| Alterar usuário   | `usermod -c "Novo Nome" usuario`            |
| Definir senha     | `passwd usuario`                            |
| Criar grupo       | `groupadd GRP_NOME`                         |
| Adicionar a grupo | `usermod -aG grupo usuario`                 |
| Remover de grupo  | `gpasswd -d usuario grupo`                  |
| Bloquear usuário  | `passwd -l usuario`                         |
| Remover usuário   | `userdel -r usuario`                        |

---

## 🎯 Dica Profissional

Em ambientes reais (produção):

1. **Prefira autenticação por chave SSH** ao invés de senha
2. **Evite acesso direto como `root`** → use `sudo`
3. **Use controle de acesso por grupos** (`sudo`, `adm`, etc.)
4. **Monitore alterações** de usuários:

bash

```bash
   sudo lastlog
   sudo w
```

---

## 📞 Referências

- Man pages: `man useradd`, `man usermod`, `man passwd`
- Arquivo de configuração: `/etc/login.defs`
- Arquivo de senhas: `/etc/shadow` (acesso restrito)

---

✍️ **Autor:** Douglas Bezerra Ferreira  
📅 **Data:** 31 de Março de 2026  
🏷️ **Versão:** 1.0 (Refatorado e completo)

---

💡 **Se esse guia te ajudou, compartilha ou contribui no repositório!** ⭐

Se esse guia te ajudou, salva aí no repositório ou usa como base para seus labs 😄
