from flask import Flask,jsonify,request
app = Flask(__name__)

from statistics import mean

from pymongo.mongo_client import MongoClient
from pymongo.server_api import ServerApi

uri = "mongodb+srv://robbywm22:bfLbUAhrAbtBJhHR@cluster0.t4bfw.mongodb.net/?appName=Cluster0"

# Create a new client and connect to the server
client = MongoClient(uri, server_api=ServerApi('1'))

#Bikin database
db = client['MyDatabase']
my_collections = db['SensorData']

def store_data(data):
    results = my_collections.insert_one(data)
    return results.inserted_id

def get_data():
    get_result = my_collections.find()
    return get_result

@app.route('/',methods = ['POST', 'GET'])
def entry_point():
    return jsonify(message="Hello World")

@app.route('/sensor1',methods = ['POST', 'GET'])
def data_sensor():
    if request.method == 'POST':
        body = request.get_json()
        temperature = body['temperature']
        humidity = body['humidity']
        timestamp = body['timestamp']
        data_final = {
            "temperature":temperature,
            "humidity":humidity,
            "timestamp":timestamp
        }

        id = store_data(data_final)
        return{
            "message":f"Hello, i have processed your request with id {id}"
        }
    elif request.method == 'GET':
        result = get_data()
        temp_list = []
        for x in result:
            temp_list.append(x['temperature'])
            avg = mean(temp_list)
            return jsonify(message="Succesfully get sensor data!",average_temperature=avg)

if __name__ == '__main__':
    app.run(debug=True)    
