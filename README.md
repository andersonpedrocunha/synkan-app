# Synkan App · Cloudflare Pages Deploy

Aplicação principal do Synkan Management Lab — `app.synkan.com.br`.

## Estrutura

```
app-deploy/
├── index.html              ← o app inteiro (era kanban_v8.html)
├── mestre_kan_avatar.png   ← avatar da IA Mestre Kan
├── dojo-bg.png             ← fundo do hero do Dojo
├── manifest.webmanifest    ← PWA manifest
├── _headers                ← segurança + cache
├── robots.txt              ← noindex (app é privado)
└── .gitignore
```

## Subir o app (~15 min)

### 1. Criar repositório no GitHub

1. Vai em https://github.com/new
2. **Repository name:** `synkan-app`
3. **Public** (mesmo motivo do site — não tem segredo aqui, e facilita Cloudflare)
4. Não marca nenhuma checkbox
5. **Create repository**

### 2. Subir os arquivos

**Caminho A — GitHub Desktop (recomendado, você já tá usando):**

1. Abre o GitHub Desktop
2. **File → Clone repository → URL**
3. Cola: `https://github.com/SEU_USUARIO/synkan-app.git`
4. Local path: `/Users/andi_twd/Documents/Claude/Projects/Board unificado/synkan-app`
5. Clone

Aí no Finder, copia o conteúdo da pasta `app-deploy/` pra dentro de `synkan-app/`. GitHub Desktop vai mostrar 7 arquivos modificados.

6. Summary: `Initial commit: Synkan app`
7. **Commit to main → Push origin**

**Caminho B — pelo navegador (mais lento):**

1. Vai no repo synkan-app no GitHub
2. **Add file → Upload files**
3. Arrasta TODO o conteúdo da pasta `app-deploy/`
4. Commit changes

### 3. Cloudflare Pages

1. dash.cloudflare.com → **Compute → Workers & Pages → Create**
2. Aba **Pages** → **Connect to Git**
3. Escolhe **synkan-app**
4. Begin setup → configurações:
   - Project name: `synkan-app`
   - Framework preset: **None**
   - Build command: *(vazio)*
   - Build output directory: `/`
5. **Save and Deploy**

Em ~1 min sai no ar em `synkan-app.pages.dev`.

### 4. Conectar domínio app.synkan.com.br

1. Pages → projeto `synkan-app` → **Custom domains**
2. **Set up a custom domain** → digita `app.synkan.com.br`
3. **Continue → Activate**
4. Se pedir para criar CNAME no DNS, faz como antes:
   - DNS → Records → Add → CNAME, Name: `app`, Target: `synkan-app.pages.dev`, Proxied

### 5. Testar

Abre https://app.synkan.com.br no navegador.

O app vai carregar. Funcionalidades:
- ✅ Modo local (offline) funciona — você pode criar boards, cards, mover cards, etc
- ✅ Tema dojo aplicado
- ✅ Onboarding tour aparece no primeiro acesso
- ⚠️ IA do Mestre Kan vai dar erro (servidor backend ainda não está no ar)
- ⚠️ Sync com Supabase requer configuração adicional
- ⚠️ Cobrança Stripe requer subir o billing_server

Esses 3 pontos são os próximos passos do roadmap (subir backend Python).

### 6. Atualizar o link da landing

Já está. Os botões "Entrar →", "Começar trial →", "Faixa Azul →", etc na landing (`synkan.com.br`) já apontam pra `https://app.synkan.com.br`. Quando o app subir, esses links começam a funcionar automaticamente.

## Otimizações futuras

O `dojo-bg.png` está com **9MB** — tá pesado pra carregamento. Quando tiver tempo:
- Comprime via [tinypng.com](https://tinypng.com) ou [squoosh.app](https://squoosh.app)
- Idealmente abaixa pra <500KB
- O ideal seria converter pra WebP (~70% menor)

Mas pra MVP funciona. Cloudflare faz cache agressivo e os usuários só baixam 1x.

## Workflow daqui pra frente

Pra atualizar o app:

1. Eu edito `kanban_v8.html` (a fonte de verdade)
2. Sincronizo as mudanças pra `synkan-app/index.html`
3. Você abre GitHub Desktop → Commit → Push
4. ~1 min depois `app.synkan.com.br` tá atualizado

Pra evitar duplicação, podemos também fazer o oposto:
- Eu edito direto em `synkan-app/index.html`
- E sincronizo de volta pra `kanban_v8.html`

A gente decide isso depois.
