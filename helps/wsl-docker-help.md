Docker

Status das instâncias Linux
    - wsl -l -v

Se ocorrer erros no Ubunto, que não inicia por conta de login inválido, rodar o comando:
    - Restart-Service vmcompute
Habilitar Hyper-V no powershell (requer admim) (só no windows Pro, Home => só lamento)
    - Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

Rodar toda vez que o Linux (wsl) for inicido
    - sudo service docker start

Para acessar os diretórios use no explorer
    - \\wsl$\Ubuntu-20.04

Para liberar memória
    - echo 1 | sudo tee /proc/sys/vm/drop_caches

Para usa o VSCode do Windows e editar arquivos dentro do WSL
    - https://docs.microsoft.com/pt-br/windows/wsl/tutorials/wsl-vscode
	
Dar permissão para o Ubundo acessar arquivos do windows
    - https://www.ti-enxame.com/pt/windows-10/nao-e-possivel-alterar-permissoes-de-arquivo-no-ubuntu-bash-para-windows-10/944516209/

Dar permissão para o windows acessar arquivos do wls
    - chmod 777 ./dir -R
	

