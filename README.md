# Sistema de Orçamentos

Um sistema de orçamentos desenvolvido para simplificar e otimizar a gestão de produtos e a criação de orçamentos personalizados. Este projeto foi construído utilizando **Svelte**, **Skeleton**, **TailwindCSS**, **Tauri**, **Rust** e **Python**.

## Funcionalidades

- **Visualização de Produtos**: Tabela com paginação para listar os produtos.
- **Importação de Produtos**: Botão para importar produtos via um arquivo PDF, utilizando uma função em Python que converte PDF para JSON.
- **Configuração de Margens de Lucro**: Defina margens de lucro para repassar os preços baseados no custo.
- **Tema Claro e Escuro**: Interface com suporte a alternância entre temas claro e escuro.

## Tecnologias Utilizadas

- **Frontend**:
  - [Svelte](https://svelte.dev)
  - [Skeleton](https://skeleton.dev)
  - [TailwindCSS](https://tailwindcss.com)
- **Backend**:
  - [Tauri](https://tauri.app) (com Rust)
  - [Python](https://www.python.org)

## Como Utilizar

1. Clone o repositório:
   ```bash
   git clone https://github.com/DFaltGP/budgets_system.git
   cd budgets_system
   ```
2. Instale as dependências e execute em modo de desenvolvimento:
   ```
   npm install
   npm run dev
   ```

## Contribuição

Contribuições são bem-vindas! Sinta-se à vontade para abrir uma issue ou enviar um pull request.

## Licença

Este projeto está licenciado sob a MIT License.
