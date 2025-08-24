# frontend/Dockerfile
FROM node:20-alpine AS build
# AS should also be in upper case

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
#build angular app for production
RUN npm run build --prod

# Stage 2 - serve with nginx
FROM nginx:alpine
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]