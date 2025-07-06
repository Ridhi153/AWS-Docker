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


    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------CLOUD FRONT:
    
Sairidhi <ridhisriramula@gmail.com>
Tue 1 Jul, 20:07 (5 days ago)
to Srinidhi Sriramula

import React, { useState } from 'react';

import axios from 'axios';

import './App.css';

 

function App() {

  const [rollNo, setRollNo] = useState('');

  const [name, setName] = useState('');

  const [subject, setSubject] = useState('Math');

  const [rating, setRating] = useState('5');

 

  const handleSubmit = async (e) => {

    e.preventDefault();

    const data = { rollNo, name, subject, rating };

 

    try {

      await axios.post('https://<API_GATEWAY_URL>/feedback', data);

      alert('Feedback submitted!');

    } catch (error) {

      console.error(error);

      alert('Submission failed.');

    }

  };

 

  return (

    <div className="App">

      <h1>Feedback Form</h1>

      <form onSubmit={handleSubmit}>

        <input placeholder="Roll No" value={rollNo} onChange={(e) => setRollNo(e.target.value)} required /><br/><br/>

        <input placeholder="Name" value={name} onChange={(e) => setName(e.target.value)} required /><br/><br/>

        <select value={subject} onChange={(e) => setSubject(e.target.value)}>

          <option value="Math">Math</option>

          <option value="Science">Science</option>

          <option value="English">English</option>

        </select><br>

        </br><br/>

        <select value={rating} onChange={(e) => setRating(e.target.value)}>

          {[1, 2, 3, 4, 5].map(n => <option key={n} value={n}>{n}</option>)}

        </select><br/><br/>

        <button type="submit">Submit</button>

      </form>

    </div>

  );

}

 

export default App;

LAMBDAAAAAAA

import json
import boto3
import uuid

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('FeedbackTable-22bd1a057u')

def lambda_handler(event, context):
    body = json.loads(event['body'])

    item = {
        'id': str(uuid.uuid4()),
        'rollNo': body['rollNo'],
        'name': body['name'],
        'subject': body['subject'],
        'rating': body['rating']
    }

    table.put_item(Item=item)

    return {
        'statusCode': 200,
        'headers': { 'Access-Control-Allow-Origin': '*' },
        'body': json.dumps({ 'message': 'Feedback received!' })
    }




















--------------------------------------------------STUDENT DB-------------------------------------------------------------------------------------------------------------------------------------------------


import boto3
import random
from boto3.dynamodb.conditions import Attr
from collections import defaultdict

# Initialize DynamoDB
dynamodb = boto3.resource('dynamodb', region_name='eu-north-1')  # Change region if needed
table_name = '22bd1a057u_StudentDB'

# A. Create Table
def create_table():
    try:
        table = dynamodb.create_table(
            TableName=table_name,
            KeySchema=[
                {'AttributeName': 'StudentRollNo', 'KeyType': 'HASH'},
            ],
            AttributeDefinitions=[
                {'AttributeName': 'StudentRollNo', 'AttributeType': 'S'},
            ],
            ProvisionedThroughput={'ReadCapacityUnits': 5, 'WriteCapacityUnits': 5}
        )
        table.wait_until_exists()
        print("Table created successfully.")
    except dynamodb.meta.client.exceptions.ResourceInUseException:
        print("Table already exists.")

# B. Insert 15 Random Students
def insert_random_students():
    table = dynamodb.Table(table_name)
    names = ["Alice", "Bob", "Charlie", "Daisy", "Evan", "Fiona", "George", "Hannah", "Ian", "Jane",
             "Kevin", "Lily", "Mike", "Nina", "Oscar"]
    subjects = ["Math", "Science", "English", "History", "Geography"]

    for i in range(15):
        roll_no = f"R{100 + i}"
        name = names[i]
        subject = random.choice(subjects)
        rating = random.randint(1, 5)

        table.put_item(Item={
            'StudentRollNo': roll_no,
            'StudentName': name,
            'FavouriteSubject': subject,
            'SubjectRating': rating
        })
    print("Inserted 15 random student records.")

# C. Print Alphabetical Order by Student Name
def print_sorted_names():
    table = dynamodb.Table(table_name)
    response = table.scan()
    sorted_items = sorted(response['Items'], key=lambda x: x['StudentName'])
    print("Students sorted by name:")
    for item in sorted_items:
        print(item['StudentName'])

# D. Count of Ratings = 5
def count_rating_5():
    table = dynamodb.Table(table_name)
    response = table.scan(FilterExpression=Attr('SubjectRating').eq(5))
    print(f"Total students with rating 5: {len(response['Items'])}")

# E. Average Rating per Subject
def avg_rating_per_subject():
    table = dynamodb.Table(table_name)
    response = table.scan()

    subject_ratings = defaultdict(list)
    for item in response['Items']:
        subject_ratings[item['FavouriteSubject']].append(item['SubjectRating'])

    print("Average rating per subject:")
    for subject, ratings in subject_ratings.items():
        avg = sum(ratings) / len(ratings)
        print(f"{subject}: {avg:.2f}")

# F. Update Student Name by Roll No
def update_student_name(roll_no, new_name):
    table = dynamodb.Table(table_name)
    table.update_item(
        Key={'StudentRollNo': roll_no},
        UpdateExpression="SET StudentName = :name",
        ExpressionAttributeValues={':name': new_name}
    )
    print(f"Updated student name for {roll_no} to {new_name}")

# G. Delete Duplicate Names
def delete_duplicates():
    table = dynamodb.Table(table_name)
    response = table.scan()
    seen_names = {}
    for item in response['Items']:
        name = item['StudentName']
        roll_no = item['StudentRollNo']
        if name in seen_names:
            table.delete_item(Key={'StudentRollNo': roll_no})
            print(f"Deleted duplicate student: {roll_no}")
        else:
            seen_names[name] = roll_no

# Main Driver
if __name__ == "__main__":
    create_table()
    insert_random_students()
    print_sorted_names()
    count_rating_5()
    avg_rating_per_subject()
    update_student_name("R100", "UpdatedName")  # You can change RollNo
    delete_duplicates()










import boto3
import random
from boto3.dynamodb.conditions import Attr
from decimal import Decimal

# Initialize DynamoDB
dynamodb = boto3.resource('dynamodb', region_name='eu-north-1')  # Adjust region as needed
table_name = '22bd1a057u_EmployeeDB'

# 1. Create Table
def create_table():
    try:
        table = dynamodb.create_table(
            TableName=table_name,
            KeySchema=[{'AttributeName': 'Employee_No', 'KeyType': 'HASH'}],
            AttributeDefinitions=[{'AttributeName': 'Employee_No', 'AttributeType': 'S'}],
            ProvisionedThroughput={'ReadCapacityUnits': 5, 'WriteCapacityUnits': 5}
        )
        table.wait_until_exists()
        print("✅ Table created successfully.")
    except dynamodb.meta.client.exceptions.ResourceInUseException:
        print("ℹ️ Table already exists.")

# 2. Insert Random 10 Employee Records
def insert_employees():
    table = dynamodb.Table(table_name)
    names = ["Alice", "Bob", "Charlie", "Daisy", "Ethan", "Fiona", "George", "Helen", "Ian", "Jenny"]
    departments = ["HR", "IT", "Finance", "Marketing", "Sales"]

    for i in range(10):
        emp_no = f"E{100 + i}"
        name = names[i]
        dept = random.choice(departments)
        salary = random.randint(10000, 50000)

        table.put_item(Item={
            'Employee_No': emp_no,
            'Employee_Name': name,
            'Employee_Dept': dept,
            'Employee_sal': Decimal(str(salary))
        })

    print("✅ 10 employee records inserted.")

# 3. Print Employees in Alphabetical Order of Name
def print_sorted_employees():
    table = dynamodb.Table(table_name)
    response = table.scan()
    items = response['Items']
    sorted_items = sorted(items, key=lambda x: x['Employee_Name'])

    print("\n📋 Employees sorted by name:")
    for emp in sorted_items:
        print(f"{emp['Employee_Name']} - {emp['Employee_Dept']} - ₹{emp['Employee_sal']}")

# 4. Display Total Salary
def total_salary():
    table = dynamodb.Table(table_name)
    response = table.scan()
    total = sum(emp['Employee_sal'] for emp in response['Items'])

    print(f"\n💰 Total Salary of all employees: ₹{total}")

# 5. Update Salary from 10000 to 10500
def update_salary():
    table = dynamodb.Table(table_name)
    response = table.scan(FilterExpression=Attr('Employee_sal').eq(Decimal(10000)))

    for emp in response['Items']:
        table.update_item(
            Key={'Employee_No': emp['Employee_No']},
            UpdateExpression="SET Employee_sal = :new_sal",
            ExpressionAttributeValues={':new_sal': Decimal(10500)}
        )
        print(f"🔁 Updated salary of {emp['Employee_No']} to ₹10500")

# 6. Delete Employee (Example)
def delete_employee(emp_no):
    table = dynamodb.Table(table_name)
    table.delete_item(Key={'Employee_No': emp_no})
    print(f"🗑️ Deleted employee: {emp_no}")

# ==========================
# Main Driver
# ==========================
if __name__ == "__main__":
    create_table()
    insert_employees()
    print_sorted_employees()
    total_salary()
    update_salary()
    delete_employee("E101")  # Change Employee_No as needed
