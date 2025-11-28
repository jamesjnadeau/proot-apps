ARG REPO
FROM ghcr.io/${REPO}:alpine-321 AS rootfs
ARG REPO

# Install packages
RUN \
  echo "**** install packages ****" && \
  apk add --no-cache flatpak gnome-software-plugin-flatpak \
    && flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo \
    && flatpak install -y flathub com.prusa3d.PrusaSlicer \
  echo "**** customize desktop file ****" && \
  # sed -i \
  #   -e "/^Exec=/c Exec=/bin/sh -c \"\$HOME\/.local\/bin\/proot-apps run ghcr.io/${REPO}:prusa-slicer %U\"" \
  #   # -e "s/Name=PrusaSlicer/Name=Prusa Slicer/g" \
  #   -e '/^TryExec=/d' \
  #   /usr/share/applications/firefox.desktop && \
  # mv \
  #   /usr/share/applications/firefox.desktop \
  #   /usr/share/applications/firefox-pa.desktop && \
  echo "**** set bin name ****" && \
  echo "prusa-slicer-pa" > /bin-name && \
  echo "**** cleanup ****" && \
  rm -rf \
    /tmp/*

COPY ./entrypoint /entrypoint
COPY ./install /install
COPY ./remove /remove

FROM scratch
COPY --from=rootfs / /
