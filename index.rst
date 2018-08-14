.. Rádio liquidsoap documentation master file, created by
   sphinx-quickstart on Mon Aug  6 19:56:33 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Documentação Rádio c/ Liquidsoap
============================================
Documentação relativa a Rádio implementada com Liquidsoap.
Nesta documentação estão presentes as configurações necessárias 
para reinstalar e manipular o projeto em questão.

.. toctree::
   :maxdepth: 2
   :caption: Contents:


Manipulação e Execução do Projeto 
======================================
A elaboração deste projeto foi adaptada a partir da sua versão inicial, substituindo 
a ferramenta ICES por Liquidsoap. Para rodar o autodj é necessário que os seguintes 
processos sejam executados.:

1. Banco MySQL com os dados importados para autenticação das lives.

2. Icecast2 com sua respectiva config. (Playlist e Live) 

3. Script do Liquidsoap apontando para as config. do Icecast.

4. JSON da playlist com o formato que será apresentado.

Executando Liquidsoap
#####################
Com a ferramenta devidamente instalada, veja a secção :ref:`Instalação do Liquidsoap com Encoders`
se necessário, execute o script ``web/app.dist.liq`` utilizando como parâmetro o caminho arquivo 
.conf que a principio era utilizado pelo ICES. Ex.:

``$ liquidsoap app.dist.liq -- /home/streaming/web/app/config/liquid.conf.dist``

``--`` É necessário para informar ao liquid que as linhas seguintes referem-se a parâmetros 
do script. A partir deste ponto o liquidsoap já estará rodando e transmitindo a rádio através 
das configurações do Icecast.

Arquivo XML de configuração do Liquidsoap
#####################
Para manter a compatibilidade com a versão inicial deste projeto foi implementado a bind dos atributos 
presentes no arquivo de configuração XML dos clientes do autodj. O arquivo a seguir foi utilizado 
para realizar os testes no servidor.

.. literalinclude:: conf.example.xml
  :language: JSON

Nem todos os atributos deste arquivo são utilizados pelo script do Liquidsoap, mas podem ser mantidos 
como linguagem de marcação. Segue os atributos obrigatórios para o funcionamento do script.

.. csv-table:: Tabela de atributos obrigatórios Conf.xml
   :file: conf-strct.csv
   :widths: 30, 70
   :header-rows: 1

Arquivo JSON da playlist
#####################
Para que os scripts funcionem corretamente o arquivo JSON de configuração
de playlist deve obrigatóriamente conter os seguintes atributos.:

.. literalinclude:: playlist-strct.json
  :language: JSON

.. csv-table:: Tabela de atributos obrigatórios Playlist.json
   :file: structure_playlist.csv
   :widths: 30, 70
   :header-rows: 1

Scripts para Manipulação da Playlist
#####################

Os scripts desenvolvidos que manipulam as playlists do autodj assim como 
a playlist estão em sua maioria localizados na pasta ``app/service`` e são utilizados 
em sua maioria como ``$php + _nome-do-script``.

.. csv-table:: Tabela de Scripts que permitem chamadas externas
   :file: structure.csv
   :widths: 25 25 25 25
   :header-rows: 1

Instalação do Liquidsoap com Encoders
======================================
A aplicação assim como as dependências do liquidsoap são gerenciadas 
através do Package Manager Opam.

1. Instalação do opam:
``$ sudo apt-get update && sudo apt-get install opam``

Em seguida:
``$ opam init && eval `opam config env```

2. Instalação do Liquidsoap
 
Verifique as dependências necessárioas através do plugin depext:  
``$ opam install depext && opam depext taglib mad lame vorbis cry ssl samplerate magic opus liquidsoap``

Para instalar:  ``$ opam install taglib mad lame vorbis cry ssl samplerate magic opus liquidsoap``

3. Instalação dos pacotes de codificação para os principais formatos diferentes de MP3
``$ opam depext fdkaac && opam install fdkaac``

``$ opam depext aacplus.0.2.2 && opam install aacplus.0.2.2``


Dependências do Servidor
======================================
No projeto o Liquidsoap realiza chamadas externas para manipular as playlists
e fazer a leitura das configurações da rádio. Essas chamadas se utilizam do ``PHP``. 
O ``MYSQL`` também é uma dependência deste projeto e 
está sendo utilizado nas configurações da live do projeto. 
As dependências citadas nesta secção podem ser intaladas com os seguintes comandos:  

``$ sudo apt-get install php, php-xml, php-simplexml, php-mysql``

``$ sudo apt-get install mysql-server``

Observações Pertinentes
#####################
De preferência a usar caminhos absolutos para referenciar o caminho dos arquivos,
tanto no arquivo JSON quanto nos parâmetros de script.
