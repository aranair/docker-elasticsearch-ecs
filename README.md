# Docker Elasticsearch in ECS

An elasticsearch docker-compose containers setup that is meant to be hosted on ECS.

## 2.3.X

- Master branch
- http://aranair.github.io/posts/2016/12/05/aws-ecs-elasticsearch-cluster/

## 5.2.2

- Use 5.2.2 branch for ElasticSearch 5.2.2
- https://aranair.github.io/posts/2017/04/03/getting-elasticsearch-5.2.2-to-work-on-amazon-ecs/

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

```
{
    "networkMode": "bridge",
    "taskRoleArn": null,
    "containerDefinitions": [
        {
            "volumesFrom": [],
            "memory": 512,
            "extraHosts": null,
            "dnsServers": null,
            "disableNetworking": null,
            "dnsSearchDomains": null,
            "portMappings": [
                {
                    "hostPort": 9200,
                    "containerPort": 9200,
                    "protocol": "tcp"
                },
                {
                    "hostPort": 9300,
                    "containerPort": 9300,
                    "protocol": "tcp"
                }
            ],
            "hostname": null,
            "essential": true,
            "entryPoint": null,
            "mountPoints": [
                {
                    "containerPath": "/usr/share/elasticsearch/data",
                    "sourceVolume": "elasticsearchdata",
                    "readOnly": null
                }
            ],
            "name": "elasticsearch",
            "ulimits": null,
            "dockerSecurityOptions": null,
            "environment": [
                {
                    "name": "ES_HEAP_SIZE",
                    "value": "256m"
                }
            ],
            "links": null,
            "workingDirectory": null,
            "readonlyRootFilesystem": null,
            "image": "YOUR_DOCKER_REGISTRY_URL:latest",
            "command": null,
            "user": null,
            "dockerLabels": null,
            "logConfiguration": null,
            "cpu": 0,
            "privileged": null,
            "memoryReservation": null
        }
    ],
    "volumes": [
        {
            "host": {
                "sourcePath": "/usr/share/elasticsearch/data"
            },
            "name": "elasticsearchdata"
        }
    ],
    "family": "elasticsearch"
}
```

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
