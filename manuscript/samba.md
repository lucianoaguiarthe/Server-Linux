# SERVIÇO SAMBA
<p style="text-align: justify;">O Samba é um serviço disponível para Sistemas Operacionais Linux que permite compartilhar diretórios e impressoras, bem como implementar um controlador de domínio. A primeira versão do Samba foi disponibilizada em 1992, e escrita por Andrew Tridgell, um Australiano que na época era estudante de ciências da computação, atualmente encontra-se em sua versão 4, a grande novidade desta versão são as implementações de Diretivas de Grupos. Para o seu funcionamento necessita da instalação dos seguintes pacotes:</p>
 * <b>Samba</b> – Que disponibiliza o samba propriamente dito;
 * <b>Kerberos</b> – Protocolo desenvolvido para prover altenticação em aplicações e serviços cliente/servidor;
 * <b>Winbind</b> -  Proporciona uma integração de usuários Windows com o samba;
 * <b>Smbclient</b> – Permite acessar compartilhamentos em outras máquinas.

<p style="text-align: justify;">Para que você possa configurar um ambiente totalmente funcional com controlador de domínio samba, ferramenta de administração do Samba RSAT e um cliente para o teste criei um Appliance no endereço:</p>

<p style="text-align: justify;">Este Appliance terá um servidor Samba <b>samba-dc</b>, um cliente para administrar graficamente o Samba a máquina <b>RSAT-PC</b>, no diretório c:\samba encontra-se o instalador, e a máquina <b>Cliente-PC</b>, para testes com o cliente, conforme diagrama abaixo:</p>

![](images/samba/01_diagrama-samba.png)
<h4 align="middle">Figura 01 - Diagrama de Rede</h4>


<p style="text-align: justify;">Conforme apresentado no diagrama de rede acima Fig. 01 o ip da interface <b>enp0s3</b> é 192.168.5.1, a senha tanto do usuário <b>root</b> como do usuário <b>aluno</b> é <b>123456</b></p>

<p style="text-align: justify;">É importante pontuar que a memória reservada para as máquinas virtuais foram de 1255 MB, todavia podendo diminuir este tamanho conforme configuração máquina que estará executando o appliance, podemos observar a alocação de memória deseja na Fig. 02.</p>

![](images/samba/02_virtualbox.png)
<h4 align="middle">Figura 02 - Alocação de Memória VM</h4>

## INSTALAÇÃO SAMBA

Para instalação do Samba devemos baixar os pacotes: samba, kerbero, smbcliente e winbind, conforme comando a seguir:

<h4 align="middle">apt-get install samba krb5-config winbind smbclient</h4>

Ao final da instalação é solicitado informações sobre configuração do kerberos, a primeira tela Fig. 03, solicita qual o nome do domínio, onde configuraremos aluno.com.br.

![](images/samba/07_kerberos.png)
<h4 align="middle">Figura 03 - Realm Kerberos</h4>


Será solicitado também o ip do servidor responsável pelo domínio, iremos configurar o endereço loopback, já que o kerberos está sendo executado no mesmo host do samba:

![](images/samba/08_kerberos.png)
<h4 align="middle">Figura 04 - Servidor Kerberos</h4>

E ainda é solictado o ip do servidor administrativo que também será o endereço loopback, conforme explicações anteriores:

![](images/samba/09_kerberos.png)
<h4 align="middle">Figura 05 - Servidor Administrativo</h4>

Ao concluir a instalação devemos renomear o arquivo principal de configuração do samba o <b>smb.conf</b>, para que ao aprovisionar o domínio, seja gerado um novo smb.conf, de acordo com o comando a seguir:

<h4 align="middle">cp /etc/samba/smb.conf /etc/samba/smb.conf.original</h4>

<p style="text-align: justify;">O passo seguinte deveremos configurar o samba como controlador de domínio, é importante pontuar que o nome do nosso domínio será <b>aluno.com.br</b> e o nome da máquina q está executando o samba é <b>samba-dc</b>, através do comando <b>samba-tool</b> conforme apesentado na Fig. 06:</p>
<p style="text-align: justify;"><b>IMPORTANTE: A senha cadastrada deve possuir um nível alto de complexidade, use número, letras e símbolos, caso coloque uma senha simples dará um erro no final da instalação</b></p>

![](images/samba/10_domain_provision.png)
<h4 align="middle">Figura 06 - Comando samba-tool</h4>

<p style="text-align: justify;">Onde você informará qual realm do domínio, em nosso exemplo <b>aluno.com.br</b>, qual o domain, o mondo como o serviço samba está operando, se <b>DC</b> como controlador de domínio ou <b>standalone</b> somente para compartilhamento de arquivos, qual tipo de dns estará fazendo a resolução de nomes, em nosso exemplo estaremos utilizando o do próprio samba <b>SAMBA_INTERNAL</b> e qual endereço do dns estará redirecionando as solicitações de resolução de nome. Responda todas estas perguntas conforme Fig. 06.</p>

<p style="text-align: justify;">Ao concluir a configuração será gerado um novo smb.conf bem como o arquivo de configuração do kerberos e apresetnado um relatório conforme Fig. 07:</p>

![](images/samba/11_domain_provision.png)
<h4 align="middle">Figura 07 - domain_provision</h4>

<p style="text-align: justify;">O arquivo smb.conf gerado no comando <b>samba-tool domain provision</b> é apresentado na Fig. 08 e corresponde a configuração inicial do samba:</p>

![](images/samba/12_smb.conf.png)
<h4 align="middle">Figura 08 - Arquivo smb.conf</h4>

<p style="text-align: justify;">Ao concluir toda a configuração podemos fazer um teste no samba utilizando o smbcliente conforme apresentado na Fig. 09</p>

![](images/samba/13_teste_samba.png)
<h4 align="middle">Figura 09 - Teste Samba</h4>

<p style="text-align: justify;">Faz-se necessário copiar o arquivo gerado pelo kerberos durante o aprovisionamento do domínio para a pasta <b>etc</b>:</p>
<h4 align="middle">cp /var/lib/samba/private/krb5.conf /etc/</h4>

<p style="text-align: justify;">O administrador do sistema deverá derrubar todos os serviços do samba, objetivando alterar configurações, conforme comando abaixo:</p>
<h4 align="middle">systemctl stop smbd.service nmbd.service winbind.service </h4>

<p style="text-align: justify;">Devemos desativar todos os serviços do samba para não serem iniciados no boot, impossibilitando o AD de subir:</p>

<h4 align="middle">systemctl disable smbd.service nmbd.service winbind.service</h4>

<p style="text-align: justify;">O serviço samba-ad-dc ao ser instalado ele vem mascarado, desta forma o mesmo é impedido de ser iniciado ou parado, para que possamos parar o iniciar o serviço manualmente devemos desmascarar o samba com o comando:</p>

<h4 align="middle">systemctl unmask samba-ad-dc.service</h4>

<p style="text-align: justify;">Como o samba-dc-ad foi parado anteriormente devemos inicializá-lo:</p>

<h4 align="middle">systemctl start samba-ad-dc.service </h4>

<p style="text-align: justify;">Habilite com o comando abaixo, o serviço samba-ad-dc para inicializar no boot:</p>

<h4 align="middle"><b>systemctl enable samba-ad-dc.service</b></h4>

<p style="text-align: justify;">Podemos ainda tirar a complexidade da senha usando o samba-tool, conforme apresentado na Fig. 10</p>

![](images/samba/14_complexity_samba.png)
<h4 align="middle">Figura 10 - Alteração Complexidade de Senha</h4>

<p style="text-align: justify;">Caso o administrador de rede deseje, pode especificar o tamanho mínimo de uma senha, na Fig. 11, configuramos que o usuário tenha uma senha como no mínimo 6 caracteres ou símbolos</p>

![](images/samba/15_password_length.png)
<h4 align="middle">Figura 11 - Tamanho da Senha</h4>

## CONFIGURAÇÃO DO RSAT

<p style="text-align: justify;">O serviço samba pode ser administrado diretamente no host em que está instalado o serviço, através do seu arquivo de configuração e o utilitário <b>samba-tool</b>, ou podemos administrá-lo remotamente através do software RSAT instalado em um computador com sistema operacional Windows, que esteja previamente autenticado no domínio, você verá que a configuração passa a ser igual a do Windows Server. Em nosso diagrama Fig. 01 iremos administrar o samba a partir do host RSAT-PC que tem o IP 192.168.5.10.</p>
<p style="text-align: justify;">

<p style="text-align: justify;">Antes de instalar o <b>RSAT</b> devemos ingressar o host no domínio, acesse o painel de controle e vá em sistemas em segurança e sistemas, será exibido uma janela conforme Fig. 12</p>

![](images/samba/16_rsat_domain.png)
<h4 align="middle">Figura 12 - Sistemas</h4>

<p style="text-align: justify;">Na área de configurações de grupo de trabalho clique em alterar configurações, no qual será exibido a janela de propriedades do sistemas Fig. 13, clique no botão <b>Alterar</b> para informar o domínio que deveremos ingressar.</p>

![](images/samba/17_rsat_domain.png)
<h4 align="middle">Figura 13 - Propiedades do Sistemas</h4>

<p style="text-align: justify;">Na janela de alterações do domínio Fig. 14, você terá a posibilidade de alterar o domínio e o nome do computador, marque o item domínio e coloqueo o nome aluno.</p>

![](images/samba/18_rsat_domain.png)
<h4 align="middle">Figura 14 - Alteração de Domínio</h4>

<p style="text-align: justify;">Ao alterarmos o domínio será solicatado um usuário e senha para ingressar no domínio, o usuário é o <b>Administrator</b> e a senha é a cadastrada quando foi aprovisionado o domínio</p>

![](images/samba/19_rsat_domain.png)
<h4 align="middle">Figura 15 - Senha Domínio</h4>

<p style="text-align: justify;">Se a configuração feita no samba estiver toda correta, será apresentado uma janela de confirmação dando boas vindas ao domínio Fig. 16.</p>

![](images/samba/20_rsat_domain.png)
<h4 align="middle">Figura 16 - Confirmação Domínio</h4>

<p style="text-align: justify;">Conforme abordado anteriormente deve-se instalar o RSAT somente depois que o host fizer parte do domínio, no appliance disponilizado, o instalador do RSAT está localizado no diretório samba na unidade C, Fig. 17, faça a instação do mesmo.</p>

![](images/samba/21_folder_rsat.png)
<h4 align="middle">Figura 17 - Diretório Instalador RSAT</h4>

<p style="text-align: justify;">Ao concluir a instalação, será criado um item no menu iniciar Ferramentas Administrativa Fig. 18, que possuirá várias ferrametas para administração do domínio.</p> 

![](images/samba/22_user_pc_ad.png)
<h4 align="middle">Figura 18 - Ferramentas Administrativas</h4>

<p style="text-align: justify;">A primeira configuração a ser relizada é referente a usuários, selecione o item Usuários e Computadores do Active Directory, no qual abrirá uma janela para administração de usuário e unidades organizacionais Fig. 19.</p>

<p style="text-align: justify;">As Unidades Organizacionais-OU são contêineres que armazenam informações sobre usuários e computadores, em nosso exemplo iremos criar uma OU para armazenar nossos novos usuários a ser cadastrado, para clique com o botão direito em cima do nome do domínio selecione no menu suspenso o item Novo => Unidade organizacional.</p>

![](images/samba/23_create_ou.png)
<h4 align="middle">Figura 19 - Usuários e Computadores do AD</h4>

<p style="text-align: justify;">Vamos criar uma Unidade Organizacional chamada nassau, desmarque a opção proteger contêiner, conforme Fig. 20</p>

![](images/samba/24_create_ou.png)
<h4 align="middle">Figura 20 - Unidade Organizacional</h4>

<p style="text-align: justify;">Para cadastrar um usuário no domínio devemos clicar com o botão direito em cima da Unidade Organizacional nassau e no menu suspenso selecionar Novo => Usuário.</p>

![](images/samba/25_create_user.png)
<h4 align="middle">Figura 21 - Menu Usuário</h4>

<p style="text-align: justify;">Na janela de cadastro do usário preencha os campos de nome e usuário conforme Fig. 22Para cadastrar um usuário no domínio devemos clicar com o botão direito em cima da Unidade Organizacional nassau e no menu suspenso selecionar Novo => Usuário.</p>

![](images/samba/26_create_user.png)
![](images/samba/27_create_user.png)
<h4 align="middle">Figura 22 - Cadastro Usuário</h4>

<p style="text-align: justify;">Com o objetivo de implementar algumas restrições do sistema operacional dos usuários devemos criar as Group Police-GPO, as mesmas possuem diversas implementações para personalização dos sistemas operacionasi que fazem parte do domínio, como restrição ao painel de controle e configuração de rede. É importante pontuar que tal configuração é identica no Windows Server, todavia a possibilidade desta implementação no samba só veio a partir da versão 4.</p>

<p style="text-align: justify;">Para configurarmos uma GPO devemos acessar o menu de Ferramentas Administrativas e Gerenciamento de Políticas de Grupo Fig. 23</p>

![](images/samba/28_gpo.png)
<h4 align="middle">Figura 23 - Cadastro Usuário</h4>

<p style="text-align: justify;">Na criação da GPO devemos escolher o domínio (aluno.com.br), clicar com o botão direito em cima da OU nassau e no menu suspenso selecionar Criar uma GPO neste domínio e atribua um nome para ela, Fig. 24</p>

![](images/samba/29_create_gpo.png)
<h4 align="middle">Figura 24 - Criação GPO</h4>

<p style="text-align: justify;">Devemos editar a GPO criada, clicando com o botão diretio em cima dela e nome menu suspenso selecionar a opção Editar, Fig. 25</p>

![](images/samba/30_edit_gpo.png)
<h4 align="middle">Figura 25 - Edição GPO</h4>

<p style="text-align: justify;">Vamos implementar uma restrição para os usuários da OU nassau no qual os mesmos serão impossibilitados de realar alterações na configuração avança TCP/IP para isso selecione configuração do usuário => Modelos Administrativos => Rede => Conexões de Rede e edite a opção do lado direito da janela <b>Proibir a configuração avançada do TCP/IP</b>, selecione opção habilitado, Fig. 26</p>

![](images/samba/31_edit_gpo_lan.png)
<h4 align="middle">Figura 26 - GPO Configuração de Rede</h4>

<p style="text-align: justify;">Iremos implementar ainda como exemplo uma restrição para os usuários da OU nassau no qual não estará disponível no painel de controle a opção adicionar ou remover programas, para isso selecione configuração do usuário =>Adicionar ou Remover Programas e edite a opção do lado direito da janela <b>Remover opção Adicionar/Remover Programas</b>, selecione opção habilitado, Fig. 27</p>

![](images/samba/33_edit_gpo_programas.png)
<h4 align="middle">Figura 27 - GPO Painel de Controle</h4>

## CONFIGURAÇÃO DO CLIENTE

<p style="text-align: justify;">Para testarmos a configuração do usuário iremos utilizar o host Cliente-PC, Fig. 01, devemos ingressar o mesmo no domínio conforme abordado anteriormente, e logar com o usuário pedro</p>

<p style="text-align: justify;">Ao tentarmos acessar as configurações de rede será apresentado uma tela de autenticação Fig. 28, considerando que o usuário pedro não tem permissão de acesso a estas configurações de rede</p>

![](images/samba/36_client_edit_lan.png)
<h4 align="middle">Figura 28 - Configurações de Rede</h4>


<p style="text-align: justify;">Ao abrirmos o painel de controle opção adicionar ou remover programas, podemos observar que não existe opção para remover nenhum programa do sistema operacional Fig. 29</p>

![](images/samba/37_client_edit_software.png)
<h4 align="middle">Figura 29 - Painel de Controle</h4>

