# CampusFix — Relatório dos testes mínimos

Data da validação: **26 de outubro de 2026**.

Os oito cenários mínimos do enunciado foram verificados nos percursos descritos abaixo. A bateria incluiu **33 verificações aprovadas pela API**, complementadas por navegação e operações no browser com os três perfis. A aplicação compilou e arrancou localmente; o pacote exportado do projeto original foi extraído e passou a verificação do Mendix com **zero erros**.

## Ambiente e contas

Foi utilizada uma cópia atual do CampusFix, em Mendix Studio Pro e Runtime **11.14.0**, Java **25** e HSQLDB. A execução de teste foi isolada na porta 8180 e numa base de dados nova. Os dados reais do projeto original não foram utilizados nem alterados.

| Conta de teste | Perfil | Utilização |
| --- | --- | --- |
| demo_administrator | Administrador | Categorias, consulta global e administração. |
| demo_tecnico | Técnico | Assumir, comentar e concluir pedidos. |
| demo_user | Utilizador | Criar e consultar os pedidos do projetor e do Wi-Fi. |
| demo_utilizador2 | Utilizador | Criar e consultar o pedido da tomada; testar isolamento entre duas contas comuns. |

A conta demo_utilizador2 foi acrescentada apenas ao modelo da cópia de teste. O pacote CampusFix.mpk foi exportado do projeto original e não contém essa conta adicional nem a base de dados de teste. As palavras-passe não são publicadas neste relatório.

## Resultados por cenário do professor

| N.º | Cenário mínimo | Verificação realizada | Resultado |
| --- | --- | --- | --- |
| 1 | Criar e consultar um pedido | Cada utilizador criou pedidos; depois de guardar, os registos foram recuperados. O browser mostrou os dois pedidos de demo_user e o único pedido de demo_utilizador2. | Aprovado |
| 2 | Impedir acesso ao pedido de outro utilizador | As duas contas têm o perfil Utilizador. Ambas tentaram consultar, pelo identificador, pedidos da outra conta: nenhum registo foi devolvido. As listas foram confirmadas pela API e no browser. | Aprovado |
| 3 | Técnico assume e trata um pedido | demo_tecnico assumiu os pedidos do projetor e do Wi-Fi. A associação ao técnico foi guardada e o estado passou a EmTratamento, sem preencher a data de conclusão. A área técnica e a pesquisa foram abertas no browser. | Aprovado |
| 4 | Concluir e guardar a data de conclusão | O técnico concluiu o pedido do projetor. O estado Resolvido e a DataConclusao foram recuperados da base de dados e apresentados no detalhe. Tentativas de reassumir e alterar o pedido concluído foram bloqueadas. | Aprovado |
| 5 | Utilizador não acede à gestão de categorias | Os menus dos dois utilizadores comuns não apresentam Categorias, Utilizadores ou Gestão de Pedidos. Tentativas de criar e alterar categorias pela API foram recusadas; o nome da categoria manteve-se intacto. | Aprovado |
| 6 | Administrador cria e edita categorias | A categoria Equipamentos foi criada e renomeada para Equipamentos audiovisuais. No browser, o administrador abriu Edit Categoria, alterou a descrição e guardou; a nova descrição foi confirmada na base de dados. | Aprovado |
| 7 | Associar comentários ao pedido correto | O utilizador e o técnico registaram comentários no pedido do projetor, com associação ao pedido, autor e data. Um comentário técnico foi ainda criado no formulário do Wi-Fi e confirmado na base de dados. | Aprovado |
| 8 | Navegação sem páginas sem permissões | Foram percorridas as páginas e ações indicadas na matriz seguinte. Não surgiram páginas sem permissões nem erros visíveis de navegação nesses percursos. | Aprovado no âmbito percorrido |

## Navegação verificada no browser

| Perfil | Páginas e ações percorridas |
| --- | --- |
| Administrador | Dashboard; Os Meus Pedidos; Novo Pedido e Cancel; Gestão de Pedidos; Categorias; Edit Categoria e Save; Utilizadores. |
| Técnico | Dashboard; Os Meus Pedidos; Novo Pedido e Cancel; Gestão de Pedidos; pesquisa por Wi-Fi; Ver Detalhes; Novo Comentário; Guardar e regresso ao detalhe. |
| Utilizador demo_user | Dashboard; Novo Pedido e Cancel; Os Meus Pedidos; Ver Detalhes do pedido concluído; aviso ao tentar comentar um pedido concluído; Cancel e regresso à lista. |
| Utilizador demo_utilizador2 | Dashboard; Os Meus Pedidos; Ver Detalhes do seu pedido aberto; edição da descrição; Save e regresso à lista. |

O técnico não recebeu menus de administração. Os dois utilizadores comuns receberam apenas Dashboard, Os Meus Pedidos e Novo Pedido. No detalhe do pedido concluído, os campos estavam desativados e os botões Save, Assumir Pedido e Concluir Pedido não estavam disponíveis ao utilizador comum.

A pesquisa por Wi-Fi devolveu apenas o pedido esperado; ao limpar a pesquisa, a área técnica voltou a apresentar os três pedidos.

## Dados das capturas

| Pedido | Criador | Estado nas capturas | Técnico |
| --- | --- | --- | --- |
| Projetor sem imagem na sala 2 | demo_user | Resolvido, com data de conclusão e dois comentários | demo_tecnico |
| Ligação Wi-Fi instável na biblioteca | demo_user | EmTratamento | demo_tecnico |
| Tomada sem energia no laboratório | demo_utilizador2 | Novo | Sem técnico atribuído |

O dashboard do administrador e do técnico apresentou **1 aberto, 1 em tratamento e 1 concluído**. O dashboard de demo_user apresentou **0, 1 e 1**; o de demo_utilizador2 apresentou **1, 0 e 0**. Estes valores confirmam que o resumo do utilizador respeita a consulta dos seus próprios pedidos.

## Evidências e âmbito

As cinco capturas estão descritas em [EVIDENCIAS.md](EVIDENCIAS.md). Os resultados estruturados encontram-se em [resultados-testes.json](resultados-testes.json), [resultados-navegacao.json](resultados-navegacao.json) e [verificacao-pacote.json](verificacao-pacote.json).

Os testes pela API exercitaram o runtime Mendix e a base de dados. As operações indicadas como realizadas no browser foram executadas pela interface e, quando guardaram dados, tiveram confirmação posterior na base de dados. As capturas são imagens reais da aplicação com dados fictícios.

A verificação de navegação cobre as páginas e ações listadas. Não constitui uma análise exaustiva de todas as combinações possíveis de perfis, botões, estados, browsers e dispositivos.

## Limitações conhecidas

- O botão Novo Comentário continua visível num pedido concluído. Ao clicar, surge o aviso “Só o criador ou o técnico responsável pode comentar um pedido aberto.” e não é criado um comentário. A restrição funcional está aplicada; a visibilidade do botão pode ser melhorada.
- Alguns textos e rótulos continuam em inglês, incluindo Account. Nas contas de demonstração, os seletores de criador/técnico não apresentam nomes; as associações foram verificadas nos dados.
- A apresentação dos comentários junta texto, data e título do pedido com pouco espaçamento.
- Não existem ações dedicadas de cancelamento ou reabertura no fluxo principal; não foram implementados anexos, localização por edifício/sala ou histórico separado de estados.
- Não foi testada uma publicação em produção nem uma bateria completa de dispositivos móveis. O pacote exportado foi verificado quanto ao carregamento e aos erros do modelo; os testes funcionais correram na cópia isolada.
- Os avisos já documentados no README não foram objeto de uma limpeza geral. Zero erros de modelo não significa zero avisos.
- Não foi efetuado push nesta intervenção. O README e a documentação de entrega ainda precisam de ser incluídos num novo commit e sincronizados com o repositório da disciplina.
