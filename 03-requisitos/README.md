# Seção 4: Requisitos do software

A partir da investigação realizada, reconstrua parte dos requisitos do sistema.

## Requisitos funcionais

**RF1. O sistema deve permitir a criação de cartões com oclusão de imagem.**

Release Notes 23.10: "Anki now has built-in support for creating image occlusions. Select the automatically-added "Image Occlusion" notetype in the add screen to use it. Glutanimate intends to update his add-on soon to work with the new system. By @krmanik, with help from @glutanimate, @hikaru-y, @abdnh and @dae in [#2367](https://github.com/ankitects/anki/pull/2367), [#2485](https://github.com/ankitects/anki/pull/2485), [#2600](https://github.com/ankitects/anki/pull/2600), [#2602](https://github.com/ankitects/anki/pull/2602), [#2649](https://github.com/ankitects/anki/pull/2649), [#2689](https://github.com/ankitects/anki/pull/2689), [#2705](https://github.com/ankitects/anki/pull/2705) and more."

**RF2. O sistema deve oferecer suporte a arrastar e soltar para adicionar imagens a cartões.**

Release Notes 25.02: "Add drag/drop support when adding IO images by @iamllama in [#3776](https://github.com/ankitects/anki/pull/3776)".

**RF3. Agendar revisões com SM-2 ou FSRS.** O sistema define quando cada cartão volta a aparecer, com base na resposta do usuário. Evidência: PRs [#4956](https://github.com/ankitects/anki/pull/4956) e [#5494](https://github.com/ankitects/anki/pull/5494) (FSRS) e o [manual do Anki](https://docs.ankiweb.net/manual/background.md).

**RF4. Importar e exportar cartões em CSV e em pacotes Anki.** Evidência: PRs [#4421](https://github.com/ankitects/anki/pull/4421) e [#5328](https://github.com/ankitects/anki/pull/5328).

## Requisitos não funcionais

**RNF1. Suporte multi-plataformas.**

Release Notes 26.05b1: "The macOS builds require macOS 13 or later.", "A standard MSI installer is provided for Windows.".

**RNF2. Suporte a acessibilidade para usuários com deficiência visual.**

Issue [#4661](https://github.com/ankitects/anki/issues/4661): "A tracking issue for main screen accessibility issues and improvements. See related PRs and issues: #4553 #4511 #2347. This is something to look into after the main screen rework (probably with input from designers), but discussion is welcome.", "Before discussion of any kind can happen we need to know where all the problems are. Best way to find out, quite honestly, is to start up a screen reader, preferably close your eyes, and use the keyboard to navigate. Any places you get stuck? Any buttons, edit fields, etc. that have no clear label/other missing info? Great. Write it up, because you've likely found something actual users run into as well until it gets fixed. This excludes things like actually studying as the problems in that flow are a bit more complicated to figure out, but just the main screen... yeah have at it."

**RNF3. Segurança.** O programa deve exibir notas de decks compartilhados sem permitir a leitura de arquivos locais. Evidência: notas da release [26.09](https://github.com/ankitects/anki/releases/tag/26.09) e da 25.09.3, que corrigiu o servidor local de mídia.

**RNF4. Compatibilidade com add-ons.** Mudanças na API Python devem vir acompanhadas de aviso. Evidência: nota de compatibilidade da release [26.09b1](https://github.com/ankitects/anki/releases/tag/26.09b1) sobre a remoção de `anki.importing` e `anki.exporting`.

## Histórias de usuário

**História 1.** Como usuário que revisa conteúdo fora do Anki, quero poder atribuir uma nota a um ou mais cartões diretamente na janela de navegação, para que o histórico de revisão reflita meu recall externo sem precisar abrir a tela de revisão.

Cenário 1:
Dado que estou na janela de navegação com um cartão selecionado,
Quando pressiono o atalho "ctrl+shift+G", escolha a nota "Good" e confirmo,
Então o cartão é avaliado e o histórico de revisão é atualizado.

Cenário 2:
Dado que selecionei múltiplos cartões na janela de navegação,
Quando aplico a ação "Grade Now" com a nota "Again" a todos os cartões selecionados,
Então cada cartão recebe a avaliação "Again" e os históricos de revisão são atualizados.

**História 2.** Como estudante de anatomia, quero criar cartões de oclusão de imagem diretamente no Anki, para que eu possa estudar estruturas visuais.

Cenário 1:
Dado que estou na tela de adição de uma nova nota,
Quando seleciono o tipo de nota "Image Occlusion" e carrego uma imagem,
Então a interface me permite definir as áreas da imagem que serão ocultadas, criando as máscaras de oclusão.

Cenário 2:
Dado que criei um cartão de oclusão de imagem,
Quando inicio a revisão desse cartão,
Então as máscaras são exibidas sobre a imagem e posso revelar cada uma individualmente conforme avalio minha resposta.

**História 3.** Como estudante, quero que o Anki decida quando cada cartão volta, para estudar menos tempo e lembrar mais.

Cenário 1: dado que respondi "Errei" em um cartão, quando termino a revisão, então o cartão volta a aparecer em um intervalo curto.

Cenário 2: dado que respondi "Fácil" em um cartão, quando vejo o próximo intervalo, então ele é maior do que seria com "Bom".

**História 4.** Como estudante, quero importar um arquivo CSV com perguntas e respostas, para criar muitos cartões de uma vez.

Cenário 1: dado um CSV com duas colunas, quando importo escolhendo o baralho e o tipo de nota, então os cartões aparecem com frente e verso corretos.

Cenário 2: dado um arquivo que não é um CSV válido, quando tento importar, então o Anki mostra uma mensagem de erro e não altera a coleção.
