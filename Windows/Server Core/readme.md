# Windows-Server-Core
    HYPERV - Instalação de host Hyperv com Windows Server Core
    Requisitos: PowerShell
    
## DESCRIPTION

	  - Renomear servidor
 	  - Configurar Rede
 	  - Firewall
  	  - Ingressar no Domínio
   	  - Ativação Role e Tools HyperV
   	  - Configurações de disco
	  - Timezone GMT-3

## Renomear Servidor

	Rename-Computer -NewName "NOME_DO_SERVIDOR" -Force -Restart


## Configurar Rede
	#Listar os adaptadores de rede
	Get-NetAdapter

 	# Renomear Nome da Interface
  	Rename-NetAdapter -Name "NomeDaInterface" -NewName "NOVO_NOME_DA_NIC"

 	# Configurar IP
	New-NetIPaddress -InterfaceIndex "ID_DA_INTERFACE" -IPAddress "IP_DO_HOST" -PrefixLength "MASK" -DefaultGateway "GATEWAY"

 	# Configurar DNS
  	Set-DNSClientServerAddress –InterfaceIndex "X" -ServerAddresses "IP_DNS_PRIMARIO,IP_DNS_SECUNDARIO"

	# Configurar VLAN
	Set-NetAdapter -Name "NomeDaInterface" -vlanid "ID_DA_VLAN"

	# Desativar IPV6 (todas as interfaces)
	Disable-NetAdapterBinding -Name "*" -ComponentID ms_tcpip6



## Firewall
	
 	# Ativar ou desativar firewall para as redes de domínio, pública ou privada
	# desativar
	Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
	# ativar
	Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True

## Ingressar no Domínio

	Add-Computer -DomainName "DOMINIO_LOCAL" -Credential "DOMINIO_LOCAL\USUARIO" -Restart -Force

## Ativação Role e Tools HyperV

	Install-WindowsFeature -Name RSAT-Hyper-V-Tools -IncludeAllSubFeature
	Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

## Configurações de disco
	# listar os discos e volumes
	Get-Disk
	Get-Volume
 	
  	# inicializar o disco caso esteja Offline
	Initialize-Disk -Number "X" -PartitionStyle "GPT ou MBR"

 	# criar partição
	New-Partition -DiskNumber "NUMERO_do_DISCO" -DriveLetter "LETRA_da_UNIDADE" -UseMaximumSize
	# caso não queira criar utilizando todo disco, basta trocar a opção "-UseMaximumSize"
	# por "-Size" ex. "-Size 500MB"

## Timezone GMT-3
	tzutil /s "E. South America Standard Time"
