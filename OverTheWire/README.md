# OverTheWire

Este diretório centraliza os wargames do OverTheWire e serve como repositório de estudos, anotações, comandos e soluções para cada jogo.

Objetivo

- Fornecer um espaço organizado para documentar o que você aprendeu ao resolver os desafios do OverTheWire.
- Manter por jogo: notas por nível, comandos utilizados, payloads, explicações, referências e possíveis mitigações.
- Facilitar estudos posteriores e a reprodução dos passos (inclua exemplos de conexão SSH, comandos curl, dumps e scripts quando necessário).

Como usar

- Cada jogo fica em OverTheWire/<Jogo>/ — por exemplo `OverTheWire/Bandit/`, `OverTheWire/Natas/`, `OverTheWire/Leviathan/`.
- Para jogos baseados em níveis (como Bandit), crie subdiretórios numerados por nível: `01/`, `02/`, etc., e adicione um `README.md` em cada nível com notas e comandos.
- Para jogos de binários/criptografia, mantenha uma pasta por desafio com análise, PoC e scripts.

Descrição rápida dos diretórios principais

- Bandit: Fundamentos de Unix/Linux — comandos de shell, permissões, manipulação de arquivos, navegação e operações básicas do sistema.

- Natas: Segurança Web — vulnerabilidades em aplicações web, manipulação de requisições, injeções (XSS, SQLi), lógica de autenticação e configuração de servidores.

- Leviathan: Engenharia reversa básica e escalada de privilégios em sistemas Linux — análise de binários simples, uso de strings, gdb, strace, objdump e investigação de vetores de escalonamento.

- desafios_secundarios/: coleção de wargames focados em criptografia, exploração de binários e engenharia reversa. Contém subpastas como:
  - Krypton: Criptografia — cifras clássicas, análise de frequência, quebra de chaves simples e scripts de auxílio.
  - Narnia: Exploração de binários e engenharia reversa inicial.
  - Behemoth: Exploração de binários e engenharia reversa avançada (heap, fuzzing, bypass de mitigations).
  - Utumno: Exploração de binários com ênfase em desafios complexos de reverso/exploit.
  - Maze: Exploração de binários e engenharia reversa — mais desafios para praticar conceitos avançados.

Sugestão de ordem de estudo

1. Bandit — consolida fundamentos do Unix/Linux.
2. Escolha um foco inicial:
   - Natas (web) ou Krypton (criptografia) — para variar o conjunto de habilidades.
   - Leviathan (reverso básico) para começar a aprender análise de binários.
3. Progredir para Narnia → Behemoth → Utumno → Maze para aprofundar exploração de binários e técnicas avançadas.

Boas práticas para o repositório

- Não publique credenciais de conta ou flags completas em writeups públicos.
- Não “spoil” os desafios sem avisar (marque spoilers quando necessário).
- Use branches e pull requests para mudanças significativas; mantenha commits descritivos.
- Inclua referências e ferramentas usadas (ex.: comandos, scripts, versões das ferramentas).
- Limpe arquivos temporários e não versionáveis (use .gitignore quando necessário).

Contribuições

Sinta-se livre para adicionar novos níveis, writeups, scripts e notas. Prefira enviar mudanças em uma branch e abrir um PR para revisão.
