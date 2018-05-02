# newsleak-docker

Docker configuration to run new/s/leak

# Usage

Install docker.

Checkout this repository.

```
git clone https://github.com/tudarmstadt-lt/newsleak-docker.git
cd newsleak-docker
``` 

Run docker-compose. You may want to edit the `postgres.env` file to setup your own db password.

```
docker-compose up -d
```

Setup configuration. Edit `volumes/ui/newsleak.properties`. First, copy it to the volume location `./volumes/ui`

```
docker exec -it newsleakdocker_newsleak-ui_1 cp -r /opt/newsleak/preprocessing/conf /etc/settings
```

Then, edit the file with your favorite text editor. 

```
nano volumes/ui/newsleak.properties
```

You may use the example data or copy your own data files into the `volumes/ui` folder and point to them in the properties file. If you changed the db password in the previous step, change it in the properties file, too.

Optional languages: The default NER service contains models for English and German. To install models for additional languages, run the following command. Adapt the language code of the model files to the language your need (ISO 639-1).

```
docker exec -t newsleakdocker_newsleak-ner_1 polyglot download embeddings2.es ner2.es
```


Finally, run preprocessing for information extraction.

```
docker exec -it newsleakdocker_newsleak-ui_1 sh -c "cd /opt/newsleak/preprocessing && java -Xmx10g -jar target/preprocessing-0.5-jar-with-dependencies.jar -c /etc/settings/newsleak.properties"
```

Open the UI application in your browser

```
http://localhost:9000
```
