# 3.2 Como o software evoluiu?

## Apresente pelo menos 3 acontecimentos relevantes da evolução do software, utilizando evidências como:

**1 - Migração do backend de Python para Rust, feita de forma incremental e distribuída entre vários commits.**

Commit de exemplo:

> **Commit [f592672](https://github.com/ankitects/anki/commit/f592672)**
> dae committed on May 12, 2020
>
> add separate decks table, and start on moving deck handling to Rust
>
> The Python tests are passing, but there are still a number of issues to work through, and the table/protobuf schema is not yet finalized.
>
> main · uv-mac-test · 2.1.28 · 34 files changed

**2 - A adoção do FSRS (Free Spaced Repetition Scheduler) como o agendador nativo, substituindo o SM-2.**

Release note:

> "Support for FSRS (which improves upon the scheduling provided by SM-2) is now integrated into Anki. You can compute the model weights directly inside Anki, and no longer need to use custom scheduling. If you have previously used FSRS, please delete the text in the custom scheduling section before enabling FSRS, and ensure the helper add-on has been updated to the 23.10 version if you are using it. By @L-M-Sherlock with help from @dae and @asukaminato0721 in [#2633](https://github.com/ankitects/anki/pull/2633), [#2654](https://github.com/ankitects/anki/pull/2654) and [fsrs-rs](https://github.com/open-spaced-repetition/fsrs-rs)."

**3 - A transição de um versionamento semântico tradicional (ex: 2.1.66) para um esquema baseado em data (ano.mês.patch) sinaliza uma mudança na cadência e na filosofia de lançamento.** A cadência parece ser orientada a marcos mensais, e não a grandes "versões" anuais ou semestrais, o que facilita a absorção incremental de mudanças.

Release note:

> "Anki's version numbering scheme has changed to year.month(.patch)."

## Explique como o projeto é desenvolvido e quais práticas podem ser observadas.

A partir das diretrizes de contribuição e dos dados do repositório, o processo de desenvolvimento do Anki pode ser caracterizado por:

**Controle de Escopo Rigoroso:** A equipe mantém o núcleo "enxuto". Contribuições para novas funcionalidades são desencorajadas se puderem ser implementadas como add-ons; a preferência é manter o código principal focado e estável. PRs puramente de refatoração também são evitados para reduzir o risco de regressões.

> **Consider an Add-on First**
>
> Before submitting a PR for a new feature, please consider whether it could be implemented as an add-on instead. We aim to keep the core Anki codebase lean and maintainable. Many great ideas are better served as add-ons, where they can iterate faster and serve specific user needs without affecting all users. See the [Add-on API documentation](https://addon-docs.ankiweb.net/) for guidance on building add-ons.

**Rastreabilidade Obrigatória:** Quase todo Pull Request deve estar vinculado a uma Issue existente. PRs sem issue podem ser fechados automaticamente. Isso garante que o trabalho seja direcionado a problemas reconhecidos e evita esforço desperdiçado.

> **Linked Issues**
>
> Every pull request, except hotfixes and dependency updates, must be linked to an existing open issue. If no issue exists, please open one to describe the problem before submitting a PR. This helps us ensure we're solving the right problems and prevents wasted effort on both sides.
>
> PRs without a linked issue may be automatically closed after a short period.

**Lançamentos Frequentes e Incrementais:** A existência de versões beta e releases mensais demonstra uma abordagem de entrega contínua de valor, permitindo que os usuários testem novas funcionalidades (como as mudanças no FSRS) antes de serem promovidas a estáveis.

## Relacione essas práticas com pelo menos 3 conceitos de processos de software estudados na disciplina.

**1 - Desenvolvimento incremental:** A evolução do Anki é claramente incremental: a migração para Rust não foi um "big bang", mas ocorreu ao longo de várias versões. O FSRS foi integrado, e depois refinado em dezenas de pequenos pull requests ao longo de vários releases. Cada release adiciona ou melhora uma funcionalidade sobre uma base estável.

**2 - Manifesto Ágil e XP:** Há práticas que ressoam com o Manifesto Ágil, como a entrega frequente de software funcional (releases mensais) e a colaboração com o cliente/comunidade (via issues e feedback em pull requests). A simplicidade (manter o núcleo enxuto e empurrar features para add-ons) também é um princípio ágil. Contudo, o projeto carece de elementos centrais do XP e do Ágil "puro": não há evidência de programação em pares ou propriedade coletiva do código (o controle é dos mantenedores).

**3 - Reuso:** A criação da biblioteca rslib em Rust foi um movimento deliberado para reutilizar a mesma lógica de backend (agendamento, manipulação de coleções) em diferentes contextos: a GUI desktop (via pylib), ferramentas de linha de comando e, potencialmente, o servidor de sincronização. Este reuso é arquitetural e de código, a equipe parece adaptar as práticas conforme a necessidade de cada parte do sistema.
