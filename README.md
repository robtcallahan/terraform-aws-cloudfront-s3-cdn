<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->
[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

# terraform-aws-cloudfront-s3-cdn [![Build Status](https://travis-ci.org/cloudposse/terraform-aws-cloudfront-s3-cdn.svg?branch=master)](https://travis-ci.org/cloudposse/terraform-aws-cloudfront-s3-cdn) [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-cloudfront-s3-cdn.svg)](https://travis-ci.org/cloudposse/terraform-aws-cloudfront-s3-cdn/releases) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Terraform module to provision an AWS CloudFront CDN with an S3 origin.


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps. 
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]


[![Terraform Open Source Modules](https://docs.cloudposse.com/images/terraform-open-source-modules.svg)][terraform_modules]



It's 100% Open Source and licensed under the [APACHE2](LICENSE).







We literally have [*hundreds of terraform modules*][terraform_modules] that are Open Source and well-maintained. Check them out! 







## Usage

```hcl
module "cdn" {
  source           = "git::https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn.git?ref=master"
  namespace        = "eg"
  stage            = "prod"
  name             = "app"
  aliases          = ["assets.cloudposse.com"]
  parent_zone_name = "cloudposse.com"
}
```

Full working example can be found in [example](./example) folder.

### Generating ACM Certificate

Use the AWS cli to [request new ACM certifiates](http://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request.html) (requires email validation)
```
aws acm request-certificate --domain-name example.com --subject-alternative-names a.example.com b.example.com *.c.example.com
```



__NOTE__:

Although AWS Certificate Manager is supported in many AWS regions, to use an SSL certificate with CloudFront, it should be requested only in US East (N. Virginia) region.

https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cnames-and-https-requirements.html
> If you want to require HTTPS between viewers and CloudFront, you must change the AWS region to US East (N. Virginia) in the AWS Certificate Manager console before you request or import a certificate.

https://docs.aws.amazon.com/acm/latest/userguide/acm-regions.html
> To use an ACM Certificate with Amazon CloudFront, you must request or import the certificate in the US East (N. Virginia) region. ACM Certificates in this region that are associated with a CloudFront distribution are distributed to all the geographic locations configured for that distribution.

This is a fundamental requirement of CloudFront, and you will need to request the certificate in `us-east-1` region.

If there are warnings around the outputs when destroying using this module.
Then you can use this method for supressing the superfluous errors.
`TF_WARN_OUTPUT_ERRORS=1 terraform destroy`






## Makefile Targets
```
Available targets:

  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  lint                                Lint terraform code

```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| acm_certificate_arn | Existing ACM Certificate ARN | string | `` | no |
| aliases | List of FQDN's - Used to set the Alternate Domain Names (CNAMEs) setting on Cloudfront | list | `<list>` | no |
| allowed_methods | List of allowed methods (e.g. GET, PUT, POST, DELETE, HEAD) for AWS CloudFront | list | `<list>` | no |
| attributes | Additional attributes (e.g. `1`) | list | `<list>` | no |
| bucket_domain_format | Format of bucket domain name | string | `%s.s3.amazonaws.com` | no |
| cached_methods | List of cached methods (e.g. GET, PUT, POST, DELETE, HEAD) | list | `<list>` | no |
| comment | Comment for the origin access identity | string | `Managed by Terraform` | no |
| compress | Compress content for web requests that include Accept-Encoding: gzip in the request header | string | `false` | no |
| cors_allowed_headers | List of allowed headers for S3 bucket | list | `<list>` | no |
| cors_allowed_methods | List of allowed methods (e.g. GET, PUT, POST, DELETE, HEAD) for S3 bucket | list | `<list>` | no |
| cors_allowed_origins | List of allowed origins (e.g. example.com, test.com) for S3 bucket | list | `<list>` | no |
| cors_expose_headers | List of expose header in the response for S3 bucket | list | `<list>` | no |
| cors_max_age_seconds | Time in seconds that browser can cache the response for S3 bucket | string | `3600` | no |
| custom_error_response | List of one or more custom error response element maps | list | `<list>` | no |
| default_root_object | Object that CloudFront return when requests the root URL | string | `index.html` | no |
| default_ttl | Default amount of time (in seconds) that an object is in a CloudFront cache | string | `60` | no |
| delimiter | Delimiter to be used between `namespace`, `stage`, `name` and `attributes` | string | `-` | no |
| enabled | Select Enabled if you want CloudFront to begin processing requests as soon as the distribution is created, or select Disabled if you do not want CloudFront to begin processing requests after the distribution is created. | string | `true` | no |
| forward_cookies | Time in seconds that browser can cache the response for S3 bucket | string | `none` | no |
| forward_header_values | A list of whitelisted header values to forward to the origin | list | `<list>` | no |
| forward_query_string | Forward query strings to the origin that is associated with this cache behavior | string | `false` | no |
| geo_restriction_locations | List of country codes for which  CloudFront either to distribute content (whitelist) or not distribute your content (blacklist) | list | `<list>` | no |
| geo_restriction_type | Method that use to restrict distribution of your content by country: `none`, `whitelist`, or `blacklist` | string | `none` | no |
| is_ipv6_enabled | State of CloudFront IPv6 | string | `true` | no |
| log_expiration_days | Number of days after which to expunge the objects | string | `90` | no |
| log_glacier_transition_days | Number of days after which to move the data to the glacier storage tier | string | `60` | no |
| log_include_cookies | Include cookies in access logs | string | `false` | no |
| log_prefix | Path of logs in S3 bucket | string | `` | no |
| log_standard_transition_days | Number of days to persist in the standard storage tier before moving to the glacier tier | string | `30` | no |
| max_ttl | Maximum amount of time (in seconds) that an object is in a CloudFront cache | string | `31536000` | no |
| min_ttl | Minimum amount of time that you want objects to stay in CloudFront caches | string | `0` | no |
| minimum_protocol_version | Cloudfront TLS minimum protocol version | string | `TLSv1` | no |
| name | Name  (e.g. `bastion` or `app`) | string | - | yes |
| namespace | Namespace (e.g. `eg` or `cp`) | string | - | yes |
| null | an empty string | string | `` | no |
| origin_bucket | Name of S3 bucket | string | `` | no |
| origin_force_destroy | Delete all objects from the bucket  so that the bucket can be destroyed without error (e.g. `true` or `false`) | string | `false` | no |
| origin_path | An optional element that causes CloudFront to request your content from a directory in your Amazon S3 bucket or your custom origin. It must begin with a /. Do not add a / at the end of the path. | string | `` | no |
| parent_zone_id | ID of the hosted zone to contain this record  (or specify `parent_zone_name`) | string | `` | no |
| parent_zone_name | Name of the hosted zone to contain this record (or specify `parent_zone_id`) | string | `` | no |
| price_class | Price class for this distribution: `PriceClass_All`, `PriceClass_200`, `PriceClass_100` | string | `PriceClass_100` | no |
| stage | Stage (e.g. `prod`, `dev`, `staging`) | string | - | yes |
| static_s3_bucket | aws-cli is a bucket owned by amazon that will perminantly exist. It allows for the data source to be called during the destruction process without failing. It doesn't get used for anything else, this is a safe workaround for handling the fact that if a data source like the one `aws_s3_bucket.selected` gets an error, you can't continue the terraform process which also includes the 'destroy' command, where is doesn't even need this data source! Don't change this bucket name, it's a variable so that we can provide this description. And this works around a problem that is an edge case. | string | `aws-cli` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`) | map | `<map>` | no |
| use_regional_s3_endpoint | When set to 'true' the s3 origin_bucket will use the regional endpoint address instead of the global endpoint address | string | `false` | no |
| viewer_protocol_policy | allow-all, redirect-to-https | string | `redirect-to-https` | no |
| lambda_function_association | A config block that triggers a lambda function with specific actions | list | `<list>` | no |
| web_acl_id | ID of the AWS WAF web ACL that is associated with the distribution | string | `` | no |

## Outputs

| Name | Description |
|------|-------------|
| cf_arn | ARN of AWS CloudFront distribution |
| cf_domain_name | Domain name corresponding to the distribution |
| cf_etag | Current version of the distribution's information |
| cf_hosted_zone_id | CloudFront Route 53 zone ID |
| cf_id | ID of AWS CloudFront distribution |
| cf_status | Current status of the distribution |
| s3_bucket | Name of S3 bucket |
| s3_bucket_domain_name | Domain of S3 bucket |




## Share the Love 

Like this project? Please give it a ★ on [our GitHub](https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn)! (it helps us **a lot**) 

Are you using this project or any of our other projects? Consider [leaving a testimonial][testimonial]. =)


## Related Projects

Check out these related projects.

- [terraform-aws-cloudfront-cdn](https://github.com/cloudposse/terraform-aws-cloudfront-cdn) - Terraform Module that implements a CloudFront Distribution (CDN) for a custom origin.
- [terraform-aws-s3-log-storage](https://github.com/cloudposse/terraform-aws-s3-log-storage) - S3 bucket with built in IAM policy to allow CloudTrail logs



## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## Commercial Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide [*commercial support*][commercial_support] for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a full-time engineer. 

[![E-Mail](https://img.shields.io/badge/email-hello@cloudposse.com-blue.svg)][email]

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll [develop original modules][module_development] to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands-on support to implement our reference architectures. 



## Terraform Module Development

Are you interested in custom Terraform module development? Submit your inquiry using [our form][module_development] today and we'll get back to you ASAP.


## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Newsletter

Signup for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover. 

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2018 [Cloud Posse, LLC](https://cpco.io/copyright)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.  

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.



### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] | [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] | [![Jamie Nelson][Jamie-BitFlight_avatar]][Jamie-BitFlight_homepage]<br/>[Jamie Nelson][Jamie-BitFlight_homepage] | [![Clive Zagno][cliveza_avatar]][cliveza_homepage]<br/>[Clive Zagno][cliveza_homepage] |
|---|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://github.com/osterman.png?size=150
  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://github.com/aknysh.png?size=150
  [Jamie-BitFlight_homepage]: https://github.com/Jamie-BitFlight
  [Jamie-BitFlight_avatar]: https://github.com/Jamie-BitFlight.png?size=150
  [cliveza_homepage]: https://github.com/cliveza
  [cliveza_avatar]: https://github.com/cliveza.png?size=150



[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs
  [website]: https://cpco.io/homepage
  [github]: https://cpco.io/github
  [jobs]: https://cpco.io/jobs
  [hire]: https://cpco.io/hire
  [slack]: https://cpco.io/slack
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://cpco.io/twitter
  [testimonial]: https://cpco.io/leave-testimonial
  [newsletter]: https://cpco.io/newsletter
  [email]: https://cpco.io/email
  [commercial_support]: https://cpco.io/commercial-support
  [we_love_open_source]: https://cpco.io/we-love-open-source
  [module_development]: https://cpco.io/module-development
  [terraform_modules]: https://cpco.io/terraform-modules
  [readme_header_img]: https://cloudposse.com/readme/header/img?repo=cloudposse/terraform-aws-cloudfront-s3-cdn
  [readme_header_link]: https://cloudposse.com/readme/header/link?repo=cloudposse/terraform-aws-cloudfront-s3-cdn
  [readme_footer_img]: https://cloudposse.com/readme/footer/img?repo=cloudposse/terraform-aws-cloudfront-s3-cdn
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?repo=cloudposse/terraform-aws-cloudfront-s3-cdn
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img?repo=cloudposse/terraform-aws-cloudfront-s3-cdn
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?repo=cloudposse/terraform-aws-cloudfront-s3-cdn
  [share_twitter]: https://twitter.com/intent/tweet/?text=terraform-aws-cloudfront-s3-cdn&url=https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=terraform-aws-cloudfront-s3-cdn&url=https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn
  [share_email]: mailto:?subject=terraform-aws-cloudfront-s3-cdn&body=https://github.com/cloudposse/terraform-aws-cloudfront-s3-cdn
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/terraform-aws-cloudfront-s3-cdn?pixel&cs=github&cm=readme&an=terraform-aws-cloudfront-s3-cdn
