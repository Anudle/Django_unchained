SET UP
1) Apps should have one function. Bunch of apps for one projects.
2) Create project $ django-admin startproject 'myProjectName'
3) Create app $ python manage.py startapp 'MyAppName'
4) run server $ python manage.py runserver

VIEWS
1) in music -> views.py write this code:

***
from django.http import HttpResponse

def index(request):
    return HttpResponse('<h1> This is the music app homepage </h1>')
    ***

2) in Django_unchained -> urls.py write this:

***
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^music/', include('music.urls')),
    url(r'^admin/', admin.site.urls),
]
***

3) In myapp -> urls.py write:

***
from django.conf.urls import url
from . import views

urlpatterns = [
url(r'^$', views.index, name='index'),
]
***

MODELS
1) create a DB in music -> models.py
2) in settings under INSTALLED_APPS put 'music.apps.MusicConfig',
3) command line $python manage.py sqlmakemigrations music
4) $python manage.py migrate
5) Open python shell with $python manage.py shell
6) import db by typing >>>from music.models import Album, Song
7) check db with >>>Album.objects.all()
8) add data >>> a = Album(artist='
 Swift', album_title='Red', genre='Pop', album_logo="logo.png")
9) save data >>>a.save()
10) or >>> b = Album() *** and then >>> b.artist = 'Blink 182'
11) To see the data in string instead of object go to models.py and add     

***

def __str__(self):
        return self.album_title + ' - ' + self.artist

        ***

12) >>>exit() and restart python manage.py shell >>>from music.models import Album, Song
13) >>> Album.objects.all()
14) Look for data with filters>>> Album.objects.filter(artist__startswith='Taylor') >>> Album.objects.filter(id=1)

ADMIN
1)$python manage.py createsuperuser
2) In music -> admin.py add *** from .models import Album ***
3) add *** admin.site.register(Album)
4) refresh admin page

VIEWS 2
1) add this to music-> views.py
***
def detail(request, album_id):
    return HttpResponse('<h2>Details for Album id: ' + str(album_id) + '</h2>' )
    ***
2) and this to music -> urls.py
***
url(r'^(?P<album_id>[0-9]+)/$', views.detail, name='detail'),
]
***

CONNECTING DATABASES
1)in views add
 from models import Album
2)pull all album data
all_albums = Album.objects.all()

ADDING SONGS
1)>>> album1 = Album.objects.get(pk=1)
2) >>> album1.song_set.all()
3) >>> album1.song_set.create(song_title='Shake It Off', file_type='mp3')
