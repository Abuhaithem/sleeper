# Stage 1: Build
FROM node:alpine AS development

# Set the working directory
WORKDIR /usr/src/app

# Copy package.json and yarn.lock (or equivalent)
COPY package.json yarn.lock ./

# Install Yarn globally (if not already in the base image)
RUN npm install -g yarn

# Install dependencies
RUN yarn install

# Copy the rest of the application code
COPY . .

# Build the application
RUN yarn run build

# Stage 2: Production
FROM node:alpine AS production

# Set environment variable
ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

# Set the working directory
WORKDIR /usr/src/app

# Copy package.json and yarn.lock (or equivalent)
COPY package.json yarn.lock ./

# Install Yarn globally (if not already in the base image)
RUN npm install -g yarn

# Install production dependencies
RUN yarn install --production

# Copy built files from the development stage
COPY --from=development /usr/src/app/dist ./dist

# Command to run the application
CMD ["node", "dist/apps/reservations/main"]
