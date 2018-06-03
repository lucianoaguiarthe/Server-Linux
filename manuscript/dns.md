# MANUAL DE CONFIGURAÇÃO DNS #

<h2 align="middle">INSTALAÇÃO E CONFIGURAÇÃO</h2>


<p align="justify">O servidor DNS é responsável por fazer a tradução de nomes em endereços IP e endereço IP em nomes, caso você possua uma conexão com a internet e não tenha um DNS o acesso a algum site  só seria possível se o usuário digitasse o endereço IP do referido site no navegador, daí a grande importância deste serviço para uma rede.</p>

<p align="justify">Para que uma empresa tenha um DNS um pré-requisito é que ela possua um IP válido e fixo, e ainda possua o domínio disponível, para isso deve realizar uma pesquisa se o domínio desejado está disponível no endereço http://registro.br/index.html, após realizar a configuração deste domínio no servidor DNS o administrador pode realizar um cadastro no site citado e pagar um valor anual de R$ 40,00.</p>

<p align="justify">O servidor para o protocolo DNS mais utilizado no linux é o BIND (Berkeley Internet Name Domain) que encontra-se em sua versão 9 para instalação deste programa na distribuição debian acesse o shell como root e digite o comando abaixo:</p>
<h4 align="middle">apt-get install bind9</h2>

<p align="justify">O apt irá baixar o bind  e as dependências necessárias e instalar no Linux, após a instalação será criado um diretório localizado no /etc/bind, o arquivo de configuração principal é o named.conf, é nele que encontram-se as zonas dos servidores raízes e localhost, o arquivo db.root  é o arquivo de registro que descreve o servidor de nomes raiz no mundo e, ainda o named.conf.local que é o arquivo onde são criadas as zonas configuradas pelo administrador do sistema.</p>

<p align="justify">Neste nosso manual iremos configurar no Bind uma zona de pesquisa direta e inversa conforme descrição abaixo:</p>

 * Nome da zona: aluno.com.br
 * Servidor de nomes: será chamado ns01 que é o seu computador
 * Servidor de e-mail: será chamado server e possui o ip 192.168.5.10
 * O host ns01 responderá pelo nome www

<p align="justify">Antes de configurar propriamente dito o Bind deveremos informar para o nosso computador quem será o servidor DNS, para isso alteraremos o arquivo /etc/resolv.conf colocando conforme  linha abaixo:</p>
 
 <B>nameserver 192.168.5.1        # que é o IP do nosso computador</B>
 
<p align="justify">Considerando que em nosso ambiente uma das interfaces de rede recebe IP via dhcp o arquivo resolv.conf será alterado a cada conexão, para fixarmos o ip do DNS local devemos informar ao dhcp cliente que existe um servidor dns local, alterando o arquivo <b>/etc/dhcp/dhclient.conf</b> alterando as linhas descritas a seguir:</p>

<h4 align="left">supersede domain-name "aluno.com.br";<p>prepend domain-name-servers 192.168.5.1;</h4>


<h2 align="middle">Configuração da Zona de Pesquisa Direta</h2>

<p align="justify">A zona de pesquisa direta tem como função traduzir nomes em endereço IP, para criação da  zona aluno.com.br deveremos editar o arquivo named.conf.local localizado no diretório /etc/bind, conforme sequencia descrita a seguir:</p>

* Acesse o Diretório com o comando cd /etc/bind
* Abra o arquivo named.conf.local com o comando nano named.conf.local
* Edite o arquivo named.conf.local conforme descrição abaixo:

<p align="center"><img src="images/zona-direta.png"  width="700" height="103" align="middle"/></p>

<h2 align="middle">Criação do Arquivo de Registro da Zona de Pesquisa Direta</h2>

<p align="justify">Para criação do arquivo db.aluno.com.br aproveitaremos a estrutura de um arquivo já existente, no nosso exemplo utilizaremos o db.local, desta forma realizaremos uma cópia do mesmo, só lembrando que para isso você deverá está no diretório /etc/bind</p>

<h4 align="middle">cp db.local db.aluno.com.br</h4>

<p align="justify">Após a realização da cópia abra o arquivo db.aluno.com.br com um editor de sua preferência (comando: nano db.aluno.com.br) e altere conforme descrição abaixo:</p>

<p align="center"><img src="images/db-aluno.png"  width="740" height="364" align="middle"/></p>


Salve o arquivo e reinicie o serviço com o comando abaixo:

 <h4 align="middle">/etc/init.d/bind9 restart</h2>
 
<h2 align="middle">Realizando os testes da Zona de Pesquisa Direta</h2>
 
<p align="justify">Nas distribuições baseadas no debian existe um aplicativos para testes que se chama nslookup, para verificar se seus registros estão corretos digite no shell o comando nslookup e o nome ou ip do computador, esta última opção para verificar a zona de pesquisa inversa.</p>

<p align="justify">Geralmente os primeiros testes são a verificação do servidor de nomes,  para isto devemos acrescentar uma opção no comando nslookup conforme descreverei abaixo:</p>
 
<p align="center"><img src="images/teste-nome.png"  width="700" height="190" align="middle"/></p>

<p align="center"><img src="images/teste-ns.png"  width="700" height="201" align="middle"/></p>

<p align="center"><img src="images/teste-mx.png"  width="730" height="197" align="middle"/></p>

<p align="center"><img src="images/teste-a.png"  width="700" height="204" align="middle"/></p>
 
<p align="center"><img src="images/teste-cname.png"  width="700" height="211" align="middle"/></p>

<h2 align="middle">Configurando uma Zona de Pesquisa Inversa</h2>

<p align="justify">A zona de pesquisa inversa tem a função de traduzir endereço IP para nome, as zonas de pesquisa inversa podem ser necessárias para certos aplicativos seguros que exigem a validação de endereços IP. Para criarmos uma zona de pesquisa inversa sempre utilizaremos o mesmo arquivo de configuração de zona que é o <b>named.conf.local</b>, lembrando que foi o mesmo arquivo que você configurou a zona de pesquisa direta aluno.com.br, abra o arquivo named.conf.local localizado no diretório <b>/etc/bind</b>, e deixe-o conforme abaixo:</p>
<p align="center"><img src="images/zona-inversa.png"  width="700" height="113"/></p>
<h4 align="middle">Criação do Arquivo de Registro da Zona de Pesquisa</h4>

<p align="justify">Para criação do arquivo db.5.168.192 aproveitaremos a estrutura do arquivo db.aluno.com.br, já que o arquivo de registro da zona de pesquisa inversa possui o mesmo cabeçalho, desta forma realizaremos uma cópia do mesmo, só lembrando que para isso você deverá está no diretório /etc/bind</p>

<h5 align="middle">cp db.aluno.com.br db.5.168.192</h5>

<p align="justify">Após a realização da cópia abra o arquivo db.5.168.192 com um editor de sua preferência e altere conforme descrição abaixo:</p>

<p align="center"><img src="images/db-inverso.png"  width="740" height="375" align="middle"/></p>

<h4 align="middle">Realizando os testes da Zona de Pesquisa Inversa</h4>

<p align="justify">Para realizar os testes na zona de pesquisa inversa basta digitar o IP do host cadastrando na zona de pesquisa inversa após o comando nslookup, conforme exemplo abaixo:</p>

<p align="center"><img src="images/teste-inverso.png"  width="700" height="136"/></p>


<h2 align="middle">Configuração DNS Secundário</h2>

<p align="justify">O serviço de resolução de nomes em uma ambiente, obrigatoriamente devemos pensar em redundância, partindo deste pressuposto o DNS Secundário ocupa este papel, ele replica as zonas de um DNS Primário e responde as suas resolução. É importante pontuar que no DNS Secundário não são executadas operações de inserção de registros, suas zonas são somente de leitura respondendo as resoluções.</p>

<p align="justify">Para que possamos realizar a prática de configuração de um DNS Secundário devemos preparar o nosso ambiente. Inicialmente clone a Máquina Virtual <b>Debian 9.3 - Server</b>, para isso selecione a VM, e no menu <b>Máquina escolha</b> o item <b>Clonar</b>, conforme apresentado na imagem abaixo:
</p>

![](images/clone-vm.png)


<p align="justify">Atribua o nome para a nova Máquina Virtual de <b>Debian 9.3 - Secundário</b>, marque a opção reinicializar endereço MAC, na janela seguinte marque a opção <b>Clone Completo</b> e conclua o clone da VM.</p>

<p align="justify">Devemos também ajustar o endereçamento ip do DNS Secundário, acesso o diretório <b>/etc/network</b> e modifique o arquivo <b>interfaces</b>, conforme descrito a seguir:</p>

![](images/interfaces.png)

<p align="justify">Para finalizar altere o arquivo <b>/etc/resolv.conf</b> apontado para o novo DNS 192.168.5.2, e  o arquivo <b>/etc/dhcp/dhclient.conf</b> nas linhas conforme descrito a seguir:</p>

<h4 align="left">supersede domain-name "aluno.com.br";<p>prepend domain-name-servers 192.168.5.1, 192.168.5.2;</h4>

<h3 align="middle">Configurando DNS Primário</h3>

<p align="justify">No DNS Primário devemos somente informar as nossas zonas já criadas anteriormente que existe um segundo servidor de nomes na rede. Para isso acesso o diretório de instação do bind <b>/etc/bind</b> e acrescente as linhas descritas a seguir referente ao novo servidor no arquivo <b>db.aluno.com.br</b>:</p>
<h4 align="left">@		IN	NS	ns02.aluno.com.br.<p>
ns02		IN	A	192.168.5.2</h4>

<p align="justify">O arquivo db.aluno.com.br depois de modificado deve ficar conforme imagem abaixo:</p>

![](images/db-aluno-secundario.png)

<h3 align="middle">Configurando DNS Secundário</h3>

<p align="justify">Abra a nova VM criada, e acesse o diretório de configuração do bind <b>/etc/bind</b>, considerando que clonamos a máquina já com algumas configurações de DNS devemos apagar as zonas criadas anteriormente no arquivo <b>named.conf.local</b>, e criar a zona secundária de acordo com imagem a seguir:</p>

![](images/zona-secundaria.png)

<p align="justify">O arquivo db.aluno.com.br será copiado do DNS Primário, todavia por questão de organização devemos criar um diretório para salvar as zonas secundárias, este diretório será chamado de <b>slave</b>, e devemos criá-lo dentro do diretório <b>/etc/bind</b> conforme comando a seguir:</p>
<h3 align="middle">mkdir slave</h3>

<p align="justify">Devemos ainda alterar o dono do diretório slave, para que o bind consiga salvar os arquivos de registro da zona secundária, de acordo com comando abaixo:

<h3 align="middle">chown -R bind.bind /etc/bind/slave</h3>

<p align="justify">Após todas as alterações nos arquivos de configuração do Bind reinicie o serviço nos dois servidores:</p>

<h3 align="middle">/etc/init.d/bind9 restart</h3>

<p align="justify">Saberemos se o DNS Secundário está funcionando, quando realizarmos uma consulta ao mesmo através do <b>nslookup</b> e fazer a resolução do nome:</p>

![](images/teste-secundario.png)



