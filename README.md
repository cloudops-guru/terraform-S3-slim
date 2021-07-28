# terraform-S3

# Overview
- Create S3 bucket
- Optional, set encryption, logging, versioning, lifecycle policies

# Usage

````terraform
terraform {
  backend "s3" {
    encrypt = true
  }
}

provider "aws" {
  region = var.region
  default_tags {
   tags = {
     Environment = var.env
     CreatedBy   = "Terraform"
     Project     = var.project
   }
 }
}
 

module "S3" {
  source        = "git@github.levi-site.com:LSCO/terraform-S3.git?ref=<TAG>"
  env           = var.env
  project       = var.project
  region        = var.region
  bucket        = var.bucket
  acl           = var.acl
  force_destroy = true 

  versioning = {
    enabled = true
  }

  website = {
    index_document = "index.html"
  }

  logging = {
    target_bucket = var.logging_bucket
    target_prefix = "${var.project}-${var.bucket}-${var.env}"
  }

  server_side_encryption_configuration = {
    rule = {
      apply_server_side_encryption_by_default = {
        sse_algorithm     = "AES256"
      }
    }
  }
}

````