FROM ubuntu:bionic

ENV PATH="/usr/local/texlive/bin/x86_64-linux:${PATH}"

RUN sed -i s,http://archive.ubuntu.com/ubuntu/,mirror://mirrors.ubuntu.com/mirrors.txt, /etc/apt/sources.list \
 && apt-get update \
 && export DEBIAN_FRONTEND=noninteractive \
 && apt-get install -qy perl ca-certificates make libxslt1.1 libxslt-dev libxml2 libxml2-dev gcc wget curl git unzip cpanminus --no-install-recommends --no-install-suggests \
 && export PATH="/tmp/texlive/bin/x86_64-linux:$PATH" \
 && REMOTE="http://mirror.ctan.org/systems/texlive/tlnet" \
 && INSTALL="/tmp/install-texlive" \
 && mkdir -p $INSTALL \
 && echo "export PATH=$PATH:/usr/local/texlive/2018/bin" >> /etc/environment \
 && curl -sSL $REMOTE/install-tl-unx.tar.gz | tar -xzv -C $INSTALL --strip-components=1 \
 && echo selected_scheme scheme-basic > texlive.profile \
 && echo TEXDIR /usr/local/texlive/ >> texlive.profile \
 && echo TEXMFLOCAL /usr/local/texmf-local >> texlive.profile \
 && echo TEXMFSYSCONFIG /usr/local/texmf-config >> texlive.profile \
 && echo TEXMFSYSVAR /usr/local/texmf-var >> texlive.profile \
 && echo tlpdbopt_autobackup 0 >> texlive.profile \
 && echo tlpdbopt_install_docfiles 0 >> texlive.profile \
 && echo tlpdbopt_install_srcfiles 0 >> texlive.profile \
 && $INSTALL/install-tl --profile texlive.profile \
 && rm texlive.profile \
 && tlmgr update --self --all --reinstall-forcibly-removed \
 && tlmgr install latexmk listings xcolor float multirow tocloft \
 && cpanm JSON \
 && cpanm --notest https://github.com/HansOlsson/LaTeXML.git@UseLabel \
 && apt-get autoremove -qy make gcc curl wget git cpanminus libxml2-dev libxslt-dev \
 && latexmk --version \
 && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/ /root/.cpanm/work*
