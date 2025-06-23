<h1>🐳 Flask powered by Docker </h1> 

🔄 Flask is a popular micro web framework written in Python. <br>
🔄 Simplicity and Ease of Use: It has a gentle learning curve and is quick to get started with, even for beginners. <br>
🔄 Great for APIs and Microservices <br>
🔄 Flexibility & Lightweight <br>

![flask](https://github.com/user-attachments/assets/00a348a0-a78e-4661-96f9-596fcea6ad5c)

# 🧮 Flask-Calculator-App-Dockerized
Perform +, −, ×, ÷ operations <br>
Flask web interface <br>
Docker and Docker Compose setup <br>

![flask](https://github.com/user-attachments/assets/c906786f-8bfb-47d4-ab58-ca43a8698e98)
Structure
```
flask-calculator/
├── app/
│   ├── __init__.py
│   ├── routes.py
│   ├── templates/
│   │   └── calculator.html
│   └── static/
│       └── style.css
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
└── run.py
```
app/init.py
```
from flask import Flask

def create_app():
    app = Flask(__name__)
    from app.routes import main
    app.register_blueprint(main)
    return app
```
app/routes.py
```
from flask import Blueprint, render_template, request

main = Blueprint('main', __name__)

@main.route("/", methods=["GET", "POST"])
def calculator():
    result = None
    error = None
    if request.method == "POST":
        try:
            num1 = float(request.form['num1'])
            num2 = float(request.form['num2'])
            operation = request.form['operation']

            if operation == 'add':
                result = num1 + num2
            elif operation == 'subtract':
                result = num1 - num2
            elif operation == 'multiply':
                result = num1 * num2
            elif operation == 'divide':
                if num2 != 0:
                    result = num1 / num2
                else:
                    error = "Division by zero."
        except ValueError:
            error = "Invalid input."

    return render_template("calculator.html", result=result, error=error)
```
app/templates/calculator.html
```
<!DOCTYPE html>
<html>
<head>
    <title>Calculator</title>
</head>
<body>
    <h1>Flask Calculator</h1>
    <form method="post">
        <input name="num1" placeholder="First number">
        <input name="num2" placeholder="Second number">
        <select name="operation">
            <option value="add">+</option>
            <option value="subtract">−</option>
            <option value="multiply">×</option>
            <option value="divide">÷</option>
        </select>
        <button type="submit">Calculate</button>
    </form>
    {% if result is not none %}
        <p>Result: {{ result }}</p>
    {% elif error %}
        <p style="color:red">{{ error }}</p>
    {% endif %}
</body>
</html>
```
run.py
```
from app import create_app

app = create_app()

if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0")
```
requirements.txt
```
Flask==3.0.0
```
Dockerfile
```
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["python", "run.py"]
```
docker-compose.yml
```
version: '3.8'

services:
  web:
    build: .
    ports:
      - "5000:5000"
```
![flask_2](https://github.com/user-attachments/assets/2243d08d-b0d8-419f-b762-23741b926afb)


```
docker-compose build
docker-compose up
```

Open http://localhost:5000



