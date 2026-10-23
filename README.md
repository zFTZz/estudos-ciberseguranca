# 🛡️ Security Labs & Technical Write-ups

Repositório dedicado à documentação técnica de laboratórios práticos de segurança ofensiva (Hack The Box e OverTheWire), com foco em **análise pós-exploração e lições aprendidas para detecção (Blue/Purple Team)**.

> ⚖️ **Aviso de Conformidade Ética:** Em estrito respeito aos Termos de Serviço das plataformas educacionais (HTB ToS), este repositório contém **exclusivamente write-ups de máquinas aposentadas (Retired)** e desafios públicos permitidos. Nenhuma solução, dica ou material relativo a máquinas ativas é compartilhado.

---

## 📑 Projetos principais
- **[Purple-Honeypot-SIEM](https://github.com/zFTZz/Purple-Honeypot-SIEM):** Laboratório de telemetria e SIEM com Wazuh e honeypots customizados.
- **[HTB-studies](https://github.com/zFTZz/HTB-studies):** Cheatsheets e base de referência técnica de comandos e sistemas.

---

## 🔎 Hack The Box (Retired Boxes)

Documentação de exploração focada no vetor de ataque e na telemetria deixada no host.

| Máquina | Dificuldade | SO | Vetores Explorados | Documentação |
| :--- | :---: | :---: | :--- | :---: |
| **Lab #01** | Easy | Linux | Misconfiguration / Telnet (No Auth) | [Acessar Write-up](./writeups/htb) |

---

## 🕹️ OverTheWire Wargames

Laboratórios focados em fundamentos de sistemas operacionais, parsing de texto e baixo nível.

| Wargame | Níveis | Tópicos Cobertos | Documentação |
| :--- | :---: | :--- | :---: |
| **Bandit** | 0 $\rightarrow$ 14 | Fundamentos Linux, Streams POSIX, Redirecionamento, Regex & SSH Keys | [Acessar Walkthrough](./writeups/overthewire/bandit-0-to-14.md) |

---

## 📚 Metodologias & Próximos Tópicos

Documentações e metodologias estruturadas em desenvolvimento:

- [x] **Fundamentos de Redes & Shells:** Fluxos I/O, túneis e autenticação via chaves.
- [ ] **Linux Privilege Escalation:** Vetores de abuso em binários SUID, Capabilities e Kernel exploits.
- [ ] **Active Directory Reconnaissance:** Enumeração com BloodHound e suite Impacket.
- [ ] **Windows Internals:** Manipulação de tokens, privilégios de processos e chamadas WinAPI.
