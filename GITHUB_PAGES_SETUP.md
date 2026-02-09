# ZTech Informática - Guia de Deployment no GitHub Pages

Este documento fornece instruções para fazer deploy do site da ZTech no GitHub Pages.

## Pré-requisitos

- Conta no GitHub
- Git instalado localmente
- Node.js e npm/pnpm instalados

## Passos para Deployment

### 1. Criar um repositório no GitHub

1. Acesse [github.com](https://github.com) e faça login
2. Clique em "New repository"
3. Nomeie o repositório como `ztech-site` (ou outro nome de sua preferência)
4. Escolha "Public" para que o site seja acessível
5. Clique em "Create repository"

### 2. Preparar o repositório local

```bash
# Navegar até o diretório do projeto
cd /home/ubuntu/ztech-site

# Inicializar git (se ainda não foi feito)
git init

# Adicionar o repositório remoto
git remote add origin https://github.com/seu-usuario/ztech-site.git

# Adicionar todos os arquivos
git add .

# Fazer commit inicial
git commit -m "Initial commit: ZTech website"

# Fazer push para o repositório
git branch -M main
git push -u origin main
```

### 3. Configurar GitHub Pages

1. Acesse seu repositório no GitHub
2. Vá para **Settings** → **Pages**
3. Em "Source", selecione:
   - Branch: `main`
   - Folder: `/ (root)` ou `/dist` (dependendo de como você fizer o build)
4. Clique em "Save"

### 4. Configurar o build para GitHub Pages

Para que o site funcione corretamente no GitHub Pages, você precisa fazer o build:

```bash
# Instalar dependências
pnpm install

# Fazer o build
pnpm build

# O site será gerado na pasta 'dist'
```

### 5. Fazer deploy do site construído

Existem duas opções:

#### Opção A: Deploy automático com GitHub Actions (Recomendado)

Crie um arquivo `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: pnpm install
      
      - name: Build
        run: pnpm build
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

#### Opção B: Deploy manual

1. Faça o build localmente: `pnpm build`
2. Copie o conteúdo da pasta `dist` para a raiz do repositório
3. Faça commit e push

### 6. Acessar o site

Após alguns minutos, seu site estará disponível em:
- `https://seu-usuario.github.io/ztech-site`

Se você configurou um domínio personalizado, o site será acessível nesse domínio.

## Configuração de Domínio Personalizado (Opcional)

1. Vá para **Settings** → **Pages**
2. Em "Custom domain", digite seu domínio (ex: ztech.com.br)
3. Clique em "Save"
4. Configure os registros DNS do seu domínio para apontar para o GitHub Pages

## Troubleshooting

### O site não está aparecendo

- Verifique se o branch está correto em Settings → Pages
- Aguarde alguns minutos para o deploy ser concluído
- Verifique os logs em "Actions" para erros de build

### Imagens não estão carregando

- Certifique-se de que as URLs das imagens são absolutas ou relativas corretas
- Verifique se as imagens estão na pasta `client/public`

### Estilos não estão aplicados

- Limpe o cache do navegador (Ctrl+Shift+Delete)
- Verifique se o arquivo `index.css` está sendo carregado corretamente

## Atualizações Futuras

Para atualizar o site após fazer mudanças:

1. Faça as alterações no código
2. Faça commit: `git commit -m "Descrição das mudanças"`
3. Faça push: `git push origin main`
4. O GitHub Actions fará o build e deploy automaticamente (se configurado)

## Suporte

Para mais informações sobre GitHub Pages, visite:
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Vite Deployment Guide](https://vitejs.dev/guide/static-deploy.html#github-pages)
