## Docker Elasticsearch in ECS

An elasticsearch docker-compose containers setup that is meant to be hosted on ECS.

### Run it locally

```
docker-compose up
```

Yup that's it.

### To run it on ECS

Please refer to this blog post that I put up for a step-by-step walkthrough.

### Notable configurations

**elasticsearch.yml**: `discovery.ec2.groups` should be set to the security group that was assigned when you created the ECS cluster. Do not remove `network.host` or `network.publish_host`


```
script.inline: true
bootstrap.mlockall: true
network.host: 0.0.0.0
plugin.mandatory: cloud-aws
network.publish_host: _ec2:privateIp_
discovery.type: ec2
discovery.ec2.groups: sg-docker-es-1
discovery.zen.ping.multicast.enabled: false
```

**logging.yml**: Currently the logging level is DEBUG which generates all kinds of logs, you may want toswitch it to `INFO` after getting initial set up done.

```
es.logger.level: DEBUG
```

