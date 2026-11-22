# Desafio de Ciberseguranca 1 da DIO

Repositório contendo a solução do Desafio 1 de Cibersegurança da DIO

## Contexto

Com pelo menos 2 máquinas virtuais, uma Kali Linux e outra com Linux MetaSploitable através do software Oracle VirtualBox, uma conexão estável em rede local que possibilite a varredura de serviços e vulnerabilidades conhecidas. Visando a aplicação dos conhecimentos obtidos durante o módulo.

Na pasta de imagens, há uma breve elucidação sobre o ambiente usado, que ilustra perícia de configuração e uso do software Oracle VirtualBox.

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

Tal feito, elucida que não é exigido uma alta qualidade técnica em conhecimentos na área de TI, embora seja algo potencializador, para que se consiga explorar vulnerabilidades em um sistema.

## Ataque FTP

O processo realizado está levemente elucidado pelos arquivos elencados abaixo, presentes neste repositório:

- `~/materiais/bruteforce/enumeracao_nmap1.txt`;
- `~/materiais/bruteforce/resultados1.txt`;

### Observações

Com a elaboração de uma wordlist simples, pudemos constatar que o protocolo é facilmente vulnerável, isso sem considerar a versão do serviço disponibilizado pelo MetaSploitable, que com uma busca levemente mais aprofundada em sites como [CVE](https://www.cve.org/), poderemos encontrar mais meios de acesso ao explorar mais vulnerabilidades.

### Possíveis mitigações

- Uma das mais efetivas, não executá-lo ou não expor serviços se não será usado;
- Utilizar protocolo mais atuais e seguros como o SFTP;
- Aplicar políticas de senha de usuários fortes contendo por exemplo:
  - Pelo menos 8 caracteres;
  - Conter números;
  - Letras maiúsculas e minúsculas;
  - Caracteres especiais;
- Aplicar políticas de rotação de senhas/credenciais aos usuários do serviço;

## Ataque HTTP

Ilustrações do ataque presentes em `~/images/Screenshot_http_1.png` e `~/materiais/bruteforce/resultado_http.txt`.

### Observações

Através da exploração, com DevTools, da requisição feita ao servidor da aplicação, obtivemos o formato esperado da requisição do tipo POST, no formato 'username=usuario&password=senha&Login=Login'. - Ilustrado por Screenshot_http_0.png (em /images neste repositório).

A partir disso, podemos replicar a requisição no Medusa, adicionando os _placeholders_ (`^USER^` e `^PASS^`) da ferramenta para que a mesma realize a substituição automatizada, pelas palavras disponíveis em nosso dicionário, resultando em um texto assim: `’username=^USER^&password=^PASS^&Login=Login’` - ilustrado por Screenshot_http_1.png (em /images neste repositório).

Dito isso, é imprescindível que na execução do comando seja incluso os parâmetro `-U` e `-P`.

### Possíveis mitigações

- Exigir Headers HTTP específicos;
- Exigir usos de Tokens CSRF;
- No processo de login, realizar mascaramento dos campos enviados, tanto em sua nomenclatura quanto em valores enviados;
  - Exigir a configuração de um serviço de segundo fator de autenticação;

## Ataque SMB (SAMBA)

### Observações

### Possíveis mitigações

# Autor

- [Alexsander de A. Oliveira](https://github.com/Alexz96)
