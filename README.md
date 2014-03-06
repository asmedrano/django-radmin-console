# Radmin the totally rad django admin console.


##UPDATE:
I recommend you use https://docs.djangoproject.com/en/dev/ref/contrib/admin/actions/
Instead of this package! This is a silly experiment.

## Install & Config
Make sure the ```django.template.loaders.app_directories.Loader``` is enabled

Ex:
```
TEMPLATE_LOADERS = (
	'django.template.loaders.filesystem.Loader',
	'django.template.loaders.app_directories.Loader',
)
```

Add radmin to your installed apps. Make sure you place it before the django admin

Ex:
```
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'radmin', # !!! This needs to come before django.contrib.admin
    'django.contrib.admin',
    'myapp',
)
```

Add radmin urls to you projects urls.py

Ex:

```
urlpatterns = patterns('',
    url(r'^radmin/', include('radmin.urls')), #NEED
    url(r'^admin/', include(admin.site.urls)),
)
```

## Usage Examples
Register radmin items in your apps admin.py 

Ex:
```
from myapp.models import Thing
from radmin import console

admin.site.register(Thing)

# ----------------------------------------------radmin starts here-------------------------------------
# shows up everwhere
console.register_to_all('Everywhere Action', 'myapp.views.current_datetime', True) # True means show the return value of the method called.

# shows up only at /admin
console.register_to_admin_index('Admin Index Action', 'myapp.views.current_datetime', True) 

#shows up at app index /admin/app
console.register_to_app('MyApp', 'App level action', 'myapp.views.current_datetime') 

#shows up when you are at the model list eg /admin/app/model
console.register_to_model_list(Thing, 'Model List Level Action','myapp.views.current_datetime') 

# shows up at single model /admin/app/model/id. The call back should expect to recieve the model id back
console.register_to_model(Thing, 'Model Level Action', 'myapp.views.get_current_model_name') 
```
## Sample Callbacks
Call backs should return something. Here are some examples of callbacks I use.
```
154 def clear_memcache():
155     try:
156         import memcache
157         mc = memcache.Client(['127.0.0.1:11211'], debug=0)
158         mc.flush_all()
159         return "Memcache cleared"
160     except ImportError:
161         return "Failed: python-memcache not installed"
162 
```

## Screenshots
Click the link to open it up
![ScreenShot](https://lh3.googleusercontent.com/-lg5zbvmkYTM/UJBN4OzDqxI/AAAAAAAAAoI/mti49XMzvRg/s568/Screenshot+from+2012-10-30+14%3A41%3A12.png)

Return values are displayed in console
![ScreenShot](https://lh6.googleusercontent.com/-maNSz8KqMIE/UJBN4Knef5I/AAAAAAAAAoE/s-GM-sbZEcM/s496/Screenshot+from+2012-10-30+14%3A42%3A44.png)
