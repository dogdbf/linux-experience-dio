# 🔐 Permissões no Ubuntu

Guia prático sobre o sistema de permissões no Linux (rwx) e como utilizar `chmod` e `chown`.

---

## 📚 Sumário

1. [Sistema de Permissões](#-sistema-de-permiss%C3%B5es-no-linux)
2. [Como Ler Permissões](#-como-ler-permiss%C3%B5es-na-pr%C3%A1tica)
3. [chmod Numérico](#-alterando-permiss%C3%B5es-com-chmod-num%C3%A9rico)
4. [chmod com Letras](#-chmod-com-letras-x-r-w)
5. [chown - Alterar Dono/Grupo](#-alterar-dono-e-grupo-chown)
6. [Casos de Uso Práticos](#-casos-de-uso-pr%C3%A1ticos)

---

## 🔐 Sistema de Permissões no Linux

### Estrutura visual

```
d rw- r-- r--
│ │   │   │
│ │   │   └── Outros (others)
│ │   └────── Grupo (group)
│ └────────── Dono (owner/user)
└──────────── Tipo do arquivo
```

---

## 🔹 Tipo do Arquivo (1º caractere)

| Símbolo | Significado              |
| ------- | ------------------------ |
| `-`     | Arquivo comum            |
| `d`     | Diretório                |
| `l`     | Link simbólico           |
| `c`     | Dispositivo de caractere |
| `b`     | Dispositivo de bloco     |

---

## 📖 Permissões (próximos 9 caracteres)

Cada nível tem **3 permissões**:

```
rwx = Leitura + Escrita + Execução
│    │        │        │
│    │        │        └── Execução (execute)
│    │        └───────────── Escrita (write)
│    └──────────────────────── Leitura (read)
```

| Letra | Significado        | Valor numérico |
| ----- | ------------------ | -------------- |
| `r`   | Leitura (read)     | 4              |
| `w`   | Escrita (write)    | 2              |
| `x`   | Execução (execute) | 1              |
| `-`   | Sem permissão      | 0              |

---

## 📝 Como Ler Permissões na Prática

### Exemplo 1: Arquivo comum

```
-rw-r--r-- 1 root root 0 mar 31 03:05 arquivo_root.txt
```

**Quebra das permissões:**

```
-   rw-  r--  r--
│   │    │    │
│   │    │    └── Outros → read only (4)
│   │    └─────── Grupo → read only (4)
│   └──────────── Dono → read + write (6)
└───────────────── Arquivo comum (-)
```

**Interpretação:**

- **Dono (`root`)**: pode ler e escrever
- **Grupo (`root`)**: só pode ler
- **Outros**: só podem ler
- **Notação numérica**: `644`

---

### Exemplo 2: Diretório

```
drwxr-x--- 5 douglasdbf douglasdbf 4096 mar 31 00:21 douglasdbf
```

**Quebra das permissões:**

```
d   rwx  r-x  ---
│   │    │    │
│   │    │    └── Outros → sem acesso (0)
│   │    └─────── Grupo → read + execute (5)
│   └──────────── Dono → rwx (7)
└───────────────── Diretório (d)
```

**Interpretação:**

- **Dono**: acesso total (ler, escrever, acessar)
- **Grupo**: pode acessar e listar arquivos
- **Outros**: sem acesso nenhum
- **Notação numérica**: `750`

---

## 🔢 Alterando Permissões com chmod (Numérico)

### Como funciona

bash

```bash
chmod XYZ arquivo_ou_diretorio
      ││└── Outros (others)
      │└─── Grupo (group)
      └──── Dono (user)
```

Cada posição é a **soma** das permissões:

| Permissão     | Valor |
| ------------- | ----- |
| `r` (read)    | 4     |
| `w` (write)   | 2     |
| `x` (execute) | 1     |

---

### Exemplos práticos de cálculo

#### Exemplo A: chmod 755 (comum em scripts)

```
7 = 4+2+1 = rwx  (dono)
5 = 4+0+1 = r-x  (grupo)
5 = 4+0+1 = r-x  (outros)
```

**Resultado:**

bash

```bash
chmod 755 script.sh
```

- Dono: pode tudo
- Grupo e Outros: podem ler e executar

---

#### Exemplo B: chmod 750 (recomendado para diretórios sensíveis)

```
7 = 4+2+1 = rwx  (dono)
5 = 4+0+1 = r-x  (grupo)
0 = 0+0+0 = ---  (outros)
```

**Resultado:**

bash

```bash
chmod 750 /adm/
```

- Dono: acesso total
- Grupo: pode acessar
- Outros: sem acesso

---

#### Exemplo C: chmod 644 (padrão para arquivos)

```
6 = 4+2+0 = rw-  (dono)
4 = 4+0+0 = r--  (grupo)
4 = 4+0+0 = r--  (outros)
```

**Resultado:**

bash

```bash
chmod 644 arquivo.txt
```

- Dono: pode ler e escrever
- Grupo e Outros: só podem ler

---

### Tabela de padrões comuns

| Número | Permissão   | Uso                                  |
| ------ | ----------- | ------------------------------------ |
| `700`  | `rwx------` | Diretório privado                    |
| `750`  | `rwxr-x---` | Diretório compartilhado (grupo)      |
| `755`  | `rwxr-xr-x` | Script executável público            |
| `600`  | `rw-------` | Arquivo privado (senhas, chaves SSH) |
| `644`  | `rw-r--r--` | Arquivo padrão                       |
| `777`  | `rwxrwxrwx` | Sem restrições (evite usar!)         |

---

## 🔤 chmod com Letras (+x, -r, +w)

Além da forma numérica, `chmod` aceita **letras** para ajustes finos.

### Estrutura

bash

```bash
chmod [quem][ação][permissão] arquivo
```

### Componentes

**Quem:**

- `u` = user (dono)
- `g` = group (grupo)
- `o` = others (outros)
- `a` = all (todos)

**Ação:**

- `+` = adiciona permissão
- `-` = remove permissão
- `=` = define exatamente

**Permissão:**

- `r` = read
- `w` = write
- `x` = execute

---

### Exemplos práticos

#### ➕ Adicionar execução apenas para o dono

bash

```bash
chmod u+x script.sh
```

**Antes:**

```
-rw-r--r--
```

**Depois:**

```
-rwxr--r--
```

---

#### ➕ Adicionar execução para todos

bash

```bash
chmod a+x script.sh
```

**Antes:**

```
-rw-r--r--
```

**Depois:**

```
-rwxr-xr-x
```

---

#### ➕ Dar leitura e execução para o grupo

bash

```bash
chmod g+rx arquivo.txt
```

---

#### ➖ Remover execução de outros

bash

```bash
chmod o-x script.sh
```

---

#### 🎯 Definir exatamente (equivalente a 750)

bash

```bash
chmod u=rwx,g=rx,o= arquivo.txt
```

**Resultado:**

```
-rwxr-x---
```

---

### ⚠️ Diferença: Numérico vs Letras

| Tipo                | Comportamento                      | Uso                        |
| ------------------- | ---------------------------------- | -------------------------- |
| **Numérico** (750)  | Define TUDO de uma vez (substitui) | Quando quer controle total |
| **Letras** (+x, -x) | Altera apenas uma parte            | Quando quer ajustes finos  |

**Exemplo:**

Arquivo atual:

```
-rw-r--r--  (644)
```

Se fizer `chmod +x arquivo`:

```
-rwxr-xr-x  (755) ← execução adicionada pra todos
```

Se fizer `chmod 644 arquivo`:

```
-rw-r--r--  (644) ← volta exatamente a isso
```

---

## 📁 Alterar Dono e Grupo (chown)

### Alterar dono e grupo

bash

```bash
chown flavia:GRP_ADM /adm/
```

Significa: usuário `flavia` e grupo `GRP_ADM` são os donos do diretório `/adm/`.

---

### Alterar apenas o dono

bash

```bash
chown flavia /adm/
```

---

### Alterar apenas o grupo

bash

```bash
chown :GRP_ADM /adm/
```

---

### Aplicar recursivamente (em todo diretório e subdiretórios)

bash

```bash
chown -R flavia:GRP_ADM /adm/
```

O `-R` garante que tudo dentro de `/adm/` também mude de dono.

---

## 🔐 Casos de Uso Práticos

### Caso 1: Arquivo de configuração sensível

**Objetivo:** Apenas `root` pode ler/editar.

bash

```bash
touch /etc/config_privado.conf
chmod 600 /etc/config_privado.conf
chown root:root /etc/config_privado.conf
```

**Resultado:**

```
-rw-------  root  root  /etc/config_privado.conf
```

---

### Caso 2: Diretório compartilhado entre grupo

**Objetivo:** `root` tem total acesso, grupo `developers` pode ler/executar, outros sem acesso.

bash

```bash
mkdir /adm/
chown root:GRP_DEVOPS /adm/
chmod 750 /adm/
```

**Resultado:**

```
drwxr-x---  root  GRP_DEVOPS  /adm/
```

---

### Caso 3: Script executável para todos

**Objetivo:** Todos podem executar, mas apenas `root` pode editar.

bash

```bash
chmod 755 /usr/local/bin/backup.sh
chown root:root /usr/local/bin/backup.sh
```

**Resultado:**

```
-rwxr-xr-x  root  root  /usr/local/bin/backup.sh
```

---

### Caso 4: Arquivo com permissão restrita (ex: chave SSH)

**Objetivo:** Apenas o dono pode ler (segurança).

bash

```bash
chmod 600 ~/.ssh/id_rsa
chown $USER:$USER ~/.ssh/id_rsa
```

**Resultado:**

```
-rw-------  douglas  douglas  ~/.ssh/id_rsa
```

---

### Caso 5: Dar acesso de grupo a arquivo do root

**Objetivo:** Arquivo de `root`, mas grupo `admins` também pode ler e executar.

bash

```bash
touch /arquivo_root.txt
chown root:GRP_ADM /arquivo_root.txt
chmod 750 /arquivo_root.txt
```

**Resultado:**

```
-rwxr-x---  root  GRP_ADM  /arquivo_root.txt
```

---

## 📊 Resumo Rápido

| Ação            | Comando                     |
| --------------- | --------------------------- |
| Dar execução    | `chmod u+x arquivo`         |
| Remover escrita | `chmod g-w arquivo`         |
| Definir 755     | `chmod 755 arquivo`         |
| Definir 600     | `chmod 600 arquivo`         |
| Mudar dono      | `chown user:group arquivo`  |
| Mudar recursivo | `chown -R user:group /dir/` |
| Ver permissões  | `ls -l arquivo`             |

---

## 🎯 Boas Práticas

✅ **Sempre faça:**

- Use `chmod 600` para arquivos sensíveis (senhas, chaves SSH)
- Use `chmod 755` para scripts públicos
- Use `chmod 750` para diretórios com dados sensíveis
- Verifique com `ls -l` antes de fazer mudanças críticas

❌ **Nunca faça:**

- Use `chmod 777` (sem restrições)
- Não deixe chaves SSH com permissão aberta
- Não dê `sudo` a usuários desnecessários

---

## 📞 Referências

- Man pages: `man chmod`, `man chown`, `man ls`
- Simulador online: [https://chmod-calculator.com/](https://chmod-calculator.com/)

---

✍️ **Autor:** Douglas Bezerra Ferreira  
📅 **Data:** 31 de Março de 2026  
🏷️ **Versão:** 1.0 (Refatorado e completo)

---

💡 **Se esse guia te ajudou, compartilha ou contribui no repositório!** ⭐
