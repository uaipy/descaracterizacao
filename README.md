# Descaracterização — UAI.py

Repositório com a documentação técnica do processo de **descaracterização** de TV Boxes apreendidas, convertidas em **Centrais de Tratamento de Dados (CTD)** de baixo custo no âmbito do projeto **UAI.py**, desenvolvido em parceria com a Receita Federal do Brasil através do programa **Além do Horizonte**.

## Contexto

O avanço tecnológico impulsiona um ciclo contínuo de consumo e descarte de dispositivos eletrônicos, gerando um volume crescente de lixo eletrônico (*e-waste*), um dos desafios ambientais mais significativos da atualidade. Paralelamente, a expansão de soluções de Internet das Coisas (IoT) em setores como a agricultura e a indústria é frequentemente limitada pelo alto custo do hardware especializado.

Nesse cenário, a Receita Federal do Brasil anualmente apreende e descarta milhares de dispositivos TV Box, equipamentos originalmente destinados ao acesso não autorizado a conteúdo de mídia. Apenas entre 2019 e 2020, mais de 97 mil unidades foram destruídas, gerando custos operacionais, desperdício de materiais e componentes eletrônicos valiosos, além de contribuir para a poluição ambiental.

O projeto **UAI.py** surge como uma ponte entre esses dois problemas. A iniciativa propõe uma solução sustentável que transforma esse passivo em um ativo tecnológico: a reutilização das TV Boxes apreendidas, descaracterizando-as e convertendo-as em CTDs de baixo custo. A CTD resultante é uma plataforma agnóstica, projetada para atuar como um *gateway* local que coleta, processa e envia dados de sensores para um *back-end* robusto na nuvem, fomentando a economia circular e a inovação social.

Dado o potencial de impacto ambiental, é importante tornar essa solução acessível à população, divulgando-a e democratizando seu uso, com o fito de possibilitar que mais projetos sejam incorporados e impulsionados pela facilidade de envio de dados à nuvem. A apresentação da solução em eventos de divulgação científica e tecnológica também contribui para despertar o interesse de estudantes e jovens pesquisadores, incentivando a criatividade, a inovação e o desenvolvimento de novas propostas capazes de enfrentar problemas reais por meio da tecnologia.

## Metodologia

O processo de conversão de uma TV Box em uma CTD funcional envolve uma seleção criteriosa de hardware e um procedimento técnico rigoroso de descaracterização, que é o núcleo desta pesquisa.

### Seleção e análise do hardware

A diversidade de modelos de TV Box disponibilizados pela Receita Federal exigiu uma análise técnica. Foram realizados testes de estresse computacional para avaliar a estabilidade do processador, o gerenciamento de memória e a performance de rede sob carga. Os testes incluíram a execução de *benchmarks* sintéticos para CPU e memória, monitoramento da temperatura do System-on-a-Chip (SoC) durante longos períodos de operação e testes de *throughput* de rede.

Os modelos **TX6-p** e **TX9** foram selecionados por apresentarem o melhor equilíbrio entre capacidade computacional e eficiência energética. Ambos são equipados com o SoC **Amlogic S905W**, um processador Quad-core ARM Cortex-A53 e 2 GB de memória RAM. Essas especificações, embora modestas para computação de desktop, são mais do que adequadas para atuar como um orquestrador local de dados, capaz de gerenciar múltiplas conexões de sensores e manter a comunicação com a nuvem.

### O processo de descaracterização

A descaracterização é um requisito obrigatório da Receita Federal e o coração técnico do projeto. Seu objetivo é garantir a remoção permanente do software de pirataria, adaptando o dispositivo para um novo propósito. O processo é dividido em etapas fundamentais:

1. **Substituição do Android nativo** — A distribuição [Armbian](https://www.armbian.com/) (baseada em Debian Linux) foi escolhida por seu baixo uso de hardware, estabilidade e suporte da comunidade para a arquitetura ARM, incluindo SoCs Amlogic e Allwinner. Foram selecionadas imagens com kernels customizados (**5.7.0** para o TX6-p e **5.9.0** para o TX9), com drivers para Ethernet, USB e demais periféricos essenciais.

2. **Gravação e boot** — A imagem do sistema é gravada em cartão microSD; em alguns modelos inclui-se um *bootloader* personalizado (U-Boot) que altera a sequência de inicialização para carregar o Linux a partir do SD em vez da memória interna onde o Android reside.

3. **Instalação na eMMC** — Com acesso de superusuário (*root*), scripts de instalação (`install-aw.sh` ou `install-aml.sh`, conforme o modelo) particionam a memória interna (*eMMC*), copiam o *rootfs* do cartão e instalam o *bootloader*, sobrescrevendo o Android original e consolidando a TV Box como dispositivo de propósito geral.

## Guias por modelo de placa

Cada arquivo abaixo documenta o procedimento completo de descaracterização para um modelo específico de TV Box — etapas no **Windows** (preparação do SD, flash da imagem, edição de boot) e na **UAI.py** (primeira inicialização, configuração e cópia para a eMMC).

| Modelo | Documento | SoC / referência |
|--------|-----------|------------------|
| **TX6-p** | [TX6-p.md](./TX6-p.md) | Allwinner H6 (Tanix TX6) — Armbian 20.05.3, kernel 5.7.0 |
| **TX9** | [TX9-p.md](./TX9-p.md) | Amlogic S905W (Tanix TX3 Mini) — [ophub/amlogic-s9xxx-armbian](https://github.com/ophub/amlogic-s9xxx-armbian), kernel 5.9.0 |

> Os nomes **TX6-p** e **TX9** identificam as variantes de placa validadas no projeto UAI.py após testes de estresse; os guias reproduzem o fluxo operacional usado na conversão de cada unidade em CTD.

## Estrutura do repositório

```
descaracterizacao/
├── README.md      # Contexto, metodologia e índice dos modelos
├── TX6-p.md       # Procedimento — modelo TX6-p
└── TX9-p.md       # Procedimento — modelo TX9
```
