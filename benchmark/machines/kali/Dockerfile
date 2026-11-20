FROM lucagioacchini/kali-linux-headless:latest

USER root

ENV DEBIAN_FRONTEND noninteractive
ENV KALI_PKG=kali-linux-headless

RUN pip3 install pycryptodome
RUN mkdir /root/scripts

# update mirrors

RUN KEYRING_DEB=$(wget -qO- https://http.kali.org/pool/main/k/kali-archive-keyring/ | grep -oE 'kali-archive-keyring_[0-9][^"]*_all\.deb' | sort -V | tail -1) \
    && wget -q "https://http.kali.org/pool/main/k/kali-archive-keyring/${KEYRING_DEB}" \
    && dpkg -i "${KEYRING_DEB}" \
    && rm "${KEYRING_DEB}"
RUN sed -i 's|http://http.kali.org/kali|http://kali.mirror.garr.it/mirrors/kali|g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get full-upgrade -y -o Dpkg::Options::="--force-overwrite"
RUN apt -y install --no-install-recommends ${KALI_PKG}
RUN apt-get remove -y hashcat-data || true

RUN apt-get full-upgrade -y -o Dpkg::Options::="--force-overwrite"
RUN apt-get purge -y vim vim-nox vim-tiny
RUN apt-get install -y openssh-server
RUN apt-get install -y xxd
RUN setcap -r /usr/lib/nmap/nmap

# Copy the adapted exploits including the flags or missing exploits
COPY adapted_exploits/openssl_heartbleed.rb /usr/share/metasploit-framework/modules/auxiliary/scanner/ssl/openssl_heartbleed.rb
COPY adapted_exploits/geoserver_unauth_rce_cve_2024_36401.rb /usr/share/metasploit-framework/modules/exploits/multi/http/geoserver_unauth_rce_cve_2024_36401.rb
COPY adapted_exploits/log4shell_scanner.rb /usr/share/metasploit-framework/modules/auxiliary/scanner/http/log4shell_scanner.rb
