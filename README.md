# terraform-aws-cloudfront-s3-website
Forked from "chgangaraju/cloudfront-s3-website/aws", to add more parameters such as CloudFront `lambda_function_association`

Terraform module for creating a s3 static website with cloudfront distribution

The following resources will be created
  
  - S3 Bucket
  - CloudFront distribution
  - Route53 record
  - Upload sample html file (optional)
  
Prerequisites (Optional in example2):

  - Route 53 hosted zone for example.com
  - ACM certificate for *.example.com in us-east-1 region
  
### Example 1

    provider "aws" {
      region = "us-east-1"
    }

    module "cloudfront_s3_website_with_domain" {
        source                 = "oudams/cloudfront-s3-website/aws"
        version                = "1.2.2"
        hosted_zone            = "example.com" 
        domain_name            = "test.abc.example.com"
        acm_certificate_domain = "*.abc.example.com"
        upload_sample_file     = true
    }

### Example 2

    module "cloudfront_s3_website_without_domain" {
        source                 = "oudams/cloudfront-s3-website/aws"
        version                = "1.2.2"
        domain_name            = "test-application-1232" // Any random identifier for s3 bucket name
        use_default_domain     = true
        upload_sample_file     = true
    }

### Example 3

    provider "aws" {
      region = "us-east-1"
    }

    module "cloudfront_s3_with_lambda_function_association" {
        source                 = "oudams/cloudfront-s3-website/aws"
        version                = "1.2.2"
        hosted_zone            = "example.com" 
        domain_name            = "test.abc.example.com"
        acm_certificate_domain = "*.abc.example.com"

        lambda_function_association = {
            event_type : "origin-request"
            include_body : false
            lambda_arn : <lambda-arn:version>
        }
    }
