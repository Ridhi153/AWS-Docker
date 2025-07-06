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


import java.util.Random;

public class RandomNumberGenerator {

    // Generates 'count' random numbers between 'min' and 'max'
    public void generateRandomNumbers(int count, int min, int max) {
        Random random = new Random();

        System.out.println("Generating " + count + " random numbers between " + min + " and " + max + ":");

        for (int i = 0; i < count; i++) {
            int randomNumber = random.nextInt((max - min) + 1) + min;
            System.out.println("Random Number " + (i + 1) + ": " + randomNumber);
        }
    }

    public static void main(String[] args) {
        RandomNumberGenerator rng = new RandomNumberGenerator();
        
        // Example: Generate 5 numbers between 10 and 100
        rng.generateRandomNumbers(5, 10, 100);
    }
}



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
https://<ec2-ip>:3000
















---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------LEX
Weather:
import urllib.request
import json

def lambda_handler(event, context):
    city = "Hyderabad"  # Or extract from event["sessionState"]["intent"]["slots"]["City"]["value"]["interpretedValue"]
    api_key = '61a1d8419f9d307924240958d89e284d';
    base_url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric"

    try:
        with urllib.request.urlopen(base_url) as response:
            data = json.loads(response.read())

        weather = data['weather'][0]['description'].capitalize()
        temp = data['main']['temp']
        feels_like = data['main']['feels_like']
        humidity = data['main']['humidity']

        message = (
            f"Weather report for {city}:\n"
            f"Condition: {weather}\n"
            f"Temperature: {temp}°C (feels like {feels_like}°C)\n"
            f"Humidity: {humidity}%"
        )
    except Exception as e:
        message = "Sorry, I couldn't fetch the weather report at the moment."

    return {
        "sessionState": {
            "dialogAction": { "type": "Close" },
            "intent": {
                "name": "getWeatherInfo",
                "state": "Fulfilled"
            }
        },
        "messages": [
            {
                "contentType": "PlainText",
                "content": message
            }
        ]
    }








gold:

import urllib.request
from bs4 import BeautifulSoup

def lambda_handler(event, context):
    url = 'https://www.bankbazaar.com/gold-rate.html'
    headers = {"User-Agent": "Mozilla/5.0"}
    req = urllib.request.Request(url, headers=headers)

    with urllib.request.urlopen(req) as response:
        html = response.read()

    soup = BeautifulSoup(html, 'html.parser')
    table = soup.find('table')
    hyderabad_rate_24k = None

    if table:
        rows = table.find_all('tr')
        for row in rows[1:]:
            cols = row.find_all('td')
            if len(cols) >= 3:
                city = cols[0].get_text(strip=True).lower()
                rate_24k = cols[2].get_text(strip=True).split('(')[0].strip()
                if city == 'hyderabad':
                    hyderabad_rate_24k = rate_24k
                    break

    if hyderabad_rate_24k:
        message = f"The current 24K gold rate in Hyderabad is {hyderabad_rate_24k}."
    else:
        message = "Sorry, I couldn’t find the gold rate for Hyderabad."

    return {
        "sessionState": {
            "dialogAction": {
                "type": "Close"
            },
            "intent": {
                "name": "GetGoldRate",
                "state": "Fulfilled"
            }
        },
        "messages": [
            {
                "contentType": "PlainText",
                "content": message
            }
        ]
    }
