FROM alpine:3.22.2

WORKDIR /app

COPY ./src/. /app

RUN apk add --no-cache R R-dev R-doc libdeflate ca-certificates \
    curl gcc g++ cmake make python3-dev musl-dev \
    linux-headers zeromq zeromq-dev bash \
    fontconfig font-noto-cjk cairo-dev freetype-dev \
    cairo-dev pango-dev harfbuzz-dev fribidi-dev libpng-dev && \
    curl -LsSf https://astral.sh/uv/install.sh | sh && \
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

ENV PATH="/root/.cargo/bin:/app/.venv/bin:/root/.local/bin:${PATH}"

RUN uv sync --no-cache --python 3.12 && \
      mkdir -p /usr/share/doc/R/html && \
      R -e "install.packages('IRkernel', repos='https://cloud.r-project.org')" && \
      R -e "IRkernel::installspec(name='r', displayname='R (irkernel)')" && \
      cargo install evcxr evcxr_repl evcxr_jupyter evcxr_runtime && \
      evcxr_jupyter --install

ENTRYPOINT [ "jupyter" ]

CMD [ "lab", "--allow-root", "--config", "/app/jupyter_lab_config.py" ]
