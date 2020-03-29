# Covidex

Covidex represents an effort to build natural language processing and information retrieval components on top of the [COVID-19 Open Research Dataset (CORD-19)](https://pages.semanticscholar.org/coronavirus-research).

This project contains the API server built with [FastAPI](https://fastapi.tiangolo.com), frontend client built with [React](https://reactjs.org) and [Typescript](https://www.typescriptlang.org/) and neural models for building a multi-stage search pipeline. We use [T5](https://github.com/google-research/text-to-text-transfer-transformer) for reranking results from [Anserini](https://github.com/castorini/anserini) and highlight relevant paragraphs using from the [Hugging Face transformers](https://github.com/huggingface/transformers).

#### Local Deployment

To use GPUs, first install [`nvidia-docker`](https://github.com/NVIDIA/nvidia-docker).

Next set the default Docker runtime to `nvidia` in `/etc/docker/daemon.json`:

```
sudo vim /etc/docker/daemon.json
```

```
{
    "default-runtime": "nvidia"
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
        "runtimeArgs": []
        }
    }
}
```

Restart Docker if it is running:

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

Copy the environment variables and modify as needed:
```
cp .env.sample .env
```

Download the latest Anserini paragraph index for CORD-19'and move it into the `lucene-index-covid-paragraph` folder:
```
INDEX_NAME=lucene-index-covid-paragraph-2020-03-27
INDEX_URL=https://www.dropbox.com/s/o95pehyzem0yalp/lucene-index-covid-paragraph-2020-03-27.tar.gz

rm -rf lucene-index-covid-paragraph
wget ${INDEX_URL}
tar xvfz ${INDEX_NAME}.tar.gz && rm ${INDEX_NAME}.tar.gz
mv ${INDEX_NAME} lucene-index-covid-paragraph
```

Run the containers:
```
docker-compose up -d
```

#### Production Deployment

Run the deployment script:
```
sh deploy-prod.sh
```

*Optional:* set the environment variable `$PORT` to change the server port (defaults to 8081):
```
export PORT=8081
sh deploy-prod.sh
```
