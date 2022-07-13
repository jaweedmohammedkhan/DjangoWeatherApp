#first step
django-admin startproject weather

cd weather

#second step

python manage.py runserver 
#above command is to check whether django is running or not 

ctrl-c

#third step
python manage.py startapp main
#above step is used to create a app named as main in the project 


cd main
and create a folder with index.html file: templates/main/index.html
Now add main app in settings.py

#step five
Edit urls.py file in weather :
from django.contrib import admin
from django.urls import path, include


urlpatterns = [
	path('admin/', admin.site.urls),
	path('', include('main.urls')),
]

edit urls.py file in main :

from django.urls import path
from . import views

urlpatterns = [
		path('', views.index),
]


edit views.py in main :

from django.shortcuts import render
# import json to load json data to python dictionary
import json
# urllib.request to make a request to api
import urllib.request


def index(request):
	if request.method == 'POST':
		city = request.POST['city']
		''' api key might be expired use your own api_key
			place api_key in place of appid ="your_api_key_here " '''

		# source contain JSON data from API

		source = urllib.request.urlopen(
			'http://api.openweathermap.org/data/2.5/weather?q ='
					+ city + '&appid = your_api_key_here').read()

		# converting JSON data to a dictionary
		list_of_data = json.loads(source)

		# data for variable list_of_data
		data = {
			"country_code": str(list_of_data['sys']['country']),
			"coordinate": str(list_of_data['coord']['lon']) + ' '
						+ str(list_of_data['coord']['lat']),
			"temp": str(list_of_data['main']['temp']) + 'k',
			"pressure": str(list_of_data['main']['pressure']),
			"humidity": str(list_of_data['main']['humidity']),
		}
		print(data)
	else:
		data ={}
	return render(request, "main/index.html", data)


You can get your own API key from : Weather API

Navigate to templates/main/index.html and check the html code and what you like to call from api 

python manage.py runserver