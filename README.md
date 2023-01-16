# Restful-APIs
Create Best Restful APIs for sign in and sign up using any token authentication  Sinup Inputs  1.Profile 2.Username 3.Password 4.Confirm password
from flask import Flask, request, jsonify
from werkzeug.security import generate_password_hash, check_password_hash
import jwt

app = Flask(__name__)
app.secret_key = 'my_secret_key'

@app.route('/signup', methods=['POST'])
def signup():
    profile = request.json.get('profile')
    username = request.json.get('username')
    password = request.json.get('password')
    confirm_password = request.json.get('confirm_password')
    if password != confirm_password:
        return jsonify({'error': 'Passwords do not match'}), 400
    hashed_password = generate_password_hash(password)
    # Save the user to the database
    # ...
    token = jwt.encode({'username': username}, app.secret_key, algorithm='HS256')
    return jsonify({'token': token.decode('utf-8'), 'profile': profile, 'username': username}), 201
