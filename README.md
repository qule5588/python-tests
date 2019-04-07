from requests import get
from datetime import datetime, timedelta
from json import loads
from pprint import pprint
import RPi.GPIO as GPIO

KEY = '0c04e5ddacbc68d164d7af4e9a487982'

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)
GPIO.setup(10, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(12, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(16, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(18, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(22, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(24, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(26, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(3, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)

def get_city_id():
        with open('city.list.json') as f:
            data = loads(f.read())
        city = input('Which is the closest city to the place you are travelling to?' )
        city_id = False
        for item in data:
            if item['name'] == city:
                answer = input('Is this in ' + item['country'])
                if answer == 'y':
                        city_id = item['id']
                        break

        if not city_id:
                print('Sorry, that location is not available')
                exit()
                
        return city_id

def get_weather_data(city_id):
        weather_data = get('http://api.openweathermap.org/data/2.5/forecast?id={}&APPID={}'.format(city_id, KEY))
        return weather_data.json()

def report_timing():
    arrival = datetime.now()
    return arrival

def get_forecast(arrival, weather_data):
    for forecast in weather_data['list']:
        if forecast['dt_txt'] == arrival:
            return forecast

def get_readable_forecast(forecast):
    weather = {}
    weather['temperature'] = float(forecast['main']['temp'])
    return weather

while True:
    if GPIO.input(10) == GPIO.HIGH: #Aspen uses pin input 10
        weather_data = get_weather_data(5412230)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
    elif GPIO.input(12) == GPIO.HIGH: #Copper uses pin input 12
        weather_data = get_weather_data(5422503)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
    elif GPIO.input(16) == GPIO.HIGH: #Eldora uses pin input 16
        weather_data = get_weather_data(5432410)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
    elif GPIO.input(18) == GPIO.HIGH: #Steamboat uses pin input 18
        weather_data = get_weather_data(5582371)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
    elif GPIO.input(22) == GPIO.HIGH: #Vail uses pin input 22
        weather_data = get_weather_data(5319311)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
    elif GPIO.input(24) == GPIO.HIGH: #Telluride  uses pin input 24
        weather_data = get_weather_data(5441199)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
    elif GPIO.input(26) == GPIO.HIGH: #Breckenridge uses pin input 26
        weather_data = get_weather_data(4676181)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
    elif GPIO.input(3) == GPIO.HIGH: #Winter park uses pin input 3
        weather_data = get_weather_data(4178560)
        arrival = report_timing()
        forecast = get_forecast(arrival, weather_data)
        get_readable_forecast(forecast)
        break
        
        

