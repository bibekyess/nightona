# Custom default snapshot layered on top of the upstream sandbox image.
# Bump the tag on every rebuild: the API only registers DEFAULT_SNAPSHOT once
# per name, so re-pushing the same tag will not be picked up.
FROM ghcr.io/nightona-co/sandbox:0.6.0

USER root

# Desktop wallpaper (read by xfdesktop through the xfce4-desktop xfconf channel)
COPY wallpaper.png /usr/share/backgrounds/nightona/wallpaper.png
COPY xfce4-desktop.xml /etc/xdg/xfce4/xfconf/xfce-perchannel-xml/xfce4-desktop.xml

# Extra packages for custom workflows go here, e.g.:
# RUN apt-get update && apt-get install -y --no-install-recommends libreoffice \
#     && rm -rf /var/lib/apt/lists/*

# The computer-use plugin and toolbox expect to run as the nightona user
USER nightona
