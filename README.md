# Kanban Klippel

Aplicativo de quadro Kanban para organização de tarefas, com painel de anotações,
níveis de urgência, modo claro/escuro e salvamento dos projetos em arquivo local —
com opção de proteger o arquivo por senha. Funciona como aplicativo de desktop no
Linux e no Windows (empacotado com [Tauri](https://tauri.app)). Também pode ser aberto
no navegador a partir do `index.html` para uma prévia rápida; a experiência completa
(diálogos nativos de arquivo, atalhos de teclado e senha) fica no aplicativo de desktop.

<!-- Substitua pela captura de tela do app -->
<!-- ![Tela do Kanban Klippel](docs/screenshot.png) -->

## Recursos

- **Colunas personalizáveis** — crie colunas com nome e cor (paleta pronta + cor livre); reordene arrastando o cabeçalho ou pelos botões ◀ ▶.
- **Itens completos** — título, descrição, **nível de urgência** (Nenhuma, Baixa, Média, Alta) e **cor própria**, com destaque no cartão.
- **Arrastar e soltar** — mova cartões entre e dentro das colunas, e reordene as próprias colunas, arrastando com o mouse (além das setas em cada cartão).
- **Mostrar/ocultar descrição** — por item ou em todo o quadro de uma vez.
- **Anotações (post-its)** — painel lateral com lembretes coloridos, salvos junto do projeto.
- **Modo claro e escuro** — azul Oxford no tema claro; navy no tema escuro.
- **Arquivos do projeto** — salve e abra projetos em arquivos `.kanban`; na primeira gravação você escolhe o local e, dali em diante, o mesmo arquivo é sobrescrito.
- **Proteção por senha** — opcionalmente, defina uma senha ao salvar pela primeira vez; o arquivo é criptografado (AES-GCM) e exige a senha para ser aberto.
- **Confirmação ao iniciar novo projeto** — se houver um quadro aberto, você escolhe entre salvar e continuar, descartar ou cancelar.
- **Guia integrado** — botão que explica as funções e os atalhos.
- **Atalhos de teclado** — para as ações mais usadas (veja abaixo).

## Atalhos de teclado

| Atalho | Ação |
| --- | --- |
| `Ctrl` + `N` | Novo projeto |
| `Ctrl` + `S` | Salvar projeto atual |
| `Ctrl` + `A` | Abrir projeto |
| `Ctrl` + `1` | Exibir ou ocultar as anotações |
| `Ctrl` + `2` | Alternar entre modo claro e escuro |
| `Ctrl` + `Q` | Nova coluna |

## Download e instalação

Baixe a versão mais recente na página de
[**Releases**](https://github.com/miqueiasklippel/kanban-klippel/releases).

**Fedora / RHEL / openSUSE (pacote `.rpm`)**

```bash
sudo dnf install ./kanban-klippel-1.0.1-1.x86_64.rpm
```

**Debian / Ubuntu (pacote `.deb`)**

```bash
sudo apt install ./kanban-klippel_1.0.1_amd64.deb
```

**Qualquer distribuição (AppImage, portátil)**

```bash
chmod +x kanban-klippel_1.0.1_amd64.AppImage
./kanban-klippel_1.0.1_amd64.AppImage
```

> O AppImage exige uma versão recente da glibc. Em distribuições mais antigas,
> prefira o `.rpm` ou o `.deb`.

**Windows 10/11 (instalador `.exe`)**

Baixe o arquivo `.exe` na página de Releases e dê dois cliques para instalar. Como o
aplicativo ainda não tem assinatura digital, o Windows pode exibir um aviso do
SmartScreen — clique em **"Mais informações" → "Executar assim mesmo"**.

## Como usar

1. Clique em **Nova coluna** para criar etapas (ex.: A fazer, Em andamento, Concluído).
2. Em cada coluna, use **＋ Adicionar item** para criar tarefas com título, descrição, urgência e cor.
3. Reordene cartões e colunas com as setas/botões ou arrastando com o mouse.
4. Use o painel **Anotações** para lembretes avulsos.
5. Em **Salvar**, escolha onde gravar o projeto na primeira vez (e, se quiser, defina uma senha); depois, o mesmo arquivo é sobrescrito. Use **Abrir** para retomar um projeto.

Os projetos são gravados como arquivos `.kanban` — texto em formato JSON quando sem
senha, ou criptografados quando você define uma. Se proteger um arquivo, **guarde bem
a senha: não há como recuperá-la.**

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

### Gerar os pacotes

```bash
NO_STRIP=true npm run tauri build -- --bundles deb rpm appimage
```

> O `NO_STRIP=true` contorna um problema do `strip`/linuxdeploy em distribuições novas
> (como o Fedora 44) ao montar o AppImage. Os arquivos são gerados em
> `src-tauri/target/release/bundle/`.

> Para um AppImage que rode também em sistemas mais antigos, compile dentro de um
> container Ubuntu 22.04.

### Versão Windows

O instalador do Windows (`.exe`) é gerado automaticamente pelo GitHub Actions — não é
preciso ter uma máquina Windows. O fluxo está em `.github/workflows/release-windows.yml`:
ao enviar uma tag de versão (ou ao acionar o workflow manualmente pela aba **Actions**),
o GitHub compila numa máquina Windows e anexa o instalador ao release correspondente.

## Tecnologias

- Interface em HTML, CSS e JavaScript (arquivo único, sem dependências de frontend).
- Arrastar e soltar com Pointer Events, para funcionar de forma confiável no WebKitGTK.
- Criptografia de arquivo com a Web Crypto API (AES-GCM + PBKDF2).
- Empacotamento de desktop com Tauri 2 (Rust), usando os plugins `dialog` e `fs`
  para os diálogos nativos de abrir/salvar.

## Autor

Desenvolvido por **M. A. KLIPPEL** — [github.com/miqueiasklippel](https://github.com/miqueiasklippel?tab=repositories)

## Licença

Distribuído sob a Licença MIT — Copyright © 2026 M. A. Klippel.
Veja o arquivo [`LICENSE`](LICENSE).
