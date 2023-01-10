#NOTE root 以外のユーザーをコンテナーに追加する: https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user

FROM debian:bullseye

# 変数定義
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# ユーザー作成
RUN groupadd --gid ${USER_UID} ${USERNAME} \
    #BUG useradd: Warning: missing or non-executable shell '/bin/zsh' シェルを指定する前にインストールの必要ありか?
    && useradd --uid ${USER_UID} --gid ${USER_GID} --create-home ${USERNAME} \
    # --shell /bin/zsh \
    #[オプション】sudoのサポートを追加します。接続後にソフトをインストールする必要がない場合は省略。
    && apt-get update \
    && apt-get install -y --no-install-recommends \
    #BUG E: Version 'latest' for 'sudo' was not found
    #BUG E: Version '3.11.0' for 'python' was not found
    sudo \
    python \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \

    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME


# RUN pip install --upgrade \
#     pip && \
#     setuptools
# RUN pip install -r requirements.txt
