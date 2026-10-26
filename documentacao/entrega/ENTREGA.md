# CampusFix — Entrega do projeto

Esta pasta contém a documentação, os resultados dos testes e as cinco capturas exigidas pelo enunciado. Os ficheiros CampusFix.mpk e CampusFix-historico.bundle são fornecidos separadamente no pacote CampusFix_Entrega.zip; a pasta aplicacao/ referida neste documento corresponde ao conteúdo desse pacote.

## Identificação

- Aplicação: **CampusFix**.
- Plataforma: Mendix Studio Pro **11.14.0**, Java **25**, HSQLDB na execução local.
- Repositório da disciplina: [Git Mendix do CampusFix](https://git.api.mendix.com/a2e5542a-3436-4c0d-a908-59cb80a7bc88.git/).
- Identificador do repositório Mendix: `a2e5542a-3436-4c0d-a908-59cb80a7bc88`.
- Commit funcional atual: `8501eb5`.

O repositório utiliza o serviço Git do Mendix. O acesso ao endereço depende das permissões da conta da disciplina. Não foi efetuado push nem verificada a publicação do último commit no servidor nesta intervenção.

## Ficheiros da entrega

| Entregável | Ficheiro ou referência |
| --- | --- |
| Aplicação Mendix | `aplicacao/CampusFix.mpk` |
| Projeto e repositório | Identificação e endereço acima |
| Histórico real de commits | `historico-commits.txt` e `aplicacao/CampusFix-historico.bundle` |
| Funcionalidades e perfis | [README.md](README.md) |
| Testes mínimos | [RELATORIO_TESTES.md](RELATORIO_TESTES.md) e resultados JSON |
| Cinco capturas | [EVIDENCIAS.md](EVIDENCIAS.md) e pasta `evidencias/` |
| Limitações conhecidas | Secção final do relatório e README |

## Abrir a aplicação

1. Instalar ou selecionar o Mendix Studio Pro 11.14.0.
2. Abrir `aplicacao/CampusFix.mpk` no Studio Pro e escolher uma nova pasta de destino.
3. Executar Run locally e abrir View App.
4. No ambiente de desenvolvimento, usar as contas de demonstração dos três perfis.

O MPK foi exportado do projeto original, sem snapshot da base de dados. A importação não inclui os dados fictícios das capturas nem a segunda conta de utilizador criada exclusivamente para testar o isolamento.

## Consultar o histórico sem acesso ao servidor

O ficheiro bundle guarda os quatro commits reais do projeto e o respetivo conteúdo versionado. Foi criado a partir do HEAD local e verificado pelo Git.

Para obter uma cópia com esse histórico, executar a partir desta pasta:

```bash
git clone aplicacao/CampusFix-historico.bundle CampusFix-historico
cd CampusFix-historico
git log --oneline
```

A documentação adicionada depois de `8501eb5` encontra-se nesta entrega, mas ainda não faz parte do bundle. O histórico não foi fabricado nem dividido artificialmente.

## Estado da validação

- Compilação da cópia de teste: concluída com sucesso.
- Testes estruturados pela API: **33 aprovados em 33**.
- Navegação: verificada com Administrador, Técnico e dois utilizadores comuns nos percursos documentados.
- Pacote MPK: exportado, extraído e verificado com zero erros de modelo.
- Bundle Git: histórico completo verificado pelo Git.
- Publicação em produção: não realizada.

## Antes de entregar ao professor

Confirmar o acesso do professor ao repositório e incluir o README e a documentação num novo commit, sincronizando depois com o remoto da disciplina. Estes passos não foram efetuados automaticamente.
