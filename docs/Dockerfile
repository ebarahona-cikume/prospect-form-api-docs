# Usa una imagen base de Python
FROM python:3.11-slim

# Instala make y dependencias
RUN apt-get update && apt-get install -y make && rm -rf /var/lib/apt/lists/*

# Instala Sphinx, el tema ReadTheDocs y la extensión para Markdown
RUN pip install --no-cache-dir \
    sphinx \
    sphinx-rtd-theme \
    myst-parser

# Define el directorio de trabajo dentro del contenedor
WORKDIR /docs

# Copia todo el contenido de la carpeta docs al contenedor
COPY . /docs

# Comando por defecto para construir la documentación
CMD ["make", "html"]
