# Prescritor Digital

Sistema web para geração de prescrições médicas digitais com suporte a receituário de controle especial (ANVISA), geração de PDF, impressão direta e múltiplas funcionalidades de produtividade para o médico prescritor.

## Funcionalidades

### Gestão de Medicamentos
- Adicionar, remover e reordenar medicamentos na prescrição
- Busca inteligente de medicamentos com base ANVISA (typeahead, mínimo 3 letras)
- 9 vias de administração: Oral, Tópico, Injetável, Nasal, Oftálmico, Contínuo, Retal, Sublingual, Transdérmico
- Campo de posologia/instruções por medicamento
- Controle de cópias/vias (1 a 12) por medicamento
- Opção de ocultar data individualmente por medicamento
- Marcar medicamento como controlado (aplica layout ANVISA automaticamente)
- Modo de receita separada (1 medicamento por página)

### Receituário de Controle Especial (ANVISA)
Layout em conformidade com a Portaria SVS/MS nº 344/98, com as 4 seções obrigatórias:

1. **Identificação do Emitente** (topo) — preenchida automaticamente com dados do médico, CRM/UF, RQE, endereço da clínica, cidade, UF e telefone
2. **Dados do Paciente e Prescrição** (centro) — nome, endereço do paciente, medicamentos numerados com posologia, data e assinatura
3. **Identificação do Comprador** (rodapé esquerdo) — campos para preenchimento na farmácia: nome, identidade, órgão emissor, endereço, cidade, UF e telefone
4. **Identificação do Fornecedor** (rodapé direito) — espaço para carimbo da farmácia, assinatura do farmacêutico e data de dispensação

- Geração automática de 1ª Via (Retenção da Farmácia) e 2ª Via (Orientação ao Paciente)
- Máximo de 3 medicamentos controlados por página
- Fontes dimensionadas para legibilidade em impressão A5

### Quadro Resumo
- Quando a prescrição contém medicamentos controlados, uma página de resumo é gerada automaticamente como primeira página
- Lista todos os medicamentos (simples e controlados) no formato de receituário comum
- Medicamentos controlados marcados com asterisco (*)
- Nota explicativa referenciando a Portaria SVS/MS nº 344/98

### Texto Livre e Templates
- Blocos de texto livre com título e conteúdo (cada um em página separada)
- Templates de acesso rápido:
  - **Atestado Médico** — com campo para dias de afastamento e CID
  - **Atestado de Comparecimento** — com horários de permanência
  - **Atestado de Acompanhamento** — com dados do acompanhante

### Busca CID-10
- Typeahead com 100+ códigos CID-10 focados em neurologia
- Busca por código (ex: G43) ou descrição (ex: enxaqueca)
- Ao selecionar, insere automaticamente no último texto livre ou cria um atestado com o CID
- Substitui placeholder `CID: ________` quando presente

### Gestão de Pacientes
- Campos de nome, CPF e endereço
- **Máscara automática de CPF** no formato `XXX.XXX.XXX-XX`
- **Histórico de pacientes** com autocomplete:
  - Salva automaticamente ao imprimir ou gerar PDF (até 50 pacientes)
  - Dropdown com busca por nome ou CPF
  - Preenchimento automático de todos os campos ao selecionar

### Data Personalizada (Pós-datada)
- Seletor de data para receitas de uso contínuo
- Sobrescreve a data atual em todas as páginas
- Botão "Usar Hoje" para reverter à data atual
- Preview em tempo real da data formatada

### Geração de PDF
- Captura fiel do HTML renderizado via `html2pdf.js`
- Logos, formatação e layout idênticos ao preview na tela
- Cada página convertida individualmente para máxima qualidade (scale 2x)
- Abre automaticamente em nova aba do navegador
- Download automático com nome: `prescricao_[paciente]_[data].pdf`

### Impressão
- Impressão nativa via `window.print()`
- CSS `@media print` otimizado com quebras de página automáticas
- Área de impressão separada da interface

### Persistência (Auto-save)
- Salva automaticamente no `localStorage`:
  - Dados do médico (nome, CRM, especialidade)
  - Clínica selecionada e cidade
  - Rascunho completo (paciente, medicamentos, textos livres, data personalizada)
  - Histórico de pacientes
- Restauração automática ao reabrir a página
- Sem necessidade de backend — funciona 100% offline

### Responsividade
- **Desktop**: Layout split-pane (painel de controle 440px + preview flexível)
- **Mobile**: Layout empilhado com scroll vertical, preview escalado para caber na tela
- Interface inspirada no Apple Design System

## Clínicas Configuradas

| Clínica | Endereço |
|---------|----------|
| EIXO Neurologia Especializada | SGAS 610, Bloco 02, Salas 136-138, Centro Médico Lúcio Costa, Brasília-DF |
| NA Neurologistas Associados | Centro Médico Júlio Adnet, SEPS 709/909 Bloco B, Sala 12, Brasília-DF |

## Lógica de Agrupamento de Páginas

As páginas são geradas automaticamente com base na combinação de:

- **Controlado vs. Simples** — receitas separadas por tipo
- **Com data vs. Sem data** — agrupados separadamente
- **Cópias** — cada nível de cópia gera suas próprias páginas
- **Modo separado** — opcionalmente 1 medicamento por página

Ordem de impressão quando há controlados:
1. Quadro Resumo (todos os medicamentos)
2. Receituário Simples (não controlados)
3. Receituário de Controle Especial — 1ª Via
4. Receituário de Controle Especial — 2ª Via

## Stack Tecnológica

| Tecnologia | Uso |
|------------|-----|
| React 18 | Framework de UI (via CDN) |
| Tailwind CSS | Estilização (via CDN) |
| html2pdf.js | Geração de PDF (html2canvas + jsPDF) |
| Babel Standalone | Transpilação JSX no navegador |
| LocalStorage API | Persistência de dados |

## Estrutura do Projeto

```
prescricao_rapida/
├── index.html        # Aplicação completa (single-file)
├── eixo_logo.png     # Logo da clínica EIXO
├── na_logo.png       # Logo da clínica NA
└── README.md
```

## Como Usar

1. Abra o `index.html` em qualquer navegador moderno
2. Configure os dados do médico no botão de engrenagem (salva automaticamente)
3. Preencha os dados do paciente
4. Adicione medicamentos manualmente ou pela busca ANVISA
5. Marque como "Controlado" os medicamentos que exigem receita especial
6. Use os templates de acesso rápido para atestados
7. Clique em **Imprimir** ou **Baixar PDF**

## Requisitos

- Navegador moderno com suporte a ES6+ (Chrome, Firefox, Safari, Edge)
- JavaScript habilitado
- LocalStorage habilitado
- Conexão com internet apenas no primeiro carregamento (CDNs)
