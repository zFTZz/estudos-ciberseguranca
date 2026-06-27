---
# 01 - (Very Easy) - (Red) - Meow
---
---
title: "Meow"
difficulty: "Very Easy"
track: "Network"
team_focus: "Red"
tags: ["telnet","enum","default-credentials"]
date: 2026-06-27
status: "complete"
htblink: "https://www.hackthebox.eu/"
---

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

```
b40abdfe23665f766f9c61ecba8a4c19
```

Flag capturada! ✅

---

**Aprendizado:** Telnet é inseguro demais, transmite tudo em texto plano e ainda permite senha vazia. Moral da história: sempre usar SSH e colocar uma senha forte nas coisas.
