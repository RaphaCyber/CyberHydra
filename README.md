# CyberHydra Pentest Lab — Metasploitable 2 | FTP Brute Force

## Visão Geral

Este projeto apresenta a execução de um laboratório prático de *Penetration Testing (Pentest)* em ambiente controlado, utilizando *Kali Linux, **Metasploitable 2, **Nmap* e *Hydra, com foco na avaliação da segurança do serviço **FTP (File Transfer Protocol)* exposto na porta TCP/21.

O objetivo principal foi reproduzir, de forma controlada e autorizada, um cenário de *ataque baseado em força bruta contra credenciais*, avaliando o impacto de credenciais fracas e previsíveis sobre a segurança de um serviço de rede.

A atividade contempla as principais etapas de um processo de avaliação de segurança, desde a *descoberta e enumeração de serviços* até a *validação de acesso, pós-exploração e demonstração de impacto*.

> *Disclaimer:* Todas as atividades descritas neste projeto foram executadas exclusivamente em ambiente de laboratório controlado, utilizando a máquina vulnerável Metasploitable 2 para fins educacionais e de validação de conceitos de segurança da informação.

---

## Ambiente de Laboratório

O laboratório foi estruturado utilizando uma arquitetura virtualizada, permitindo o isolamento das atividades de segurança e a reprodução segura do cenário de ataque.

### Tecnologias utilizadas

* *Kali Linux* — Plataforma utilizada para execução das ferramentas de segurança e atividades de análise.
* *Metasploitable 2* — Máquina virtual intencionalmente vulnerável utilizada como alvo.
* *VMware* — Plataforma de virtualização utilizada para composição do ambiente.
* *Nmap* — Ferramenta utilizada para descoberta e enumeração de serviços de rede.
* *Hydra* — Ferramenta utilizada para realização de testes de força bruta contra serviços autenticados.
* *RockYou* — Wordlist utilizada para avaliação da robustez das credenciais.

---

# Objetivos do Assessment

O objetivo do laboratório foi simular um cenário de *avaliação de segurança ofensiva*, permitindo compreender o ciclo básico de exploração de um serviço de rede vulnerável.

Durante o assessment foram abordadas as seguintes etapas:

* Descoberta e enumeração da superfície de ataque;
* Identificação de portas e serviços expostos;
* Análise do serviço FTP;
* Identificação de possíveis fragilidades relacionadas à autenticação;
* Execução de teste de força bruta controlado;
* Validação das credenciais identificadas;
* Avaliação do nível de acesso obtido;
* Análise do impacto pós-autenticação;
* Acesso e manipulação de arquivos disponíveis ao usuário;
* Demonstração de potencial exfiltração de informações sensíveis.

---

# Metodologia do Pentest

## 1. Reconhecimento e Enumeração

A primeira etapa consistiu na realização de *reconhecimento ativo da máquina alvo*, utilizando o Nmap para identificar portas TCP abertas e serviços disponibilizados pelo host.

Durante o processo de enumeração, foi identificada a exposição do serviço:

*FTP — TCP/21*

A exposição do serviço FTP representou um dos principais vetores de ataque disponíveis no ambiente de laboratório e passou a ser objeto da análise de segurança.

---

## 2. Avaliação de Autenticação — Brute Force

Após a identificação do serviço FTP, foi realizada uma avaliação da robustez do mecanismo de autenticação.

Para isso, foi utilizado o *Hydra*, ferramenta amplamente empregada em atividades de Security Assessment para validação de credenciais em serviços autenticados.

O teste foi conduzido utilizando a wordlist *RockYou*, permitindo avaliar a existência de credenciais fracas, previsíveis ou presentes em listas de senhas conhecidas.

Durante a execução do teste, foi identificada uma combinação válida de credenciais:

msfadmin / msfadmin

O resultado demonstra uma deficiência significativa no controle de autenticação do serviço, uma vez que foi possível identificar uma credencial extremamente previsível.

---

## 3. Validação de Acesso ao Serviço FTP

Após a identificação da credencial, foi realizada a validação manual do acesso diretamente no serviço FTP.

O processo confirmou que as credenciais descobertas durante o teste de força bruta eram válidas, permitindo autenticação no servidor.

Essa etapa teve como finalidade eliminar falsos positivos e comprovar efetivamente a existência da vulnerabilidade identificada.

---

## 4. Análise de Acesso e Manipulação de Arquivos

Com o acesso autenticado estabelecido, foi realizada uma análise do nível de exposição proporcionado pelo serviço.

Durante essa etapa, foram avaliados diretórios e recursos disponíveis ao usuário autenticado, incluindo estruturas como:

* /etc
* /bin
* /root

A análise permitiu demonstrar que uma vulnerabilidade aparentemente limitada à camada de autenticação poderia resultar em uma exposição significativamente maior dependendo das permissões associadas à conta comprometida.

---

## 5. Pós-Exploração e Exfiltração de Dados

Na etapa de pós-exploração, foi realizada uma demonstração controlada de acesso a informações do sistema.

Como prova de conceito (*PoC — Proof of Concept*), foi obtido o arquivo:

/etc/passwd

O arquivo contém informações relacionadas às contas locais existentes no sistema e, embora não armazene diretamente as senhas em sistemas Linux modernos, sua exposição pode fornecer informações relevantes para etapas posteriores de um processo de exploração.

Essa atividade foi utilizada exclusivamente para demonstrar o impacto potencial decorrente do comprometimento das credenciais.

---

# Resultados do Assessment

| *Etapa*                 | *Resultado*                                             |
| ------------------------- | --------------------------------------------------------- |
| Reconhecimento            | Host e serviços de rede identificados                     |
| Enumeração                | Serviço FTP identificado na porta TCP/21                  |
| Avaliação de autenticação | Vulnerabilidade associada a credencial fraca identificada |
| Brute Force               | Credencial msfadmin/msfadmin identificada               |
| Validação                 | Autenticação FTP realizada com sucesso                    |
| Pós-exploração            | Acesso a recursos do sistema validado                     |
| Exposição de dados        | Arquivo /etc/passwd obtido como PoC                     |

---

# Análise de Impacto

O laboratório demonstrou como uma *falha aparentemente simples na política de autenticação* pode representar um risco significativo para a segurança de um ambiente.

A utilização de uma credencial previsível possibilitou sua descoberta por meio de ataque automatizado de força bruta. Após a autenticação, o nível de acesso disponibilizado pelo serviço permitiu a realização de atividades adicionais de enumeração e acesso a informações do sistema.

Em um ambiente corporativo real, um cenário semelhante poderia resultar em:

* Comprometimento de contas;
* Acesso indevido a informações corporativas;
* Movimentação lateral;
* Escalada de privilégios, dependendo das permissões disponíveis;
* Persistência no ambiente;
* Exfiltração de informações;
* Utilização do host comprometido como ponto de apoio para novos ataques.

---

# Recomendações de Segurança

Com base nos resultados observados, recomenda-se a implementação de controles preventivos e detectivos, incluindo:

### Controle de credenciais

* Implementação de políticas robustas de senha;
* Bloqueio de credenciais padrão;
* Utilização de senhas complexas e não previsíveis;
* Implementação de MFA sempre que tecnicamente aplicável;
* Rotação periódica de credenciais privilegiadas.

### Segurança do serviço FTP

* Desabilitar serviços FTP que não sejam necessários;
* Priorizar protocolos seguros, como *SFTP/SSH*;
* Restringir o acesso ao serviço por meio de firewall e ACLs;
* Limitar os diretórios disponíveis para usuários FTP;
* Aplicar o princípio do menor privilégio.

### Monitoramento e Detecção

* Implementação de logs e auditoria de autenticação;
* Monitoramento de tentativas consecutivas de login;
* Detecção de comportamento compatível com ataques de força bruta;
* Integração dos eventos de segurança com soluções de *SIEM*;
* Criação de mecanismos de bloqueio ou rate limiting.

---

# Conclusão

O laboratório demonstrou, de forma prática, o ciclo de um *Security Assessment orientado à identificação e exploração controlada de vulnerabilidades relacionadas à autenticação*.

A partir da etapa de reconhecimento e enumeração, foi possível identificar um serviço FTP exposto na porta TCP/21. Posteriormente, a utilização de técnicas de força bruta permitiu identificar uma credencial fraca e previsível, possibilitando a validação do acesso ao serviço.

A exploração controlada demonstrou que o comprometimento de credenciais pode representar um ponto inicial para atividades de pós-exploração e exposição de informações do sistema.

O exercício reforça a importância da aplicação de *defesa em profundidade (Defense in Depth)*, políticas robustas de autenticação, princípio do menor privilégio, redução da superfície de ataque, monitoramento contínuo e utilização de protocolos seguros.

Do ponto de vista de *Cyber Security*, o laboratório evidencia que a segurança de um ambiente não depende exclusivamente da existência de controles tecnológicos, mas também da correta configuração dos serviços, gestão de identidades e credenciais e capacidade de detecção e resposta a comportamentos anômalos.

---

## Classificação do Laboratório

*Tipo:* Penetration Testing / Security Assessment
*Categoria:* Credential Security / Network Security
*Vetor:* Brute Force
*Protocolo:* FTP
*Porta:* TCP/21
*Plataforma Alvo:* Metasploitable 2
*Ferramentas:* Nmap, Hydra, Kali Linux
*Ambiente:* Virtualizado e controlado
*Finalidade:* Educação, pesquisa e validação de controles de segurança
