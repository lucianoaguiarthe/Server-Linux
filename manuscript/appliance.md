# Appliance
<p align="justify">Para que o aluno possa configurar o ambiente proposto no presente documento o mesmo deve possuir um computador com no mínimo Processador <b>Quadcore</b> e Memória RAM de <b>3 Giga</b>.
Realizar o download do aplicativo <a href = "https://www.virtualbox.org/wiki/Downloads" target="blank">VirtualBox</a>, conforme seu sistema operacional (A versão utilizada para criar o Appliance foi a 6.0.14).</p>


Baixar o arquivo do Appliance no link descrito a seguir:

<p align="justify"><a href="https://drive.google.com/file/d/1U2rafGda8JGtTnvQG5ywv9GddCabGBF_/view?usp=sharing" target="blank"> Appliance </a></p>

<p align="justify">O Appliance é um arquivo que vem empacotado máquinas virtuais prontas para uso, em nosso laboratório existem duas, o server-linux e o debian-client.</p>

<p align="justify">Após concluir o download do Appliance, execute o arquivo, e será aberto uma janela do VirtualBox, aceite as opções defaults e aguarde a conclusão da importação das duas máquinas virtuais para seu ambiente.</p>

## DIAGRAMA DE REDE

<p align="justify">Todo o ambiente prático realizado em sala de aula será baseado no ambiente virtual criado pelo VirtualBox, onde é utilizado o sistema operacional <b>Debian 10</b> em ambas as máquinas virtuais, conforme diagrama descrito na Figura 1.</p>
<p align="justify">O Firewall denominado <b>debian-server</b> já possui os seguintes serviços instalados sem configuração:</p>

 * DNS – Bind;
 * Servidor Web – Apache;
 * Proxy Squid;
 * Servidor DHCP.

<p align="justify">Com o objetivo de deixar o host <b>debian-client</b> acessando a internet foi criado um pequeno script no fwl-server para compartilhamento da conexão da interface enp0s3, localizado no diretório /etc/init.d chamado fwl_server, conforme descrito abaixo:</p>

<p align="center"><img src="images/firewall.png"  width="650" height="136" align="middle"/></p>

<p align="center"><img src="images/Basic-Network.png"  width="650" height="447" align="middle"/></p>
<h4 align="middle">Figura 1 - Diagrama de Rede</h4>

## ADMINISTRAÇÃO DOS SISTEMAS

<p align="justify">O firewall da rede <B>server-linux</B> não possui ambiente gráfico instalado e já vem com dois usuários previamente criados o <b>root</b> (administrador do sistema) e o usuário <b>aluno</b>, ambos possuem a senha <b>123456</b>, o firewall pode ser acessado via ssh a partir do <b>debian-client</b> pelo comando ssh root@192.168.5.1 (usando a senha 123456), conforme podemos observar na figura 2:</p>


<p align="center"><img src="images/login.png"  width="650" height="290" align="middle"/></p>

<h4 align="middle">Figura 2</h4>

<p align="justify">O host <b>debian-client</b> vem configurado com um ambiente gráfico mais leve chamado LXDE, o mesmo está configurado para receber ip automaticamente do firewall, o usuário para autenticação no ambiente gráfico é <b>aluno</b> e senha <b>123456</b>, a senha do usuário root também é <b>123456</b>.
É importante pontuar que para o debian-client navegar na internet se faz necessário que o debian-server seja inicializado previamente.</p>

