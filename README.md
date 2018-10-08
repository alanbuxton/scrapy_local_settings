# Example scrapy app supporting local config settings files.

I've been working on a project using scrapy to store scraped content into a local database and wanted to be able to run it in test mode sometimes (with a test database) and in dev mode other times (using a dev database). I'm used to working with Ruby on Rails that makes this approach very easy, so was surprised that there didn't seem to be an obvious way to do this in the python/scrapy world.

To support this, I took some inspiration from Rails. The solution was to move the ```settings.py``` file to a config directory and introduce ```settings_<envname>.py```, ```secrets.py``` and ```secrets_<envname>.py``` files. These files are used as follows:

- ```settings.py```: General settings that apply to all environments (e.g. scrapy middleware configurations)
- ```settings_<envname>.py```: Settings specific to an environment (e.g. log level)
- ```secrets.py```: Any special secrets for this local machine (e.g. some API keys)
- ```secrets_<envname>.py```: Secrets for this environment on this machine (e.g database connection string)

The ```.gitignore``` only includs the ```settings.py``` file, as this is the only file that should be shared across all instances of the application. All other files should be created locally.

To test this repo locally, just clone the repo, remove the ```.sample``` suffix from the sample files in the config dir, and compare the output of:

```$ SCRAPY_ENV=dev scrapy crawl quotes```

with

```$ SCRAPY_ENV=test scrapy crawl quotes```

Feels a bit hacky, but it works :smile:
