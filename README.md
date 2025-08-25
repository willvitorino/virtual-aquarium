---
difficulty: 2
training: true
chapter: "Chapter 5: Challenge Roundup"
tags: vue
---

# Virtual Aquarium 🐠

Aquário virtual funcional criado com Vue.js, permitindo adicionar peixes, alimentá-los e vê-los nadar pelo aquário.

## 🌐 Demo Online

Acesse o projeto rodando no GitHub Pages: [https://willvitorino.github.io/virtual-aquarium/](https://willvitorino.github.io/virtual-aquarium/)

## 🚀 Funcionalidades

- ✨ Exibir diferentes tipos de peixes no aquário (imagens fornecidas na pasta public)
- 📝 Formulário para adicionar peixes:
  - Selecionar tipo de peixe
  - Dar nome ao peixe
  - Adicionar ao aquário
- 🏊‍♂️ Peixes "nadam" pelo aquário com movimento automático
- 🍽️ Sistema de alimentação com nível de fome que aumenta com o tempo
- 🎨 Interface responsiva e animada

## 🛠️ Tecnologias Utilizadas

- Vue.js 3
- Vite
- TailwindCSS
- GitHub Pages (deploy)

## 📦 Instalação e Execução Local

```bash
# Clone o repositório
git clone https://github.com/seu-usuario/virtual-aquarium.git

# Entre na pasta do projeto
cd virtual-aquarium

# Instale as dependências
yarn install

# Execute o projeto em modo de desenvolvimento
yarn dev

# Acesse http://localhost:5173
```

## 🚀 Deploy para GitHub Pages

### Deploy Automático (Recomendado)

1. Faça push para a branch `main`
2. O GitHub Actions irá automaticamente fazer o build e deploy
3. Ative o GitHub Pages nas configurações do repositório:
   - Vá em Settings > Pages
   - Source: Deploy from a branch
   - Branch: gh-pages
   - Folder: / (root)

### Deploy Manual

```bash
# Instale as dependências se necessário
yarn install

# Execute o deploy manual
yarn deploy
```

## 🔧 Scripts Disponíveis

- `yarn dev` - Executa o projeto em modo de desenvolvimento
- `yarn build` - Faz o build para produção
- `yarn preview` - Visualiza o build de produção
- `yarn deploy` - Deploy manual para GitHub Pages
- `yarn test:unit` - Executa os testes unitários
- `yarn lint` - Executa o linter

## 📁 Estrutura do Projeto

```
virtual-aquarium/
├── public/               # Imagens dos peixes e assets estáticos
├── src/
│   ├── App.vue          # Componente principal
│   ├── Aquarium.vue     # Componente do aquário
│   ├── FishBase.vue     # Componente base dos peixes
│   ├── FishForm.vue     # Formulário para adicionar peixes
│   └── main.js          # Arquivo principal
├── .github/workflows/   # GitHub Actions para deploy
└── ...
```

## 🐠 Tipos de Peixes Disponíveis

- Peixe Dourado (gold-fish.png)
- Peixe Tropical (tropical-fish.png)
- Peixe Roxo Dourado (golden-purple-fish.png)
- Guppy (guppie-fish.png)
- Atum (tuna-fish.png)

## 🎮 Como Usar

1. Selecione um tipo de peixe no formulário
2. Digite um nome para o peixe
3. Clique em "Adicionar Peixe"
4. Veja seu peixe nadar pelo aquário
5. Alimente os peixes clicando neles quando estiverem com fome

## 📄 Licença

Este projeto faz parte de um desafio de treinamento Vue.js.

---

*Nenhum peixe foi machucado durante a produção desta aplicação* 🤪
