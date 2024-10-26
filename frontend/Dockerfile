# Use a Node.js image for the frontend
FROM node:18-slim AS build

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./
COPY tsconfig.json ./

# Install dependencies
RUN npm install

# Copy the rest of the frontend application code
COPY . .

RUN npm run build

FROM node:18-slim AS run

ENV NODE_ENV=development

WORKDIR /app

# Copy from the build layer
COPY --from=build /app/.svelte-kit ./.svelte-kit
COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/package.json ./package.json
COPY --from=build /app/vite.config.ts ./vite.config.ts
COPY --from=build /app/svelte.config.js ./svelte.config.js

# Expose the port on which the frontend runs
EXPOSE 5173

# Start the frontend application
CMD ["npm", "run", "dev", "--", "--host"]
