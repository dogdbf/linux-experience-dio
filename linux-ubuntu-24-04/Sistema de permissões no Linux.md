# Permissões no ubuntu## 🔐 Sistema de permissões no Linux

### 📌 Estrutura geral:

d rw- r-- r--  
│  │    │   │  
│  │    │   └── Outros  
│  │    └───── Grupo  
│  └──────── Dono  
│
└─────────── Tipo do arquivo

---

## 🔹 1º caractere → **Tipo do arquivo**

| Símbolo | Significado              |
| ------- | ------------------------ |
| `-`     | Arquivo comum            |
| `d`     | Diretório                |
| `l`     | Link simbólico           |
| `c`     | Dispositivo de caractere |
| `b`     | Dispositivo de bloco     |

Cada arquivo/diretório tem **3 níveis de acesso**:

| Dono  | Grupo | Outros |
| ----- | ----- | ------ |
| R W X | R W X | R W X  |

E cada letra significa:

- `R` → leitura (read)
- `W` → escrita (write)
- `X` → execução (execute)

---

## 📌 Como ler na prática

Exemplo:

-rw-r--r-- 1 root root 0 mar 31 03:05 arquivo_root.txt

### Quebra da permissão:

- rw- r-- r--  
  │   │   │  
  │   │   └── Outros → só leitura  
  │   └────── Grupo → só leitura  
  └────────── Dono → leitura e escrita

### Interpretação:

- Dono (`root`) → pode ler e escrever
- Grupo (`root`) → só pode ler
- Outros → só podem ler

---

## 📁 Exemplo com diretório

drwxr-x--- 5 douglasdbf douglasdbf 4096 mar 31 00:21 douglasdbf

### Quebra:

d rwx r-x ---  
  │   │   │  
  │   │   └── Outros → nenhum acesso  
  │   └────── Grupo → leitura e execução  
  └────────── Dono → total acesso

### Interpretação:

- `d` → é um diretório
- Dono → pode tudo (ler, escrever, acessar)
- Grupo → pode acessar e listar
- Outros → não podem fazer nada

| Dono  | Grupo | Outros |
| ----- | ----- | ------ |
| R W X | R W X | R W X  |

## 📁 Alterar dono e grupo de arquivos/diretórios (`chown`)

### Alterar dono e grupo (change owner)

```bash
chown flavia:GRP_adm /adm/  
chown douglas:GRP_ven /ven/  
```

### Explicação:

- `flavia:GRP_adm` → usuário `flavia` e grupo `GRP_adm`  
- `douglas:GRP_ven` → usuário `douglas` e grupo `GRP_ven`  

---  

### Alterar apenas o dono

```bash
chown flavia /adm/  
```

---  

### Alterar apenas o grupo

```bash
chown :GRP_adm /adm/  
```

---  

### Aplicar recursivamente (tudo dentro do diretório)

```bash
chown -R flavia:GRP_adm /adm/  
```

## 🔐 Alterando permissões de diretório (`chmod`)

```
chmod XYZ  
      ││└── Outros  
      │└──── Grupo  
      └────── Dono  
```

### Exemplo prático

Objetivo:  

- Dono → acesso total (rwx)  
- Grupo → leitura e execução (r-x)  
- Outros → sem acesso (---)  

```bash
chmod 750 /adm/  
```

---  

### 🧠 Como funciona o número `750`

Cada número representa a soma das permissões:  

| Permissão | Valor |
| --------- | ----- |
| R (read)  | 4     |
| W (write) | 2     |
| X (exec)  | 1     |

---  

### 📊 Conversão do exemplo

| Nível  | Permissão | Cálculo | Valor |
| ------ | --------- | ------- | ----- |
| Dono   | rwx       | 4+2+1   | 7     |
| Grupo  | r-x       | 4+0+1   | 5     |
| Outros | ---       | 0+0+0   | 0     |

👉 Resultado final:  

```
750  
```

---  

### 📌 Tabela de padrões numéricos do `chmod`

| Número | Permissão | Significado                 |
| ------ | --------- | --------------------------- |
| 7      | rwx       | leitura, escrita e execução |
| 6      | rw-       | leitura e escrita           |
| 5      | r-x       | leitura e execução          |
| 4      | r--       | somente leitura             |
| 3      | -wx       | escrita e execução          |
| 2      | -w-       | somente escrita             |
| 1      | --x       | somente execução            |
| 0      | ---       | nenhuma permissão           |

---  

## 🔐 Permitir acesso de grupo a arquivo do root

### 🎯 Cenário

Você tem um arquivo pertencente ao `root` e deseja que **outro grupo** também tenha:  

- leitura (r)  
- execução (x)  

---  

### 🧪 Passo a passo

#### 1️⃣ Criar o arquivo como root

```bash
touch /arquivoroot.txt  
```

---  

#### 2️⃣ Alterar o grupo do arquivo

```bash
chown root:GRP_ADM /arquivoroot.txt  
```

👉 Agora:  

- Dono → root  
- Grupo → GRP_ADM  

---  

#### 3️⃣ Definir permissões

```bash
chmod 750 /arquivoroot.txt  
```

---  

### 📊 Resultado final

| Nível  | Permissão | Descrição                 |
| ------ | --------- | ------------------------- |
| Dono   | rwx       | root tem acesso total     |
| Grupo  | r-x       | grupo pode ler e executar |
| Outros | ---       | sem acesso                |

---

## 🔐 chmod com letras (+x, +r, +w)

Além da forma numérica (750, 755), o `chmod` também pode ser usado com **letras**.  

---  

### 🧠 Estrutura do comando

```bash
chmod [quem][ação][permissão] arquivo  
```

### Onde:

- **quem**:  

- `u` → dono (user)  

- `g` → grupo (group)  

- `o` → outros (others)  

- `a` → todos (all)  

- **ação**:  

- `+` → adiciona permissão  

- `-` → remove permissão  

- `=` → define exatamente aquela permissão  

- **permissão**:  

- `r` → leitura  

- `w` → escrita  

- `x` → execução  

---  

## 📌 Exemplos práticos

### ➕ Adicionar execução para o dono

```bash
chmod u+x arquivo.sh  
```

👉 Só o dono passa a poder executar  

---  

### ➕ Adicionar execução para todos

```bash
chmod a+x arquivo.sh  
```

👉 Todo mundo pode executar  

---  

### ➕ Dar leitura e execução para o grupo

```bash
chmod g+rx arquivo.txt  
```

---  

### ➖ Remover execução de outros

```bash
chmod o-x arquivo.sh  
```

---  

### 🎯 Equivalente ao `chmod 750`

Você pode montar assim:  

```bash
chmod u=rwx,g=rx,o= arquivo.txt  
```

👉 Isso significa:  

- dono → rwx  
- grupo → r-x  
- outros → nenhuma permissão  

---  

## ⚠️ Diferença importante

### 🔢 Numérico (750)

- Define TUDO de uma vez (substitui)  

### 🔤 Letras (+x, -x)

- Altera apenas uma parte  
- Mantém o restante intacto  

---  

## 🧠 Exemplo comparativo

Arquivo atual:  

```
-rw-r--r--  
```

Se fizer:  

```bash
chmod +x arquivo  
```

Resultado:  

```
-rwxr-xr-x  
```

👉 Ele adiciona `x` para todos, sem mexer no resto  

---  

## 🎯 Dica prática

- Use **numérico** quando quiser definir tudo  
- Use **letras** quando quiser fazer ajustes finos  

---

✍️ Autor

**Douglas Bezerra Ferreira**  
📅 26 de Março de 2026

---

## ⭐ Observação

Se esse guia te ajudou, salva aí no repositório ou usa como base para seus labs 😄
