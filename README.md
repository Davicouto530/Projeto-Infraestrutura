# Projeto-Infraestrutura
Projeto-Infraestrutura pelo Senac Tatuapé com o professor Vaamonde.


* Primeiro dia do Projeto-infraestrutura 03/07/2024 *

Certificando os pontos 9, 10, 11, 12, 28 dos mv8, e todos estão aprovados.

Conectados na porta do Switch 9, 10, 11, 28.

Estamos utilizando um Switch Cisco Catalyst Layer-3 3560 e 1 (um) Router Cisco 2911

Switch porta 11 conectada com o cabo rolover.

Quem está responsavel pelo Switch acessou o "PuTTY", que serve para configurar o Switch.
Reiniciar e inicializar o Switch

comandos: 
          Acessar o modo privilegiado de comandos
          "en"

          "dil flash:"

          Remove o banco de dados da VLAN e flash
          "vlan.dat"

          serve para remover os backups anteriores
          "delete flash:startup-config"
          
Retirar o cabo 11 console do Switch e conectar no roteador.

Resetar o roteador

cabo console ja esta no roteador
entrar no terminal
na hora que ligar o roteador, fazer isso no terminal:
na hora do boot, que aparecer as "####", apertar no titulo (COMIT - PUTTY) com o botão direito, ir em "Special" e depois em "break" para PARAR a inicialização do roteador.
vai entrar no romon l>
comandos para digitar no terminal do roteador estando no "romon 1>":
Comando: confreg 0x2142 (vai subir sem ler as configurações atuais)
Comando: reset
vai aparecr um [yes/no] e colocar "no"
Depois vai entrar no "Router>"

Limpando as configurações do Router Cisco 2911 e voltando a ler o arquivo de configuração: startup-config da NVRAM

Acessar o modo privilegiado de comandos 
Router> enable
Router# dir flash
Router# erase startup-config
Router# delete flash:startup-config
Router# configure terminal
Router (config)# config-register 0x2102 
Router (config)# end
Router# copy running-config startup-config 
Router# reload
Router> enable
Router# show version
TEM QUE APARECER "Configuration register is 0x2102"


CONFIGURAÇÕES BÁSICAS DO SWITCH:

!Acessando o modo Exec Privilegiado
enable

!Configuração de data/hora em inglês, abreviado ou completo
!Exemplo: October ou Oct
!Primeiro Hora no formato: 00:00:00 (hora:minutos:segundos) depois Data no formato: Dia Mês Ano
clock set ??:??:?? ?? ???????? ????

	!Acessar modo de Configuração Global
	configure terminal

	!Configuração do nome do Switch 3560
	!Obrigatório para a configuração do SSH e demais serviços de redes
	!Mudar o nome do Switch 3560 para cada equipamento do seu grupo
	!OBSERVAÇÃO IMPORTANTE: veja o arquivo 00-DocumentacaoDaRede.txt a partir da linha: 68 
	!(#03_ Nome dos Switches, Routers e Access Point de Cada Grupo:)
	hostname sw-g???

	!Habilitar o serviço de Criptografia de Senhas do Tipo-7 Password 
	service password-encryption
	
	!Habilitar o serviço de marcação de Data/Hora detalhado nos Logs
	service timestamps log datetime msec
	
	!Verificar tentativas de conexão simultâneas, fazer o bloqueio de um
	!período determinado de tempo, proteção contra Brute Force
	login block-for 120 attempts 4 within 60

	!Desativar a resolução de nomes de domínio
	no ip domain-lookup

	!Configuração do Banner da mensagem do dia
	!Desafio: Buscar na Internet imagens ASCII para o Banner
	!Dica de site: https://manytools.org/hacker-tools/ascii-banner/
	banner motd #AVISO: acesso autorizado somente a funcionarios#

	!Habilitar a senha do Tipo-5 secret para o modo enable privilegiado
	enable secret 123@senac
  
	!Criação dos usuários, senhas do Tipo-5 e privilégios diferenciados
	!Consultar Planilha de Nomes de Usuários
	!OBSERVAÇÃO: Caso o grupo tenha menos integrantes, desconsiderar a
	!criação de 04 (quatro) usuários
	username ???nome_do_primeiro_integrante??? privilege 15 secret 123@senac
	username ???nome_do_segundo_integrante??? privilege 15 secret 123@senac
	username ???nome_do_terceiro_integrante??? privilege 15 secret 123@senac
	username ???nome_do_quarto_integrante??? privilege 15 secret 123@senac
	
	!Acessando a linha console
	line console 0
		
		!Habilitando senha do Tipo-7 Password 
		password 123@senac
		
		!Forçando fazer login com usuário e senha
		login local
		
		!Sincronizando os logs na tela
		logging synchronous
		
		!Habilitando o tempo de inatividade do console
		exec-timeout 5 30
		
		!Saindo de todos os níveis
		end

!Salvando as configurações
copy running-config startup-config
	
!Visualizando as configurações
show running-config

!Saindo do modo EXEC privilegiado
disable
exit


CONFIGURAÇÕES BÁSICAS DO ROUTER:

!Acessando o modo Exec Privilegiado
enable

!Configuração de data/hora em inglês, abreviado ou completo
!Exemplo: October ou Oct
!Primeiro Hora no formato: 00:00:00 (hora:minutos:segundos) depois Data no formato: Dia Mês Ano
clock set ??:??:?? ?? ???????? ????

	!Acessar modo de Configuração Global
	configure terminal

	!Configuração do nome do Router 2911
	!Obrigatório para a configuração do SSH e demais serviços de redes
	!Mudar o nome do Router 2911 para cada equipamento do seu grupo
	!OBSERVAÇÃO IMPORTANTE: veja o arquivo 00-DocumentacaoDaRede.txt a partir da linha: 68 
	!(#03_ Nome dos Switches, Routers e Access Point de Cada Grupo:)
	hostname rt-g???

	!Habilitar o serviço de Criptografia de Senhas do Tipo-7 Password 
	service password-encryption
	
	!Habilitar o serviço de marcação de Data/Hora detalhado nos Logs
	service timestamps log datetime msec
	
	!Comprimento mínimo da criação das senhas do Tipo-5 ou Tipo-7
	security passwords min-length 8

	!Verificar tentativas de conexão simultâneas, fazer o bloqueio de um
	!período determinado do login
	login block-for 120 attempts 4 within 60

	!Desativar a resolução de nomes de domínio
	no ip domain-lookup

	!Configuração do Banner da mensagem do dia
	!Desafio: Buscar na Internet imagens ASCII para o Banner
	!Dica de site: https://manytools.org/hacker-tools/ascii-banner/
	banner motd #AVISO: acesso autorizado somente a funcionarios#

	!Habilitar a senha do Tipo-5 secret para o modo enable privilegiado
	enable secret 123@senac

	!Criação dos usuários, senhas do Tipo-5 e privilégios diferenciados
	!Consultar Planilha de Nomes de Usuários
	!OBSERVAÇÃO: Caso o grupo tenha menos integrantes, desconsiderar a
	!criação de 04 (quatro) usuários
	username ???nome_do_primeiro_integrante??? privilege 15 secret 123@senac
	username ???nome_do_segundo_integrante??? privilege 15 secret 123@senac
	username ???nome_do_terceiro_integrante??? privilege 15 secret 123@senac
	username ???nome_do_quarto_integrante??? privilege 15 secret 123@senac
	
	!Acessando a linha console
	line console 0
	
		!Habilitando senha do tipo Password Tipo-7
		password 123@senac
		
		!Forçando fazer login com usuário e senha local
		login local
		
		!Sincronizando os logs na tela
		logging synchronous
		
		!Habilitando o tempo de inatividade do console
		exec-timeout 5 30
		
		!Saindo de todos os níveis
		end

!Salvando as configurações
copy running-config startup-config
	
!Visualizando as configurações
show running-config

!Saindo do modo EXEC privilegiado
disable
exit

Depois de fazer as básicas configurações do Switch e do Router, tirar o cabo console do Router e voltar para o Switch.
