---
layout: page
title: Example
description: Webpage example illustrating µs infrastructure as code.
nav: true
nav_rank: 2
nav_url: /#example # Url to navigate to, e.g., on article page (defaults to .url)
---

<div class="row">
    <div class="col-12 col-lg-6" markdown="1">
<figure class="card border float-start me-4">
    <img src="{{ '/assets/img/website.svg' | relative_url }}" alt="Webpage deployment illustration" class="card-header card-img-top" />
    <figcaption class="card-footer figure-caption text-center border-0">Webpage.</figcaption>
</figure>
Lets consider a simple static website with a single `index.html` page hosted in an AWS S3 bucket.
While this is unrealistically simple, it suffices to showcase our approach.
You can find the full source code and instructions to execute it yourself in [our <i></i>{: .fab .fa-github } repository](https://github.com/mjuz-iac/mjuz/tree/main/webpage){: target="_blank" }.

In descriptive infrastructure as code (IaC) systems, users define a directed acyclic graph (DAG)
where each node is a resource, e.g., a db, container, load balancer, or network ACL entry,
and arcs are dependencies between them.
These dependencies are transitive and order the deployment,
i.e., if resource *R* depends on *S*, *S* must be deployed before *R* and *R* must not be deployed when *S* is not deployed.
For the webpage,
the index must be deployed after and undeployed before the bucket.
Have a look at the deployment graph and code describing it for µs using regular TypeScript.
</div>
    <div class="col-12 col-lg-6">
        <figure class="card border">
            <div class="card-header">
                <div class="row justify-content-center">
                    <img class="col-5 col-xxl-4 mb-3" src="{{ '/assets/img/website-centralized.svg' | relative_url }}" alt="Webpage deployment graph" />
                    <div class="col-12 col-xxl-8" markdown="1">
```ts
const bucket = new Bucket('bucket', {
    website: { indexDocument: 'index.html' }
}); // Creates the S3 bucket of the website
const index = new BucketObject('index', {
    bucket: bucket,
    content: content,
    key: 'index.html',
    contentType: 'text/html; charset=utf-8'
}); // Saves the index.html in the bucket
```
{: .mb-n3 }
</div>
                </div>
            </div>
            <figcaption class="card-footer figure-caption text-center border-0">Webpage deployment graph and the µs deployment definition.</figcaption>
        </figure>
    </div>
</div>

<div class="row">
    <div class="col-12 col-lg-6" markdown="1">
In DevOps organizations, ideally,
each team operates its own resources, including deployment.
To ensure correct deployment and undeployment order, teams need to manually coordinate
whenever resources are deployed, updated or undeployed.
This decreases the flexibility of the teams and wastes time due to synchronization.
To decouple the deployment times,
µs treats deployments not as one-off tasks – as it is the case with all common IaC systems –
but as continuously running processes,
updating the deployed resources reactively based on the deployment definition
*and* external signals, e.g., changes in another deployment.

In the website example, the *hoster* could be responsible for the bucket
and the *editor* for the index page in it.
To ensure that the page is only and correctly deployed when the bucket is,
both teams need to manually coordinate whenever the bucket
is deployed, updated or undeployed.
This means that the editor starts their deployment independently and runs it continuously.
Whenever the hoster starts or updates their deployment,
the editor's deployment automatically deploys, updates and undeploys the index page,
without manual intervention or synchronization.

To enable such behavior,
the connections between deployments and
inter-deployment dependencies of their resources must be explicit in the deployment definitions.
µs introduces three new resource types:
`RemoteConnection` for connections to other deployments,
`Offer` to provide resources or information to a connected deployment,
and `Wish` to access the offer of a connected deployment.
Using them, the hoster and editor specify their connection to the other deployment.
The hoster offers its bucket to the editor deployment.
The editor specifies its expectation of the offer
by defining a wish, allowing to use the offered bucket via `wish.offer`.
</div>
    <div class="col-12 col-lg-6">
        <figure class="card border">
            <div class="card-header text-center">
                <img class="" src="{{ '/assets/img/website-decentralized.svg' | relative_url }}" alt="Decentralized webpage deployment graph using µ_s" />
            </div>
            <figcaption class="card-footer figure-caption text-center border-0">Decentralized webpage deployment graph using µs.</figcaption>
        </figure>
        <figure class="card border">
            <div class="card-header" markdown="1">
```ts
const editor = new RemoteConnection('editor');
const bucket = new Bucket('bucket', {
    website: { indexDocument: 'index.html' }
});
new Offer(editor, 'bucket', bucket);
```
{: .mb-n3 }
</div>
            <figcaption class="card-footer figure-caption text-center border-0">µs deployment definition of the hoster.</figcaption>
        </figure>
        <figure class="card border">
            <div class="card-header" markdown="1">
```ts
const hoster = new RemoteConnection('hoster');
const wish = new Wish<Bucket>(hoster, 'bucket');
const index = new BucketObject('index', {
    bucket: wish.offer,
    content: content,
    key: 'index.html',
    contentType: 'text/html; charset=utf-8'
});
```
{: .mb-n3 }
</div>
            <figcaption class="card-footer figure-caption text-center border-0">µs deployment definition of the editor.</figcaption>
        </figure>
    </div>
</div>
