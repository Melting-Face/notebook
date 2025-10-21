FROM debian:12.12-slim

WORKDIR /app

COPY ./src/. /app

# RUN apk add --no-cache R R-dev R-doc libdeflate ca-certificates \
#     curl gcc g++ cmake make python3-dev musl-dev \
#     linux-headers libzmq zeromq zeromq-dev bash \
#     fontconfig font-noto-cjk cairo-dev freetype-dev \
#     cairo-dev pango-dev harfbuzz-dev fribidi-dev libpng-dev && \
RUN apt-get update -y && apt-get install --no-install-recommends -y curl ca-certificates build-essential r-base && \
  apt-get clean && \
  apt-get autoremove --purge && \
  rm -rf /var/lib/apt/lists/* && \
  curl -LsSf https://astral.sh/uv/install.sh | sh && \
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

ENV PATH="/root/.cargo/bin:/app/.venv/bin:/root/.local/bin:${PATH}"

RUN uv sync --no-cache --python 3.12 && \
  R -e "install.packages('IRkernel', repos='https://cloud.r-project.org')" && \
  R -e "IRkernel::installspec(name='r', displayname='R (irkernel)')" && \
  cargo install evcxr_jupyter && \
  evcxr_jupyter --install

# ENTRYPOINT [ "jupyter" ]
#
# CMD [ "lab", "--allow-root", "--config", "/app/jupyter_lab_config.py" ]
