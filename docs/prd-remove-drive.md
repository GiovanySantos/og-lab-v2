# PRD — Remover o Drive pessoal

**Issue:** [#9 — Remover funcionalidade de Drive](https://github.com/GiovanySantos/og-lab-v2/issues/9)
**Status:** Especificação inicial; implementação ainda não iniciada.

## Resumo

Remover do OG Lab a experiência de Drive como biblioteca pessoal de arquivos — navegação, organização em pastas, busca/seleção de arquivos existentes e páginas de gerenciamento — sem interromper o envio, armazenamento, entrega e exibição de mídia usada pelo restante da rede.

Neste PRD, **Drive pessoal** significa a interface e as operações para o usuário organizar arquivos já enviados. **Infraestrutura de mídia** significa upload e processamento de arquivos, armazenamento local/objeto, entrega de URLs e anexos vinculados a conteúdo. A segunda permanece fora da remoção.

## Estado atual

OG Lab é um fork federado do Misskey, organizado como workspace pnpm. O recurso de Drive atravessa frontend, API, backend e persistência:

- **Interface e navegação:** `packages/frontend/src/pages/drive.vue` monta `MkDrive`; `router.definition.ts` registra `/my/drive`, rotas de pastas e detalhes de arquivos. Há páginas de arquivos, componentes de navegação/seleção e utilitários de menu.
- **Configurações da conta:** `pages/settings/drive.vue` concentra uso/capacidade, estatísticas, pasta padrão de upload, limpeza de arquivos e preferências de mídia (sensibilidade, marca d'água/moldura e compressão). `drive-cleaner.vue` lista e permite administrar arquivos pessoais.
- **Seleção e envio:** componentes e utilitários do Drive são chamados por fluxos de composição e edição. A remoção do navegador não pode deixar esses fluxos sem um meio de enviar ou associar mídia.
- **API:** `packages/backend/src/server/api/endpoint-list.ts` registra `drive`, operações de arquivos/pastas, charts e operações administrativas. Há operações de organização pessoal e outras necessárias a upload, anexos e ferramentas administrativas; não são intercambiáveis.
- **Backend e armazenamento:** `DriveService` registra arquivos, processa mídia e integra armazenamento interno ou object storage. `FileServerService` atende `/files/:key` e o proxy de mídia. Esses caminhos servem conteúdo da aplicação, não apenas a página de Drive.
- **Persistência e dependências:** `MiDriveFile` e `MiDriveFolder` são modelos do backend. Registros de arquivo são referenciados por outros recursos; apagar a tabela/objetos ou remover o serviço de arquivos quebraria conteúdo e mídia existente.

## Objetivos

1. Retirar a biblioteca pessoal de arquivos e o gerenciamento de pastas do produto.
2. Remover ou substituir referências de interface e contratos que existam exclusivamente para a biblioteca.
3. Preservar uploads novos, arquivos existentes, anexos, processamento e entrega de mídia.
4. Tornar explícita a distinção entre gestão pessoal de arquivos e infraestrutura compartilhada.

## Fora de escopo

- Remover uploads, anexos, imagens, áudio ou vídeo de notas, chat, páginas, galeria, perfis, canais ou emojis.
- Desativar armazenamento local/S3, proxy, processamento de imagem/vídeo, moderação ou limites de upload.
- Apagar arquivos já armazenados ou registros associados a conteúdo.
- Remover ferramentas administrativas de moderação/limpeza sem confirmar que não são necessárias para operação.
- Editar migrations já publicadas.

## Requisitos

### Produto e frontend

- Remover entradas de navegação, rotas e páginas de biblioteca, pastas, detalhes e limpeza pessoal; acessos diretos às rotas removidas devem ter comportamento definido (404 ou redirecionamento seguro), sem tela quebrada.
- Remover o fluxo de seleção de arquivos já guardados no Drive. Mapear cada chamador e manter uma alternativa adequada — por exemplo, upload local — para composição de notas, chat, edição de perfil, páginas, galeria e demais formulários.
- Manter visualização, preview, thumbnail e ações necessárias para mídia já anexada.
- Separar preferências de upload/processamento da página removida e realocá-las em uma área de configurações de mídia. Não descartar preferências de sensibilidade, marca d'água, moldura ou compressão sem decisão explícita.
- Remover capacidade/uso, estatísticas e limpeza pessoal da interface apenas quando seus equivalentes de produto/operacionais estiverem resolvidos.

### API e backend

- Inventariar usos internos, cliente `misskey-js`, integrações e eventuais consumidores externos antes de remover endpoints.
- Retirar apenas operações exclusivas de navegação/organização pessoal (ex.: listagem/busca de biblioteca, operações de pastas e movimentação em massa) depois de migrar todos os chamadores.
- Preservar contratos usados por upload, associação/consulta de mídia, entrega de arquivos, moderação e administração. Se algum contrato atual combinar responsabilidades, separá-las com mudanças explícitas e atualizar os tipos/clientes gerados.
- Manter `DriveService` (ou seu sucessor), `FileServerService`, processamento e armazenamento enquanto forem necessários às funções de mídia preservadas.
- Manter compatibilidade ou documentar a quebra de API para consumidores que dependam dos endpoints removidos antes de desativá-los.

### Dados e migrações

- Preservar registros e objetos existentes, incluindo arquivos remotos e referências em conteúdo publicado.
- Não remover a entidade/tabela de arquivos nem campos usados por anexos sem um inventário completo de relações e plano de migração.
- Se a entidade/pastas forem eliminadas após a remoção funcional, criar migration nova e reversível (`up()` e `down()`), com estratégia que não apague arquivos nem perca associações necessárias. Nunca alterar migration já integrada.

## Plano de trabalho

1. **Inventário e desenho:** rastrear rotas, menu, widgets, configurações, seletores, chamadas API, eventos, endpoints, charts, entidades, jobs, ferramentas administrativas, tipos gerados e referências de arquivo nos recursos da aplicação. Classificar cada item como pessoal, mídia compartilhada ou operacional.
2. **Remoção da experiência pessoal:** remover rotas/páginas/componentes exclusivamente de biblioteca e substituir cada seletor usado por um fluxo de upload ou outra UX aprovada. Realocar configurações de mídia preservadas.
3. **Separação dos contratos:** remover APIs exclusivas de gestão pessoal após atualizar chamadores; manter endpoints de mídia e administrativos necessários. Atualizar `misskey-js` gerado quando a API pública mudar.
4. **Persistência e limpeza:** somente depois de comprovar ausência de dependências, retirar suporte a pastas/tabelas/campos exclusivos. Preservar dados e oferecer rollback da migration.
5. **Validação e documentação:** executar testes de upload/anexos, renderização e entrega; atualizar documentação técnica e verificar que não restaram rotas, chamadas ou tipos obsoletos.

## Critérios de aceite

- Não há entrada navegável nem rota funcional para biblioteca, pastas, detalhes ou limpeza pessoal do Drive.
- Nenhum seletor ou ação removida deixa um formulário de publicação/edição sem alternativa para anexar mídia.
- É possível enviar e visualizar mídia em notas e nos demais fluxos preservados; conteúdo existente continua acessível.
- A entrega de arquivos funciona com armazenamento interno e object storage/proxy, sem remoção de arquivos existentes.
- Preferências de upload/processamento mantidas continuam acessíveis após a retirada da página atual.
- Endpoints removidos não têm chamadores ativos; endpoints de mídia/admin e tipos gerados permanecem coerentes com a implementação.
- Testes próximos aos fluxos alterados passam. Qualquer migration tem `up()`/`down()` e validação de schema; nenhuma migration publicada é editada.

## Riscos e pontos de atenção

- O nome “Drive” aparece em conceitos internos que também representam a infraestrutura de arquivos. Remoção literal de todos os símbolos/endpoints pode causar regressões de mídia e federação.
- A API e o cliente gerado têm superfície pública; remoção exige avaliar compatibilidade além do frontend desta instância.
- A tela de configurações mistura organização de arquivos com preferências de upload e processamento; a separação deve acontecer antes de apagar a tela.
- Uma remoção de tabelas/campos pode afetar dados existentes e precisa ser independente da remoção visual, com migração e rollback testáveis.
- Charts e rotinas administrativas precisam ser classificados por uso antes de serem eliminados.
