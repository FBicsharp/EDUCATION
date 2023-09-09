# 1. installing node
FROM node:19-alpine

# 2. copy from src local folder to app folder on  image
COPY . /app/
# change direcory to app
WORKDIR /app 

# 3. installing package -> npm install inside folder
RUN npm install
# 4. running application -> node server.js
CMD ["node","server.js"]