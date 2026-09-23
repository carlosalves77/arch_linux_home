# home_arch

Meus dotfiles do Arch Linux com **Hyprland**, **Waybar**, **Kitty** e **Rofi**.

## Estrutura do repositório e destino em `~/.config`

| Arquivo no repositório | Destino em `~/.config` | Observação |
| --- | --- | --- |
| `hyperland/hyprland.conf` | `~/.config/hypr/hyprland.conf` | Config principal do Hyprland |
| `hyperland/hyprland.lua` | `~/.config/hypr/hyprland.lua` | Mesma config convertida para Lua (via `hyprconf2lua`). Use **apenas uma** das duas |
| `kitty_console/kitty.conf` | `~/.config/kitty/kitty.conf` | Terminal Kitty |
| `rofi/config.rasi` | `~/.config/rofi/config.rasi` | Lançador de apps Rofi (config + tema) |
| `waybar/config.jsonc` | `~/.config/waybar/config.jsonc` | Config da barra (em uso) |
| `waybar/style.css` | `~/.config/waybar/style.css` | Tema da barra (em uso) |
| `waybar/scripts/waybar-wttr.py` | `~/.config/waybar/scripts/waybar-wttr.py` | Script do módulo de clima (precisa ser executável) |
| `waybar/scripts/xclip` | `~/.config/waybar/scripts/xclip` | Arquivo auxiliar do script de clima |
| `waybar/config-gitlab.jsonc` | — | Config de exemplo/alternativa, não é carregada automaticamente |
| `waybar/*.bak`, `waybar/scripts/*.bak`, `waybar/backup/` | — | Backups de versões antigas; não precisam ser copiados |

> Observação: a pasta no repositório se chama `hyperland/`, mas o Hyprland lê de `~/.config/hypr/`.

Árvore final esperada:

```
~/.config/
├── hypr/
│   └── hyprland.conf
├── kitty/
│   └── kitty.conf
├── rofi/
│   └── config.rasi
└── waybar/
    ├── config.jsonc
    ├── style.css
    └── scripts/
        ├── waybar-wttr.py
        └── xclip
```

## Instalação

### Copiando os arquivos

```bash
git clone <url-do-repo> ~/home_arch
cd ~/home_arch

mkdir -p ~/.config/hypr ~/.config/kitty ~/.config/rofi ~/.config/waybar/scripts

cp hyperland/hyprland.conf          ~/.config/hypr/hyprland.conf
cp kitty_console/kitty.conf         ~/.config/kitty/kitty.conf
cp rofi/config.rasi                 ~/.config/rofi/config.rasi
cp waybar/config.jsonc              ~/.config/waybar/config.jsonc
cp waybar/style.css                 ~/.config/waybar/style.css
cp waybar/scripts/waybar-wttr.py    ~/.config/waybar/scripts/
cp waybar/scripts/xclip             ~/.config/waybar/scripts/

chmod +x ~/.config/waybar/scripts/waybar-wttr.py
```

### Alternativa: links simbólicos

Assim, alterações no repositório refletem direto no sistema (faça backup das configs existentes antes):

```bash
cd ~/home_arch

mkdir -p ~/.config/hypr ~/.config/kitty ~/.config/rofi ~/.config/waybar

ln -sf "$PWD/hyperland/hyprland.conf"  ~/.config/hypr/hyprland.conf
ln -sf "$PWD/kitty_console/kitty.conf" ~/.config/kitty/kitty.conf
ln -sf "$PWD/rofi/config.rasi"        ~/.config/rofi/config.rasi
ln -sf "$PWD/waybar/config.jsonc"      ~/.config/waybar/config.jsonc
ln -sf "$PWD/waybar/style.css"         ~/.config/waybar/style.css
ln -sfn "$PWD/waybar/scripts"          ~/.config/waybar/scripts

chmod +x waybar/scripts/waybar-wttr.py
```

### Aplicando

```bash
hyprctl reload              # recarrega o Hyprland
killall -SIGUSR2 waybar     # recarrega o Waybar (ou reinicie a sessão)
```

O Kitty relê a config ao abrir uma nova janela (ou `Ctrl+Shift+F5`). O Rofi lê a config a cada execução, então não precisa recarregar.

## Detalhes de cada config

### Hyprland (`~/.config/hypr/hyprland.conf`)

- **Monitores:** `DP-3` 1920x1080@144 (à direita) e `HDMI-A-1` 1920x1080@100 (à esquerda); workspace 1 fixo no `DP-3`. Ajuste os nomes com `hyprctl monitors`.
- **Teclado:** layout `us`, variante `intl`.
- **Autostart:** `waybar`, `hyprpaper`, `swaybg` (wallpaper em `/home/carl/Pictures/arch_linux.jpg`), `~/.config/hyprsunset/nightshift.sh`, `swaync` e tema escuro via `gsettings`.
  - O script `~/.config/hyprsunset/nightshift.sh` **não está neste repositório**; crie-o ou remova a linha.
  - Ajuste o caminho do wallpaper para o seu usuário.

Principais atalhos (`SUPER` = tecla Windows):

| Atalho | Ação |
| --- | --- |
| `SUPER + Q` | Abre o Kitty |
| `SUPER + C` | Fecha a janela ativa |
| `SUPER + E` | Abre o Nautilus |
| `SUPER + R` | Rofi (lançador de apps) |
| `SUPER + V` | Alterna janela flutuante |
| `SUPER + J` | Alterna split (dwindle) |
| `SUPER + M` | Sai do Hyprland |
| `SUPER + SHIFT + H` | Mostra/esconde o Waybar |
| `SUPER + 1..0` | Vai para o workspace 1..10 |
| `SUPER + SHIFT + 1..0` | Move janela para o workspace 1..10 |
| `SUPER + S` | Workspace especial (scratchpad) |
| `SUPER + SHIFT + =` / `-` | Volume +5% / -5% |
| `SUPER + SHIFT + M` | Muta o áudio |
| `Print` | Screenshot da tela inteira para o clipboard |
| `SUPER + SHIFT + P` | Screenshot de área para o clipboard |
| `SUPER + SHIFT + S` | Screenshot de área e abre no Swappy |

> `SUPER + SHIFT + S` também está ligado a "mover para o workspace especial"; como os dois binds disparam, vale remover um deles.

### Kitty (`~/.config/kitty/kitty.conf`)

Tema neon escuro (roxo/ciano/verde), fonte **JetBrainsMono Nerd Font Mono** 13pt com ligaduras, fundo com 82% de opacidade e blur, cursor beam e barra de abas powerline na parte inferior.

### Rofi (`~/.config/rofi/config.rasi`)

Lançador de apps no modo `drun` (com ícones), aberto por `SUPER + R` ou pelo clique no logo do Arch no Waybar.

- Janela de 450px, fundo escuro `#0d0e1a`, borda branca de 2px e cantos arredondados (8px).
- Lista de 1 coluna com 7 linhas, sem scrollbar; o item selecionado fica com fundo branco e texto preto.
- Fonte **Figtree** 13pt e placeholder "Search Apps".

Para testar sem o atalho: `rofi -show drun`.

### Waybar (`~/.config/waybar/`)

Layout da barra:

- **Esquerda:** logo do Arch (clique abre Rofi, clique direito abre `wlogout`), relógio, contagem de updates do pacman, player de mídia (MPRIS).
- **Centro:** workspaces do Hyprland.
- **Direita:** clima, microfone, volume, Bluetooth e notificações (swaync).

O módulo de clima executa `~/.config/waybar/scripts/waybar-wttr.py`, que consulta `wttr.in` para **Recife**. Para mudar a cidade, edite a URL no script.

## Dependências

```bash
sudo pacman -S hyprland hyprpaper hyprsunset waybar kitty rofi nautilus \
  swaybg swaync grim slurp swappy wl-clipboard wireplumber pavucontrol \
  playerctl brightnessctl blueman pacman-contrib python-requests \
  ttf-jetbrains-mono-nerd
```

- `pacman-contrib` fornece o `checkupdates` usado pelo módulo de updates.
- `python-requests` é necessário para o script de clima.
- `wlogout` está disponível no AUR (`yay -S wlogout`).
- A fonte **Figtree** usada pelo Rofi está no AUR (`yay -S ttf-figtree`); sem ela o Rofi usa a fonte padrão do sistema.
