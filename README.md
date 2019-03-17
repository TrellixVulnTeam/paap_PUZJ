# Predicting Art Auction Prices

This project satisfies the requirements for Montana State University course STAT 575. The project seeks to predict art prices using a large dataset of historical and publicly available auction prices. The dataset includes a variety of predictor types from sale metadata to artwork characteristics.

## Setup

The data were scraped from an auction house website. This project used Google Cloud Platform infrastructure, including Compute Engine, Storage, and BigQuery services. To replicate the data acquisition, you will need to perform the following:

* Create a python3 virtual environment and install `requirements.txt`.
* Build a docker image based on the `Dockerfile`, tag it, and upload it to Cloud Repository

```bash
# Set up google registry
gcloud auth login
gcloud auth configure docker
docker build -t us.gcr.io/art-auction-prices/paap .
docker push us.gcr.io/art-auction-prices/paap
```

* Create a virtual machine

```bash
# List VMs
gcloud compute instances list
# Deploy VM
gcloud compute instances create-with-container paap-1 \
    --container-image=us.gcr.io/art-auction-prices/paap \
    --container-restart-policy "never" \
    --machine-type=n1-standard-1 \
    --scopes=storage-rw,logging-write
# SSH into VM
gcloud compute --project art-auction-prices ssh paap-1
# stop VM
gcloud compute --project art-auction-prices instances stop paap-1
# delete VM
gcloud compute instances delete --zone us-central1-c paap-1
```

* Verify that the container is running.

```bash
gcloud compute ssh paap-1 --command "docker container ps -a"
```

## Analysis

The data analysis is outlined in an Rmarkdown document, `analysis.Rmd`.

