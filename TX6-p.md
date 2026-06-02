- Software escolhido:
    - Armbian - *version* 20.05.3
    
    http://www.youtube.com/watch?v=GHk22VIxoIU
    

---

- **EXECUÇÃO NO WINDOWS**
    - **Softwares requisitados:**
        - Balena Etcher
            
            https://www.balena.io/etcher/
            
    - **Passos de execução:**
        1. **Formatação do cartão SD:**
            1. Insira o cartão SD no dispositivo; 
            2. Abra o *cmd* (*Press:* ”win + r” → “Enter”);
            3. Na janela que abrir, digite:
                
                ```python
                DISKPART
                ```
                
            4. Abrirá outra janela, digite:
                
                ```python
                #1
                list disk
                
                #2: Selecionar disco "x" correspondente ao cartão SD
                select disk "x"
                
                #3
                clean
                
                #4
                create partition primary
                
                #5
                format fs=fat32 quick
                ```
                
            5. O cartão SD estará pronto para uso!
        2. **Colocar a imagem do Armbian no SD**
            1. Abra o Balena Etcher
            2. Selecione a imagem do software desejado 
                
                No caso: Armbian_20.05.3_Arm-64_buster_current_5.7.0-rc2_desktop_20200425.img
                
            3. “*Flash to disk*”
            4. Espere a execução; 
            5. Ao finalizar, retire o SD e coloque-o novamente; 
            
            > **Ignore as caixas de diálogo pedindo para formatar a unidade**
            > 
        3. **Colocar a imagem do u-boot no SD**
            1. Abra o Balena Etcher
            2. Selecione a imagem do u-boot; 
                
                No caso: u-boot-allwinner-h6-tanix-tx6
                
            3. “*Flash to disk*”
            4. Espere a execução; 
            5. Ao finalizar, retire o SD e coloque-o novamente; 
        4. **Editar o arquivo “*uEnv.txt”;*** 
            
            ```python
            ## **Passo 01: Comente as linhas (add # ao início da linha) de configurações referente a rk-3399**
            	
            	# rk-3399
            	#FDT=/dtb/rockchip/rk3399-rock-pi-4.dtb
            	#FDT=/dtb/rockchip/rk3399-nanopc-t4.dtb
            	#APPEND=root=LABEL=ROOTFS rootflags=data=writeback rw console=uart8250,mmio32,0xff1a0000 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=
            	
            **## Passo 02: Remova os comentários (remover # ao início da linha) presentes nas configuração para aw h6**
            	
            	# aw h6
            	FDT=/dtb/allwinner/sun50i-h6-tanix-tx6.dtb
            	APPEND=root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyS0,115200 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0
            	APPEND=root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyS0,115200 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0 mem=2048M video=HDMI-A-1:e
            ```
            
        5. **Editar arquivos base:** 
            1. Abra o explorador de arquivos;
                
                <aside>
                💡 Se no seu explorador de arquivos não estiver aparecendo as referentes extensões, siga os seguintes passos: 
                
                1. Na barra superior, clique na seção “*Exibir*”;
                2. Aperte na aba “*Opções*”, alocada mais á direita; 
                3. Pressione “*Alterar opções de pasta e pesquisa*”; 
                4. Na janela que abrir, clique na seção “*Modo de Exibição*”; 
                5. Desmarque a opção “*Ocultar as extensões dos tipos de arquivos conhecidos*”;
                
                </aside>
                
            2. Adicionar a extenção “*orig*” nos arquivos a seguir:
                
                **boot.cmd** → **boot.cmd.orig**
                **boot.scr** → **boot.scr.orig**
                **boot-emmc.cmd** → **boot-emmc.cmd.orig**
                **boot-emmc.scr** → **boot-emmc.scr.orig**
                
            3. Excluir a extensão “*aw*” nos arquivos a seguir:
                
                **boot.cmd.aw** → **boot.cmd**
                **boot.scr.aw** → **boot.scr**
                **boot-emmc.cmd.aw** → **boot-emmc.cmd**
                **boot-emmc.scr.aw** → **boot-emmc.scr**
                
        6. **Remova a unidade SD do computador;**

---

- **EXECUÇÃO NA UAI.py**
    - **Considerações:**
        - Conecte o SD card no leitor da Uai.py;
        - Conecte um cabo de rede;
        - Conecte um teclado e mouse;
        - Conecte o Uai.py á alimentação de energia;
    - **Passos de execução**
        1. **Inicialização**
            1. Entre no root, quando solicitado:
                
                ```python
                *arm-64 login:* root
                *Password:* 1234
                ```
                
            2. Defina uma nova senha para o root; 
            3. Quando solicitado, defina um usuário e senha; 
        2. **Sistema inicializado**
            
            > Recomenda-se atualizar o sistema, seguindo:
            > 
            1. Inicialize o terminal; 
            2. Execute os comando a seguir:
                
                ```python
                sudo apt update && sudo apt upgrade
                ```
                
        3. **Formatação**
            
            <aside>
            💡 Todas as personalizações realizadas no cartão até então, serão mantidas ao utilizá-lo em um outro aparelho.
            Logo, **caso se queira fazer personalizações**, colocar um papel de parede, instalar um software, fazer atualizações, **faça-as antes dessa etapa** para que todos os aparelhos também as tenham!
            
            </aside>
            
            1. Inicialize o terminal;
            2. Execute os comandos a seguir:
                
                ```python
                **## Passo 01: Acesse o super-usuário** 
                	sudo su -
                
                **## Passo 02:** 
                	ls
                
                **## Passo 03: Utilizando o editor nano, edite o arquivo install-aw.sh**
                	nano install-aw.sh
                
                **## Passo 04: modificar o arquivo install-aw.sh**
                		## Passo 4.1. Comente as primeiras linhas até o comando "DEV_EMMC" tal como na *imagem 01*
                		## Passo 4.2. Ao finalizar os comentários, modifique o comando **DEV_EMMC:
                			DEV_EMMC="/dev/$emmc" -> DEV_EMMC="/dev/mmcblk1"
                		## Passo 4.3. Pressionar "ctrl + x" e salvar as alterações
                		## Passo 4.4. Pressionar "Enter" para retornar ao terminal** 
                
                **## Passo x: Formatar a memória eMMC**
                	./install-aw.sh
                	## Quando executado corretamente, o arquivo irá retornar após um tempo a mensagem "Complete copy OS to eMMC"
                ```
                
                !**Imagem 01.:** Arquivo [*install-aw.sh* editado](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d977015e-bdd0-46cc-9e83-2366f36a8ac4/Untitled.png)
                
                **Imagem 01.:** Arquivo *install-aw.sh* editado
                
            3. Feche o terminal; 
            4. Desligue o UAI.py; 
            5. Retire o cartão SD; 
            6. Religue o aparelho e ele irá iniciar o Linux;
