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
E.g., the editor starts its deployment before the provider.
The index first remains not deployed as its dependency to the bucket is unsatisfied.
Once the provider deploys the bucket, µs deploys the editor's index page.
</div>
<div class="col-12 col-lg-4" markdown="1">
## Safe Undeployment

µs ensures that all resources depending on a resource *R*
are not deployed anymore, when *R* is undeployed.
If *R* shall be undeployed, the undeployment of all resources depending on it is triggered
and *R* is only undeployed after completion.
E.g., when the provider undeploys the bucket, µs automatically undeploys the editor's index page before.
</div>
<div class="col-12 col-lg-4" markdown="1">
## Reactive Updates

µs automatically transports configuration changes across deployments, triggering reactive updates.
E.g., the editor might show the bucket's name (`wish.offer.name`) in the content of the page.
If the provider updates the name of the bucket,
this change is transported to the editor's deployment,
which subsequently automatically updates the page's content to the new name.
</div>
</div>

<div id="uc-anim" class="row">
    <input type="radio" name="slides" id="play" checked />
    {% for slide in (1..15) %}
        <input type="radio" name="slides" id="slide-{{ slide }}" />
    {% endfor %}
    <div class="controls col-lg-6 my-lg-3">
        <div class="card border col-12">
            <img class="card-img-top p-3 inject-svg" src="{{ '/assets/img/website-decentralized.svg' | relative_url }}" alt="Animated deployment use cases animation." />
            <div class="card-footer text-center">
                <label class="form-check-input" type="radio" for="play"><i class="fas fa-play"></i></label>
                {% for slide in (1..15) %}
                    <label class="form-check-input" type="radio" for="slide-{{ slide }}"></label>
                {% endfor %}
            </div>
        </div>
    </div>
    <div class="captions col-12 col-lg-6 my-3">
        <div class="card list-group">
            <label for="slide-1" class="list-group-item list-group-item-action">The editor starts their deployment.
                <div class="list-group-flush">
                    <label for="slide-2" class="list-group-item list-group-item-action">The remote connection to the provider is deployed.</label>
                    <label for="slide-3" class="list-group-item list-group-item-action">The wish for the bucket is deployed but unsatisfied. Therefore, index is not deployed.</label>
                </div>
            </label>
            <label for="slide-4" class="list-group-item list-group-item-action">The provider starts their deployment.
                <div class="list-group-flush">
                    <label for="slide-5" class="list-group-item list-group-item-action">The remote connection to the editor and the bucket are deployed in parallel.</label>
                    <label for="slide-6" class="list-group-item list-group-item-action">The offer is deployed.</label>
                    <label for="slide-7" class="list-group-item list-group-item-action">The offer is forwarded to the editor.</label>
                    <label for="slide-8" class="list-group-item list-group-item-action">The index is deployed automatically, because the wish it depends on is now satisfied.</label>
                </div>
            </label>
            <label for="slide-9" class="list-group-item list-group-item-action">The provider stops their deployment.
                <div class="list-group-flush">
                    <label for="slide-10" class="list-group-item list-group-item-action">The offer withdrawal is sent to the editor deployment, which automatically undeploys the index, because the wish turns unsatisfied.</label>
                    <label for="slide-11" class="list-group-item list-group-item-action">The editor signals that no resource depending on the wish is deployed anymore, which completes the undeployment of the offer.</label>
                    <label for="slide-12" class="list-group-item list-group-item-action">The remote connection to the editor and the bucket are undeployed in parallel.</label>
                </div>
            </label>
            <label for="slide-13" class="list-group-item list-group-item-action">The editor stops their deployment.
                <div class="list-group-flush">
                    <label for="slide-14" class="list-group-item list-group-item-action">The wish is undeployed.</label>
                    <label for="slide-15" class="list-group-item list-group-item-action">The remote connection to the provider is undeployed.</label>
                </div>
            </label>
        </div>
    </div>
</div>
