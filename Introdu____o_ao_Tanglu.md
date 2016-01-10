---
title: Introdução ao Tanglu
permalink: /Introdução_ao_Tanglu/
---

Tanglu é um sistema operacional baseado em GNU/Linux, que busca proporcionar a melhor experiência de desktop para usuários e entusiastas regulares.

O que significa o nome de "tanglu" significa? Encontrar um nome atraente não de forma alguma uma tarefa fácil, os nossos critérios foram: Precisa ser fácil de lembrar, talvez um nome exclusivo não usado em nenhum outro lugar. Que soe bem em Inglês, Alemão e Português. Domínios disponíveis (.org, .net). A palavra, em seguida, evoluiu a partir das palavras tangerina e iglu (iglu), depois de tentar um grande número de palavras por dias.

Com que frequência eles ele será lançado? Já está previsto liberar duas vezes por ano, mas isso não está escrito em pedra ainda. O primeiro lançamento, Tanglu 1 (Aequorea Victoria) aconteceu em fevereiro de 2014.

O firmware estará disponível em CDs? Sim, é muito frustrante para o usuário médio tentar por conta própria para instalar firmware, especialmente quando está impedindo-os de ficar on-line. Vamos incluir firmware, mesmo que não os que não são open source. Mas, exceto para o firmware, não haverá módulos de propriedade sobre a distribuição. Também vamos incluir apenas firmware que faz parte do Linux.

Ele é baseado no Debian, ele terá o congelamento de funcionalidades longos? Não, estamos a criar o nosso próprio arquivo de software para que os nossos usuários possam obter o software mais recente que nós acreditamos que é estável o suficiente.

Você tem um ambiente de desktop padrão? Não, todas as principais áreas de trabalho será apoiadas. Vamos fornecer screenshots e uma mesa pequena de características para ajudar o usuário a decidir qual área de trabalho escolher na página de download tanglu.

Você pretende fortemente modificar o software que você envia? Não, nós queremos que o usuário tenha a aparência pretendida pelo desenvolvedor do software original. Se acreditamos que a montante não está fazendo um bom trabalho, vamos falar com o autor primeiro para encontrar uma solução que funcione para todos. Se isso não funcionar, em casos raros, pode-se decidir usar um diferente upstream. Queremos evitar soluções internas, e tudo desenvolvido para Tanglu estará disponível para outras distribuições também.

Por que você não contribuir para o Debian diretamente e criar ainda uma outra distribuição?

Nós contribuimos para Debian. O ponto é que o Debian não pode cobrir todos os possíveis casos de uso, por exemplo, para pessoas que querem usar o software mais recente, congelamentos Debian podem ser irritantes. Mas os congelamentos são uma grande força, se você quiser usar o Debian como sistema operacional de servidor. Com Tanglu queremos construir uma distribuição que fornece uma experiência Debian para usuários de desktop, os novatos do Linux e os usuários que querem o software mais recente, com atualizações.

Você pode perguntar por que temos que criar uma nova distribuição para que, em vez de criar melhorias dentro Debian? Criação de uma nova distribuição nos permite fazer coisas que nunca poderíamos fazer no Debian, por exemplo incluindo mais firmware por padrão e ter um ciclo de lançamento com base no tempo. Estass já são coisas que são um nó górdio para o Debian, e isso é bom. Nós não queremos Debian para apoiar estes casos, uma vez que já é um grande distribuição. Queremos oferecer uma distro tão perto do Debian quanto possível, mas com algumas modificações para casos de uso que não são cobertos pelo próprio Debian, e fazendo isso iremos agregar valor ao ecossistema Debian. É claro que vamos participar do projeto DEX e trabalhar em estreita colaboração com o Debian.

Como é a relação com o Debian? Tanglu não é um projeto "oficial" do Debian, embora ele tenha se iniciado em conjunto com alguns desenvolvedores do Debian. Mas nós pensamos que poderia ajudar Tanglu Debian. Vamos sincronizar as permissões de todos os desenvolvedores do Debian, para que possam manter seus pacotes de Tanglu durante um congelamento Debian, se eles queiserem fazer isso, então Tanglu é isso, os usuários obterão o software mais recente upstream, enquanto Debian pode iniciar o novo ciclo de lançamento com tudo já testado nos pacotes de Tanglu. Acreditamos que isso vai aumentar a qualidade dos pacotes e o resultado será um começo menos áspero de desenvolvimento do Debian após o congelamento. No entanto, tanglu não é uma distribuição Debian experimental ou um playground para software não testado. Tudo em Tanglu deve ser utilizável para seus usuários e será liberado software upstream. Estamos em contato com o Debian para tentar manter o delta entre Tanglu e Debian o mais baixo possível, de modo a não apresentar incompatibilidades.

Existe uma empresa por trás deste sistema operacional? Sim e não, sim porque precisamos de dinheiro para alugar os nossos servidores e outras coisas, e não, porque não há nenhuma empresa a ditar o fluxo do projeto. Acreditamos na meritocracia, se alguma empresa decide colocar um monte de desenvolvedores para trabalhar com a gente, é natural que eles vão ter uma palavra a dizer sobre o que acontece no projeto, da mesma forma que um desenvolvedor que mantém alguns pacotes terão o direito de decidir o que é melhor para aqueles.

Em caso de desacordo você vai começar outro projeto? Quem sabe? O processo de tomada de decisão do projeto vai acabar em sessões de votação para que novamente todos os envolvidos (incluindo funcionários de uma empresa) terá o direito de votar. Tanglu é inteiramente voltado para a comunidade, por isso, esperamos que não haverá problemas com frequencia, mas nós pensamos que estes podem ser resolvidos. Tomamos todas as comunidades-membro e não-membro a sério e tentaremos encontrar uma solução que funcione para a maioria das pessoas.

Porque é que as fontes de pacotes não-livres são ativado por padrão no tanglu? Você não está comprometido com software livre?

Sim, nós somos comprometidos com software livre! Mas também queremos construir um sistema operacional que é utilizável out-of-the-box (fora da ciaxa), o que significa o transporte com alguma firmware proprietário importante, que é ainda não é livre agora. Porque este não é o ideal, esse estado só será o caso no nosso primeiro lançamento, em versões posteriores, planejamos algun tratamento especial para drivers proprietários e firmware.

Tanglu 1.0 é baseado no Debian Testing ou instável?

Nenhum, nem outro. Enquanto a maioria dos pacotes vem de Teste, uma quantidade significativamente grande é feita a partir de instável. Alguns pacotes ainda remontam a Debian Experimental. Ao construir esta versão tentamos obter tecnicamente a melhor constelação de pacotes, incluindo as versões mais recentes onde fizerem sentido. Usando as versões do Teste como primeira opção temos a vantagem de que essas versões já receberam alguns testes em Debian antes de desembarcaram em tanglu.