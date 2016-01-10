---
title: Fontes perfeitas no Tanglu
permalink: /Fontes_perfeitas_no_Tanglu/
---

Primeiro, vamos criar um ou editar o arquivo de fontes, em /etc/fonts/local.conf. Para isso execute o comando abaixo no Terminal:

1.  nano /etc/fonts/local.conf

Criado o arquivo, copie, cole e em seguida salve o conteúdo do arquivo local,conf

<?xml version='1.0'?>
<!DOCTYPE fontconfig SYSTEM 'fonts.dtd'>
<fontconfig>

<match target="font">
` `<edit mode="assign" name="rgba">
`  `<const>`rgb`</const>
` `</edit>
</match>
<match target="font">
` `<edit mode="assign" name="hinting">
`  `<bool>`true`</bool>
` `</edit>
` `<edit name="autohint">
`  `<bool>`true`</bool>
` `</edit>
</match>
<match target="font">
` `<edit mode="assign" name="hintstyle">
`  `<const>`hintslight`</const>
` `</edit>
</match>
<match target="font">
` `<edit mode="assign" name="hinting">
`  `<bool>`true`</bool>
` `</edit>
</match>
<match target="font">
` `<edit mode="assign" name="antialias">
`  `<bool>`true`</bool>
` `</edit>
</match>
<match target="font">
` `<edit mode="assign" name="lcdfilter">
`  `<const>`lcddefault`</const>
` `</edit>
</match>

</fontconfig>

Se vocês estiver usando o editor nano, acione Ctrl + O e ENTER, em seguida Ctrl + X para sair.Para ativar a renderização, basta você sair de sua seção atual (logoff) e retornar em um novo login, seu desktop Tanglu agora está muito mais bonito.