# Étape 1 : Builder
FROM node:22.12-alpine AS builder

# Copier uniquement les fichiers nécessaires pour le build
COPY src/google-maps /app
COPY tsconfig.json /tsconfig.json

WORKDIR /app

# Installer les dépendances sans utiliser les options de cache
RUN npm install

# Build du projet (assurez-vous que la commande `build` existe dans le package.json)
RUN npm run build

# Étape 2 : Release
FROM node:22-alpine AS release

# Copier les fichiers nécessaires depuis l'étape "builder"
COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json

# Définir l'environnement en production
ENV NODE_ENV=production

WORKDIR /app

# Installer uniquement les dépendances nécessaires pour la production
RUN npm ci --ignore-scripts --omit=dev

# Point d'entrée de l'application
ENTRYPOINT ["node", "dist/index.js"]
