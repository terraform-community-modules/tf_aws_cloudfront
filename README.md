# tf_aws_cloudfront
A Terraform module containing typical AWS CloudFront distribution.

## Assumptions
* HTTPS traffic (so CF will offload HTTPS to HTTP)
* You've uploaded an SSL certificate to AWS/IAM
* Logging is created by default to separate S3 bucket

## Input Variables
* `name` - [mandatory] name you will see in i.e. in tags.
* `certificate_arn` - [mandatory] Existing certificate arn.
* `domains` - list of CNAME's to be associated with the CF (can be empty).
* `bucket_name` - S3 bucket name to be source for data served by CF.
* `compress` - Whether you want CloudFront to automatically compress content for web requests that include Accept-Encoding: gzip in the request header.
* `ipv6_enabled` - Whether the IPv6 is enabled for the distribution.
* `comment` - Comment used in both `aws_cloudfront_origin_access_identity` and `aws_cloudfront_distribution`.
* `log_include_cookies` - Specifies whether you want CloudFront to include cookies in access logs.
* `log_bucket` - Name of the bucket to place CF access logs.
* `log_prefix` - An optional string that you want CloudFront to prefix to the access log filenames for this distribution, for example, `cf_logs/.`.
* `price_class` - The price class for this distribution. One of PriceClass_All, PriceClass_200, PriceClass_100 (default).
* `viewer_protocol_policy` - Use this element to specify the protocol that users can use to access the files in the origin specified by TargetOriginId when a request matches the path pattern in PathPattern. One of allow-all, https-only, or redirect-to-https (default).
* `allowed_methods` - Controls which HTTP methods CloudFront processes and forwards to your Amazon S3 bucket or your custom origin.
* `cached_methods` - Controls whether CloudFront caches the response to requests using the specified HTTP methods.
* `min_ttl` - The minimum amount of time that you want objects to stay in CloudFront caches before CloudFront queries your origin to see whether the object has been updated.
* `max_ttl` - The maximum amount of time (in seconds) that an object is in a CloudFront cache before CloudFront forwards another request to your origin to determine whether the object has been updated. Only effective in the presence of Cache-Control max-age, Cache-Control s-maxage, and Expires headers.
* `default_ttl` - The default amount of time (in seconds) that an object is in a CloudFront cache before CloudFront forwards another request in the absence of an Cache-Control max-age or Expires header.
* `tags` - A mapping of tags to assign to the resource. By default `Name` is set.

## Outputs
* cf_id - The identifier for the distribution. For example: EDFDVBD632BHDS5.
* cf_arn - The ARN (Amazon Resource Name) for the distribution. For example: arn:aws:cloudfront::123456789012:distribution/EDFDVBD632BHDS5, where 123456789012 is your AWS account ID.
* cf_status - The current status of the distribution. Deployed if the distribution's information is fully propagated throughout the Amazon CloudFront system.
* active_trusted_signers - The key pair IDs that CloudFront is aware of for each trusted signer, if the distribution is set up to serve private content with signed URLs.
* cf_domain_name - The domain name corresponding to the distribution. For example: d604721fxaaqy9.cloudfront.net.
* cf_etag - The current version of the distribution's information. For example: E2QWRUHAPOMQZL.
* cf_hosted_zone_id - The CloudFront Route 53 zone ID that can be used to route an Alias Resource Record Set to. This attribute is simply an alias for the zone ID Z2FDTNDATAQYW2.
* s3_bucket_id - S3 bucket id
* s3_bucket_arn - S3 bucket arn

## Usage example:
```
module "cf_distro" {
  source = "github.com/terraform-community-modules/tf_aws_cf"
  name = "tf-${terraform.env}-my-cf"
  domains = ["whatever-${terraform.env}.${var.domain}"]
  certificate_arn = "arn:aws:acm:us-east-1:123456789123:certificate/11111111-2222-3333-4444-555555555555"

  bucket_name = "tf-${terraform.env}-my-cf-bucket"
  comment = "Managed by Terraform"

  log_include_cookies = "false"
  log_bucket          = "tf-${terraform.env}-log-bucket-for-my-cf"

  allowed_methods = ["GET", "HEAD"]

  tags = {
    "Terraform" = "true"
    "Environment" = "${terraform.env}"
  }
}
```

## Contributing
Report issues/questions/feature requests on in the [Issues](https://github.com/terraform-community-modules/tf_aws_cf/issues) section.

Pull requests are welcome! Ideally create a feature branch and issue for every
individual change you make. These are the steps:

1. Fork the repo.
2. Create your feature branch from master (`git checkout -b my-new-feature`).
4. Commit your awesome changes (`git commit -am 'Added some feature'`).
5. Push to the branch (`git push origin my-new-feature`).
6. Create a new Pull Request and tell us about your changes.

## Change log
For description of changes please refer to commit descriptions

## Authors
Created and maintained by [Marek Kwasecki](https://github.com/kwach) - marek.kwasecki@radpoint.pl.

## License
Apache v2.0 Licensed. See [LICENSE](LICENSE) for full details.

