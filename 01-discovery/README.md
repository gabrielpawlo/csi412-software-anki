# Parte 3.1 – O que é o software?

O Anki é um programa livre de cartões de memorização (flashcards) que usa repetição espaçada para ajudar a lembrar informações por mais tempo. A análise cobre o repositório [ankitects/anki](https://github.com/ankitects/anki), descrito no GitHub como o backend e os componentes web compartilhados do Anki, mais a interface Qt da versão para computador. AnkiDroid e AnkiMobile são projetos separados e ficam fora da análise.

## 1. Problema que o software resolve

Memorizar muito conteúdo por muito tempo é difícil, e revisar cartões à mão deixa de funcionar quando eles chegam a centenas ou milhares. O [manual do Anki](https://docs.ankiweb.net/manual/background.md) apresenta o programa como a solução para esse problema, baseada em dois princípios: teste de recordação ativa (tentar lembrar a resposta antes de vê-la) e repetição espaçada.

Depois de cada resposta, o usuário informa o quanto lembrou e o programa decide quando mostrar o cartão de novo: o que foi bem lembrado volta mais tarde, o que foi difícil volta antes. O agendamento usa o algoritmo SM-2 ou, como alternativa, o FSRS, integrado ao Anki a partir da versão [23.10](https://github.com/ankitects/anki/releases/tag/23.10). Segundo o manual, o resultado é estudar menos tempo ou aprender mais no mesmo tempo, em comparação com métodos tradicionais.

## 2. Público-alvo

O público-alvo são estudantes no geral, não importando o nível acadêmico nem a área acadêmica. O manual cita como exemplos o estudo para provas de medicina e direito, a memorização de nomes e rostos, geografia, poemas longos e até acordes de violão ([introdução do manual](https://docs.ncnynl.com/en/anki/print.html)). O aprendizado de idiomas é outro uso tradicional: a [página de manual do Anki no Debian](https://manpages.debian.org/wheezy/anki/anki.1.en.html) o descreve como programa de cartões para aprender idiomas.

## 3. Principais usuários

Os principais usuários são, em primeiro lugar, estudantes e autodidatas, que criam cartões e fazem as revisões diárias agendadas pelo programa, como descreve o [manual do Anki](https://docs.ankiweb.net/manual/background.md). Um segundo grupo reúne quem usa e quem desenvolve add-ons: eles ampliam o Anki com extensões em Python e são diretamente afetados por mudanças na API, como mostra a nota de compatibilidade da release [26.09b1](https://github.com/ankitects/anki/releases/tag/26.09b1) sobre a remoção dos módulos `anki.importing` e `anki.exporting`, que afeta o AnkiConnect.

Há ainda quem sincroniza a coleção entre dispositivos ou hospeda o próprio servidor de sincronização, inclusive em contêiner Docker, o que aparece nos PRs [#5021](https://github.com/ankitects/anki/pull/5021), [#5036](https://github.com/ankitects/anki/pull/5036) e [#4560](https://github.com/ankitects/anki/pull/4560). Por fim, os contribuidores voluntários corrigem erros, traduzem e mantêm a infraestrutura; cada release lista dezenas deles e os novos, por exemplo a [26.08](https://github.com/ankitects/anki/releases/tag/26.08).

## 4. Principais funcionalidades

A funcionalidade central é a repetição espaçada com os algoritmos SM-2 e FSRS: o usuário avalia cada resposta e o programa define a próxima revisão, e o FSRS traz parâmetros, retenção desejada e simulador (PRs [#4452](https://github.com/ankitects/anki/pull/4452), [#4956](https://github.com/ankitects/anki/pull/4956) e [#5494](https://github.com/ankitects/anki/pull/5494)). O conteúdo é organizado em modelos de nota com campos e templates configuráveis, incluindo lacunas (cloze) e cloze de múltiplos cartões, e em decks com opções de estudo e decks filtrados. A oclusão de imagem esconde partes de uma imagem para testar a memória sobre elas. Para trabalhar com a coleção, o navegador de cartões permite localizar e organizar cartões por texto, tags e propriedades, e as estatísticas mostram o histórico e o desempenho de estudo. A importação e a exportação trazem e levam cartões em CSV e em pacotes Anki. O editor de notas edita o conteúdo dos cartões e tem uma versão experimental, ativável em Preferences > Experiments, e o programa toca e grava áudio e usa voz sintetizada (TTS) nos cartões.

Por fim, a sincronização mantém a coleção igual entre dispositivos por meio de um servidor, os add-ons e a API Python permitem estender o programa com código de terceiros (nota de compatibilidade da release [26.09b1](https://github.com/ankitects/anki/releases/tag/26.09b1)), e o próprio programa se instala e se atualiza por Tools > Check for Updates, graças ao instalador novo.

## 5. Tecnologias utilizadas

O Anki usa várias linguagens ao mesmo tempo. A parte principal do programa é escrita em Rust e fica na pasta `rslib/`. É lá que ficam as coisas mais pesadas, como o agendador dos cartões, a busca e a sincronização, e isso está explicado na [documentação de arquitetura](https://docs.ankiweb.net/developers/architecture). Em cima dela existe uma biblioteca em Python, a `anki` (`pylib/anki/`), que conversa com o Rust por um módulo chamado `rsbridge`. A janela do programa no computador é feita com Qt, usando Python (PyQt), na pasta `qt/aqt/`. Várias telas, como as de estatísticas e de opções de deck, são na verdade páginas web feitas com TypeScript e Svelte, na pasta `ts/`, como aparece no [CLAUDE.md do repositório](https://raw.githubusercontent.com/ankitects/anki/main/CLAUDE.md).

Para essas partes conseguirem conversar entre si, o projeto usa arquivos do tipo Protocol Buffers, na pasta `proto/`, que descrevem as funções que o backend oferece. Os textos do programa ficam em arquivos `.ftl` (Fluent), na pasta `ftl/`, o que permite traduzir o Anki para vários idiomas. O agendamento do FSRS usa uma biblioteca chamada fsrs-rs (PRs [#4956](https://github.com/ankitects/anki/pull/4956) e [#5494](https://github.com/ankitects/anki/pull/5494)) e o áudio toca pelo mpv (PR [#4913](https://github.com/ankitects/anki/pull/4913)).

Para compilar e testar o projeto, eles usam o `just`, junto com um sistema de build que fica na pasta `build/`. O instalador é feito com uma ferramenta chamada Briefcase e já vem com o Python 3.13 (release [26.05](https://github.com/ankitects/anki/releases/tag/26.05) e PR [#4629](https://github.com/ankitects/anki/pull/4629)). Pelas notas de release, o projeto também usa várias ferramentas automáticas para cuidar da qualidade do código: GitHub Actions, SonarCloud, Dependabot, Playwright e cargo-nextest (PRs [#4593](https://github.com/ankitects/anki/pull/4593), [#4996](https://github.com/ankitects/anki/pull/4996), [#4726](https://github.com/ankitects/anki/pull/4726), [#4864](https://github.com/ankitects/anki/pull/4864) e [#5169](https://github.com/ankitects/anki/pull/5169)).

## 6. Licença

O código do Anki é licenciado sob a GNU Affero General Public License (AGPL), versão 3 ou posterior, conforme o arquivo [LICENSE](https://github.com/ankitects/anki/blob/main/LICENSE). As partes contribuídas por usuários estão sob a licença BSD de 3 cláusulas, e o logotipo também está sob AGPL3, com uma licença alternativa limitada para uso do logo.

Na prática, quem modificar o código e distribuí-lo, ou oferecê-lo como serviço de rede, deve disponibilizar o código-fonte alterado sob a AGPL. O projeto pede que as contribuições ao repositório oficial sejam feitas sob a BSD de 3 cláusulas, porque trechos do código também são usados no AnkiWeb e no AnkiMobile. Os PRs [#5292](https://github.com/ankitects/anki/pull/5292) e [#5267](https://github.com/ankitects/anki/pull/5267) mostram que a licença da documentação também foi tratada à parte.

## 7. Versão/release analisada

A release analisada é a [26.09.3](https://github.com/ankitects/anki/releases/tag/26.09.3), publicada em 23/09/2026 e marcada como "Latest" na consulta de 04/11/2026. A tag é `26.09.3` e o commit é `29bb700`. O esquema de versão é ano.mês(.patch), adotado a partir da 23.10, e as notas da release pedem atualização para a 26.09 ou mais nova, por conterem correções importantes de segurança.

## 8. Personas e Jobs to Be Done (JTBD)

### Persona 1: estudante preparando provas de grande volume de conteúdo

O perfil é o de quem estuda para exames longos, como medicina ou direito, usos citados no [manual](https://docs.ncnynl.com/en/anki/print.html), e precisa lembrar muito conteúdo até o dia da prova. O JTBD é: quando tenho muito conteúdo para memorizar antes de uma prova, quero revisar cada item apenas no momento em que estou perto de esquecê-lo, para reter mais em menos tempo de estudo. As funcionalidades que atendem essa necessidade são a repetição espaçada (SM-2 e FSRS), os cartões cloze, a oclusão de imagem e as estatísticas. A evidência está no [manual do Anki](https://docs.ankiweb.net/manual/background.md) e nos PRs [#4956](https://github.com/ankitects/anki/pull/4956) e [#4333](https://github.com/ankitects/anki/pull/4333).

### Persona 2: pessoa que desenvolve ou mantém add-ons

O perfil é o de quem mantém uma extensão em Python para o Anki, como o AnkiConnect, e depende da API interna do programa. O JTBD é: quando uma nova versão do Anki muda ou remove partes da API Python, quero saber com antecedência o que muda e qual é a alternativa, para atualizar meu add-on sem quebrar o uso de quem o instalou. As funcionalidades que atendem essa necessidade são a API Python e os avisos de compatibilidade nas notas de release. A evidência é a nota de compatibilidade da release [26.09b1](https://github.com/ankitects/anki/releases/tag/26.09b1) sobre a remoção de `anki.importing` e `anki.exporting`, que afeta o AnkiConnect, e o aviso da release [26.08](https://github.com/ankitects/anki/releases/tag/26.08) de que o editor experimental pode afetar add-ons.
