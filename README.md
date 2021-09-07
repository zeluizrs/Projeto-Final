# Projeto-BIMaster
# Tratamento de informações de inventário de equipamentos de rede IP: roteadores e switches Cisco

## Aluno: Jose Luiz Gomes Ramos
## Orientadora: Evelyn Batista
Trabalho apresentado ao curso BI Master como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

### Resumo:

Para que as informações possam fluir dentro de uma determinada empresa ou desta para a internet são utilizados sistemas de tecnologia de informação e comunicação. Dentre esses sistemas está presente a rede IP, composta principalmente por equipamentos responsáveis pelo roteamento e comutação dos pacotes de dados: roteadores e switches.
Garantir o pleno funcionamento destes equipamentos e reduzir impactos causados por falhas nestes faz parte da rotina de áreas de operações de rede. Para que isso seja possível, os especialistas que atuam nestas áreas necessitam ter uma visão ampla dos equipamentos que estão operando, não só monitorando e corrigindo falhas, como garantindo um nível adequado de sobressalentes e inserindo tais equipamentos em contratos de suporte com os fabricantes para garantir atualizações, correções de bugs e até mesmo reposição de peças com defeito. 
Tendo como fonte inicial de informação a saída de comandos de inventário deste equipamentos, contendo informações de todas as partes que os compõe, como fontes, módulos de I/O, módulos de supervisão, dentre outros, o presente trabalho tem como objetivo tratar tal inventário, criando um dataset a partir deste arquivo texto, extraindo informações do nome dos equipamentos e consultando informações de cada item diretamente no site do fabricante através de API.

### Metodologia:

Parte 1: o arquivo com o inventário dos equipamentos foi gerado com a linguagem Ansible, onde foi realizado acesso aos equipamentos do grupo definido e aplicado o comando de leitura de inventário.
O arquivo show_inventory_real-anonimo.txt contém uma amostra da saída do comando de inventário coletado através de Ansible e que, após uma alteração de marcador, será tratado usando a biblioteca TextFSM no código em Python.
Então, com o uso da biblicoteca TextFSM foi realizado uma "tradução" da informação para que fosse construído um dataset com os parâmetros dos equipamentos.

Parte 2: com o dataset pronto, a partir da informação de hostname dos equipamentos foi criada uma nova coluna com a informação de localização, chamada site.
Inicialmente foi feito uso de expressão regular para extrair a informação, porém ela saiu como uma tupla. Foi necessário, então, criar uma função para deixar somente a string.

Parte 3:
O primeiro passo para realizar as consultas via API foi a criação de credenciais no site do fabricante, para a partir daí gerar o token para as consultas.
Então, foram criadas funções para as consultas baseadas no número de série e na identificação dos produtos. Durante a execução do script, usando loop para realizar as consultas linha por linha foi observado que a API só respondia as primeiras consultas e posteriormente apresentava erros. Posteriormente, foram criadas novas funções para uso do "apply" no Pandas. O desempenho melhorou consideravelmente e não ocorreram mais erros. Importante lembrar algumas restrições das consultas via API: a)o token emitido pelo fabricante é válido por 3599 segundos, b) há um limite de 1000 consultas de número de série por dia (SN2Info) e 5000 consultas de produtos por dia (EOX).

Por fim, com um dataset pronto, foi gerado um aquivo .csv com as informações. Esse arquivo pode ser tratado então em outras ferramentas de business inteligence para as devidas tomadas de decisão.

### Considerações:
O template utilizado neste trabalho é aplicável a equipamentos do fabricante Cisco Systems. Portanto, para outros fabricantes, devem ser inseridos novos templates para tratamento.
Para manter confidencialidade de informações, os códigos de produto, hostnames, formação de hostnames e números de série são somente exemplos criados para representar a estrutura dos arquivos e informações dos equipamentos. 
