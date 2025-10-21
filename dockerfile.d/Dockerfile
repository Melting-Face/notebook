FROM ubuntu:25.10

WORKDIR /app

COPY ./src/. /app

# RUN apk add --no-cache R R-dev R-doc libdeflate ca-certificates \
#     curl gcc g++ cmake make python3-dev musl-dev \
#     linux-headers libzmq zeromq zeromq-dev bash \
#     fontconfig font-noto-cjk cairo-dev freetype-dev \
#     cairo-dev pango-dev harfbuzz-dev fribidi-dev libpng-dev && \
RUN apt-get update -y && \
  apt-get install --no-install-recommends -y curl ca-certificates build-essential gnupg openjdk-17-jdk \
  r-base-core cmake pkg-config libzmq5 libssl3 && \
  apt-get clean && \
  apt-get autoremove --purge && \
  rm -rf /var/lib/apt/lists/* && \
  curl -LsSf https://astral.sh/uv/install.sh | sh && \
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

ENV PATH="/root/.cargo/bin:/app/.venv/bin:/root/.local/bin:${PATH}"

RUN uv sync --no-cache --python 3.12 && \
  curl -fsSLO https://github.com/SpencerPark/IJava/releases/download/v1.3.0/ijava-1.3.0.zip && \
  unzip ijava-1.3.0.zip && \
  python install.py && \
  rm -rf ./ijava-1.3.0* && \
  R -e "install.packages('IRkernel', repos='https://cloud.r-project.org')" && \
  R -e "IRkernel::installspec(name='r', displayname='R (irkernel)')" && \
  cargo install evcxr_jupyter && \
  evcxr_jupyter --install

ENTRYPOINT [ "jupyter" ]

CMD [ "lab", "--allow-root", "--config", "/app/jupyter_lab_config.py" ]
