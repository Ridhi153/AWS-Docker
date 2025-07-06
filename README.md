//vpc: ----------------------------ubuntu:AMI
create vpc -12.0.0.0/16 
create internetgateway
create subnets -12.0.0.0/20-public
                -12.0.16.0/20 private
create routes
---edit routes
---edit subnet associstion
creation of ec2 -2 instance private,public edit network config: private-sg of public.

in puttyconsole -public nano aws_privatekey_ec2.pem > copy the private key
chmod 400 aws_privatekey_ec2.pem
ssh -i "aws_privatekey_ec2.pem" ec2-user@<EC2-PUBLIC-IP>
 connected to private...


 //Docker -----------------ubuntu AMI
🐳 Step 3: Install Docker on EC2

sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker ubuntu
newgrp docker



docker --version
🧬 Step 4: Clone the GitHub Repo

git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

📄 Step 5: Check/Add Dockerfile
Ensure your repo has a Dockerfile. If not, create one:

Dockerfile


FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
RUN npm install -g serve
CMD ["serve", "-s", "build", "-l", "3000"]

🏗 Step 6: Build Docker Image

docker build -t my-app .
🚀 Step 7: Run the Container

docker run -d -p 3000:3000 my-app
