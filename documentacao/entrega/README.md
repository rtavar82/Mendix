# CampusFix

Aplicação de gestão de pedidos de suporte técnico e manutenção, desenvolvida em Mendix para um projeto de formação da ATEC.

O CampusFix permite registar problemas, acompanhar o seu tratamento e comunicar através de comentários associados a cada pedido.

## Funcionalidades

- Autenticação e navegação de acordo com o perfil.
- Criação de pedidos com título, descrição, prioridade e categoria.
- Preenchimento automático do criador, da data de criação e do estado inicial.
- Consulta dos pedidos próprios e acesso ao respetivo detalhe.
- Edição dos pedidos próprios enquanto estiverem abertos.
- Consulta e tratamento de pedidos pelo técnico.
- Associação do técnico autenticado ao assumir um pedido.
- Conclusão de pedidos com registo automático da data de conclusão.
- Comentários associados ao pedido, ao autor e à data de registo.
- Criação, edição e desativação de categorias pelo administrador.
- Pesquisa e filtros nas listas de pedidos.
- Dashboard com três indicadores: pedidos abertos, em tratamento e concluídos.
- Formulários de criação e detalhe com largura limitada, campos organizados e botões junto ao conteúdo.

## Perfis de acesso

| Perfil | Ações principais |
| --- | --- |
| Utilizador | Criar pedidos, consultar os seus pedidos, editar informação permitida enquanto estiverem abertos e acrescentar comentários. |
| Técnico | Consultar pedidos, assumir um pedido, comentar os pedidos de que é responsável e concluir o respetivo tratamento. |
| Administrador | Consultar todos os pedidos, gerir categorias e utilizadores. Tem também acesso às operações de tratamento, respeitando as regras de atribuição ao técnico responsável. |

O perfil Administrador corresponde à User Role `Administrator`. As permissões do módulo distinguem `Utilizador`, `Tecnico` e `Administrador`.

As restrições são aplicadas às entidades e aos microflows, além da navegação e dos botões apresentados em cada página.

## Fluxo de um pedido

1. O utilizador cria um pedido, que fica no estado **Novo**.
2. Um técnico assume o pedido, que passa a **Em tratamento**.
3. O criador e o técnico responsável podem acrescentar comentários enquanto o pedido estiver aberto.
4. O técnico responsável conclui o pedido, que passa a **Resolvido**, com data de conclusão preenchida.
5. O pedido concluído deixa de poder ser editado ou assumido novamente através das operações implementadas.

O modelo contém ainda os estados **Em análise** e **Cancelado**. O fluxo principal implementado utiliza Novo, Em tratamento e Resolvido.

As prioridades disponíveis são Baixa, Média, Alta e Urgente. Os novos pedidos são inicializados com prioridade Média.

## Modelo de dados

| Entidade | Finalidade |
| --- | --- |
| `Pedido` | Título, descrição, datas, prioridade, estado, categoria, criador e técnico responsável. |
| `Categoria` | Nome, descrição e indicação de categoria ativa. |
| `Comentario` | Texto, data, pedido e autor. |
| `Administration.Account` | Contas de acesso à aplicação. |
| `DashboardResumo` | Entidade não persistente que transporta os três contadores do dashboard. |

O dashboard respeita as permissões de consulta: o utilizador comum recebe indicadores relativos aos seus próprios pedidos.

## Microflows

Os microflows da aplicação encontram-se no módulo `MyFirstModule`.

| Microflow | Responsabilidade |
| --- | --- |
| `ACT_Pedido_Criar` | Inicializar o pedido, associar o criador e abrir o formulário. |
| `ACT_Pedido_Salvar` | Validar os campos obrigatórios, a categoria ativa, o criador e o estado antes de guardar. |
| `ACT_Pedido_Assumir` | Associar o técnico autenticado e colocar o pedido em tratamento. |
| `ACT_Pedido_Concluir` | Registar o estado Resolvido e a data de conclusão. |
| `ACT_Comentario_Novo` | Inicializar um comentário com o pedido e o autor corretos. |
| `ACT_Comentario_Salvar` | Validar e guardar o comentário num pedido aberto e autorizado. |
| `DS_Dashboard` | Calcular os indicadores apresentados na página inicial. |

## Executar localmente

1. Instalar o **Mendix Studio Pro 11.14.0**, versão utilizada neste projeto.
2. Clonar o repositório ou obter a pasta completa do projeto.
3. Abrir o ficheiro `CampusFix.mpr` no Studio Pro.
4. Confirmar que os módulos e as dependências do projeto foram carregados.
5. Executar **Run locally**. Quando necessário, permitir a sincronização da base de dados local, depois de confirmar as alterações propostas.
6. Abrir **View App**, normalmente em `http://localhost:8080`.
7. Utilizar as contas de demonstração disponíveis no ambiente de desenvolvimento para verificar cada perfil.

A configuração local utiliza HSQLDB. O projeto está configurado para Java 25; o ambiente do Studio Pro deve disponibilizar uma instalação compatível.

As contas de demonstração são `demo_user`, `demo_tecnico` e `demo_administrator`. As palavras-passe devem ser consultadas no ambiente de desenvolvimento e não são publicadas neste README.

Para visualizar alterações ao tema, voltar a executar a aplicação e atualizar a página do browser.

## Validação realizada

- Verificação do modelo original com **zero erros**.
- Compilação concluída com sucesso na cópia de validação, incluindo as melhorias visuais.
- **33 verificações aprovadas** numa instância local isolada, com base de dados e dados fictícios.
- Navegação verificada no browser com Administrador, Técnico e dois utilizadores comuns.
- Edição de categoria, comentário técnico e edição de pedido realizadas no browser e confirmadas na base de dados.
- Cinco capturas de ecrã preparadas para a entrega.
- Confirmação posterior do funcionamento pelo utilizador no seu ambiente.

As verificações funcionais cobriram autenticação dos três perfis, categorias, criação de pedidos, isolamento dos dados, restrições das operações de técnico, comentários, conclusão, bloqueio de alterações após conclusão e contadores por perfil.

A verificação alargada identificou 27 avisos, incluindo avisos de templates, widgets e roles de módulos. Zero erros de modelo não significa ausência de avisos.

## Limites e âmbito

- Não estão implementados anexos, localização por edifício/sala ou histórico separado de alterações de estado.
- Não existem ações dedicadas de cancelamento ou reabertura no fluxo principal documentado.
- Parte dos textos da interface mantém os nomes originais em inglês, incluindo os rótulos Account.
- O botão Novo Comentário continua visível nos pedidos concluídos; ao usá-lo, a regra de negócio apresenta um aviso e impede a criação do comentário.
- Nas contas de demonstração, os seletores de criador/técnico não apresentam nomes; as associações foram confirmadas nos testes à base de dados.
- Os testes descritos foram locais; não foi realizada validação de uma publicação em produção nem uma bateria completa em dispositivos móveis.

## Estrutura do projeto

- `CampusFix.mpr`: ficheiro principal do projeto Mendix.
- `mprcontents/`: elementos do modelo, incluindo páginas, entidades, segurança e microflows.
- `themesource/myfirstmodule/web/main.scss`: estilos específicos dos formulários de pedidos.
- `modules/`, `widgets/` e `userlib/`: módulos, widgets e dependências.
- `deployment/`: ficheiros gerados para execução local.

Os ficheiros Java gerados pelo Mendix não devem ser editados para alterar as regras de negócio. As alterações funcionais devem ser realizadas no modelo do Studio Pro.

## Repositório e evidências de entrega

O projeto utiliza o repositório Mendix da disciplina:

[Repositório Git do CampusFix](https://git.api.mendix.com/a2e5542a-3436-4c0d-a908-59cb80a7bc88.git/)

A documentação de entrega encontra-se em `documentacao/entrega/`:

- [Relatório dos testes mínimos](RELATORIO_TESTES.md).
- [Índice das cinco capturas](EVIDENCIAS.md).
- [Identificação do projeto e dos entregáveis](ENTREGA.md).
- [Histórico de commits](historico-commits.txt).

A segunda conta comum utilizada na validação foi acrescentada apenas à cópia isolada de teste. Não foi adicionada ao modelo original nem ao pacote da aplicação entregue.
