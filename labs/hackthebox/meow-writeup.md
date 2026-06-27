# Hack The Box - Meow Machine Writeup

## Resumo da Máquina

A máquina **Meow** é um laboratório de iniciação no Hack The Box, classificado como **nível iniciante**, projetado para familiarizar novos penetration testers com conceitos fundamentais de enumeração, identificação de serviços desatualizados e exploração de configurações inseguras.

O objetivo principal é obter acesso ao sistema e capturar a flag de usuário, demonstrando compreensão sobre:
- Reconhecimento e enumeração de serviços
- Exploração de protocolos legados inseguros
- Gerenciamento de credenciais fracas
- Navegação básica em ambientes Linux

---

## Enumeração

### 1. Scan Inicial com Nmap

O primeiro passo na penetração de um alvo é mapear os serviços disponíveis e suas portas abertas. Utilizamos o Nmap para realizar um scan de reconhecimento:

```bash
nmap -sV -p- <IP_DA_MAQUINA>
```

**Parâmetros utilizados:**
- `-sV`: Realiza detecção de versão dos serviços
- `-p-`: Escaneia todas as 65.535 portas
- `<IP_DA_MAQUINA>`: Endereço IP do alvo

### 2. Resultados da Enumeração

O scan revelou:

```
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
```

**Análise do resultado:**
- **Porta 23/tcp**: Serviço Telnet ativo
- **Versão**: Linux telnetd
- **Risco**: **CRÍTICO** - O Telnet é um protocolo de acesso remoto legado sem criptografia, transmitindo credenciais em texto plano

### 3. Investigação Complementar

Realizamos um scan mais agressivo para obter informações adicionais:

```bash
nmap -sC -sV -p 23 <IP_DA_MAQUINA>
```

**Resultado:**
```
23/tcp open  telnet  Linux telnetd
```

A porta estava claramente aberta e receptiva para conexões Telnet.

---

## Exploração

### 1. Tentativa de Conexão via Telnet

Com o serviço identificado, prosseguimos com a tentativa de conexão remota:

```bash
telnet <IP_DA_MAQUINA>
```

### 2. Teste de Credenciais Padrão

Após a conexão ser estabelecida, foi apresentado um prompt de login. Testamos credenciais padrão:

```
Login: root
Password: [deixar em branco - pressionar Enter]
```

**Resultado:** Acesso concedido como usuário **root** com sucesso.

### 3. Análise da Vulnerabilidade

Este acesso foi possível devido a:

1. **Serviço Telnet Desabilitado**: Telnet transmite todas as comunicações, incluindo credenciais, sem criptografia
2. **Senha Vazia**: A conta root não possuía senha configurada
3. **Falta de Política de Segurança**: Ausência de mecanismos de autenticação forte ou restrições de acesso

```bash
root@meow:~#
```

A shell foi obtida com privilégios de superusuário (root).

---

## Captura da Flag

### 1. Navegação no Sistema de Arquivos

Após obter acesso à máquina, iniciamos a busca pela flag:

```bash
pwd
```

**Output:**
```
/root
```

Estávamos no diretório home do usuário root.

### 2. Listagem de Arquivos

```bash
ls -la
```

**Output:**
```
total 24
drwx------ 1 root root 4096 Dec 10 08:00 .
drwxr-xr-x 1 root root 4096 Dec 10 08:00 ..
-rw-r--r-- 1 root root  220 Feb 25  2020 .bashrc
-rw-r--r-- 1 root root  148 Aug 17  2015 .profile
-rw-r--r-- 1 root root   33 Dec 10 08:00 flag.txt
```

Identificamos o arquivo `flag.txt` no diretório atual.

### 3. Leitura da Flag

```bash
cat flag.txt
```

**Output:**
```
b40abdfe23665f766f9c61ecba8a4c19
```

**Flag obtida com sucesso:** `b40abdfe23665f766f9c61ecba8a4c19`

---

## Lições Aprendidas

### 1. Riscos do Telnet

O protocolo Telnet, desenvolvido em 1969, representa um **risco crítico de segurança** em ambientes modernos:

- **Ausência de Criptografia**: Todas as comunicações, inclusive credenciais, trafegam em texto plano
- **Vulnerabilidade a Ataques MITM**: Qualquer ator na rede pode interceptar comunicações
- **Obsolescência**: Completamente substituído pelo SSH (Secure Shell) desde os anos 1990

**Mitigação:**
```bash
# Substituir Telnet por SSH
ssh root@<IP_DA_MAQUINA>

# Desabilitar completamente o serviço Telnet no sistema
sudo systemctl disable telnetd
sudo systemctl stop telnetd
```

### 2. Gestão de Credenciais e Senhas

A vulnerabilidade de senha vazia é uma falha de configuração crítica:

- **Senhas em Branco**: Permitir acesso sem autenticação elimina a primeira camada de segurança
- **Políticas de Senha Fracas**: A ausência de políticas obriga uma senha forte
- **Contas Padrão**: Contas administrativas nunca devem possuir credenciais fracas

**Boas Práticas:**

```bash
# Forçar configuração de senha robusta na primeira autenticação
sudo passwd root

# Implementar politica de senhas fortes
# Arquivo: /etc/login.defs
PASS_MAX_DAYS   90
PASS_MIN_DAYS   1
PASS_MIN_LEN    12
PASS_WARN_AGE   7

# Usar PAM (Pluggable Authentication Modules) para validação
# Arquivo: /etc/security/pwquality.conf
minlen = 12
dcredit = -1
ucredit = -1
ocredit = -1
lcredit = -1
```

### 3. Princípio do Menor Privilégio

O acesso root direto via Telnet viola este princípio fundamental:

- **Separação de Privilégios**: Usuários normais devem acessar sem privilégios administrativos
- **Sudo Configuration**: Apenas ações necessárias devem requerir privilégios elevados
- **Auditoria**: Todas as ações administrativas devem ser registradas

**Configuração Recomendada:**

```bash
# Criar usuário sem privilégios
sudo useradd -m -s /bin/bash usuario_normal

# Permitir sudo apenas para tarefas específicas
# Arquivo: /etc/sudoers
usuario_normal ALL=(ALL) /bin/systemctl restart nginx
usuario_normal ALL=(ALL) /usr/bin/apt update
```

### 4. Importância da Enumeração Adequada

O reconhecimento detalhado foi fundamental para identificar a vulnerabilidade:

- **Mapeamento de Serviços**: Nmap revelou o serviço inseguro
- **Versionamento**: Identificar software desatualizado permite pesquisa de exploits conhecidos
- **Documentação**: Manter registro de todos os serviços facilita análise de risco

---

## Conclusão

A máquina Meow demonstra como **configurações inseguras** e **protocolos legados** representam riscos críticos à segurança da infraestrutura. Através da enumeração metódica e compreensão dos protocolos, foi possível comprometer o sistema rapidamente.

**Pontos-chave para profissionais de segurança:**

✅ Sempre migrar de Telnet para SSH  
✅ Implementar políticas de senha robustas e obrigatórias  
✅ Aplicar o princípio do menor privilégio  
✅ Desabilitar contas padrão ou mudar credenciais imediatamente  
✅ Manter logs detalhados de acesso e auditoria  
✅ Realizar enumeração completa antes de exploração  

Este laboratório reforça a necessidade de vigilância constante sobre a postura de segurança dos sistemas, mesmo em ambientes que parecem simples ou legados.

---

**Máquina:** Meow  
**Dificuldade:** Iniciante  
**Flag:** `b40abdfe23665f766f9c61ecba8a4c19`  
**Data:** 27 de Junho de 2026
