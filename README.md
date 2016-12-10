# Docker Elasticsearch in ECS

An elasticsearch docker-compose containers setup that is meant to be hosted on ECS.

## Run it locally

```
docker-compose up
```

Yup that's it.

## To push the docker images to a repository

Update `push-tag.sample` in the bin folder with your own docker registry repository.

Then `bin/push-tag`

## Run it on ECS

Most of the legwork has already been done in the configurations so it should be fairly easy.

This [blog post][blog post] that I wrote runs through some of the details inside the files and
configurations that you should have in the ECS and task definitions.

Also feel free to refer to [ecs-task-definition.sample][task definition sample] as a start to guide you.

## Notable configurations

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

## LICENSE

MIT

[blog post]: http://aranair.github.io/posts/2016/12/05/aws-ecs-elasticsearch-cluster/
[task definition sample]: https://github.com/aranair/docker-elasticsearch-ecs/blob/master/ecs-task-definition.sample
