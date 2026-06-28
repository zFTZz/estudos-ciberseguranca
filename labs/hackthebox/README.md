# Hack The Box — Lab Notes e Writeups

Este diretório contém modelos, anotações e writeups de laboratórios do Hack The Box (HTB) organizados por tipo/nível e com foco tanto em técnicas ofensivas quanto na perspectiva defensiva (Blue Team).

Objetivo
- Registrar passos, evidências e aprendizados de cada desafio/máquina para estudo pessoal e para ajudar visitantes a entenderem as técnicas exploradas.
- Fornecer uma estrutura clara para navegar pelos materiais e facilitar contribuições.

Estrutura das pastas
- Challenges/
  - Contém writeups e soluções individuais de desafios (máquinas e boxes). Cada desafio deve ter uma pasta própria com nome claro (ex: "BoxName - Categoria").
  - Dentro de cada pasta, inclua arquivos como: `README.md` (resumo), `enumeration.md` (enumeração), `exploit/` (scripts e PoCs), `privilege_escalation.md` e `notes.txt`.

- Fortress(AVANÇADO+)/
  - Máquinas/boxes de nível avançado. Use quando o desafio envolver técnicas complexas (exploração de binários, chaining de exploits, misconfigurations profundas).
  - Estrutura interna similar à `Challenges/`, mas com mais ênfase em PoCs e scripts reutilizáveis.

- Starting Point - Offensive/
  - Conteúdo para iniciantes na trilha ofensiva: guias passo a passo, listas de comandos úteis (enumeração, nmap, enum4linux, smbclient), e exemplos comentados.
  - Ideal para quem está começando em pentesting e quer aprender boas práticas de investigação.

- Sherlocks - Defensive/
  - Writeups e análises com foco defensivo (Blue Team). Cada item deve destacar: indicadores de comprometimento (IOCs), artefatos de logs relevantes, e recomendações de monitoramento/mitigação.
  - Inclua regras de detecção (EDR/IDS/IPS), assinaturas YARA, queries para SIEM, e passos de resposta.

- Tracks - (FOCO REAL DE ESTUDO)/
  - Roteiros de estudo (tracks) agrupando máquinas/temas por competência (ex: web, windows, priv-esc, forense). Projetado para organizar um plano de estudos realista.
  - Cada track pode ter um `README.md` descrevendo objetivos, pré-requisitos e checklist de habilidades.

- General/
  - Recursos gerais: ferramentas, cheatsheets, templates (inclui este template de writeup), referências e links úteis.

Writeup template (padrão)
- Nome do desafio:
- Nível de dificuldade:
- Ferramentas utilizadas:
- Conceito(s) explorado(s): (ex.: misconfiguração, RCE, buffer overflow, credenciais vazadas)
- Passo a passo resumido:
  1. Enumeração inicial (com comandos e resultados principais)
  2. Descoberta da vulnerabilidade / Prova de conceito
  3. Escalada de privilégios / post-exploitation
  4. Remediação sugerida
- Perspectiva defensiva (para Blue Team):
  - Indicadores (IOCs): arquivos, nomes de processos, conexões de rede, comandos suspeitos
  - Artefatos de logs para coletar (ex.: autoruns, event logs, syslog, auditd)
  - Regras de detecção recomendadas (SIEM/EDR/IDS)
- Conclusão / Aprendizado:
- Arquivos anexos: scripts, PoCs, capturas (enumeração, screenshots)

Boas práticas para contribuições
- Nomeie pastas de forma clara: `<NOME_DO_BOX> - <NÍVEL>` ou `track/<nome>-<ordem>`.
- Nunca publique informações que violem regras de HTB (ex.: spoilers antes de liberar publicamente). Respeite a política de divulgação da plataforma.
- Inclua sempre um resumo no `README.md` da pasta do desafio com um link para arquivos maiores (PoC, scripts).
- Use commits explicativos: "add writeup: <BoxName>" / "update defensive notes: <BoxName>".

Como usar este repositório
- Navegue por `labs/hackthebox/` e escolha a pasta que corresponde ao seu objetivo de estudo (iniciantes → "Starting Point", ofensiva prática → "Challenges" ou "Fortress(AVANÇADO+)", defesa → "Sherlocks - Defensive").
- Siga o template de writeup para documentar suas descobertas. Isso facilita revisão posterior e criação de material didático.

Contato / Créditos
- Autor: zFTZz
- Para dúvidas ou contribuições, abra uma issue ou envie um PR com o conteúdo organizado conforme as instruções acima.

---

Esse arquivo foi atualizado para facilitar a navegação e explicar o propósito de cada pasta para visitantes e colaboradores.