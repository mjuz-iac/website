---
layout: page
title: Use Cases
description: µs enables asynchronous deployment, safe undeployment, and reactive updates across teams in addition to the standard infrastructure as code use cases covered by other state-of-the-art solutions.
nav: true
nav_rank: 3
nav_url: /#use-cases # Url to navigate to, e.g., on article page (defaults to .url)
---

In addition to IaC use cases covered by solutions like
[Pulumi](https://www.pulumi.com){: target="_blank" } or [AWS CDK](https://aws.amazon.com/cdk/){: target="_blank" },
µs enables the following three use cases,
which we illustrate with the [webpage example]({{ '/#example' | relative_url }}).

<div class="row">
<div class="col-12 col-lg-4" markdown="1">
## Asynchronous Deployment

µs enables teams to start their deployments independently without synchronization.
Resources are asynchronously deployed once their dependencies are satisfied.
E.g., the editor starts its deployment before the hoster.
The index first remains not deployed as its dependency to the bucket is unsatisfied.
Once the hoster deploys the bucket, µs deploys the editor's index page.
</div>
<div class="col-12 col-lg-4" markdown="1">
## Safe Undeployment

µs ensures that all resources depending on a resource *R*
are not deployed anymore, when *R* is undeployed.
If *R* shall be undeployed, the undeployment of all resources depending on it is triggered
and *R* is only undeployed after completion.
E.g., when the hoster undeploys the bucket, µs automatically undeploys the editor's index page before.
</div>
<div class="col-12 col-lg-4" markdown="1">
## Reactive Updates

µs automatically transports configuration changes across deployments, triggering reactive updates.
E.g., the editor might show the bucket's name (`wish.offer.name`) in the content of the page.
If the hoster updates the name of the bucket,
this change is transported to the editor's deployment,
which subsequently automatically updates the page's content to the new name.
</div>
</div>
