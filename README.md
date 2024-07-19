# Contact App - Backend

## Descripción

Contact App es una aplicación web para gestionar contactos, desarrollada con un backend en Django y un frontend en Vue.js. La aplicación permite a los usuarios agregar, editar, ver y eliminar contactos.

Para crear un backend en Django para tu proyecto de contactos con una base de datos MySQL, sigue estos pasos:
## Configuración del Backend (Django)

### 1. Configuración del Entorno
1. **Instala Django y mysqlclient**:
    ```bash
    pip install django mysqlclient
    ```

2. **Crea un proyecto Django**:
    ```bash
    django-admin startproject contacts_backend
    cd contacts_backend
    ```

3. **Configura la base de datos MySQL** en `settings.py`:
    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',
            'NAME': 'contacts_db',
            'USER': 'your_mysql_user',
            'PASSWORD': 'your_mysql_password',
            'HOST': 'localhost',
            'PORT': '3306',
        }
    }
    ```

### 2. Crear la Aplicación de Contactos
1. **Crea una aplicación llamada `contacts`**:
    ```bash
    python manage.py startapp contacts
    ```

2. **Añade la aplicación `contacts` a `INSTALLED_APPS`** en `settings.py`:
    ```python
    INSTALLED_APPS = [
        ...
        'contacts',
    ]
    ```

### 3. Definir el Modelo de Contacto
En `contacts/models.py`:

```python
from django.db import models

class Contact(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    phone = models.CharField(max_length=15, blank=True)
    address = models.TextField(blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return f"{self.first_name} {self.last_name}"
```

### 4. Crear y Aplicar Migraciones
1. **Crea las migraciones**:
    ```bash
    python manage.py makemigrations
    ```

2. **Aplica las migraciones**:
    ```bash
    python manage.py migrate
    ```

### 5. Crear una API con Django REST Framework
1. **Instala Django REST Framework**:
    ```bash
    pip install djangorestframework
    ```

2. **Añade `rest_framework` a `INSTALLED_APPS`** en `settings.py`:
    ```python
    INSTALLED_APPS = [
        ...
        'rest_framework',
    ]
    ```

3. **Crear un Serializador para el Modelo Contact** en `contacts/serializers.py`:
    ```python
    from rest_framework import serializers
    from .models import Contact

    class ContactSerializer(serializers.ModelSerializer):
        class Meta:
            model = Contact
            fields = '__all__'
    ```

4. **Crear las Vistas para la API** en `contacts/views.py`:
    ```python
    from rest_framework import viewsets
    from .models import Contact
    from .serializers import ContactSerializer

    class ContactViewSet(viewsets.ModelViewSet):
        queryset = Contact.objects.all()
        serializer_class = ContactSerializer
    ```

5. **Configurar las URLs de la API** en `contacts/urls.py`:
    ```python
    from django.urls import path, include
    from rest_framework.routers import DefaultRouter
    from .views import ContactViewSet

    router = DefaultRouter()
    router.register(r'contacts', ContactViewSet)

    urlpatterns = [
        path('', include(router.urls)),
    ]
    ```

6. **Incluir las URLs de `contacts` en el proyecto principal** en `contacts_backend/urls.py`:
    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/', include('contacts.urls')),
    ]
    ```

### 6. Configuración Final
1. **Crea un superusuario para acceder al administrador de Django**:
    ```bash
    python manage.py createsuperuser
    ```

2. **Ejecuta el servidor**:
    ```bash
    python manage.py runserver
    ```
El backend en Django con una base de datos MySQL está listo y puedes interactuar con él a través de la API REST.

Configuración de `django-cors-headers` esté correcta y completamente configurada.

### Pasos detallados para configurar CORS en Django:

1. **Instalar `django-cors-headers`:**

   ```bash
   pip install django-cors-headers
   ```

2. **Agregar `corsheaders` a tu archivo `settings.py`:**

   Abre tu archivo `settings.py` y realiza los siguientes cambios:

   a. **Agregar `corsheaders` a `INSTALLED_APPS`:**

   ```python
   INSTALLED_APPS = [
       ...
       'corsheaders',
       ...
   ]
   ```

   b. **Agregar `CorsMiddleware` a `MIDDLEWARE`:**

   Asegúrate de agregarlo antes de `CommonMiddleware`:

   ```python
   MIDDLEWARE = [
       ...
       'corsheaders.middleware.CorsMiddleware',
       'django.middleware.common.CommonMiddleware',
       ...
   ]
   ```

   c. **Configurar CORS en el archivo `settings.py`:**

   Permitir solicitudes desde `http://localhost:8080`:

   ```python
   CORS_ALLOWED_ORIGINS = [
       "http://localhost:8080",
   ]
   ```

   O permitir todas las solicitudes (solo para desarrollo):

   ```python
   CORS_ALLOW_ALL_ORIGINS = True
   ```

3. **Reiniciar el servidor Django:**

   ```bash
   python manage.py runserver
   ```

### Verificar configuración de Axios

Asegúrate de que tu configuración de Axios esté correctamente establecida en tu frontend:

```javascript
import axios from 'axios';

// Configurar la URL base de Axios
axios.defaults.baseURL = 'http://127.0.0.1:8000/api/contacts/';
```

### Verificación de la API en el navegador

1. **Verificar manualmente la API en el navegador:**

   Abre tu navegador y visita `http://127.0.0.1:8000/api/contacts/`. Asegúrate de que la API esté respondiendo correctamente y que la cabecera `Access-Control-Allow-Origin` esté presente en la respuesta.

### Código Completo de `settings.py`

Aquí tienes un ejemplo completo de cómo debería verse tu `settings.py` con las configuraciones de CORS:

```python
# settings.py

# ...

INSTALLED_APPS = [
    ...
    'corsheaders',
    ...
]

MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
]

CORS_ALLOWED_ORIGINS = [
    "http://localhost:8080",
]

# O para permitir todas las solicitudes (solo para desarrollo)
CORS_ALLOW_ALL_ORIGINS = True

# ...
```

## Licencia

Este proyecto está licenciado bajo la Licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.

## Contacto

Para cualquier consulta o problema, puedes contactarme a través de [shalomsolutiontech@gmail.com](mailto:shalomsolutiontech@gmail.com).
