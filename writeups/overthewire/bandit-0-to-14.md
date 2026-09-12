# OverTheWire: Bandit (Levels 0 → 14) — Linux Fundamentals & CLI Mastery

- **Plataforma:** OverTheWire Wargames
- **Wargame:** Bandit
- **Níveis Documentados:** 0 a 14
- **Foco Técnico:** Administração Linux, Manipulação de Streams (POSIX), Expressões Regulares, Arquivos Binários e Autenticação SSH.
- **Autor:** Felipe (zFTZz)

---

## 📌 Sumário Executivo dos Níveis

| Nível | Conceito / Desafio Principal | Ferramenta / Comando Chave |
| :---: | :--- | :--- |
| **0 → 1** | Conexão SSH em porta não-padrão | `ssh -p 2220` |
| **1 → 2** | Ambiguidade de traço (`-`) como *stdin* | `cat ./-` ou `cat < -` |
| **2 → 3** | Espaços em branco no sistema de arquivos | `cat "./--spaces in this filename--"` |
| **3 → 4** | Arquivos ocultos (Dotfiles) | `ls -la` |
| **4 → 5** | Inspeção de tipos de arquivo (MIME/ASCII) | `file ./*` |
| **5 → 6** | Busca avançada com predicados e negação | `find . -type f -size 1033c ! -executable` |
| **6 → 7** | Busca no sistema e supressão de *stderr* | `find / -user ... -group ... 2>/dev/null` |
| **7 → 8** | Filtragem de padrões em grandes volumes | `grep millionth data.txt` |
| **8 → 9** | Tratamento de duplicatas adjacentes | `sort data.txt \| uniq -u` |
| **9 → 10** | Extração de caracteres legíveis de binários | `strings -a data.txt \| grep ==` |
| **10 → 11** | Decodificação de stream em Base64 | `base64 -d data.txt` |
| **11 → 12** | Cifra de Substituição / Rotação (ROT13) | `tr 'a-zA-Z' 'n-za-mN-ZA-M'` |
| **12 → 13** | Engenharia reversa de compactação e Hexdump | `xxd -r`, `tar`, `gzip`, `bzip2`, `file` |
| **13 → 14** | Autenticação por chave privada assimétrica | `ssh -i sshkey.private -p 2220` |

---

## 🛠️ Resolução Detalhada por Nível

### Level 0 → 1: Conexão Remota Básica
* **Objetivo:** Acessar o servidor remoto via SSH em uma porta alternativa.
* **Comando:**
  ```bash
  ssh bandit0@bandit.labs.overthewire.org -p 2220
  cat readme
  ```
* **Conceito:** O utilitário `ssh` utiliza por padrão a porta 22/TCP. O parâmetro `-p` força a conexão para a porta personalizada `2220`.
* **Flag Obtida:** `6y2k****************************`

---

### Level 1 → 2: Ambiguidade de Nomes Reservados (`-`)
* **Objetivo:** Ler um arquivo cujo nome é exatamente um hífen/traço (`-`).
* **Comando:**
  ```bash
  cat ./-
  # Alternativa via redirecionamento de descritor:
  cat < -
  ```
* **Conceito Técnico:** Na maioria dos utilitários POSIX, o caractere `-` isolado é reservado para representar a entrada padrão (*stdin*). Passar `cat -` força o utilitário a esperar entrada do teclado. Para desambiguar e forçar o tratamento como caminho relativo de arquivo, utiliza-se `./-`.
* **Flag Obtida:** `PK8f****************************`

---

### Level 2 → 3: Manipulação de Espaços em Nomes de Arquivos
* **Objetivo:** Ler um arquivo com espaços e traços (`--spaces in this filename--`).
* **Comando:**
  ```bash
  cat "./--spaces in this filename--"
  ```
* **Conceito Técnico:** O interpretador Bash utiliza espaços em branco como delimitadores de argumentos. O uso de aspas duplas (`""`) impede o *word splitting*, garantindo que a string inteira seja interpretada como um único argumento de caminho.
* **Flag Obtida:** `7ZZ2****************************`

---

### Level 3 → 4: Enumeração de Arquivos Ocultos
* **Objetivo:** Acessar o diretório `inhere` e ler um arquivo oculto.
* **Comando:**
  ```bash
  cd inhere && ls -la
  cat ...Hiding-From-You
  ```
* **Conceito Técnico:** Arquivos iniciados por ponto (`.`) são tratados como ocultos pelo sistema de arquivos UNIX. A flag `-a` (*all*) do comando `ls` desativa esse filtro de visualização.
* **Flag Obtida:** `xzTX****************************`

---

### Level 4 → 5: Inspeção de Assinaturas de Arquivos
* **Objetivo:** Identificar o único arquivo legível por humanos dentro de uma série de arquivos (`-file00` a `-file09`).
* **Comando:**
  ```bash
  cd inhere
  file ./*
  cat ./-file07
  ```
* **Conceito Técnico:** Em sistemas UNIX, extensões de arquivos são meramente cosméticas. O comando `file` analisa os *magic numbers* (assinatura de bytes no cabeçalho) para determinar o MIME-type real. Apenas o `-file07` apresentava cabeçalho compatível com `ASCII text`.
* **Flag Obtida:** `6C7h****************************`

---

### Level 5 → 6: Busca Avançada por Metadados
* **Objetivo:** Localizar um arquivo com propriedades estritas: 1033 bytes, legível por humanos e não-executável.
* **Comando:**
  ```bash
  find . -type f -size 1033c ! -executable -exec file '{}' \; | grep ASCII
  # Leitura direta do arquivo identificado:
  cat ./maybehere07/.file2
  ```
* **Conceito Técnico:** Uso avançado de predicados no utilitário `find`:
  * `-type f`: Restringe a busca a arquivos regulares.
  * `-size 1033c`: Filtra pelo tamanho exato de 1033 bytes (`c` = bytes).
  * `! -executable`: Negação lógica (`!`), ignorando arquivos com permissão de execução ativa.
* **Flag Obtida:** `pXa2****************************`

---

### Level 6 → 7: Busca Sistêmica e Redirecionamento de Streams
* **Objetivo:** Varrer todo o sistema operacional buscando um arquivo de 33 bytes de propriedade do usuário `bandit7` e grupo `bandit6`.
* **Comando:**
  ```bash
  find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
  cat /var/lib/dpkg/info/bandit7.password
  ```
* **Conceito Técnico:** Varrer a raiz (`/`) sem privilégios de `root` gera centenas de erros de *Permission Denied*. O operador `2>/dev/null` redireciona o descritor de arquivo 2 (*stderr*) para o dispositivo de descarte nulo (*bit bucket*), mantendo a tela limpa apenas com os resultados bem-sucedidos do *stdout*.
* **Flag Obtida:** `Bmnn****************************`

---

### Level 7 → 8: Filtragem Eficiente com `grep`
* **Objetivo:** Extrair a senha localizada ao lado da palavra-chave `millionth` em um arquivo de grandes proporções (`data.txt`).
* **Comando:**
  ```bash
  grep "millionth" data.txt
  ```
* **Conceito Técnico:** Varredura rápida de texto baseada em correspondência de padrões via expressões regulares sem carregar o buffer completo na memória interativa.
* **Flag Obtida:** `VR1l****************************`

---

### Level 8 → 9: Manipulação de Registros Únicos
* **Objetivo:** Identificar a única linha de texto não-repetida dentro de um arquivo massivo de dados duplicados.
* **Comando:**
  ```bash
  sort data.txt | uniq -u
  ```
* **Conceito Técnico:** O utilitário `uniq` requer que a entrada esteja previamente ordenada, pois ele avalia apenas linhas adjacentes contíguas. O encadeamento via pipe (`|`) garante a ordenação com `sort` antes da extração de ocorrências únicas com `-u`.
* **Flag Obtida:** `EjmO****************************`

---

### Level 9 → 10: Extração de Strings em Dados Binários
* **Objetivo:** Recuperar uma senha precedida de múltiplos caracteres `=` contida dentro de um arquivo compilado/binário.
* **Comando:**
  ```bash
  strings -a data.txt | grep "=="
  ```
* **Conceito Técnico:** O utilitário `strings` percorre sequências binárias identificando cadeias contínuas de caracteres ASCII imprimíveis (por padrão, sequências com comprimento $\ge 4$). Combinado ao `grep`, filtra o ruído de executáveis e bibliotecas.
* **Flag Obtida:** `B0s2****************************`

---

### Level 10 → 11: Decodificação de Dados em Base64
* **Objetivo:** Decodificar dados representados no padrão RFC 4648 (Base64).
* **Comando:**
  ```bash
  base64 -d data.txt
  ```
* **Conceito Técnico:** Base64 não é criptografia, mas sim um esquema de codificação binário-para-texto utilizado para transporte seguro de dados em meios que suportam apenas texto legível.
* **Flag Obtida:** `pYfO****************************`

---

### Level 11 → 12: Cifra de Substituição (ROT13)
* **Objetivo:** Reverter uma cifra de deslocamento de 13 posições (ROT13 / Cifra de César).
* **Comando:**
  ```bash
  cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
  ```
* **Conceito Técnico:** O utilitário `tr` (*translate*) mapeia o conjunto de caracteres da entrada para o conjunto especificado na saída. Como o alfabeto latino possui 26 letras, aplicar uma rotação de 13 posições duas vezes restaura o texto original ($\text{ROT13}(\text{ROT13}(x)) = x$).
* **Flag Obtida:** `GROo****************************`

---

### Level 12 → 13: Engenharia Reversa de Descompressão em Camadas
* **Objetivo:** Restaurar um arquivo ofuscado por um hexdump reverso e sucessivas camadas de compressão (*GZIP*, *BZIP2*, *TAR*).
* **Metodologia de Resolução:**
  1. **Criação de Scratchpad:** Como o diretório home não permite escrita, foi criado um diretório seguro em `/tmp`:
     ```bash
     mktemp -d
     cp data.txt /tmp/tmp.XXXXXX/ && cd /tmp/tmp.XXXXXX/
     ```
  2. **Reversão do Hexdump:**
     ```bash
     xxd -r data.txt > payload.bin
     ```
  3. **Desempacotamento Iterativo Orientado a Cabeçalho:**
     * `file payload.bin` → Identificado **gzip** → `mv payload.bin p.gz && gunzip p.gz`
     * `file p` → Identificado **bzip2** → `mv p p.bz2 && bunzip2 p.bz2`
     * `file p` → Identificado **tar** → `tar -xf p`
     * *Processo repetido através das ferramentas `file`, `tar -xf`, `bunzip2` e `gunzip` até a obtenção do arquivo em texto ASCII.*
* **Conceito Técnico:** Compreensão de empacotamento de arquivos (*TAR*, que apenas une arquivos) versus algoritmos de compactação de fluxo com perdas nulas (*Gzip/Deflate* e *Bzip2/Burrows-Wheeler*).
* **Flag Obtida:** `qQYQ****************************`

---

### Level 13 → 14: Autenticação Criptográfica Assimétrica (SSH Keys)
* **Objetivo:** Autenticar-se como o próximo usuário (`bandit14`) sem senha, utilizando a chave privada presente em `/etc/bandit_pass/` ou no diretório local.
* **Comando:**
  ```bash
  ssh -i sshkey.private bandit14@localhost -p 2220
  # Leitura da credencial do nível:
  cat /etc/bandit_pass/bandit14
  ```
* **Conceito Técnico:** Utilização de criptografia de chave pública no protocolo SSH. O parâmetro `-i` (*identity file*) instrui o cliente a fornecer a chave privada RSA para resolução do desafio criptográfico de autenticação, dispensando o envio de senhas em texto puro.
* **Flag Obtida:** `f3wq****************************`