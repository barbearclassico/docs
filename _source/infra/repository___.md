Architecting Sceptre
====================

Sceptre is written in a way that aims to be unopinionated in how it is used. It
is designed to work equally well with simple and complex infrastructures.
Sceptre’s flexible nature, and the variation in how people organise their AWS
accounts, makes it difficult to give generic advice on how best to use it, as
it can be use-case specific. However, the following patterns have emerged from
our use of Sceptre at Cloudreach.

Project layout
--------------

Sceptre’s nested StackGroups means that it is possible to store an entire
company or department’s infrastructure in a single Sceptre project. While this
is possible, it isn’t recommended. Having a large number of developers interact
with a single repository can be difficult from a version control point of view,
and it might be dangerous to let developers touch infrastructure that they are
not directly involved with.

We recommend different Sceptre projects for each large ‘section’ of the
infrastructure being built.

Directory layout
----------------

You need to store your :doc:`Sceptre templates <templates>` in the ``templates`` directory within
your Sceptre Project.

.. code-block:: text

   .
   ├── config
   └── templates

StackGroup structure
--------------------

StackGroups can be arbitrarily nested, and StackGroup commands can be applied
to any level of the StackGroup tree. This can make it difficult to know how to
divide StackGroups up.

When considering how to split StackGroups up, it’s useful to remember the
following properties of StackGroups:

1. Region specific. There is no way for an StackGroup to launch stacks in
   multiple regions; a StackGroup can, however, contain sub-StackGroups in
   different regions. A StackGroup should therefore contain Stacks that belong
   in the same region.

2. Command related. StackGroup level commands (like ``launch``) are applied to
   every stack in a StackGroup. There is no way to exclude stacks from an
   StackGroup level command. Therefore, StackGroups should contain stacks that
   can be launched and deleted together.

   Some stacks are inherently longer-lived than others. Stacks containing
   VPC-level infrastructure are likely to be longer lived than stacks
   containing an ephemeral testing environment. It therefore makes sense to
   split these up into separate StackGroups.

Example architectures
---------------------

The following examples demonstrate how we might architect various Sceptre
projects. Only configuration layout is shown, and the examples are merely meant
to demonstrate ways of organising different projects.

-  Application development within an externally defined VPC

   .. code-block:: yaml

      - config
          - prod
              - application
                  - asg.yaml
                  - security-group.yaml
              - database
                  - rds.yaml
                  - security-group.yaml

-  DevOps team who manage all the infrastructure for their service

   .. code-block:: yaml

      - config
          - prod
              - network
                  - vpc.yaml
                  - subnet.yaml
              - frontend
                  - api-gateway.yaml
              - application
                  - lambda-get-item.yaml
                  - lambda-put-item.yaml
              - database
                  - dynamodb.yaml

-  Centralised, company-wide networking

   .. code-block:: yaml

      - config
          - prod
              - vpc.yaml
              - public-subnet.yaml
              - application-subnet.yaml
              - database-subnet.yaml
          - dev
              - vpc.yaml
              - public-subnet.yaml
              - application-subnet.yaml
              - database-subnet.yaml

-  IAM management

   .. code-block:: yaml

      - config
          - account-1
              - iam-role-admin.yaml
              - iam-role-developer.yaml
          - account-2
              - iam-role-admin.yaml
              - iam-role-developer.yaml
Oficina
=======

Armário de parede
-----------------

`right|300px <Imagem:Armario_parede_oficina.jpg>`__

+-------------+----------------------------------------------+-----+
| Part Number | Descrição                                    | Qtd |
+=============+==============================================+=====+
| 4840-7100   | Kit de Integração Gaveta IBM SurePOS 500     | 5   |
+-------------+----------------------------------------------+-----+
| 41J7807     | Gaveta IBM estreita                          | 1   |
+-------------+----------------------------------------------+-----+
| 10H3331     | Miolo de Gaveta IBM estreita                 | 1   |
+-------------+----------------------------------------------+-----+
| 41J7807     | Gaveta IBM estreita reacondicionada          | 1   |
+-------------+----------------------------------------------+-----+
| 10H3331     | Miolo de Gaveta IBM estreita reacondicionada | 1   |
+-------------+----------------------------------------------+-----+
| 25L5094     | TFT IBM 12"                                  | 2   |
+-------------+----------------------------------------------+-----+
| 30L5795     | TFT IBM 12"                                  | 5   |
+-------------+----------------------------------------------+-----+
| TSQF0000701 | TFT IBM 12"                                  | 1   |
+-------------+----------------------------------------------+-----+
| TSQF0000801 | TFT IBM 12"                                  | 2   |
+-------------+----------------------------------------------+-----+
| 14J1298     | Tela SurePOS 500 modelos Types 1/2           | 2   |
+-------------+----------------------------------------------+-----+

Arquivo CDs
~~~~~~~~~~~

Gavetas
~~~~~~~

Gaveta Cima
^^^^^^^^^^^

-  Disco Mentes Virtuais - Clonagem feita em 2009 aquando o transplante
   de disco para novo servidor.

Gaveta Meio
^^^^^^^^^^^

-  Placas PCI-série
-  Memórias

Gaveta Baixo
^^^^^^^^^^^^

Armário Metálico
----------------

`right|200px <Imagem:Armario_metalico01.jpg>`__

+-----------+-----------+-----------+-----------+-----------+-----------+
| Prat      | Categoria | Marca     | Part      | Descrição | Qtd       |
|           |           |           | Number    |           |           |
+===========+===========+===========+===========+===========+===========+
| 2         | Display   | IBM       | 42V3978   | IBM       | 2         |
|           | Cliente   |           |           | Client    |           |
|           |           |           |           | Display   |           |
|           |           |           |           | SurePOS   |           |
|           |           |           |           | 500 type  |           |
|           |           |           |           | 3/4       |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Display   | IBM       | 14R1896   | IBM       | 1         |
|           | Cliente   |           |           | Client    |           |
|           |           |           |           | Display   |           |
|           |           |           |           | SurePOS   |           |
|           |           |           |           | 500 type  |           |
|           |           |           |           | 3/4       |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Display   | IBM       | 15K2012   | IBM       | 2         |
|           | Cliente   |           |           | Client    |           |
|           |           |           |           | Display   |           |
|           |           |           |           | (ficha    |           |
|           |           |           |           | db9)      |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Display   | IBM       | 93F1090   | IBM       | 1         |
|           | Cliente   |           |           | Client    |           |
|           |           |           |           | Display   |           |
|           |           |           |           | (ficha    |           |
|           |           |           |           | db9)      |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | MSR       | IBM       | 47L7229   | IBM MSR   | 9         |
|           |           |           |           | monitor   |           |
|           |           |           |           | branco    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | MSR       | IBM       | 47L7229   | IBM MSR   | 3         |
|           |           |           |           | monitor   |           |
|           |           |           |           | preto     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Rato      | IBM       | -         | Rato de   | 3         |
|           |           |           |           | bola PS2  |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Rato      | IBM       | -         | Rato de   | 1         |
|           |           |           |           | bola USB  |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 4         | Cod.Barra | PSC       | HS1250    | Leitor    | 1         |
|           | s         |           |           | Código de |           |
|           |           |           |           | barras de |           |
|           |           |           |           | Mesa      |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Transform | IBM       | 42H1176   | IBM       | 2         |
|           | ador      |           |           | 4610-TF6  |           |
|           |           |           |           | Adaptador |           |
|           |           |           |           | AC        |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Media     | IBM       | -         | Drive de  | 1         |
|           | externo   |           |           | disquetes |           |
|           |           |           |           | IBM USB   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Media     | IBM       | -         | Drive de  | 1         |
|           | externo   |           |           | disquetes |           |
|           |           |           |           | IBM POS   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Media     | IBM       | -         | Cabo      | 1         |
|           | externo   |           |           | disquetes |           |
|           |           |           |           | IBM type  |           |
|           |           |           |           | 1         |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Media     | IBM       | -         | Cabo      | 1         |
|           | externo   |           |           | disquetes |           |
|           |           |           |           | IBM type  |           |
|           |           |           |           | 2         |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Media     | LG        | GSA-2166D | DVD-RW    | 1         |
|           | externo   |           |           | USB       |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Acessório | EPSON     | -         | Epson -   | 1         |
|           | s         |           |           | Módulo    |           |
|           | EPSON     |           |           | Série     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Acessório | EPSON     | -         | Epson -   | 7         |
|           | s         |           |           | Módulo    |           |
|           | EPSON     |           |           | Paralelo  |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Conv.     | KONIG     | SB-2045   | Conversor | 2         |
|           | USB-Série |           |           | USB-Série |           |
|           |           |           |           | KONIG     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Conv.     | ATEN      | SB-2040   | Conversor | 2         |
|           | USB-Série |           |           | USB-Série |           |
|           |           |           |           | KONIG     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Conv.     | SUNIX     | PSU4037A2 | Conversor | 3         |
|           | USB-Série |           |           | PCI-Série |           |
|           |           |           |           | SUNIX     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Conv.     | SUNIX     | -         | cabo para | 2         |
|           | PCI-Série |           |           | PCI-Série |           |
|           |           |           |           | SUNIX     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Conv.     | DeLock    | -         | Conversor | 1         |
|           | PCI-Série |           |           | PCI-Série |           |
|           |           |           |           | DeLock    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | MSR       | -         | MSR-430PK | MSR       | 1         |
|           |           |           |           | Branco    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | MSR       | KDE       | KT-1282   | KDE MSR   | 1         |
|           |           |           |           | Branco    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Modem     | 3com/USRo | -         | Modem     | 1         |
|           |           | botics    |           | 3com      |           |
|           |           |           |           | USrobotic |           |
|           |           |           |           | s         |           |
|           |           |           |           | 56k Séris |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Modem     | vários    | -         | Modem PCI | 5         |
|           |           |           |           | 56k       |           |
|           |           |           |           | encaixe   |           |
|           |           |           |           | de pinos  |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Conv.     | -         | -         | Expansão  | 1         |
|           | ISA-Série |           |           | ISA -     |           |
|           |           |           |           | Série     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | ISA Ether | Realtek   | -         | Ethernet  | 1         |
|           |           |           |           | Realtek   |           |
|           |           |           |           | ISA       |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | PCMCIA    | Micronet  | SP160A    | PCMCIA -  | 1         |
|           | Ether     |           |           | Ethernet  |           |
|           |           |           |           | -         |           |
|           |           |           |           | Micronet  |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | PCMCIA    | Level One | -         | PCMCIA -  | 3         |
|           | Ether     |           |           | Ethernet  |           |
|           |           |           |           | - Level   |           |
|           |           |           |           | One       |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | PCMCIA    | -         | -         | PCMCIA -  | 1         |
|           | Ether     |           |           | Ethernet  |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Conv.     | Sweex     |           | Expansão  | 1         |
|           | PCI-Paral |           |           | PCI -     |           |
|           | ela       |           |           | Paralela  |           |
|           |           |           |           | - Sweex   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Conv.     | Linksys   | WDT11     | Expansão  | 1         |
|           | PCI-PCMCI |           |           | PCI -     |           |
|           | A         |           |           | PCMCIA -  |           |
|           |           |           |           | Linksys   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | PCMCIA    | SMC       | SMC2531W- | Wireless  | 1         |
|           | WIFI      |           | B         | PCMCIA    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Access    | COMPAQ    | PC24E-H-F | Access    | 1         |
|           | Point     |           | C         | Point     |           |
|           |           |           |           | Compaq    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | CCTV -    | VIDO      | AU-VC-803 | Camara    | 1         |
|           | camara    |           | S         | VIDO      |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | PDA       | HP        | -         | iPAQ      | 1         |
|           |           |           |           | PocketPC  |           |
|           |           |           |           | h5500     |           |
|           |           |           |           | Series    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Acessório | EPSON     | -         | Wall      |           |
|           | s         |           |           | Mount     |           |
|           | EPSON     |           |           | Epson     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | MSR       | FEC       | 0507805G3 | MSR Preto | 1         |
|           |           | RichPOS   |           | para POS  |           |
|           |           |           |           | RichPOS   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 1         | Dataswitc |           | AB7030    | Dataswitc | 3         |
|           | h         |           |           | h         |           |
|           |           |           |           | porta     |           |
|           |           |           |           | série     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Media     | Revoltek  | -         | Caixa     | 1         |
|           | externo   |           |           | disco IDE |           |
|           |           |           |           | 2.5" -    |           |
|           |           |           |           | USB 2.0   |           |
|           |           |           |           | (em       |           |
|           |           |           |           | caixa)    |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 3         | Media     | -         | -         | Caixa     | 1         |
|           | externo   |           |           | azul      |           |
|           |           |           |           | disco IDE |           |
|           |           |           |           | 3.5" -    |           |
|           |           |           |           | USB 1.0   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
| 2         | Display   | MyPOS     | -         | Display   | 1         |
|           | Cliente   |           |           | de        |           |
|           |           |           |           | Cliente   |           |
|           |           |           |           | MyPOS     |           |
|           |           |           |           | preto     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+

Escritório
==========

Caixa Consumíveis
-----------------

+-----------+-----------+-----------+-----------+-----------+-----------+
| Prat      | Categoria | Marca     | Part      | Descrição | Qtd       |
|           |           |           | Number    |           |           |
+===========+===========+===========+===========+===========+===========+
|           | Consumíve | EPSON     | ERC-38    | Fita      | 1         |
|           | is        |           | B/R       |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | EPSON     | T016      | Cartucho  | 1         |
|           | is        |           |           | de tinta  |           |
|           |           |           |           | 5 cores   |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | EPSON     | S020025   | Cartucho  | 1         |
|           | is        |           |           | de tinta  |           |
|           |           |           |           | preto     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | EPSON     | S020089-G | Cartucho  | 1         |
|           | is        |           |           | de tinta  |           |
|           |           |           |           | cores     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | EPSON     | T005      | Cartucho  | 1         |
|           | is        |           |           | de tinta  |           |
|           |           |           |           | cores     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | Lexmark   | 83        | Cartucho  | 1         |
|           | is        |           |           | tinta     |           |
|           |           |           |           | cores     |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | Niko      | Star      | Fita      | 1         |
|           | is        |           | SP200     |           |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | ?         | ?         | Fita      | 4         |
|           | is        |           |           | 13mmx10mt |           |
|           |           |           |           | s         |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | HP        | 92A       | HP        | 2         |
|           | is        |           |           | Laserjet  |           |
|           |           |           |           | Series    |           |
|           |           |           |           | 1100 e    |           |
|           |           |           |           | 3200      |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | Konica    | C 2200    | Toner     | 2         |
|           | is        | Minolta   | Series    | azul para |           |
|           |           |           | Ciano     | Minolta   |           |
|           |           |           |           | 2200Serie |           |
|           |           |           |           | s         |           |
+-----------+-----------+-----------+-----------+-----------+-----------+
|           | Consumíve | Konica    | Y 2200    | Toner     | 1         |
|           | is        | Minolta   | Series    | azul para |           |
|           |           |           | Amarelo   | Minolta   |           |
|           |           |           |           | 2200Serie |           |
|           |           |           |           | s         |           |
+-----------+-----------+-----------+-----------+-----------+-----------+

Equipamento
===========

Mala de Assistência
-------------------

`right|300px <Imagem:Mala_ATs.jpg>`__

**Descrição:** Mala metalizada, em alumínio. É a mala de AT ́s no geral.
Transporta ferramenta genérica aquela indispensável a qualquer AT. Deve
ser encarada como a mala que se leva para qualquer AT, transporta também
a folhas de relatório de AT.

-  Registo

   -  Capa preta com folhas de AT.
   -  Caneta

-  Alicates

   -  Alicate de Corte verde
   -  Alicate de pontas laranja
   -  Alicate de pontas Vermelho grande
   -  Alicate de universal Vermelho grande
   -  Alicate pequeno preto universal
   -  Alicate pequeno vermelho pontas curvas
   -  Alicate pequeno vermelho pontas chatas

-  Chaves

   -  Conjunto de 5 chaves – transparentes/verde/vermelho
   -  Duas Chaves de Fendas cinza/laranja
   -  Chave estrela preta/verde comprida
   -  Chave estrela hexagonal azul nº 0.8
   -  Chave bocas/luneta nº 10
   -  Chave roquete com ponta luneta nº 10

-  Equipamento de Rede

   -  Alicate Rj45
   -  Fichas Rj45
   -  Multímetro testa redes
   -  Pilha suplente de 9V
   -  Carapuças para fichas RJ45

-  Equipamento de Limpeza

   -  Solvente Kontakt nº 60
   -  pincel

-  Equipamento de Soldadura

   -  Duas pinças
   -  Laranja Beta – pontas cur vas
   -  Lisa direita – metalizada
   -  Rolo Vermelho de estanho
   -  Lata com resina
   -  Ferro de solda JBC
   -  Duas agulhas azuis – auxiliares de soldadura

-  Colas Adesivos e Abraçadeiras

   -  Pistola de cola
   -  Saco com cargas suplentes
   -  Fita isoladora
   -  Abraçadeiras plásticas

Equipamento de Rede
-------------------

`right|300px <Imagem:Equip_Rede.jpg>`__ **Descrição:** Saco preto com
cinta para transportar ao ombro, contém todo o material de rede, desde
ferramentas e material de rede, até material e ferramentas para VNC.

Material:

-  2 Alicates de cravar fichas RJ45
-  Fixas e carapuças RJ45
-  Fichas BNC
-  Alicate de Cravar fichas BNC
-  Multímetro testa redes
-  X-acto

Estojo de Limpeza
-----------------

`right|300px <Imagem:Equip_Limpeza.jpg>`__

**Descrição:** Estojo indispensável, para limpezas de máquinas.

Material:

-  Conjunto de pincéis
-  Latas de solvente (indispensável Vermelho no60)
-  Panos de limpeza e panos com características mais rugosas ou
   abrasivas
-  Spray de Lubrificação
-  Secador de cabelo - “Punisher”
-  Conjunto de Pequenas escovas laranja
-  Escova de Aço

Saco de Colas
-------------

`right|300px <Imagem:Equip_Colas.jpg>`__ **Descrição:** O material e
ferramentas aqui guardadas, destinam-se a ultrapassar pequenas
dificuldades físicas como fixar uma antena wireless num local, colar uma
calha técnica.

Material:

-  Pistola de cola quente
-  Cargas de cola quente
-  Fitas de Velcro Auto-colante
-  Fita-cola Dupla Face
-  Carga de Silicone para Vidros (Não danifica o meio onde é aplicado)
-  Álcool Etílico (destina-se a limpar uma zona antes da aplicação de
   uma cola)
-  Pistola de Silicone

Estojo de Soldadura
-------------------

`right|300px <Imagem:Equip_Soldadura.jpg>`__

**Descrição:** Transporta ferramentas de soldadura a estanho.

Material:

-  Ferro de soldar a estanho JBC
-  Estanho (em bobine e pequenos rolos)
-  Lata com pasta de solda
-  Esponja para limpeza do ferro
-  Suporte de peças para soldar (robô)
-  3 pinças
-  Conjunto de 3 agulhas para suporte á soldadura
-  Malha para soldar
-  Aspirador de Soldadura

Xerox - Repositório de Imagens
------------------------------

`right|300px <Imagem:Server_Xerox.jpg>`__

Usar o clonezilla para gerar um instalador automatico
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Criar a imagem
^^^^^^^^^^^^^^

Clonezilla:

Depois de terminar a imagem ir para a linha de comandos e escrever
::
    sudo su
    dhclient -v eth0
    /etc/init.d/ssh start

No computador remoto fazer:
::
    ssh ip -l user # password: "live"``
    ocs-live-dev -c \
        -g en_US.UTF-8 \
        -t \
        -k NONE \
        -e "-g auto -e1 auto -e2 -c -r -j2 -p true restoredisk icreTest01 sda" \
        icreTest01

Menu de ajuda do **ocs-live-dev**:
::
    | ``OPTION:``
    | ``-l, --language INDEX Set the language to be shown by index number:``
    | ``      [0|en_US.UTF-8]: English,``
    | ``      [2|zh_TW.UTF-8]: Traditional Chinese (UTF-8, Unicode) - Taiwan``
    | ``      [a|ask]: Prompt to ask the language index``
    | ``--file-name-prefix NAME    Assign the output file name as NAME.zip or NAME.tar. /usr/sbin/ocs-live-dev will auto append '.zip' or '.tar' in the end of filename.``
    | ``-b, --bg-mode  [text|graphic]  Assign the background of boot menu. Default is graphic``
    | ``-e, --extra-param  PARAM  Assign extra parameter PARAM for clonezilla live to run, PARAM will be appended when run in ocs-live-restore or ocs.``
    | ``-g, --ocs-live-language LANGUAGE Assign the language when using clonezilla live, available languages are en_US.UTF-8, zh_TW.UTF-8 ``
    | ``-k, --ocs-live-keymap KEYBOARD_LAYOUT Assign the keyboard layout when using clonezilla live. You can find the keyboard layout in /usr/share/X11/xkb/rules/base.lst. e.g. use '-k fr' for French keyboard layout. If 'NONE' is used, the default one (US keyboard) will be use. For more info, please check the live manual on Debian Live website.``
    | ``-m, --custom-ocs  PATH/custom-ocs  Use the customized ocs program 'custom-ocs' instead of the default one. Note! PATH should be assigned so that it can be found. This is advanced mode.``
    | ``-t, --ocs-live-batch  Set clonezilla live to run in batch mode, usually it's for restoring. If this mode is set, some dialog question will be ignored.``
    | ``-j, --debian-iso ISO_FILE  Assign Debian live template iso file name as ISO_FILE to be used to create Clonezilla live. By default the ISO_FILE is "debian-live-for-ocs.iso".``
    | ``-p, --image-path   Assign the clonezilla image source path where CLONEZILLA_IMAGE_NAME exists in this server.  Default = /home/partimag``
    | ``-d, --dev DEV      Write the output to DEV (such as /dev/sdg1)``
    | ``-a, --boot-loader [grub|syslinux]  Force to use grub or syslinux as boot loader in target device``
    | ``-i, --assign-version-no NO  Assign the version no as NO instead of date. This only works when using with option -s and -c.``
    | ``-f, --batch-mode   Run ocs-live-dev in batch, unattended mode.``
    | ``-s, --skip-image   Do not include any clonezilla image. The is used to created a live CD or USB flash drive with DRBL/Clonezilla programs only.``
    | ``-q, --existing-img To use image exists on the destination device when creating recovery device (e.g. /dev/sdg1). the image might already exist (copied or created on the device before). For other mode, image is copied or linked in this program.``
    | ``-c, --create-release  Create a USB package release file, this will not create a bootable device, instead it will create a zip file contain all necessary files to make it boot and run clonezilla live.``
    | ``-o, --normal-menu  When a clonezilla image is inserted, by default only restore menu will be shown in the created ISO file. If you want to show normal menu, i.e. with save and restore menu, use this one.``
    | ``-x, --extra-boot-param  EXTRA_PARAM  Assign extra boot parameter EXTRA_PARAM for clonezilla live kernel to read. These parameters are the same with that from live-initramfs. Ex. "noeject" can be use to not prompt to eject the CD on reboot.``
    | ``-y, --syslinux-ver VER  Assign the syslinux version as VER. E.g. 6.02, 6.03-pre1``
    | ``-u, --include-dir DIR   Include a dir in the target zip file.``
    | ``-n, --ocs-live-boot-menu-option EXTRA_OPTION Assign an extra option for ocs-live-boot-menu. //NOTE// Do not put '-' in this EXTRA_OPTION, /usr/sbin/ocs-live-dev will add that automatically. e.g. if you want to add -s1 for ocs-live-boot-menu to run, use 's1' only.``
    | ``ocs-live-dev will download a template Debian live CD for clonezilla iso file if ncecessary. You can also download it by yourself, and put it in the working directory when you run ocs-live-dev. If you want to create that template iso file in Debian Etch, run create-debian-live.``
    | ``NOTE! You have to prepare the target partition first, and the filesystem should be ready (FAT or ext2/3).``
    | ``Ex:``
    | ``To put clonezilla image squeeze-ocs (located in /home/partimag in clonezilla server) to USB device /dev/sdg1, you can run:``
    | ``  ocs-live-dev -d /dev/sdg1 squeeze-ocs``
    | ``To put more images, just append them, such as:``
    | ``  ocs-live-dev -d /dev/sdg1 squeeze-ocs etch-ocs``
    | ``To create a bootable, recovery USB device (e.g. /dev/sdg1, VFAT file system) with image "precise-x86-20140206" included:``
    | ``  ocs-live-dev -g en_US.UTF-8 -t -k NONE -d /dev/sdg1 -e "-g auto -e1 auto -e2 -c -r -j2 -p choose restoredisk precise-x86-20140206 sda" precise-x86-20140206``
    | ``To create a Live USB device with DRBL/Clonezilla programs, which can be used to save image:``
    | ``  ocs-live-dev -s -d /dev/sdg1``
    | ``To create a zip file of general purpose Clonezilla live. Later it can be put in an USB flash drive or similar device:``
    | ``  ocs-live-dev -s -c``
    | ``To create a zip file for restoring with clonezilla image squeeze-r5 builtin, and make it boot then restore the image squeeze-r5 to sda in unattended mode (Only confirmation in the beginning), you can run:``
    | ``  ocs-live-dev -c -g en_US.UTF-8 -t -k NONE -e "-b -c restoredisk squeeze-r5 sda" squeeze-r5``
    | ``To create a zip file to run your own custom-ocs program:``
    | ``  ocs-live-dev -g en_US.UTF-8 -k NONE -s -c -m ./custom-ocs``

.. code:: bash

    #!/bin/bash
    #
    #
    # 
    # image creation command: 
    # /usr/sbin/ocs-sr -q2 -c -j2 -z1p -i 4096 -p true savedisk icreTest01 sda
    #
    # image restore command
    # /usr/sbin/ocs-sr -g auto -e1 auto -e2 -c -r -j2 -p true restoredisk icreTest01 sda
    #
    # Author: Steven Shiau <steven _at_ nchc org tw>
    # License: GPL
    #
    # In this example, it will allow your user to use clonezilla live to choose 
    # (1) A samba server as clonezilla home image where images exist.
    # (2) Choose an image to restore to disk.   
    # When this script is ready, you can run
    # ocs-iso -g en_US.UTF-8 -k NONE -s -m ./custom-ocs-1
    # to create the iso file for CD/DVD. or
    # ocs-live-dev -g en_US.UTF-8 -k NONE -s -c -m ./custom-ocs-1
    # to create the zip file for USB flash drive. 
    # Begin of the scripts:
    # Load DRBL setting and functions
    DRBL_SCRIPT_PATH="${DRBL_SCRIPT_PATH:-/usr/share/drbl}"
    . $DRBL_SCRIPT_PATH/sbin/drbl-conf-functions
    . /etc/drbl/drbl-ocs.conf
    . $DRBL_SCRIPT_PATH/sbin/ocs-functions
    # load the setting for clonezilla live.
    [ -e /etc/ocs/ocs-live.conf ] && . /etc/ocs/ocs-live.conf
    # Load language files. For English, use "en_US.UTF-8". For Traditional Chinese, use "zh_TW.UTF-8"
    ask_and_load_lang_set en_US.UTF-8
    # The above is almost necessary, it is recommended to include them in your own custom-ocs.
    # From here, you can write your own scripts.
    # 1. Configure network
    # If you are sure there is a DHCP server, you can use "dhclient -v eth0" instead of ocs-live-netcfg.
    # ocs-live-netcfg
    # 2. Mount the clonezilla image home. Available types:
    # local_dev, ssh_server, samba_server, nfs_server
    # prep-ocsroot -t samba_server
    # 3. Restore the image
    if mountpoint $ocsroot &>/dev/null; then
    # Here we use "-p true" (not "-p choose", "-p reboot", or "-p poweroff") to avoid 
    # the reboot/poweroff command being run in jfbterm/bterm.
    # Let the ocs-live-final-action and ocs-live-run-menu to take care of the reboot/poweroff actions.
    # Otherwise the Debian live "Press Enter to continue" message after poweroff/shutdown command 
    # is issued might be coverd by jfbterm/bterm and user will not have any idea what's happening after ocs-sr is run.
    # ocs-sr -g auto -e1 auto -e2 -c -r -j2 -p true restoredisk "ask_user" "ask_user"
    ocs-sr -g auto -e1 auto -e2 -c -r -j2 -p true restoredisk icreTest01 sda
    else
     [ "$BOOTUP" = "color" ] && $SETCOLOR_FAILURE
     echo "Fail to find the Clonezilla image home $ocsroot!"
     echo "Program terminated!"
     [ "$BOOTUP" = "color" ] && $SETCOLOR_NORMAL 
    fi
     
    #
    umount $ocsroot &>/dev/null

Montar a imagem na Pen drive
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Instruçoes para carregar o zip na pendrive:
::
    | ``Insert your USB flash drive or USB hard drive into the USB port on your Linux machine and wait a few seconds. ``
    | ``Next, run the command "dmesg" to query the device name of the USB flash drive or USB hard drive. ``
    | ``Let's say, for example, that you find it is /dev/sdd1. ``
    | ``In this example, we assume /dev/sdd1 has FAT filesystem, and it is automatically mounted in dir /media/usb/. ``
    | ``If it's not automatically mounted, manually mount it with commands such as ``\ **``mkdir``\ ````\ ``-p``\ ````\ ``/media/usb;``\ ````\ ``mount``\ ````\ ``/dev/sdd1``\ ````\ ``/media/usb/``**\ ``.``
    | ``Unzip all the files and copy them into your USB flash drive or USB hard drive. ``
    | ``You can do this with a command such as: ``\ **``unzip``\ ````\ ``gparted-live-0.4.5-2.zip``\ ````\ ``-d``\ ````\ ``/media/usb/``**\ ``). Keep the directory architecture, for example, file   "GPL" should be in the USB flash drive or USB hard drive's top directory (e.g. /media/usb/GPL).``
    | ``To make your USB flash drive bootable, first change the working dir, e.g. ``\ **``cd``\ ````\ ``/media/usb/utils/linux``**\ ``, then run ``\ **``bash``\ ````\ ``makeboot.sh``\ ````\ ``/dev/sdd1``**\ `` (replace /dev/sdd1 with your USB flash drive device name), and follow the prompts.``
    | ``WARNING! Executing makeboot.sh with the wrong device name could cause your GNU/Linux not to boot. Be sure to confirm the command before you run  it.``
    | `` ``
    | **``NOTE:``**\ `` There is a known problem if you run makeboot.sh on Debian Etch, since the program utils/linux/syslinux does not work properly. Make sure you run it on newer GNU/Linux, such as Debian Lenny, Ubuntu 8.04, or Fedora 9.``
    
Repositórios
============

Existem disponíveis dois repositórios Debian. Instruções de como `criar
um repositório <criar_um_repositório>`__.

Sarge
-----

Para subscrever ao repositório,
`thumb|right|100px <Imagem:debiansarge.jpg>`__ 

Acrescentar esta linha em **/etc/apt/sources.list**
::
  deb http://unue.no-ip.org/debian/ sarge main contrib non-free

fazer:
::
    apt-get update

Etch
----

Para subscrever ao repositório,
`thumb|right|100px <Imagem:debianetch.jpg>`__

Acrescentar esta linha em **/etc/apt/sources.list**
::
    deb http://unue.no-ip.org/debian/ sarge main contrib non-free

Fazer:
::
    apt-get update

ISOs de CDs
===========

Alguns CDs de instalação podem ser encontrados em
::
    http://unue.no-ip.org/debian

Acesso Remoto
=============

Pacote "*no-ip*" em Fedora/Red-hat
----------------------------------

Foi criado um meio de instalação de no-ip para red-hat 9 e Fedora Core.

Somente é necessário o ficheiro **no-ip.tar.gz**

Procedimento:
::
    tar zxvf no-ip.tar.gz
    cd no-ip
    ./install

VNC
---

ssh
---

`thumb|right|100px <Imagem:Tunel.jpg>`__

X forwarding
~~~~~~~~~~~~

O **ssh** do Debian Sarge, por defeito não permite fazer *forward* do
ambiente gráfico.

Para que possa fazer, é necessário activá-lo em **/etc/ssh/sshd_config**
e reiniciar o ssh.

/etc/ssh/sshd_config:
::
    (...)
    X11Forwarding yes
    (...)

Para que se possa então usar, é necessário reiniciar o serviço:
::
    # /etc/init.d/ssh restart

e iniciar uma nova sessão de ssh
::
    # ssh -XC user@host

Túneis *ssh*
~~~~~~~~~~~~

Usando cliente ssh em linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Usando cliente ssh putty
^^^^^^^^^^^^^^^^^^^^^^^^

Túnel reverso *ssh*
~~~~~~~~~~~~~~~~~~~

SSH is an extremely useful tool in that it allows you to do many things
in a secure fashion that you might not otherwise be able to do. One of
the things SSH allows you to do is to set up a reverse encrypted tunnel
for data transfer. Typically, when you initiate an SSH tunnel, you
forward a port on the local machine to a remote machine which can allow
you to connect to an insecure service in a secure way, such as POP3 or
IMAP. However, you can also do the reverse. You can forward a port on
the remote machine to the local machine while still initiating the
tunnel from the local machine.

This is useful if you have a service on the remote end that you want to
have connected to something on the local machine, but you don't wish to
open up your firewall or have SSH private keys stored on the remote
machine. By using a reverse tunnel, you maintain all of the control on
the local machine. An example usage for this would be for logging
messages; by setting up a reverse SSH tunnel, you can have a logger on
the remote system send logs to the local system (i.e., syslog-ng).

To set up the reverse tunnel, use:
::
    ssh -nNT -R 1100:local.mydomain.com:1100 remote.mydomain.com

What this does is initiate a connection to remote.mydomain.com and
forwards TCP port 1100 on remote.mydomain.com to TCP port 1100 on
local.mydomain.com.

   The "**-n**" option tells ssh to associate standard input with
   **/dev/null**,

   "**-N**" tells ssh to just set up the tunnel and not to prepare a
   command stream, and

   "**-T**" tells ssh not to allocate a pseudo-tty on the remote system.

These options are useful because all that is desired is the tunnel and
no actual commands will be sent through the tunnel, unlike a normal SSH
login session.

   The "**-R**" option tells ssh to set up the tunnel as a reverse
   tunnel.

Now, **if anything connects to port 1100 on the remote system, it will
be transparently forwarded to port 1100 on the local system.**

ssh sem password: *trusted ssh*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Hardware
========

ATX Power Connector
-------------------

Pinout
~~~~~~

.. figure:: atx_pinout.jpg
   :alt: atx_pinout.jpg

   atx_pinout.jpg

Please note that this connector's pins are numbered oddly - not in the
normal manner Instead it is numbered like the odd-ball Sub-d connector.
Probably someone who had not laid out many PCBs did this. (A normal
approach would have been to number it counterclockwise looking at the
sockets or clockwise looking at the pins)

+-----+-------------------+--------------------+
| Pin | Signal            | Wire Color         |
+=====+===================+====================+
| 1   | +3.3Vdc           | Orange             |
+-----+-------------------+--------------------+
| 2   | +3.3Vdc           | Orange             |
+-----+-------------------+--------------------+
| 3   | GND               | Black              |
+-----+-------------------+--------------------+
| 4   | +5Vdc             | Red                |
+-----+-------------------+--------------------+
| 5   | GND               | Black              |
+-----+-------------------+--------------------+
| 6   | +5Vdc             | Red                |
+-----+-------------------+--------------------+
| 7   | GND               | Black              |
+-----+-------------------+--------------------+
| 8   | PWR-OK            | Gray               |
+-----+-------------------+--------------------+
| 9   | | +5Vdc           | Purple             |
|     | | VSB             |                    |
|     | | standby Voltage |                    |
+-----+-------------------+--------------------+
| 10  | +12Vdc            | Yellow             |
+-----+-------------------+--------------------+
| 11  | +3.3Vdc           | | Orange           |
|     |                   | | {brown is 3.3Vdc |
|     |                   | | sense]           |
+-----+-------------------+--------------------+
| 12  | -12Vdc            | Blue               |
+-----+-------------------+--------------------+
| 13  | GND               | Black              |
+-----+-------------------+--------------------+
| 14  | PS-ON             | Green              |
+-----+-------------------+--------------------+
| 15  | GND               | Black              |
+-----+-------------------+--------------------+
| 16  | GND               | Black              |
+-----+-------------------+--------------------+
| 17  | GND               | Black              |
+-----+-------------------+--------------------+
| 18  | -5Vdc             | White              |
+-----+-------------------+--------------------+
| 19  | +5Vdc             | Red                |
+-----+-------------------+--------------------+
| 20  | +5Vdc             | Red                |
+-----+-------------------+--------------------+

-  Sometimes `other power
   supplies <http://secure.transtronics.com/Power%20Supplies.html>`__
   may be of more use.

Testing ATX power supplies
~~~~~~~~~~~~~~~~~~~~~~~~~~

Cheap power supplies are the source of many intermittent computer
failures. `Recommended computer
components. <Computer_Component_Standard>`__ There are some `passive
testers <http://www.extensiontech.net/reviews/ad/coolmax/ps-101/>`__,
but they really don't do a good job of testing. You would want to both
use a passive load and a real computer load and look at the noise on the
signals with an oscilloscope.

To test the basic function of an ATX power supply, short the Green wire
with one of the grounds. This should turn the power supply on.

Dispositivos de Massa
---------------------

`thumb|right|100px <Imagem:OpenHardDrive.jpg>`__

Discos IDE
~~~~~~~~~~

Discos SCSI
~~~~~~~~~~~

Discos SATA
~~~~~~~~~~~

*Pen* Drives
~~~~~~~~~~~~

Sectores Danificados
~~~~~~~~~~~~~~~~~~~~

Em caso de haver sectores danificados, é recomendável a substituiçao do
disco, mas pode-se marcar os sectores danificados no sistema de
ficheiros para que simplesmente o sistema operativo não aloque dados
nessas zonas

Identificação de Sectores danificados
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Marcação de Sectores danificados
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Rede
----

Periféricos
-----------

Cablagem
~~~~~~~~

Cabos de impressora Epson Série
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ligação de cabos de impressora para Epson com módulo de comunicação
série:

-  Ficha DB9 Femea;
-  Ficha DB25 Macho.

`Imagem:db9F-db25M.jpg <Imagem:db9F-db25M.jpg>`__

Conversores USB-Série
^^^^^^^^^^^^^^^^^^^^^

pode-se forçar um evento a ficar num *device* específifico acrescentando
uma regra ao udev:
::
    KERNEL=="input/event*", SYSFS{idVendor}=="0403", NAME=="input/%k", SYMLINK="input/touchscreen%e"

no caso de monitores TFT da LG, pode-se utilizar, em vez de
**/dev/input/event[0-9]** apenas
::
    ln -s  /dev/input/by-path/pci-0000\:00\:1d.0-usb-0\:2\:1.0-event-  \
        /dev/input/touchscreen


Adaptar obviamente o *device* ao dispositivo indicado.

Conflitos entre conversor USB-Série e placas 3G
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Há um problema quando se utiliza um conversor USB-Série e uma placa 3g
na mesma máquina, pois ambos competem pelo mesmo *device*.

-  o driver do USB-Série é flexivel, e utiliza o próximo *device*
   disponível;
-  o driver da placa 3G (também dependente de usbserie) é menos
   tolerante e só funciona nos dois primeiros dispositivos:
   **/dev/ttyUSB0** e **/dev/ttyUSB1** (e utiliza os dois!)

A solução passa por obrigar o conversor usb-série a carregar o módulo
por último.

Para isso é necessário saber qual o módulo correspondente ao dispositivo
e colocá-lo no **blacklist** do modprobe.d

**/etc/modprobe.d/blacklist**:
::
    # This file lists modules which will not be loaded as the result of
    # alias expansion, with the purpose of preventing the hotplug subsystem
    # to load them. It does not affect autoloading of modules by the kernel.
    # This file is provided by the udev package.
    
    # evbug is a debug tool and should be loaded explicitly
    blacklist evbug
    
    # these drivers are very simple, the HID drivers are usually preferred
    blacklist usbmouse
    blacklist usbkbd
    
    # no fim colocamos o módulo que não queremos que carregue no arranque
    blacklist pl2303

Reiniciando a máquina pode-se verificar após o arranque de que o módulo
nao foi carregado
::
     # lsmod  | grep  pl2303
     #

Agora pode-se carregar o módulo, agora com a certeza de que não vai
gerar conflito com o outro
::
    # modprobe pl2303

Esta linha pode ser inserida em **~/.bash_profile**, ficando com este
aspecto:
::
    export USERNAME BASH_ENV PATH
    if [ $(tty) == /dev/tty1 ]; then
            '''sleep 5'''              # 5 segundos para dar tempo do modem se ligar completamente
            '''modprobe pl2303 &'''    # carrega o novo módulo
            '''sleep 2'''              # aguarda 2 segundos antes de arrancar o software
            startx               # arranca o interface gráfico e frontoffice
    fi

Ficheiros de Koncepto
=====================

Documentos EPSON e IBM
----------------------

Ficheiros de linguagem do Koncepto
----------------------------------

Base de Dados
=============

Correcções
----------

Eliminar dados superiores a um determinado valor
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

É necessário manipular 3 tabelas: documents, documentdetails e cashflow

Dado que as tabelas documents e documentdetails estão ligadas, não é
necessário mexer em documentdetails

Os documentos que podem ser emitidos são:

-  Consumo a Crédito - **doc_type 23**
-  Nota de crédito de consumo a crédito - **doc_type 27**
-  Cancelamento - **doc_type 7**
-  Factura - **doc_type 2**
-  Nota de Crédito - **doc_type 9**

Consultas Base
--------------

Lista de Artigos por Família
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
             productfamily.description,
             products.description
    FROM
            products  LEFT JOIN productfamily
    ON                        products.family_id=productfamily.id
    ORDER BY
            productfamily.id;

Output:
::
          description        |                  description                   
    -------------------------+------------------------------------------------
    Comidas                  | 1/2 Pasta
    Comidas                  | P. Alfredo
    Comidas                  | Canneloni Bolognese
    Comidas                  | P. Bolognese
    Comidas                  | Bolo Mousse
    Comidas                  | Saltimbocca
    Comidas                  | x16 Tropicale
    Comidas                  | x2 Napoletana
    (...)

Mostrar produtos e preços activos separados por páginas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
        pagedetails.page_id,
        products.description,
        products.price0
    FROM
        pagedetails 
    LEFT JOIN
        products ON pagedetails.product_id=products.id ;

output:
::
     page_id |       description       | price0 
    ---------+-------------------------+--------
          12 | Cointreau               |   3800
          12 | 1/2 Whisky Novo         |   2800
          15 | Embalagem               |    500
          15 | Ementa e14.00           |  14000
    (...)``

Mostrar Preço e IVA de artigos activos separados por páginas
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
           l.text AS "Página",
           prod.description AS "Produto",
           ROUND((tax1/100.0), 2) AS "Tx IVA",
           ROUND((prod.price0/1000.0), 2)  AS "Preço"
    FROM
           pages p
           INNER JOIN pagedetails pd ON
                   pd.page_id = p.id
           INNER JOIN products prod ON
                   pd.product_id = prod.id
           LEFT JOIN labels l ON
                   l.label_id = p.label_id AND
                   l.lang_id = 3
    ORDER BY
           pd.page_id;

Output
::
             Página          |              Produto              | Tx IVA | Preço 
    -------------------------+-----------------------------------+--------+-------
     Cafetaria               | 1/2 chocolate quente              |  23.00 |  1.20
     Cafetaria               | Descafeinado                      |  23.00 |  1.20
     Cafetaria               | Menu 2                            |  23.00 |  2.70
     Cafetaria               | Capuccino                         |  23.00 |  2.10
     Cafetaria               | Menu 1                            |  23.00 |  1.75
     Cafetaria               | Café Aromas & Origens             |  23.00 |  1.55
    (...)

Mostrar lista de páginas
~~~~~~~~~~~~~~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
       pages.id,pages.label_id,labels.text
    FROM
       pages LEFT JOIN labels
    ON
       pages.label_id=labels.label_id AND labels.lang_id=3 ;

Output:
::
    id | label_id |         text         
    ---+----------+----------------------
    15 |      335 | Diversos
    16 |      336 | Dc/Fr/Qe
    17 |      337 | Gelados
    12 |      332 | Wisky / Licor
     3 |      323 | Pastas
    (...)

Consultas de Vendas
-------------------

Mesas Abertas
~~~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
            staticbills.description,
            staticbills.current_total,
            openbills.unit_price_client,
    --      openbills.product_id,    
            products.description
    FROM
           openbills
           INNER JOIN staticbills ON
                   openbills.staticbill_id=staticbills.id
           LEFT JOIN products ON
                   openbills.product_id= products.id
    order by
            staticbills.description ;

Apuros Diários
~~~~~~~~~~~~~~

Query:

.. code:: sql

       SELECT
           date_part('day', d.doc_date) as dia,
    --     d.doc_number, 
           cast(SUM(cast(d.total_with_tax as decimal(18,2))/100) as decimal(18, 2))  as total
    --     s.description,
    --     d.doc_date 
       FROM 
           documents d
           LEFT JOIN staticbills s ON 
               d.table_id=s.id  
       WHERE 
           CAST(d.doc_date AS DATE) BETWEEN '2011-08-01' AND '2011-08-31' AND 
           d.doc_type=2 
       group by
           date_part('day', d.doc_date)
       ORDER BY 
           date_part('day', d.doc_date) ASC;

Output:
::
     dia |  total  
    -----+---------
       1 | 1571.05
      20 | 4863.85
      21 | 3330.30
      22 | 1128.30
      23 | 1410.80
      24 | 2212.40
      25 | 2718.80
      26 | 2309.45
      27 | 4544.30
      28 | 3775.85
      29 | 1471.60
      30 | 2158.20
      31 | 2377.65
    (13 registros)

Grelha de Documentos
~~~~~~~~~~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
            d.doc_number,
            d.doc_type,
            dd.create_time,
            dd.product_name,
            dd.quantity,
            dd.unit_price,
            dd.info_client_price
    from
            documents d
            inner join documentdetails dd on
                    dd.document_id = d.id
    where
            d.doc_type in (2, 9)
    order by
            dd.id desc ;

Output:
::
     doc_number | doc_type |       create_time       |          product_name           | quantity | unit_price | info_client_price 
    ------------+----------+-------------------------+---------------------------------+----------+------------+-------------------
     NOLV6347   |        2 | 2012-06-19 16:08:06.895 | Brilho de Cor                   |        1 |      25000 |              2500
     NOLV6346   |        2 | 2012-06-19 15:15:45.657 | Manutencao Cor                  |        1 |      40000 |              4000
     NOLV6345   |        2 | 2012-06-19 13:55:08.426 | Manutencao Cor                  |        1 |      40000 |              4000
     NOLV6344   |        2 | 2012-06-19 13:12:49.15  | Manutencao Cor                  |        1 |      40000 |              4000
     NOLV6343   |        2 | 2012-06-19 12:37:51.947 | Manutencao Cor                  |        1 |      40000 |              4000
     NOLV6342   |        2 | 2012-06-19 10:07:44.681 | Brilho de Cor                   |        1 |      25000 |              2500
     NOLV6341   |        2 | 2012-06-18 22:51:24.859 | Manutencao Cor                  |        1 |      40000 |              4000
     NOLV6340   |        2 | 2012-06-18 19:59:08.87  | Manutencao Cor                  |        1 |      40000 |              4000

Documentos emitidos
~~~~~~~~~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
            d.doc_number,
            d.total_with_tax/100 || '.' || total_with_tax%100 AS total,
            s.description,
            d.doc_date
    FROM
            documents d
            LEFT JOIN staticbills s ON
                    d.table_id=s.id
    WHERE
            CAST(d.doc_date AS DATE) BETWEEN '2011-07-16' AND '2011-08-01' AND
            d.doc_type=2
    ORDER BY
            d.doc_date ASC;

Output:
::
     doc_number | total  | description |        doc_date         
    ------------+--------+-------------+-------------------------
     99649      | 60.45  | 5EA         | 2011-07-16 00:10:54.503
     99650      | 24.90  | 10EA        | 2011-07-16 00:18:59.13
     99651      | 183.70 | M-9A        | 2011-07-16 00:41:18.887
     99652      | 33.30  | M-11A       | 2011-07-16 00:41:30.468
     99653      | 39.80  | M-12A       | 2011-07-16 00:56:09.718
     (...)

Mapa de IVA
~~~~~~~~~~~

Query:

.. code:: sql

    SELECT
            EXTRACT(DAY FROM d.doc_date) || ' de ' ||
            CASE
                    EXTRACT(MONTH FROM d.doc_date)
                    WHEN 1 THEN 'Jan'
                    WHEN 2 THEN 'Fev'
                    WHEN 3 THEN 'Mar'
                    WHEN 4 THEN 'Abr'
                    WHEN 5 THEN 'Mai'
                    WHEN 6 THEN 'Jun'
                    WHEN 7 THEN 'Jul'
                    WHEN 8 THEN 'Ago'
                    WHEN 9 THEN 'Set'
                    WHEN 10 THEN 'Out'
                    WHEN 11 THEN 'Nov'
                    WHEN 12 THEN 'Dez'
                    ELSE '??'
            END || '/' ||
            EXTRACT(YEAR FROM d.doc_date) AS "Data",
            CAST((SUM(CASE WHEN d.doc_type = 2 THEN
                dt.price_with_tax ELSE -dt.price_with_tax END) / 100.00) AS DECIMAL(18, 2)) AS  "Total Liquido (Eur)",
            CAST((SUM(CASE WHEN d.doc_type = 2 THEN
                dt.price_without_tax ELSE -dt.price_without_tax END) / 100.00) AS DECIMAL(18, 2)) AS  "Total Íliquido (Eur)",
            CAST((dt.tax1 / 100.00) AS DECIMAL(18, 2)) || '%' AS "Taxa IVA",
            CAST((SUM(CASE WHEN d.doc_type = 2 THEN
                dt.value_tax1 ELSE -dt.value_tax1 END) / 100.00) AS DECIMAL(18, 2)) AS "Total IVA  (Eur)"
    FROM
            Documents d
            INNER JOIN DocumentDetails dt ON
                            dt.document_id = d.id AND
                            dt.main_documentdetails_id = 0
    WHERE
            d.doc_type IN (2, 9) AND
            EXTRACT(YEAR FROM d.doc_date) = 2011 AND
            EXTRACT(MONTH FROM d.doc_date) IN (7) AND
            d.doc_date BETWEEN
            CAST(
                            EXTRACT(YEAR FROM d.doc_date)||'-'||
                            EXTRACT(MONTH FROM d.doc_date)||'-'||
                            EXTRACT(DAY FROM d.doc_date)||' 09:00:00.00' AS TIMESTAMP)
            AND
            CAST(
                            EXTRACT(YEAR FROM d.doc_date)||'-'||
                            EXTRACT(MONTH FROM d.doc_date)||'-'||
                            EXTRACT(DAY FROM d.doc_date)||' 02:00:00.00' AS TIMESTAMP) + INTERVAL '1 DAY'
    GROUP BY
            EXTRACT(YEAR FROM d.doc_date),
            EXTRACT(MONTH FROM d.doc_date),
            EXTRACT(DAY FROM d.doc_date),
      dt.tax1
    HAVING
            SUM(CASE WHEN d.doc_type = 2 THEN dt.price_with_tax ELSE -dt.price_with_tax END)        > 0 AND
            SUM(CASE WHEN d.doc_type = 2 THEN dt.price_without_tax ELSE -dt.price_without_tax END) > 0 AND
            SUM(CASE WHEN d.doc_type = 2 THEN dt.value_tax1 ELSE -dt.value_tax1 END) > 0
    ORDER BY
            EXTRACT(YEAR FROM d.doc_date),
            EXTRACT(MONTH FROM d.doc_date),
            EXTRACT(DAY FROM d.doc_date),
            4 DESC;

Output:
::
          Data      | Total Liquido (Eur) | Total Íliquido (Eur) | Taxa IVA | Total IVA (Eur) 
    ----------------+---------------------+----------------------+----------+-----------------
     1 de Jul/2011  |              843.80 |               746.70 | 13.00%   |           97.10
     2 de Jul/2011  |              649.10 |               574.38 | 13.00%   |           74.72
     3 de Jul/2011  |              572.75 |               506.92 | 13.00%   |           65.83
     4 de Jul/2011  |              503.60 |               445.69 | 13.00%   |           57.91
     (...)
