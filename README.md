# Terraform-GitOps
This repo combines Terraform for Infrastructure as Code (IaC) with GitOps for version-controlled, automated deployments. Changes are made via pull requests, reviewed, and applied with CI/CD pipelines. It ensures consistency, auditability, and collaboration across environments.

>> cheeck the master branch
>
>  If using a cloud provider like GCP or Azure:

Google Cloud Platform (GCP) :

sudo apt install google-cloud-sdk -y
gcloud auth login
gcloud config set project <your-project-id>


Update main.tf for GCP :

provider "google" {
  project = "<your-project-id>"
  region  = "us-central1"
}

resource "google_compute_instance" "vm_instance" {
  name         = "terraform-instance"
  machine_type = "e2-micro"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }

  network_interface {
    network = "default"
    access_config {}
  }
}


>>Store credentials in GitHub Secrets:

Go to Settings → Secrets → Actions → New Repository Secret.
Add a secret named GOOGLE_CREDENTIALS with the contents of your GCP JSON key.

- name: Authenticate to GCP
  run: |
    echo "${{ secrets.GOOGLE_CREDENTIALS }}" > gcp-key.json
    gcloud auth activate-service-account --key-file=gcp-key.json




>> Azure

azure cli

curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
az login



> Update main.tf for Azure:
>
> provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}


Store Azure credentials in GitHub Secrets:

Add secrets for ARM_CLIENT_ID, ARM_CLIENT_SECRET, ARM_SUBSCRIPTION_ID, and ARM_TENANT_ID.

- name: Authenticate to Azure
  run: az login --service-principal -u ${{ secrets.ARM_CLIENT_ID }} -p ${{ secrets.ARM_CLIENT_SECRET }} --tenant ${{ secrets.ARM_TENANT_ID }}

