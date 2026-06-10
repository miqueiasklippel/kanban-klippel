# Kanban Klippel

Aplicativo de quadro Kanban para organização de tarefas, com painel de anotações,
modo claro/escuro e salvamento dos projetos em arquivo local. Funciona como
aplicativo de desktop no Linux e no Windows (empacotado com [Tauri](https://tauri.app)) e também
abre direto no navegador a partir do `index.html`.


## Recursos

- **Colunas personalizáveis** — crie colunas com nome e cor (paleta pronta + cor livre).
- **Itens com título e descrição** — adicione, edite e exclua tarefas.
- **Movimentação** — setas para mover o item para cima/baixo ou entre colunas, e também arrastar e soltar.
- **Mostrar/ocultar descrição** — por item ou em todo o quadro de uma vez.
- **Anotações (post-its)** — painel lateral com lembretes coloridos, salvos junto do projeto.
- **Modo claro e escuro** — azul Oxford no tema claro; navy no tema escuro.
- **Arquivos do projeto** — salve e abra projetos em arquivos `.kanban`; ao salvar, o mesmo arquivo é sobrescrito.
- **Novo projeto** — começa um quadro em branco.

## Download e instalação

Baixe a versão mais recente na página de
[**Releases**](https://github.com/miqueiasklippel/kanban-klippel/releases).

**Fedora / RHEL / openSUSE (pacote `.rpm`)**

```bash
sudo dnf install ./kanban-klippel-1.0.0.x86_64.rpm
```

**Debian / Ubuntu (pacote `.deb`)**

```bash
sudo apt install ./kanban-klippel_1.0.0_amd64.deb
```

**Qualquer distribuição (AppImage, portátil)**

```bash
chmod +x "kanban-klippel v.1.0.0.AppImage"
"./kanban-klippel v.1.0.0.AppImage"
```

> O AppImage exige uma versão recente da glibc. Em distribuições mais antigas,
> prefira o `.rpm` ou o `.deb`.

**Windows 10/11 (instalador `.exe`)**

Baixe o arquivo `.exe` na página de Releases e dê dois cliques para instalar. Como o aplicativo ainda não tem assinatura digital, o Windows pode exibir um aviso do SmartScreen — clique em **"Mais informações" → "Executar assim mesmo"**.

## Como usar

1. Clique em **Nova coluna** para criar etapas (ex.: A fazer, Em andamento, Concluído).
2. Em cada coluna, use **+ Adicionar item** para criar tarefas com título e descrição.
3. Reordene com as setas ou arrastando os cartões entre as colunas.
4. Use o painel **Anotações** para lembretes avulsos.
5. Em **Salvar**, escolha onde gravar o projeto na primeira vez; depois, o mesmo arquivo é sobrescrito. Use **Abrir** para retomar um projeto.

Os projetos são gravados como arquivos `.kanban` (texto em formato JSON), fáceis de versionar ou guardar em backup.

## Compilar a partir do código

### Pré-requisitos (Fedora)

```bash
sudo dnf install webkit2gtk4.1-devel openssl-devel curl wget file \
  libappindicator-gtk3-devel librsvg2-devel libxdo-devel
sudo dnf group install "c-development"
```

Também são necessários:

- [Rust](https://www.rust-lang.org/tools/install) (via `rustup`)
- [Node.js](https://nodejs.org) 20 ou superior

### Rodar em desenvolvimento

```bash
npm install
npm run tauri dev
```

### Gerar os pacotes (Linux)

```bash
# .deb e .rpm
npm run tauri build -- --bundles deb rpm

# AppImage (NO_STRIP contorna um problema do linuxdeploy em distros novas)
NO_STRIP=true npm run tauri build -- --bundles appimage
```

Os arquivos são gerados em `src-tauri/target/release/bundle/`.

> Para um AppImage que rode também em sistemas mais antigos, compile dentro de um
> container Ubuntu 22.04 (veja o `Containerfile`). Para gerar um Flatpak, use o
> manifesto `com.klippel.kanban.yml`.

### Versão Windows

O instalador do Windows (`.exe`) é gerado automaticamente pelo GitHub Actions — não é preciso ter uma máquina Windows. O fluxo está em `.github/workflows/release-windows.yml`: ao enviar uma tag de versão (ou ao acionar o workflow manualmente pela aba **Actions**), o GitHub compila numa máquina Windows e anexa o instalador ao release correspondente.

## Tecnologias

- Interface em HTML, CSS e JavaScript (arquivo único, sem dependências de frontend).
- Empacotamento de desktop com Tauri 2 (Rust), usando os plugins `dialog` e `fs`
  para os diálogos nativos de abrir/salvar.

## Autor

Desenvolvido por **M. A. KLIPPEL** — [github.com/miqueiasklippel](https://github.com/miqueiasklippel?tab=repositories)

## Licença

veja o arquivo `LICENSE`.
