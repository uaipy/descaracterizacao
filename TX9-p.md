https://github.com/ophub/amlogic-s9xxx-armbian

Installing Armbian on Amlogic S905w Android TV Box (Tanix TX3 Mini)

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
                **#1**
                list disk
                
                **#2: Selecionar disco "x" correspondente ao cartão SD**
                select disk "x"
                
                **#3**
                clean
                
                **#4**
                create partition primary
                
                **#5**
                format fs=fat32 quick
                ```
                
            5. O cartão SD estará pronto para uso!
        2. **Colocar a imagem do Armbian no SD**
            1. Abra o Balena Etcher
            2. Selecione a imagem do software desejado 
                
                No caso: Armbian_20.10_Arm-64_bullseye_current_5.9.0_desktop.img.xz
                
            3. “*Flash to disk*”
            4. Espere a execução; 
            5. Ao finalizar, retire o SD e coloque-o novamente; 
            
            > **Ignore as caixas de diálogo pedindo para formatar a unidade**
            > 
        3. **Editar arquivos base:** 
            1. Abra o explorador de arquivos na unidade BOOT;
                
                <aside>
                💡 Se no seu explorador de arquivos não estiver aparecendo as referentes extensões, siga os seguintes passos: 
                
                1. Na barra superior, clique na seção “*Exibir*”;
                2. Aperte na aba “*Opções*”, alocada mais á direita; 
                3. Pressione “*Alterar opções de pasta e pesquisa*”; 
                4. Na janela que abrir, clique na seção “*Modo de Exibição*”; 
                5. Desmarque a opção “*Ocultar as extensões dos tipos de arquivos conhecidos*”;
                
                </aside>
                
            2. Edite os arquivos a seguir:
                
                **u-boot-s905x-s912** → **u-boot.ext**
                
        4. **Edite o arquivo “*extlinux.conf*”**
            - Abra a pasta extlinux;
            - Com o botão direito, clique sobre o arquivo “extlinux.conf” e abra-o no bloco de notas;
            - Faça as seguintes modificações:
            
            ```powershell
            ## **Passo 01: Comente as linhas (add # ao início da linha) de configurações referente a rk-3399**
            ## **Passo 02: Retire o comentário (remove # ao início da linha) da ultima linha das configurações referente a aml s9xxx
            ## Passo 03: Adicione na penultima linha de aml s9xxx o seguinte script** FDT /dtb/amlogic/meson-gxl-s905w-p281.dtb
            
            ## Exemplo abaixo:
            
            LABEL Armbian
            LINUX /zImage
            INITRD /uInitrd
            
            # rk-3399
            #FDT /dtb/rockchip/rk3399-rock-pi-4.dtb
            #FDT /dtb/rockchip/rk3399-nanopc-t4.dtb
            #FDT /dtb/rockchip/rk3399-roc-pc-mezzanine.dtb
            #APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=uart8250,mmio32,0xff1a0000 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0
            
            # rk-3328
            #FDT /dtb/rockchip/rk3328-roc-pc.dtb
            #FDT /dtb/rockchip/rk3328-box-trn9.dtb
            #FDT /dtb/rockchip/rk3328-box.dtb
            #APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=uart8250,mmio32,0xff130000 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0
            
            # aw h6
            #FDT /dtb/allwinner/sun50i-h6-tanix-tx6.dtb
            #APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyS0,115200 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0 video=HDMI-A-1:e
            #APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyS0,115200 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0 mem=2048M video=HDMI-A-1:e
            
            # aml s9xxx
            #FDT /dtb/amlogic/meson-gxbb-p200.dtb
            #FDT /dtb/amlogic/meson-gxl-s905x-p212.dtb
            #FDT /dtb/amlogic/meson-gxm-q200.dtb
            #FDT /dtb/amlogic/meson-g12a-x96-max.dtb
            #FDT /dtb/amlogic/meson-g12b-odroid-n2.dtb
            FDT /dtb/amlogic/meson-gxl-s905w-p281.dtb
            APPEND root=LABEL=ROOTFS rootflags=data=writeback rw console=ttyAML0,115200n8 console=tty0 no_console_suspend consoleblank=0 fsck.fix=yes fsck.repair=yes net.ifnames=0
            ```
            
        5. **Remova a unidade SD do computador;**

---

- **Passos na UAI.py**
    - **Considerações:**
        - Conecte o SD card no leitor da Uai.py;
            
            <aside>
            💻 Utilize um cartão SD a parte pois, ao realizar essa etapa de colocar a TVBox para “*dar boot”* por um cartão, ela cria uma pasta “LOST.DIR” no cartão, corrompendo-o e impedindo o mesmo de inicializar o Linux normalmente. 
            
            Após as etapas 1 e 2 a seguir, desligue a TVBox e coloque o cartão com a imagem do Linux.
            
            </aside>
            
            !Arquivo que corrompe o SD ao utiliza-lo na definição do boot pela TVBox;
            
            Arquivo que corrompe o SD ao utiliza-lo na definição do boot pela TVBox;
            
        - Conecte um cabo de rede;
        - Conecte um teclado e mouse;
        - Conecte o Uai.py á alimentação de energia;
    - **Passos de execução**
        1. **Inicialização**
            - Inicialize a TVBOX normalmente → O android nativo será executado;
        2. **Selecionando memória para o boot do sistema**
            1. Vá a gaveta de aplicativos da TVBOX e execute o app “UPDATE&BACKUP”;
            2. Na aba *UpdateLocale*, pressione o botão “Select”;
            3. Na tela que abrir, selecione o arquivo “/storage/32E7-01B6/aml_autoscript.zip”
                - Ele costuma ser o primeiro arquivo da tela;
                - Ao selecionar esse arquivo, você retorna-rá automaticamente para a tela anterior;
            4. Na aba *UpdateLocale*, pressione o botão “Update”;
                - Abrirá uma janela de diálogo informando que o processo não pode ser interrompido, clique no botão “Update” novamente;
                - O sistema será reiniciado e dará boot no linux;
        3. **Boot no linux**
            - Ao iniciar o linux será solicitada a troca da senha *root* do sistema;
                
                ```powershell
                senha: 1041ramle
                ```
                
            - Será solicitado um nome de usuário e senha:
                
                ```powershell
                Username: Thanos
                Password: 1041ramle
                ```
                
        4. **Linux inicializado**
            1. Acesse o Synaptic Package Manager; 
            2. Copie o SO para a memória interna da UAI.py:
                - Abra o terminal;
                - Digite os comandos a seguir:
                    
                    ```powershell
                    **## Passo 01: Acesse o super usuário**
                    	sudo su
                    
                    **## Passo 02: Acesse a pasta primária do super usuário**
                    	CD
                    
                    **## Passo 03: Copie o SO para a memória interna**
                    	./install-aml.sh
                    	## Ao final da operação deve retornar uma mensagem "*Complete copy to eMMC*"
                    ```
                    
                - Desligue a UAI.py e retire o cartão SD;
- **Opcional**
    - Instalação do GCompris - Software educacional:
        - Abra o terminal;
        - Digite os comandos a seguir:
            
            ```powershell
            sudo apt-get install gcompris-qt
            ```
