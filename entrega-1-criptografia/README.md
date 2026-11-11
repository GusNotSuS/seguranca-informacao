# Entrega 1 - Criptografia Assimétrica e Confiança (Pipeline GameDev)

## 1. Problema e Objetivo
Trata-se de uma solução criptográfica para o envio seguro de assets confidenciais de um jogo 2D (script conceitual `.txt` e *spritesheet* em pixel art `.png`) transmitidos do Diretor Criativo (Emissor) para o Desenvolvedor Principal (Receptor). A solução garante confidencialidade, integridade e autenticidade.

## 2. Ambiente e Tecnologias
* **Linguagem:** Python 3.12 (Google Colab)
* **Biblioteca Criptográfica:** cryptography (PyCA)
* **Armazenamento:** Google Drive / Sistema de Ficheiros

## 3. Algoritmos e Justificativa Técnica
* **AES-256-GCM (Simétrico):** Cifragem do payload contendo os ficheiros binários. Oferece alta performance e autenticação de dados associados.
* **RSA-OAEP (Assimétrico):** Encapsulamento seguro da chave AES de 256 bits utilizando a Chave Pública do Receptor (2048 bits).
* **RSA-PSS (Assinatura Digital):** Garantia de autoria e não-repúdio utilizando a Chave Privada do Emissor sobre o hash SHA-256 do payload cifrado.

## 4. Instruções de Execução
1. Abra o notebook Entrega1_Criptografia_GameDev.ipynb no Google Colab.
2. Certifique-se de que a pasta Segurança da Informação/Entrega 1 no seu Google Drive contém os arquivos heroi.png e script.txt.
3. Execute as células em ordem sequencial (Células 1 a 5).

## 5. Testes e Evidências
* **Teste Positivo (Célula 3):** Demonstração da decifragem bem-sucedida e reconstituição perfeita dos arquivos heroi_restaurado.png e script_restaurado.txt após a validação da assinatura RSA-PSS.
* **Teste Negativo 1 :** Tentativa de decifrar a chave AES utilizando a chave privada incorreta (impostor_priv.pem), resultando no bloqueio da operação pelo RSA-OAEP.
* **Teste Negativo 2 :** Tentativa de validar a assinatura com uma chave pública incorreta (impostor_pub.pem), resultando na exceção InvalidSignature.

## 6. Riscos, Limitações e PKI
* **Gestão de Chaves:** Em ambiente de produção, as chaves privadas não devem ser mantidas em ficheiros locais de disco, sendo recomendado o uso de cofres de segredos (Key Vaults / HSM).
* **Função da PKI e Certificados Digitais:** A infraestrutura de chaves públicas (PKI) e certificados X.509 previnem ataques do tipo *Man-in-the-Middle* ao vincular a chave pública a uma identidade verificada por uma Autoridade Certificadora (AC).

## 7. Referências Consultadas
* OWASP Top 10:2025 (A04 - Falhas Criptográficas)
* OWASP ASVS 5.0 (Capítulos V11 e V12)
* ISO/IEC 27002:2022 (Controle 8.24 - Uso de Criptografia)
