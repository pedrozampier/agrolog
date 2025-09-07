# AgroLog

Aplicativo móvel para gestão inteligente de rebanho bovino.

## Sobre o app

O AgroLog é um aplicativo desenvolvido para auxiliar pequenos e médios pecuaristas no controle e gerenciamento de seu rebanho bovino. O app permite registrar informações detalhadas de cada animal, controlar o programa sanitário, acompanhar o desenvolvimento do rebanho e gerar relatórios básicos para tomada de decisões.

O aplicativo funciona offline, armazenando dados localmente no dispositivo, sendo ideal para propriedades rurais com conectividade limitada à internet.

### Funcionalidades Básicas (Prioritárias)
- [ ] Sistema de login e autenticação do proprietário
- [ ] Dashboard com visão geral do rebanho e estatísticas
- [ ] Cadastro completo de animais (brinco, nome, raça, nascimento, peso)
- [ ] Listagem do rebanho com busca e filtros
- [ ] Perfil individual detalhado de cada animal
- [ ] Controle sanitário com alertas de vacinas vencidas
- [ ] Registro de aplicação de vacinas e medicamentos
- [ ] Sistema de status dos animais (ativo, vendido, morto)

### Funcionalidades Adicionais (Trabalhos Futuros)
- [ ] Histórico completo de pesagens com gráficos
- [ ] Controle de reprodução (cobertura, inseminação, prenhez)
- [ ] Genealogia básica (registro de pais e mães)
- [ ] Controle financeiro (custos com medicamentos, vendas)
- [ ] Backup automático na nuvem
- [ ] Captura e armazenamento de fotos dos animais
- [ ] Relatórios avançados com exportação PDF
- [ ] Integração com sistemas governamentais (SISBOV)
- [ ] Gestão de pastos e controle de lotação
- [ ] Notificações push para lembretes importantes

## Protótipos de tela

Os protótipos das telas foram desenvolvidos no Figma e podem ser visualizados através do link público:

**🔗 Link para visualização:** [Protótipos AgroLog no Figma](https://www.figma.com/design/G0ofd34PzwNJFpmLDdHrBt/Projeto-Mobile-2?node-id=0-1&t=hKVIndFuVB5KbZbd-1)

### Principais telas projetadas:

#### Telas de Autenticação
- **Tela de Login** - Interface de acesso com campos para email do proprietário e senha

#### Telas Principais (MVP)
- **Dashboard Principal** - Visão geral com estatísticas do rebanho, alertas e ações rápidas
- **Lista do Rebanho** - Catálogo completo dos animais com busca e filtros
- **Cadastrar Animal** - Formulário para registro de novos animais no rebanho
- **Perfil do Animal** - Detalhes completos de um animal específico
- **Controle Sanitário** - Gestão de vacinas e medicamentos com sistema de alertas

#### Navegação e Componentes
- **Bottom Navigation** - Navegação principal entre as seções do app
- **Componentes Reutilizáveis** - Cards, botões, campos de formulário padronizados

### Recursos de interatividade implementados:
- Fluxos de navegação entre todas as telas principais
- Demonstração do processo completo de cadastro de animal
- Simulação do sistema de alertas sanitários
- Prototipação das interações de busca e filtros

## Modelagem do banco

A modelagem do banco de dados foi desenvolvida para SQLite (armazenamento local) e pode ser visualizada através do link público:

<img width="616" height="843" alt="DiagramaAgroLog" src="https://github.com/user-attachments/assets/d5bb64d7-3424-429a-a8d0-8bc64b37a01b" />

### Estrutura das principais entidades:

#### PROPRIETARIO
- **id** (INTEGER, PK, AUTO_INCREMENT)
- **nome** (VARCHAR(100), NOT NULL)
- **email** (VARCHAR(100), UNIQUE, NOT NULL)
- **senha_hash** (VARCHAR(255), NOT NULL)
- **telefone** (VARCHAR(20))
- **nome_fazenda** (VARCHAR(100))
- **cidade** (VARCHAR(50))
- **estado** (VARCHAR(2))
- **data_cadastro** (DATETIME, DEFAULT CURRENT_TIMESTAMP)

#### ANIMAL
- **id** (INTEGER, PK, AUTO_INCREMENT)
- **proprietario_id** (INTEGER, FK)
- **brinco** (VARCHAR(20), UNIQUE, NOT NULL)
- **nome** (VARCHAR(100))
- **sexo** (CHAR(1), NOT NULL) - 'M' para Macho, 'F' para Fêmea
- **raca** (VARCHAR(50), NOT NULL)
- **data_nascimento** (DATE)
- **peso_nascimento** (DECIMAL(6,2))
- **peso_atual** (DECIMAL(6,2))
- **status** (VARCHAR(20), DEFAULT 'Ativo') - 'Ativo', 'Vendido', 'Morto', 'Descartado'
- **pai_id** (INTEGER, FK) - Referência para outro animal
- **mae_id** (INTEGER, FK) - Referência para outro animal
- **observacoes** (TEXT)
- **data_cadastro** (DATETIME, DEFAULT CURRENT_TIMESTAMP)

#### VACINA
- **id** (INTEGER, PK, AUTO_INCREMENT)
- **nome** (VARCHAR(100), NOT NULL)
- **laboratorio** (VARCHAR(100))
- **tipo** (VARCHAR(50)) - 'Obrigatória', 'Opcional'
- **intervalo_meses** (INTEGER) - Intervalo para próxima dose
- **observacoes** (TEXT)

#### APLICACAO_VACINA
- **id** (INTEGER, PK, AUTO_INCREMENT)
- **animal_id** (INTEGER, FK)
- **vacina_id** (INTEGER, FK)
- **data_aplicacao** (DATE, NOT NULL)
- **dose** (VARCHAR(20))
- **lote_vacina** (VARCHAR(50))
- **responsavel** (VARCHAR(100))
- **proxima_dose** (DATE) - Calculada automaticamente
- **observacoes** (TEXT)
- **data_registro** (DATETIME, DEFAULT CURRENT_TIMESTAMP)

#### PESAGEM
- **id** (INTEGER, PK, AUTO_INCREMENT)
- **animal_id** (INTEGER, FK)
- **peso** (DECIMAL(6,2), NOT NULL)
- **data_pesagem** (DATE, NOT NULL)
- **tipo_pesagem** (VARCHAR(20)) - 'Rotina', 'Venda', 'Compra'
- **observacoes** (TEXT)
- **data_registro** (DATETIME, DEFAULT CURRENT_TIMESTAMP)

#### EVENTO
- **id** (INTEGER, PK, AUTO_INCREMENT)
- **animal_id** (INTEGER, FK)
- **tipo_evento** (VARCHAR(20), NOT NULL) - 'Nascimento', 'Venda', 'Morte', 'Compra', 'Descarte'
- **data_evento** (DATE, NOT NULL)
- **descricao** (TEXT)
- **valor** (DECIMAL(10,2)) - Para eventos de compra/venda
- **observacoes** (TEXT)
- **data_registro** (DATETIME, DEFAULT CURRENT_TIMESTAMP)

### Principais relacionamentos:
- **Proprietário → Animal** (1:N) - Um proprietário possui vários animais
- **Animal → Aplicação de Vacina** (1:N) - Um animal pode receber várias vacinas
- **Animal → Pesagem** (1:N) - Um animal pode ter várias pesagens registradas
- **Animal → Evento** (1:N) - Um animal pode ter vários eventos registrados
- **Vacina → Aplicação de Vacina** (1:N) - Uma vacina pode ser aplicada em vários animais
- **Animal → Animal** (N:1) - Relacionamento de parentesco (pai/mãe)

### Índices para otimização:
- Índice único em `animal.brinco` para busca rápida
- Índice em `aplicacao_vacina.proxima_dose` para alertas
- Índice em `animal.status` para filtros
- Índice composto em `pesagem(animal_id, data_pesagem)`

## Planejamento de sprints

**📅 Período de Desenvolvimento:** 07 de setembro de 2025 a 23 de novembro de 2025  
**⏱️ Total:** 11 semanas de desenvolvimento  
**🎯 Data de Entrega:** 23 de novembro de 2025

### Sprint 1 (07/09 - 20/09): Configuração Base e Estrutura
**Objetivo:** Preparar ambiente de desenvolvimento e criar estrutura base do projeto  
**Duração:** 2 semanas

#### Semana 1 (07/09 - 13/09): Setup do Projeto
- **07/09 (Sábado):** Configuração do ambiente Expo CLI e criação do repositório GitHub
- **08/09 (Domingo):** Criação do projeto React Native com TypeScript
- **09/09 (Segunda):** Configuração do Git e estrutura de pastas
- **10/09 (Terça):** Setup do SQLite com expo-sqlite
- **11/09 (Quarta):** Configuração do React Navigation (Stack + Bottom Tabs)
- **12/09 (Quinta):** Criação do design system básico (cores, tipografia, spacing)
- **13/09 (Sexta):** Documentação inicial e setup de desenvolvimento

#### Semana 2 (14/09 - 20/09): Estrutura Base
- **14/09 (Sábado):** Criação dos componentes base (Button, Input, Card)
- **15/09 (Domingo):** Implementação da Splash Screen
- **16/09 (Segunda):** Setup do banco de dados SQLite
- **17/09 (Terça):** Criação das tabelas principais (scripts SQL)
- **18/09 (Quarta):** Telas estáticas sem funcionalidade (estrutura visual)
- **19/09 (Quinta):** Testes iniciais e ajustes
- **20/09 (Sexta):** Primeira entrega - Projeto configurado

**Entregáveis:** Projeto configurado, navegação básica, banco estruturado

### Sprint 2 (21/09 - 04/10): Sistema de Autenticação
**Objetivo:** Implementar login e gestão de sessão do usuário  
**Duração:** 2 semanas

#### Semana 3 (21/09 - 27/09): Tela de Login
- **21/09 (Sábado):** Desenvolvimento da interface de login
- **22/09 (Domingo):** Validação de formulários com React Hook Form
- **23/09 (Segunda):** Implementação de hash de senha (crypto-js)
- **24/09 (Terça):** Context API para gerenciamento de estado de autenticação
- **25/09 (Quarta):** Integração com banco de dados
- **26/09 (Quinta):** Testes de autenticação básica
- **27/09 (Sexta):** Ajustes e melhorias na interface

#### Semana 4 (28/09 - 04/10): Autenticação Completa
- **28/09 (Sábado):** Sistema de cadastro de proprietário
- **29/09 (Domingo):** Persistência de sessão com AsyncStorage
- **30/09 (Segunda):** Proteção de rotas (AuthGuard)
- **01/10 (Terça):** Tela de recuperação de senha básica
- **02/10 (Quarta):** Tratamento de erros de autenticação
- **03/10 (Quinta):** Testes completos do sistema de auth
- **04/10 (Sexta):** Segunda entrega - Sistema de login funcional

**Entregáveis:** Sistema de login funcional, sessão persistente

### Sprint 3 (05/10 - 18/10): Dashboard e Navegação
**Objetivo:** Criar tela principal e sistema de navegação  
**Duração:** 2 semanas

#### Semana 5 (05/10 - 11/10): Dashboard Principal
- **05/10 (Sábado):** Implementação da tela de dashboard
- **06/10 (Domingo):** Cards de estatísticas do rebanho
- **07/10 (Segunda):** Consultas SQL para métricas básicas
- **08/10 (Terça):** Botões de ação rápida funcionais
- **09/10 (Quarta):** Layout responsivo e otimização
- **10/10 (Quinta):** Implementação de loading states
- **11/10 (Sexta):** Testes da interface do dashboard

#### Semana 6 (12/10 - 18/10): Navegação Bottom Tab
- **12/10 (Sábado):** Implementação completa da bottom navigation
- **13/10 (Domingo):** Transições entre telas principais
- **14/10 (Segunda):** Sistema de badges para alertas
- **15/10 (Terça):** Otimização de performance na navegação
- **16/10 (Quarta):** Integração de todas as telas principais
- **17/10 (Quinta):** Testes de navegação e UX
- **18/10 (Sexta):** Terceira entrega - Dashboard funcional

**Entregáveis:** Dashboard funcional, navegação completa

### Sprint 4 (19/10 - 01/11): Gestão de Animais - CRUD Básico
**Objetivo:** Implementar cadastro, listagem e edição de animais  
**Duração:** 2 semanas

#### Semana 7 (19/10 - 25/10): Cadastro de Animais
- **19/10 (Sábado):** Formulário completo de cadastro de animal
- **20/10 (Domingo):** Validações de campos obrigatórios
- **21/10 (Segunda):** Geração automática de próximo número de brinco
- **22/10 (Terça):** Inserção no banco de dados SQLite
- **23/10 (Quarta):** Feedback visual de sucesso/erro
- **24/10 (Quinta):** Upload e gerenciamento de fotos (opcional)
- **25/10 (Sexta):** Testes do formulário de cadastro

#### Semana 8 (26/10 - 01/11): Listagem e Busca
- **26/10 (Sábado):** Tela de listagem do rebanho
- **27/10 (Domingo):** Sistema de busca por brinco/nome
- **28/10 (Segunda):** Filtros básicos (sexo, raça, status)
- **29/10 (Terça):** Implementação de pull-to-refresh
- **30/10 (Quarta):** Paginação ou scroll infinito
- **31/10 (Quinta):** Otimização de performance da lista
- **01/11 (Sexta):** Quarta entrega - CRUD básico funcionando

**Entregáveis:** CRUD básico de animais funcionando

### Sprint 5 (02/11 - 15/11): Perfil do Animal e Controle Sanitário
**Objetivo:** Implementar visualização detalhada e sistema de vacinas  
**Duração:** 2 semanas

#### Semana 9 (02/11 - 08/11): Tela de Perfil do Animal
- **02/11 (Sábado):** Interface detalhada do perfil do animal
- **03/11 (Domingo):** Exibição de todas as informações cadastradas
- **04/11 (Segunda):** Botões de ação (editar, excluir)
- **05/11 (Terça):** Formulário de edição pré-preenchido
- **06/11 (Quarta):** Validação de alterações e confirmações
- **07/11 (Quinta):** Atualização automática de listas
- **08/11 (Sexta):** Histórico básico de alterações

#### Semana 10 (09/11 - 15/11): Controle Sanitário
- **09/11 (Sábado):** CRUD de vacinas disponíveis
- **10/11 (Domingo):** Aplicação de vacinas em animais
- **11/11 (Segunda):** Cálculo automático de próximas doses
- **12/11 (Terça):** Tela de controle sanitário com status visual
- **13/11 (Quarta):** Sistema de cores para alertas (verde/amarelo/vermelho)
- **14/11 (Quinta):** Contadores de alertas no dashboard
- **15/11 (Sexta):** Quinta entrega - Funcionalidades principais completas

**Entregáveis:** Perfil completo dos animais e controle sanitário

### Sprint 6 (16/11 - 23/11): Polimento Final e Entrega
**Objetivo:** Finalizar MVP, corrigir bugs e preparar entrega  
**Duração:** 1 semana (final)

#### Semana 11 (16/11 - 23/11): Finalização
- **16/11 (Sábado):** Tratamento abrangente de erros
- **17/11 (Domingo):** Validações adicionais de dados
- **18/11 (Segunda):** Otimização de consultas SQL e performance
- **19/11 (Terça):** Melhorias na interface do usuário
- **20/11 (Quarta):** Testes manuais completos em dispositivos reais
- **21/11 (Quinta):** Correção de bugs críticos identificados
- **22/11 (Sexta):** Documentação final e preparação do build
- **23/11 (Sábado):** 🎯 **ENTREGA FINAL DO PROJETO**

**Entregáveis:** MVP completo, testado e documentado

### Cronograma Resumido por Funcionalidade:

| Sprint | Período | Funcionalidade | Complexidade | Status |
|---|---|---|---|---|
| 1 | 07/09 - 20/09 | Setup e Estrutura Base | Média | 🟡 Planejado |
| 2 | 21/09 - 04/10 | Sistema de Login | Baixa | 🟡 Planejado |
| 3 | 05/10 - 18/10 | Dashboard e Navegação | Média | 🟡 Planejado |
| 4 | 19/10 - 01/11 | CRUD de Animais | Média | 🟡 Planejado |
| 5 | 02/11 - 15/11 | Perfil e Controle Sanitário | Alta | 🟡 Planejado |
| 6 | 16/11 - 23/11 | Polimento Final | Baixa | 🟡 Planejado |

### Marcos Importantes (Milestones):
- **📅 20/09:** Primeira demonstração - Estrutura base
- **📅 04/10:** Segunda demonstração - Login funcional
- **📅 18/10:** Terceira demonstração - Dashboard completo
- **📅 01/11:** Quarta demonstração - CRUD de animais
- **📅 15/11:** Quinta demonstração - Sistema quase completo
- **📅 23/11:** 🎯 **ENTREGA FINAL OBRIGATÓRIA**

### Riscos e Mitigações:
- **⚠️ Atraso no cronograma:** Buffer de 1 dia por semana para ajustes
- **⚠️ Complexidade do SQLite:** Preparar queries antecipadamente (Sprint 1)
- **⚠️ Performance em listas grandes:** Implementar paginação desde Sprint 4
- **⚠️ Bugs de última hora:** Dedicar 3 dias finais apenas para testes e correções
- **⚠️ Problemas de integração:** Testes contínuos a cada funcionalidade implementada

## Como executar o projeto

### Pré-requisitos
- Node.js 18+ instalado
- Expo CLI instalado globalmente: `npm install -g @expo/cli`
- Aplicativo Expo Go no celular (para desenvolvimento)

### Instalação e execução
```bash
# Clonar o repositório
git clone https://github.com/seu-usuario/agrolog.git

# Navegar para o diretório
cd agrolog

# Instalar dependências
npm install

# Executar o projeto
expo start

# Para rodar no Android
expo start --android

# Para rodar no iOS
expo start --ios
```

### Scripts disponíveis
```bash
# Desenvolvimento
npm start              # Inicia o servidor Expo
npm run android        # Roda no emulador Android
npm run ios            # Roda no simulador iOS

# Qualidade de código
npm run lint           # Verifica problemas no código
npm run format         # Formata o código com Prettier

# Build
npm run build          # Gera build de produção
```

## Configurações de desenvolvimento

### Variáveis de ambiente
Criar arquivo `.env` na raiz do projeto:
```env
EXPO_PUBLIC_APP_NAME=AgroLog
EXPO_PUBLIC_VERSION=1.0.0
EXPO_PUBLIC_DATABASE_NAME=agrolog.db
```

### Configuração do banco de dados
O banco SQLite é criado automaticamente na primeira execução. Para reset:
```bash
# Limpar dados do simulador
expo r --clear
```

---

**Desenvolvido por:** [Pedro Zampier]  
**Disciplina:** Desenvolvimento de Projetos para Dispositivos Móvei  
**Instituição:** [UTFPR]  
**Período:** 2025.2  
**Professor:** [Andres Jessé Porfirio]
