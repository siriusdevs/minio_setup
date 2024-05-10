```
docker run -d -p 9000:9000 -p 9001:9001 -e "MINIO_ROOT_USER=user" -e "MINIO_ROOT_PASSWORD=password" minio/minio server /data --console-address ":9001"
```
```
python3 -m pip install django-storages boto3 django-minio-backend
```
```
python3 manage.py collectstatic
```
```
STATICFILES_STORAGE = 'storages.backends.s3boto3.S3StaticStorage'

if DEBUG:
    AWS_ACCESS_KEY_ID = getenv('MINIO_ACCESS_KEY_ID')
    AWS_SECRET_ACCESS_KEY = getenv('MINIO_SECRET_ACCESS_KEY')
    AWS_STORAGE_BUCKET_NAME = getenv('MINIO_STORAGE_BUCKET_NAME')
    AWS_S3_ENDPOINT_URL = getenv('MINIO_API')
    AWS_S3_USE_SSL = False

MINIO_ENDPOINT = 'localhost:9000'
MINIO_ACCESS_KEY = getenv('MINIO_ACCESS_KEY_ID')
MINIO_SECRET_KEY = getenv('MINIO_SECRET_ACCESS_KEY')
MINIO_USE_HTTPS = False
MINIO_CONSISTENCY_CHECK_ON_START = True
MINIO_PRIVATE_BUCKETS = []
MINIO_PUBLIC_BUCKETS = [
    'static',
]

INSTALLED_APPS = [
    ...
    'storages',
    'django_minio_backend',
]
```

```
from django_minio_backend import MinioBackend, iso_date_prefix

    file = models.FileField(
        null=True, blank=True, 
        storage=MinioBackend(bucket_name='static'),
        upload_to=iso_date_prefix
    )
```
