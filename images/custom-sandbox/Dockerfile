# Custom default snapshot layered on top of the upstream sandbox image.
# Bump the tag on every rebuild: the API only registers DEFAULT_SNAPSHOT once
# per name, so re-pushing the same tag will not be picked up.
FROM ghcr.io/nightona-co/sandbox:0.6.0

USER root

# Desktop wallpaper (read by xfdesktop through the xfce4-desktop xfconf channel)
COPY wallpaper.png /usr/share/backgrounds/nightona/wallpaper.png
COPY xfce4-desktop.xml /etc/xdg/xfce4/xfconf/xfce-perchannel-xml/xfce4-desktop.xml

# Brand shown in the top-right panel (the Xfce action button displays the user's full name)
RUN usermod -c "khururu" nightona

# LibreOffice with UNO bindings (python3-uno targets Debian's /usr/bin/python3) and
# AT-SPI bindings, plus fonts covering most scripts (CJK, Korean Nanum, emoji, and
# metric-compatible substitutes for Calibri/Cambria)
RUN apt-get update && apt-get install -y --no-install-recommends \
        libreoffice-writer libreoffice-calc libreoffice-impress libreoffice-gtk3 \
        python3-uno python3-pyatspi \
        fonts-noto-core fonts-noto-cjk fonts-noto-color-emoji fonts-nanum \
        fonts-crosextra-carlito fonts-crosextra-caladea \
    && fc-cache -f \
    && rm -rf /var/lib/apt/lists/*

# UNO helper and agent instructions
COPY --chmod=755 lo-uno /usr/local/bin/lo-uno
COPY --chown=nightona:nightona AGENTS.md /home/nightona/AGENTS.md

# The computer-use plugin and toolbox expect to run as the nightona user
USER nightona

# Claude Code reads CLAUDE.md rather than AGENTS.md
RUN ln -s AGENTS.md /home/nightona/CLAUDE.md
