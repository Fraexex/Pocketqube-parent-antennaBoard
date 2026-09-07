# doc/Dockerfile — reproducible build for patch-doc-v1.0.pdf
# Source image is TeX Live full (≈4 GB) so all packages used in spacelab_book.sty
# (xurl, subcaption, nomencl, booktabs, titlesec, iwona, etc.) are present.
# For a smaller image use: ghcr.io/xu-cheng/texlive-full or blang/latex:ubuntu
FROM texlive/texlive:latest

# make is required by doc/Makefile
RUN apt-get update && apt-get install -y --no-install-recommends make \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /work

# Copy doc context (respects doc/.dockerignore)
COPY . .

# Build the single output via the existing Makefile (handles build/references shim)
RUN make

# Default: keep container alive for volume-mount workflow:
#   docker build -f doc/Dockerfile -t pq-patch-doc doc
#   docker run --rm -v "%CD%/doc:/work" pq-patch-doc make
# or without volume mount, extract PDF:
#   docker create --name tmp pq-patch-doc && docker cp tmp:/work/build/patch-doc-v1.0.pdf ./doc/patch-doc-v1.0.pdf && docker rm tmp
CMD ["make"]
