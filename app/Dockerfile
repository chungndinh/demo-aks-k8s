FROM node:13-alpine

WORKDIR /app

COPY . .

RUN npm install
RUN apk --update add redis 

# Development
# CMD ["npm", "run", "dev"]

# Production
RUN npm install -g pm2
CMD ["pm2-runtime", "ecosystem.config.js", "--env", "production"]
