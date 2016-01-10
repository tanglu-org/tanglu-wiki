---
title: Tanglu 1.0
permalink: /Tanglu_1.0/
---

NOTAS DE LANÇAMENTO

EM TRADUÇÃO Nós estamos orgulhosos anunciar de anunciar a liberação de Tanglu 1.0 (Aequorea Victoria), a primeira liberação estável de Tanglu. Para esta liberação, nós suportamos o sabor do plasma de KDE. Um sabor de GNOME está disponível como uma avaliação prévia.

Índices

`  1 Tanglu 1.0 Notas de Lançamento`
`      1.1 Promover de Debian`
`      1.2 Instalando Tanglu`
`          1.2.1 Sustentação de UEFI`
`          1.2.2 Instalador Live`
`      1.3 Suporte Lifespan`
`      1.4 Pacotes Updated`
`          1.4.1 Linux 3.12`
`          1.4.2 Systemd`
`          1.4.3 GRUB2`
`      1.5 Outras características`
`          1.5.1 Jornal persistente do systemd pelo defeito`
`          1.5.2 Nenhum daemon do syslog do defeito`
`          1.5.3 Nenhum MTA do defeito`
`          1.5.4 Non-Free/Contrib permitido pelo defeito`
`      1.6 KDE`
`      1.7 GNOME`
`      1.8 Edições sabidas`
`      1.9 Infrastructure de Tanglu`
`      1.10 Relatando erros`
`      1.11 Participe em Tanglu`
`      1.12 Download`

`  1 Tanglu 1.0 Notas de Lançamento`

Tanglu é um projeto social, construindo uma distribuição aberta, compartilhando da tecnologia com o Debian e outras distribuições. Redefine também a maneira como as distribuições são construídas hoje, apontando para uma aproximação mergulhada com as aplicações enviadas separada do núcleo do sistema operando-se no futuro. Comparado a Debian, oferece liberações tempo-baseadas freqüentes e envía frequentemente um software mais recente do que Debian, empregando a tecnologia recente de Linux. A liberação 1.0 não é o producto, ele marcas justas um marco miliário principal do projeto de Tanglu.

`      1.1 Promover de Debian`

Fazer upgrade do Debian é possível, mas somente para os usuários experientes que usam Debian 7 (Wheezy). A maneira preferida começar Tanglu é uma instalação fresca, e nós não suportamos realmente um upgrade de Debian, assim, se você o fizer, faça backup antes. Um uprade do Ubuntu também não é possível

`      1.2 Instalando Tanglu`
`          1.2.1 Sustentação de UEFI`

Nós atualmente não suportamos UEFI em nosso Live-instalador, esta característica não teríamos tempo para versão. Se você for um usuário experiente, você pode manualmente instalar Tanglu em uma máquina baseada em UEFI-, se voce nao tem experiencia nós recomendamos comutar ao BIOS do legacy para esta liberação.

`          1.2.2 Instalador Live (USB, DVD ou CD)`

Nós usamos uma versão modificada do instalador de LMDE instalar, Tanglu live-Cd. O instalador é não increivelmente rápido, mas trabalhando (se você não usa UEFI, vêem a seção de UEFI). Você pôde querer dividir manualmente seu harddrive para combinar as configurações de seu equipamento.

`      1.3 Suporte Lifespan`

Tanglu 1.0 (Aequorea Victoria) estará suportado para um mês depois que a versão seguinte de Tanglu for liberada. Somente os pacotes no live-Cd são suportados inteiramente, mas todos os pacotes puderam receber updates como licenças do tempo e do interesse.

`      1.4 Pacotes Updated`
`          1.4.1 Linux 3.12`

Nós enviamos Tanglu com o Linux 3.12, a mesma base usada atualmente em Debian testing/instável.

`          1.4.2 Systemd`

Systemd está disponível na versão 204 e usado por definição como PID 1

`          1.4.3 GRUB2`

Nós usamos GRUB2 como nosso gerenciado de boot, por default. Se Tanglu for o único OS instalado, o GRUB ir\[a carregá-lo direto, sem mostrar nenhuma tela de opções. Se Tanglu não começar, uma tela do GRUB deverá aparecer no boot seguinte. Você pode força-lo acionando a tecla de carga do boot.

`      1.5 Outras características`