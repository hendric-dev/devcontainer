VERSION 0.6

EARTHLY:
  COMMAND
  RUN sudo curl -L https://github.com/earthly/earthly/releases/latest/download/earthly-linux-amd64 -o /usr/bin/earthly && \
      sudo chmod +x /usr/bin/earthly

OH_MY_BASH:
  COMMAND
  RUN bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)" && \
      sed -i 's/OSH_THEME="font"/OSH_THEME="agnoster"/' $HOME/.bashrc

SYSTEM_LIBRARIES:
  COMMAND
  ENV DEBIAN_FRONTEND=noninteractive
  ENV LANG=de_DE.UTF-8
  RUN apt-get update && \
      apt-get install -y curl git locales sudo tzdata vim zip && \
      rm -rf /var/lib/apt/lists/*
  RUN locale-gen de_DE.UTF-8

USER:
  COMMAND
  ENV USER=developer
  ENV HOME=/home/$USER
  RUN adduser --home $HOME --disabled-password --gecos "" --shell /bin/bash $USER && \
      chown -R $USER $HOME && \
      adduser $USER sudo && \
      echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
  USER $USER
  WORKDIR $HOME
