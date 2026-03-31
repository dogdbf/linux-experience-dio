# 🐧 Ubuntu Server no VirtualBox 7.2.6

Guia prático para instalação do **Ubuntu Server** em **VirtualBox**, com foco em ambiente de estudos, laboratórios e desenvolvimento.

---

## 📚 Sumário

1. [Pré-requisitos](#-pr%C3%A9-requisitos)
2. [Preparando o Windows](#-preparando-o-windows)
3. [Downloads](#-downloads)
4. [Criando a Máquina Virtual](#-criando-a-m%C3%A1quina-virtual)
5. [Configurações da VM](#-configura%C3%A7%C3%B5es-da-vm)
6. [Instalação do Ubuntu Server](#-instala%C3%A7%C3%A3o-do-ubuntu-server)
7. [Pós-instalação](#-p%C3%B3s-instala%C3%A7%C3%A3o)
8. [Acesso via SSH](#-acesso-via-ssh)
9. [Conclusão](#-conclus%C3%A3o)

---

## 🛠️ Pré-requisitos

- **Windows 10/11** (versão atualizada)
- **Permissão de administrador** na máquina
- **Mínimo de 50GB** de espaço em disco livre
- **Processador com virtualização habilitada** (Intel VT-x ou AMD-V)
- **Mínimo 4GB de RAM** disponível (recomendado 8GB)

---

## ⚙️ Preparando o Windows

Antes de instalar o VirtualBox, é importante desabilitar o **Hyper-V** para evitar conflitos.

### Verificar status do Hyper-V

1. Abra o **CMD como administrador**
2. Execute:

bash

```bash
bcdedit
```

3. Procure pela linha:

```
hypervisorlaunchtype  [OFF | AUTO]
```

### Interpretação

| Valor  | Significado                         |
| ------ | ----------------------------------- |
| `OFF`  | ✅ OK para VirtualBox                |
| `AUTO` | ⚠️ Hyper-V ativo (vai dar problema) |

---

### Se estiver `AUTO` (precisa mudar)

Execute como administrador:

bash

```bash
bcdedit /set hypervisorlaunchtype off
```

⚠️ **Atenção:**

Se você usa **WSL (Windows Subsystem for Linux)**, mantenha como `AUTO`. Nesse caso, considere:

- Usar WSL diretamente ao invés de VM
- Ou usar uma máquina Linux física

👉 [Instalar o WSL | Microsoft Learn](https://learn.microsoft.com/pt-br/windows/wsl/install)

---

Depois de alterar, **reinicie o PC**.

---

## ⬇️ Downloads

### VirtualBox

- 🔗 **Versão:** 7.2.6 ou superior
- 🔗 **Link:** https://www.virtualbox.org/wiki/Downloads
- 📥 Baixe conforme seu sistema operacional

### Ubuntu Server

- 🔗 **Versão:** Ubuntu 24.04 LTS (recomendado)
- 🔗 **Link:** https://ubuntu.com/download/server
- 📥 Escolha a versão **ISO** (não USB)

---

## 🧩 Criando a Máquina Virtual

### Passo 1: Abrir VirtualBox

Abra o VirtualBox e clique em **"Novo"**.

### Passo 2: Configuração básica

Preencha os campos:

| Campo             | Exemplo          | Descrição            |
| ----------------- | ---------------- | -------------------- |
| Nome              | ubuntu-server    | Nome da VM           |
| Pasta de máquinas | Padrão           | Onde será armazenada |
| Imagem ISO        | ubuntu-24.04.iso | Arquivo baixado      |
| Tipo              | Linux            | Tipo do SO           |
| Versão            | Ubuntu (64-bit)  | Arquitetura          |

### Passo 3: Configuração de recursos

Defina os recursos da VM:

| Recurso       | Mínimo | Recomendado |
| ------------- | ------ | ----------- |
| Memória RAM   | 2 GB   | 4 GB        |
| Processadores | 2      | 4           |
| Disco rígido  | 20 GB  | 50 GB       |

### Passo 4: Tipo de disco

- Tipo: **VDI (VirtualBox Disk Image)** ✅
- Alocação: **Dinamicamente alocado** ✅
- Tamanho: 50 GB (ou conforme sua escolha)

---

## ⚙️ Configurações da VM

Após criar a VM, ainda não inicie. Configure antes:

### 1. Armazenamento (adicionar ISO)

1. Clique em **Configurações** da VM
2. Vá em **Armazenamento**
3. Clique em **Vazio** (CD/DVD)
4. Em **Atributos**, clique no ícone do CD
5. Selecione o arquivo **ubuntu-24.04.iso**
6. Clique em **OK**

---

### 2. Rede (modo Bridge)

1. Vá em **Configurações** → **Rede**
2. **Adaptador 1:**
   - Conectado a: **Placa em modo Bridge**
   - Nome: **Sua placa de rede** (Ethernet ou Wi-Fi)
3. Clique em **OK**

---

### 3. Otimizações (opcional)

**Gráficos:**

- Ativa aceleração 3D: ✅ (se disponível)
- Memória de vídeo: 128 MB

**Sistema:**

- Chipset: PIIX3
- Relógio: UTC

---

## 🐧 Instalação do Ubuntu Server

### Inicie a VM

Clique em **Iniciar** (seta verde).

---

### Siga o instalador

O instalador do Ubuntu irá aparecer. Siga as etapas:

1. **Idioma:** Selecione seu idioma
2. **Layout do teclado:** Português (Brasil)
3. **Tipo de instalação:** Ubuntu Server (padrão)
4. **Rede:** Deixe automática (DHCP)
5. **Proxy:** Deixe em branco (se não usar)
6. **Mirror:** Padrão é OK
7. **Particionamento:** Use padrão (LVM)
8. **Perfil de usuário:**
   - Nome completo: Douglas Ferreira
   - Nome de usuário: douglas
   - Senha: **Escolha uma segura**
   - Confirme a senha

---

### Softwares adicionais

Marque (com espaço):

- ✅ **OpenSSH Server** (importante para acesso remoto)
- ✅ **Docker** (opcional, mas útil para DevOps)
- ⬜ Outros conforme sua necessidade

---

### Conclusão da instalação

- Clique em **Done**
- Aguarde o sistema reiniciar
- Se pedir para remover a ISO, pressione **Enter**

---

## 🧹 Pós-instalação

### Remover a ISO da VM

Após o Ubuntu iniciar (tela de login):

1. **Menu VirtualBox → Dispositivos → Discos Ópticos**
2. Selecione a ISO
3. Clique em **Remover disco**

---

### Atualizar o sistema

Faça login e execute:

bash

```bash
sudo apt update
sudo apt upgrade -y
```

---

### Instalações recomendadas

Se não instalou durante o setup:

bash

```bash
# OpenSSH (essencial)
sudo apt install openssh-server -y

# Git
sudo apt install git -y

# Curl e Wget
sudo apt install curl wget -y
```

---

## 🌐 Acesso via SSH

### Descobrir o IP da VM

Na VM, execute:

bash

```bash
ip a
```

Procure por algo como:

```
inet 192.168.1.100/24
```

---

### No Windows: Conectar via SSH

**Opção 1: Terminal (Windows 10/11 21H2+)**

bash

```bash
ssh douglas@192.168.1.100
```

**Opção 2: PuTTY (recomendado)**

1. Baixe: [https://putty.org/](https://putty.org/)
2. Abra e preencha:
   - Host Name: `192.168.1.100`
   - Port: `22`
   - Username: `douglas`
3. Clique em **Open**
4. Digite sua senha

**Opção 3: VS Code**

1. Instale a extensão **Remote - SSH**
2. Clique em **Remote Explorer**
3. **Add New SSH Host**
4. Digite: `ssh douglas@192.168.1.100`

---

### Usar chave SSH (mais seguro)

**Na VM, gere uma chave:**

bash

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa
```

**No Windows:**

bash

```bash
# Copie a chave pública da VM para seu Windows
scp douglas@192.168.1.100:~/.ssh/id_rsa.pub .
```

Depois configure no PuTTY ou adicione ao seu cliente SSH.

---

## ✅ Conclusão

Parabéns! 🎉

Agora você tem um **Ubuntu Server funcional em VirtualBox**, pronto para:

- ✅ Estudos de Linux/DevOps
- ✅ Testes e laboratórios
- ✅ Desenvolvimento de aplicações
- ✅ Prática de administração de sistemas

---

## 🚀 Próximos passos

Recomendado para consolidar:

1. Instalar **Docker** e praticar containers
2. Configurar um **Git server** local
3. Instalar **Node.js** ou **Python**
4. Praticar **permissões** e **usuários** do Linux

---

## 📞 Referências

- VirtualBox Manual: [User Guide for Release 7.2](https://www.virtualbox.org/manual/)
- Ubuntu Server Documentation: [Ubuntu Server documentation](https://ubuntu.com/server/docs)
- OpenSSH Manual: `man ssh` (na VM)

---

## 🔧 Troubleshooting

### VM não inicia

- Verifique se virtualização está habilitada no BIOS
- Tente desabilitar Hyper-V novamente

### Sem acesso de rede

- Verifique o modo de rede (Bridge ou NAT)
- Tente `sudo netplan apply` na VM

### SSH não funciona

bash

```bash
# Na VM:
sudo systemctl status ssh
sudo systemctl restart ssh
```

---

✍️ **Autor:** Douglas Bezerra Ferreira  
📅 **Data:** 31 de Março de 2026  
🏷️ **Versão:** 1.0 (Refatorado e completo)

---

💡 **Se esse guia te ajudou, compartilha ou contribui no repositório!** ⭐
