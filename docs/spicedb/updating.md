# Updating SpiceDB

Like all actively developed software, SpiceDB has new versions of the software released on a regular cadence.
Updates are published to the [SpiceDB GitHub releases page] and announced via [Twitter] and [Discord].

Transitioning between versions is often as simple as executing a new binary or container, but there are times when updates are more complex.
For example, releases that include [datastore migrations] will require that users update to specific versions and perform a series of actions in order to avoid downtime.
Another notable example is when a security fix follows our embargo program for all customers before being made public.
In these cases, the release page should contain the specific instructions necessary for updating.

:::warning
SpiceDB attempts to main compatibility across each version and its following minor version.
You should refer to the Upgrade Notes section of each release to find instructions for updating to avoid downtime.
:::

[SpiceDB GitHub releases page]: https://github.com/authzed/spicedb/releases
[Twitter]: https://twitter.com/authzed
[Discord]: https://authzed.com/discord
[datastore migrations]: #what-are-migrations

## Migrations

### What are migrations?

For all software that maintains data, there can be updates to code that rely on also updating the data, too.
The process of updating data to use new versions of software is called _migrating_.

SpiceDB users will see migrations crop up in two places: when they update versions of SpiceDB and when they write backwards incompatible changes to their schema.
This document's focus is on the former.

### SpiceDB migration tooling

SpiceDB ships with a migration command: `spicedb migrate`.
This command powers all of Authzed products' zero down-time migrations, so it's guaranteed to be battle-tested and supportable.
And while you are free to explore other tools to help you migrate, we cannot recommend them.

If you do not care about causing downtime or you are bringing up a new cluster, you can always run the following command to migrate:

```sh
spicedb migrate head
```

In most cases, this command will not actually cause downtime, but one should confirm that's the case before executing on production environments with uptime requirements.

## Recommendations

### Managed services

Rather than handling updates yourself, SpiceDB is offered as a [managed service].
Depending on the service, the update strategy varies:

- SpiceDB Serverless upgrades seamlessly to track the latest release (and sometimes newer)
- SpiceDB Dedicated users can select the desired version of SpiceDB

No matter which service you select, zero-downtime migrations are always performed.

[managed service]: https://authzed.com/pricing

### Operator

If you are operating SpiceDB yourself, the recommended update workflow is to use the [SpiceDB Operator].
Please see the [operator-specific update docs] for various update options.

[SpiceDB Operator]: /spicedb/operator.md
[operator-specific update docs]: /spicedb/operator.md#updating-managed-spicedbclusters

### Sequential Updates

We highly recommend updating sequentially through SpiceDB minor versions (e.g. 1.0.0 -> 1.1.0) to avoid headaches.
Jumping many versions at once might cause you to miss instructions for a particular release that could lead to downtime.
The [SpiceDB Operator](#operator) automates this process.

### Rolling Deployments

We highly recommend a deployment orchestrator to help coordinate rolling out new instances of SpiceDB without dropping traffic.
SpiceDB has been developed with an eye towards running on Kubernetes, but other platforms should be well supported.
