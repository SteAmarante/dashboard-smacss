# Dashboard SMACSS · Atividade em grupo

Aula de conclusão do Módulo 9 · Sass/SCSS e Arquitetura CSS

Este é o **projeto-final da aula passada** (BEM, SCSS, Grid e animações). Hoje vocês vão:
reorganizar o projeto em **SMACSS**, trocar as variáveis por **tokens em camadas** e fechar os dois
**desafios da aula passada**: o theme switcher e a sidebar mobile.

> **[DSC]** As cores e nomes de tokens deste kit são **ilustrativos**. No dia a dia, eles vêm do DSC Caixa.

O `index.html` já foi atualizado e o JavaScript está pronto:
- o botão **Tema escuro** alterna `data-tema="escuro"` no `<html>`;
- o botão **Menu** (só aparece no mobile) alterna a classe `.is-aberta` na sidebar;
- o HTML usa `.l-dashboard` em vez de `.dashboard`.

## Grupos e papéis (4 a 5 pessoas)
| Papel | O que faz |
|---|---|
| Quem digita | Compartilha a tela e edita os arquivos |
| Revisor DSC | Confere: nenhum hex ou px de cor fora de `abstracts/_tokens.scss` |
| Tempo | Avisa a troca de etapa |
| Apresentador | Mostra o resultado na devolutiva (1 minuto) |

## Etapas (30 minutos)

### 1 · Pastas SMACSS (7 min)
Crie as pastas e mova os arquivos:

| Hoje | Vai para |
|---|---|
| `_variables.scss` | `abstracts/_tokens.scss` |
| `_base.scss` | `base/_base.scss` (+ `base/_reset.scss`) |
| `_layout.scss` | `layout/_dashboard.scss` (renomeie `.dashboard` para `.l-dashboard`) |
| `components/*` | `module/*` |
| — | `state/_state.scss` e `theme/_escuro.scss` (novos) |

Em cada pasta, crie um `_index.scss` com `@forward` dos arquivos dela. O `main.scss` fica:
```scss
@use 'base';
@use 'layout';
@use 'module';
@use 'state';
@use 'theme';
```
**Pronto quando:** a página volta a aparecer igual à aula passada.

### 2 · Tokens em camadas (8 min)
Em `abstracts/_tokens.scss`, transforme as variáveis em dois maps:
```scss
$primitivos: ('azul-500': #3498db, 'ardosia-800': #2c3e50, /* ... */);
$semanticos: ('acao-primaria': 'azul-500', 'destaque': 'ardosia-800', /* ... */);
```
Em `base/_root.scss`, gere as variáveis com `@each`:
```scss
:root {
  @each $nome, $ref in $semanticos {
    --dsc-cor-#{$nome}: #{map.get($primitivos, $ref)};
  }
}
```
Troque `var(--color-*)` por `var(--dsc-cor-*)` nos módulos.
**Pronto quando:** o DevTools mostra `--dsc-cor-*` no `:root` e nenhum módulo tem hex.

### 3 · Estados (7 min)
- `module/_sidebar.scss`: no mobile (`max-width: 768px`), a sidebar fica oculta e aparece com `.sidebar.is-aberta`.
- `module/_header.scss`: `.header__toggle` só aparece no mobile.
- `.sidebar__link.is-ativo` destaca o link atual.
**Pronto quando:** em 375px de largura, o botão Menu abre e fecha a sidebar.

> Dica: se o botão Menu aparecer no desktop, o `display` do `.btn` está vencendo o do `.header__toggle`. Os dois têm a mesma especificidade, então vence quem vem depois: confira a ordem do `module/_index.scss`.

### 4 · Theme switcher (8 min)
Em `theme/_escuro.scss`, redefina **só tokens semânticos** dentro de `[data-tema='escuro']`.
Adicione a transição global no `body` (desafio da aula passada).
**Pronto quando:** o botão troca o tema **sem editar nenhum módulo**.

### Bônus
- Criar o modificador `.btn--outline` (desafio da aula passada) usando tokens.
- Rodar `npm run lint` sem erros. Ele vai apontar problemas que já existiam no projeto-final!

## Entrega
Colem no chat geral da aula: o link do StackBlitz (ou o `.zip` da pasta) **e** dois prints:
tema escuro no desktop e sidebar aberta no mobile.

## Critérios
- **Arquitetura:** cada regra na pasta certa, BEM nos módulos, prefixos SMACSS em layout e estado.
- **Aderência:** zero valor fixo fora de `_tokens.scss`; tema e sidebar funcionando sem editar módulos.

## Como abrir o projeto

### Opção A · GitHub Codespaces (recomendado, roda na nuvem)
1. Crie um repositório novo no GitHub e envie **todos os arquivos desta pasta**, incluindo a pasta `.devcontainer`
   (pelo site: *Add file → Upload files* e arraste o conteúdo da pasta).
2. No repositório: botão **Code → Codespaces → Create codespace on main**.
3. Espere a primeira abertura (1 a 2 minutos): o `npm install` roda sozinho.
4. No terminal do Codespaces: `npm run dev`. A prévia do Dashboard abre ao lado; se não abrir, vá na aba **Portas** e clique no globo da porta 5173.
5. `npm run lint` confere a arquitetura.

Contas pessoais do GitHub têm uma cota mensal gratuita de Codespaces, suficiente para a aula. Pare o codespace ao terminar.

### Opção B · StackBlitz (sem conta no GitHub)
1. Acesse **stackblitz.com** e crie um projeto em branco do tipo **Node.js**.
2. Apague os arquivos de exemplo e **arraste o conteúdo desta pasta** para o painel de arquivos.
3. No terminal do StackBlitz: `npm install` e depois `npm run dev`.

### Opção C · VS Code local (Node 18 ou superior)
```bash
npm install
npm run dev      # abre em http://localhost:5173
npm run lint
```

## Regras
- Dentro dos módulos, **só `var(--dsc-*)`**. Valores brutos ficam em `abstracts/_tokens.scss`.
- **BEM continua** dentro dos módulos (`card__title`, `btn--primary`).
- **Modificador `--`** é variação escolhida no HTML. **Estado `is-`** é ligado pelo JavaScript.
- Estado de um módulo fica **no arquivo do módulo** (`.sidebar.is-aberta`).
- Sem ID em seletor e no máximo 3 níveis (o lint verifica).
