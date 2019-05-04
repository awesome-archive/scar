SCAR - a static website deployment stack
----------------------------------------

Tired of reading outdated Medium posts or combing through verbose AWS
documentation? Deploying static websites on AWS shouldn't be so *scar*y.
**SCAR** is a collection of ["infrastructure as code"](https://en.wikipedia.org/wiki/Infrastructure_as_code)
templates defined using Amazon CloudFormation that make it easy for you to
deploy a static website with a custom domain, SSL, and a CDN. All you need is an
AWS account to get started in three simple steps:

#### 1. Launch a new CloudFormation stack to create all the required AWS resources

[![Launch stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=SCAR&templateURL=https://s3.amazonaws.com/cloudkj/scar_base_template.json) - click to launch the default SCAR stack, or select [another stack from below](#stacks).

#### 2. Update settings at your domain registrar to use the Route 53 name servers

Find the name servers from your newly created [Route 53 hosted zone](https://console.aws.amazon.com/route53/home?#hosted-zones:),
then update the [name server settings at your registrar](#name-server-settings).

#### 3. Validate the domain for your new [ACM certificate](https://console.aws.amazon.com/acm/home)

Find and expand the details of the certificate for your domain, then click on
*"Create record in Route 53"* in the two prompts for validation.

**_That's all, folks!_**

Grab a cup of coffee, wait a few moments, and you should be up and running in
little to no time. Check the [CloudFormation](https://console.aws.amazon.com/cloudformation/home)
console on the status of the stack. Once it has been created, you can upload
the contents of your website directly with the [S3 console](https://s3.console.aws.amazon.com/s3/home),
or use the [AWS CLI](https://aws.amazon.com/cli/) for programmatic control.

## Stacks

The **SCAR** stack is a deployment stack for static websites, using S3,
CloudFront, Amazon Certificate Manager, and Route53. For a given domain such as
`example.com`, the default SCAR stack will set up the following:

* Two S3 buckets, one for storing the contents of your static website
  (`www.example.com`) and another for redirecting requests for the apex domain
  (`example.com`) to the `www` subdomain.
* One public SSL/TLS certificate for both `example.com` and `*.example.com`
* Two CloudFront distributions, one for each S3 bucket.
* One Route 53 hosted zone, with an `A` record for each CloudFront distribution.

The behavior for the default SCAR stack is to redirect all requests for the apex
domain to the `www` subdomain, and to redirect all `http` requests to `https`.

Additional stacks with slight variations from the default stack are also
available:

| Behavior | Default | WWW->Apex |
|-------|---------|-----------|
| Apex domain requests | Redirected to `www` | |
| `www` subdomain requests | | Redirected to apex domain |
| `http` requests | Redirected to `https` | Redirected to `https` |
| | [![Launch stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=SCAR&templateURL=https://s3.amazonaws.com/cloudkj/scar_base_template.json) | ![Launch stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png) (Coming Soon) |

## Name Server Settings

* [GoDaddy](https://www.godaddy.com/help/change-nameservers-for-my-domains-664)
* [Google Domains](https://support.google.com/domains/answer/3290309?hl=en)

## Development

TODO:

* Additional templates for bare domain only, www->root
* See if there's a way to use same set of name servers for Route 53 zone to avoid
  having to always look up name servers
* Include certificate validation using CNAME DNS record as part of template.
  Until official CloudFormation support, potential solutions:
  * https://binx.io/blog/2018/10/05/automated-provisioning-of-acm-certificates-using-route53-in-cloudformation/
  * https://github.com/awslabs/aws-cdk/pull/1797
* Add custom resource to delete the CNAME DNS record used for validation in
  Route 53 hosted zone

## License

Copyright © 2019
