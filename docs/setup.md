## Installation

Install with pip:

```shell
pip install wagtail-localize
```

## Settings modifications

Add `wagtail_localize` and any optional sub modules to `INSTALLED_APPS` in `settings/base.py`:

```python
INSTALLED_APPS = [
    ...
    "wagtail_localize",
    "wagtail_localize.translation",
]
```

Add the following to `MIDDLEWARE`:

```python
"django.middleware.locale.LocaleMiddleware",
```

Ensure your settings file has:

```python
LANGUAGE_CODE = "en-gb"  # Or your preferred default language
USE_I18N = True
```

Add to following to your settings specifying any languages you would like to translate:

```python
LANGUAGES = [
    ("en", "English"),
    ("fr", "French"),
]

WAGTAIL_CONTENT_LANGUAGES = LANGUAGES
```

### URL configuration

The following additions need to be made to `./yoursite/urls.py`

```python
from django.conf.urls.i18n import i18n_patterns

# Non-translatable URLs
# Note: if you are using the Wagtail API or sitemaps,
# these should not be added to `i18n_patterns` either
urlpatterns = [
    path('django-admin/', admin.site.urls),

    path('admin/', include(wagtailadmin_urls)),
    path('documents/', include(wagtaildocs_urls)),
]

# Translatable URLs
# These will be available under a language code prefix. For example /en/search/
urlpatterns += i18n_patterns(
    path('search/', search_views.search, name='search'),
    path("", include(wagtail_urls)),
)
```
