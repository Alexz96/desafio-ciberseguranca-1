# Desafio de Ciberseguranca 1 da DIO

Repositório contendo a solução do Desafio 1 de Cibersegurança da DIO

## Contexto

Com pelo menos 2 máquinas virtuais, uma Kali Linux e outra com Linux MetaSploitable através do software Oracle VirtualBox, uma conexão estável em rede local que possibilite a varredura de serviços e vulnerabilidades conhecidas. Visando a aplicação dos conhecimentos obtidos durante o módulo.

Na pasta de imagens, há exemplos sobre o ambiente usado, que ilustra perícia de configuração e uso do software Oracle VirtualBox.

O desafio exigiu certos conhecimentos pertinentes à redes, protocolos de rede, roteamento, sistemas operacionais e uso do terminal (CLI).

## Ferramentas utilizadas

- Ping;
- NMap;
- Medusa;
- Enum4Linux;

## Técnicas usadas

1. Varredura de rede;
2. Enumeração de serviços;
3. Inspeção de requisições HTTP;
4. Criação de Wordlists (conhecido também como Ataque de Dicionário);
5. Ataques de força bruta;
6. Password Spraying;

# Resultados

Com o uso da metodologia e pensamento OSINT, obtive sucesso na exploração das vulnerabilidades disponíveis no servidor MetaSploitable 2 e nos seus serviços de FTP, SMB (SAMBA) e HTTP, após enumeração de portas de acesso, criação de wordlists mais específicas para o caso de uso e as demais técnicas elucidadas acima. 

Um ponto importante de se citar é que a efetivade do ataque, também depende da capacidade do atacante passar despercebido pelos sistemas de monitoramento do alvo. 

Nota-se que a ferramenta Medusa para ataques de força bruta é essencial na estratégia de ataque, devido à sua alta flexibilidade e recursos.

Tal feito, elucida que não é exigido uma alta qualidade técnica em conhecimentos na área de TI, embora seja algo potencializador, para que se consiga explorar vulnerabilidades em um sistema. Além disso, se aliada à prática de engenharia social, um ataque pode tomar proporções ainda maiores ou com maior efetivade.

## Ataque FTP

O processo realizado está levemente elucidado pelos arquivos elencados abaixo, presentes neste repositório:

- `~/materiais/bruteforce/enumeracao_nmap1.txt`;
- `~/materiais/bruteforce/resultados1.txt`;

### Observações

Com a elaboração de uma wordlist simples, pudemos constatar que o protocolo é facilmente vulnerável, isso sem considerar a versão do serviço disponibilizado pelo MetaSploitable, que com uma busca mais aprofundada em sites como [CVE](https://www.cve.org/), poderemos encontrar mais meios de acesso ao explorar mais vulnerabilidades.

### Possíveis mitigações

- Uma das mais efetivas, não executá-lo ou não expor o serviço se não será usado;
- Utilizar protocolos mais atuais e seguros como o SFTP;
- Aplicar políticas de senha de usuários fortes contendo por exemplo:
  - Pelo menos 8 caracteres;
  - Conter números;
  - Letras maiúsculas e minúsculas;
  - Caracteres especiais;
- Aplicar políticas de rotação de senhas/credenciais aos usuários do serviço;

## Ataque HTTP

Ilustrações do ataque presentes em `~/images/Screenshot_http_0.png`, `~/images/Screenshot_http_1.png` e `~/materiais/bruteforce/resultado_http.txt`.

### Observações

Através da exploração, com DevTools, da requisição feita ao servidor da aplicação, obtivemos o formato esperado da requisição do tipo POST, no formato `username=<usuario>&password=<senha>&Login=Login`. - Ilustrado por Screenshot_http_0.png (em /images neste repositório).

A partir disso, podemos replicar a requisição no Medusa, adicionando os _placeholders_ (`^USER^` e `^PASS^`) da ferramenta para que a mesma realize a substituição automatizada, pelas palavras disponíveis em nosso dicionário, resultando em um texto assim: `’username=^USER^&password=^PASS^&Login=Login’` - ilustrado por Screenshot_http_1.png (em /images neste repositório).

Dito isso, é imprescindível que na execução do comando seja incluso os parâmetros `-U` e `-P`.

### Possíveis mitigações

- Exigir Headers HTTP específicos;
- Exigir usos de Tokens CSRF;
- No processo de login, realizar mascaramento dos campos enviados, tanto em sua nomenclatura quanto em valores enviados;
  - Exigir a configuração de um serviço de segundo fator de autenticação;

## Ataque SMB (SAMBA)

Neste cenário simulado, temos as ilustrações, também na pasta `/images`, sendo elas:

- Screenshot_smb_0.png
- Screenshot_smb_1.png
- Screenshot_smb_2.png
- Screenshot_smb_3.png
- Screenshot_smb_4.png
- Screenshot_smb_users1.png;

Este sendo um protocolo "interno" do sistema, acaba sendo um dos mais críticos em suas vulnerabilidades e eventualmente, por ser mais negligenciado, pode vir a permitir acessos quase que irrestritos a depender de qual o privilégio do usuário em que se consegue o acesso.

### Observações

Neste caso, além de todas as demais técnicas aplicadas, foquei em realizar o ataque utilizando a estratégia de Password Spraying, pois por ter uma característica de realizar tentativas de login em diversos usuários com uma mesma, ou poucas variações de, senha, proporciona um ataque mais oculto ou de difícil observabilidade, dado que os servidores com sistemas operacionais mais atuais podem ter mecanismos de segurança e proteção que identificam estas tentativas.

Pude observar que em alguns cenários, aliando ao ataque via _Phishing_ ou o aproveitamento de dados vazados, este pode ter sua efetivade aprimorada e é um ataque com ampla utilização dado que normalmente os alvos são organizações e sua infraestrutura.

Aqui, além de usarmos o Password Spraying, aliamos à prática o uso do enum4linux, que dada uma vulnerabilidade, conseguimos obter dados de usuários do sistema e dentre outras informações, elucidadas em `materiais/enumeracaosmb/enum4_output1.txt` e em imagens.

A partir da lista de usuários obtidos e filtrados, geramos o arquivo `materiais/enumeracaosmb/smb_users.txt` que foi utilizado posteriormente no Medusa. Ao fim, foi gerado o `materiais/enumeracaosmb/resultados_smb1.txt` que ilustra cenários de sucesso na exploração da vulnerabilidade.

### Possíveis mitigações

- Aplicar restrição de acesso por _allowlist_ de IPs, ou seja, permitir que apenas determinados IPs acessem o serviço;
- Políticas de senhas/credenciais fortes e bem validadas;
- Desativar por completo o protocolo SMB ou;
  - desativar somente o SMBv1, deixando habilitados SMBv2 ou SMBv3 ou;
  - realizar bloqueio das portas TCP pertinentes ao serviço;
- Segmentação de rede (para apenas acesso interno);

# Autor

- [Alexsander de A. Oliveira](https://github.com/Alexz96)
