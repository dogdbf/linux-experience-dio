# 🐧 Ubuntu Server no VirtualBox 7.2.6

Guia prático para instalação do **Ubuntu Server** no **VirtualBox**, com foco em ambiente de estudos/lab.

---

## 📚 Sumário

- 🛠️ Pré-requisitos
- ⚙️ Preparando o Windows
- ⬇️ Downloads
- 🧩 Criando a Máquina Virtual
- ⚙️ Ajustes da VM
- 🐧 Instalação do Ubuntu Server
- 🧹 Pós-instalação
- 🌐 Acesso via SSH
- ✅ Conclusão

---

## 🛠️ Pré-requisitos

- Windows 10/11
- Permissão de administrador
- VirtualBox compatível com seu sistema

---

## ⚙️ Preparando o Windows

Antes de instalar o VirtualBox, verifique o Hyper-V:

1. Abra o **CMD como administrador**
2. Execute:
   
   ```
   bcdedit
   ```

3. Verifique o valor de:

hypervisorlaunchtype

- ✅ `OFF` → OK
- ⚠️ `AUTO` → Execute:

```
bcdedit /set hypervisorlaunchtype off
```

> ⚠️ **Atenção:**  
> Se for usar **WSL (Windows 10/11 PRO)**, mantenha como `AUTO`  
> 👉 [Instalar o WSL | Microsoft Learn](https://learn.microsoft.com/pt-br/windows/wsl/install)

---

## ⬇️ Downloads

- 🔗 https://www.virtualbox.org/wiki/Downloads
- 🔗 https://ubuntu.com/download/server

Instale o VirtualBox normalmente.

---

## 🧩 Criando a Máquina Virtual

1. Abra o VirtualBox
2. Clique em `Novo`
3. Configure:
- Nome da VM
- ISO do Ubuntu Server
- Usuário e senha
- Recursos:
  - Memória RAM
  - CPU
  - Disco

💡 **Dicas:**

- Pode manter o tipo de disco padrão

- Não precisa pré-alocar espaço
4. Clique em `Finalizar`

---

## ⚙️ Ajustes da VM

### 📀 Armazenamento

- Vá em `Detalhes` → `Armazenamento`
- Selecione o CD
- Em `Atributos`, escolha a ISO

### 🌐 Rede

- Vá em `Rede`
- Selecione: **Placa em modo Bridge**
- Escolha seu adaptador de rede

---

## 🐧 Instalação do Ubuntu Server

Siga o instalador padrão:

- Idioma
- Layout do teclado
- Rede
- Proxy (se necessário)
- Mirror (padrão)
- Particionamento (padrão)

### 👤 Criação do usuário

- Nome
- Nome do servidor
- Username
- Senha

### 🔧 Recomendações

- ✅ Marcar **OpenSSH**
- ✅ Selecionar serviços extras (Docker, AWS CLI, etc.)

---

## 🧹 Pós-instalação

Após concluir:

Dispositivos → Discos Ópticos

Remova a ISO do Ubuntu.

---

## 🌐 Acesso via SSH

Se não instalou o OpenSSH:

```bash
sudo apt-get install openssh-server
```

Para descobrir o IP da VM:

```bash
ip a
```

Com as informações do IP e openssh instalado, use um cliente SSH no Windows, exemplo:

- [Download PuTTY - a free SSH and telnet client for Windows](https://putty.org/index.html)

Se tudo estiver correto, o acesso funcionará normalmente 👍

---

## ✅ Conclusão

Ambiente pronto! 🎉

Agora você tem um **Ubuntu Server rodando em VM**, pronto para estudos, testes ou projetos.

---

## ✍️ Autor

**Douglas Bezerra Ferreira**  
📅 26 de Março de 2026

---

## ⭐ Observação

Se esse guia te ajudou, salva aí no repositório ou usa como base para seus labs 😄

Guia prático para instalação do **Ubuntu Server** no **VirtualBox**, com foco em ambiente de estudos/lab.

---



- 

- Vá em `Detalhes` → `Armazenamento`
- Selecione o CD
- Em `Atributos`, escolha a ISO

### 🌐 Rede

- Vá em `Rede`
- Selecione: **Placa em modo Bridge**
- Escolha seu adaptador de rede

---

## 🐧 Instalação do Ubuntu Server

Siga o instalador padrão:

- Idioma
- Layout do teclado
- Rede
- Proxy (se necessário)
- Mirror (padrão)
- Particionamento (padrão)

### 👤 Criação do usuário

- Nome
- Nome do servidor
- Username
- Senha

### 🔧 Recomendações

- ✅ Marcar **OpenSSH**
- ✅ Selecionar serviços extras se necessário (Docker, AWS CLI, etc.)

---

## 🧹 Pós-instalação

Após concluir:

1. Vá em:
   
   Dispositivos → Discos Ópticos

2. Remova a ISO do Ubuntu

---

## 🌐 Acesso via SSH

Se não instalou o OpenSSH:

sudo apt-get install openssh-server

Para descobrir o IP da VM:

ip a

Use um cliente SSH no Windows:

- 🔗 [PuTTY](https://putty.org/index.html)

Se tudo estiver correto, o acesso funcionará normalmente 👍

---

## ✅ Conclusão

Ambiente pronto! 🎉

Agora você tem um **Ubuntu Server rodando em VM**, pronto para estudos, testes ou projetos.

---

## ✍️ Autor

**Douglas Bezerra Ferreira**  
📅 26 de Março de 2026

---

## ⭐ Observação

Se esse guia te ajudou, salva aí no repositório ou usa como base para seus labs 😄














