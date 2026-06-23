# Dockerfile para rodar Evo CRM Community com Docker Compose no Railway
FROM docker:24-dind

# Instalar Docker Compose, Git, Make e outras ferramentas
RUN apk add --no-cache \
    docker-compose \
    git \
    make \
    bash \
    curl \
    postgresql-client

WORKDIR /app

# Copiar código do projeto
COPY . .

# Atualizar submodules
RUN git submodule update --init --recursive 2>/dev/null || true

# Criar script de inicialização
RUN cat > /app/entrypoint.sh << 'SCRIPT'
#!/bin/bash
set -e

echo "🚀 Iniciando Evo CRM Community com Docker Compose..."

# Aguardar Docker estar pronto (importante para DinD)
echo "⏳ Aguardando Docker daemon..."
sleep 5

# Copiar .env se não existir
if [ ! -f .env ]; then
    echo "📝 Criando arquivo .env..."
    if [ -f .env.example ]; then
        cp .env.example .env
    fi
fi

# Atualizar submodules
echo "📦 Atualizando submodules..."
git submodule update --init --recursive 2>/dev/null || true

# Iniciar Docker Compose
echo "🐳 Iniciando serviços com Docker Compose..."
docker-compose -f docker-compose.yml up --build

SCRIPT

chmod +x /app/entrypoint.sh

EXPOSE 3030

ENTRYPOINT ["/app/entrypoint.sh"]
