# Entrega 2 - Hash e Autenticação Segura

# Problema e Objetivo
Solução de gerenciamento de identidade e autenticação para o **Xpresso MMORPG**. O problema central consiste na necessidade de cadastrar e validar contas de jogadores sem armazenar senhas em texto puro ou utilizar funções de resumo genéricas e rápidas (como o SHA-256), garantindo proteção rigorosa contra ataques de força bruta, dicionário, *Rainbow Tables* e engenharia reversa de base de dados.

# Ambiente e Tecnologias
- **Linguagem:** Python 3.10+ (Google Colab)
- **Biblioteca Criptográfica:** `argon2-cffi` (Implementação oficial do algoritmo Argon2id)
- **Armazenamento:** Google Drive (`/content/drive/MyDrive/Segurança da Informação/Entrega 2`) com persistência estruturada em formato JSON (`usuarios_xpresso.json`)

# Algoritmos e Justificativa Técnica
- **Argon2id:** Vencedor do Password Hashing Competition e padrão ouro recomendado pelo OWASP e NIST para derivação de chaves baseadas em senhas. Combina forte resistência a ataques baseados em hardware paralelo (GPUs/ASICs) e canais laterais, exigindo controle de custo de memória (`memory_cost`), tempo/iterações (`time_cost`) e embutindo um *salt* aleatório único por registro.
- **Por que não o SHA-256 isolado?** Funções tradicionais de resumo como o SHA-256 foram projetadas para altíssima velocidade. Para senhas, essa alta performance é uma vulnerabilidade crítica, pois permite que um atacante que obtenha o banco de dados calcule bilhões de tentativas por segundo. Além disso, o SHA-256 padrão carece de *salt* automatizado nativo e de fator de custo de memória ajustável.

# Instruções de Execução
1. Abra o notebook no Google Colab.
2. Certifique-se de instalar a dependência executando a instrução `pip install argon2-cffi -q`.
3. Execute as células do script em ordem sequencial.
4. Conceda a permissão de montagem ao seu Google Drive quando solicitado pelo Colab.
5. Utilize o menu interativo no terminal para **[1]** Registrar nova conta de aventureiro, **[2]** Fazer login no servidor ou **[3]** Sair do jogo.

# Testes
- **Teste Positivo (Cadastro e Login Válido):** Inclusão bem-sucedida de novo usuário, geração correta do hash Argon2id com persistência em nuvem e autenticação bem-sucedida com a credencial exata.
- **Teste Negativo 1 (Prevenção de Duplicidade):** Bloqueio automático de novas contas para nomes de usuário já existentes no banco de dados.
- **Teste Negativo 2 (Falha de Autenticação):** Rejeição imediata de acessos com senhas incorretas (`VerifyMismatchError`), de forma segura e sem vazamento de dados confidenciais.

# Riscos, Limitações e Mitigações
- **Persistência em Arquivo JSON:** Adequada para fins acadêmicos e prototipagem, mas carece de concorrência transacional avançada e recursos de escalabilidade de um SGBD relacional corporativo.
- **Proteção contra Força Bruta na Interface (Rate Limiting):** A mitigação de tentativas massivas automatizadas no nível da interface de login requer a implementação complementar de políticas de limitação de taxa e bloqueio temporário de IP/conta.

# Referências Consultadas
- OWASP Password Storage Cheat Sheet
- OWASP Top 10 / OWASP ASVS
- NIST Special Publication 800-63B (Digital Identity Guidelines)
- ISO/IEC 27002:2022 (Controle de Criptografia e Segurança)