I think no one will be surprised if I say that OpenAPI specs are awesome. It is common practice to write OAS spec first. But what if you already have a product?

First I deployed the Camunda engine in production somewhere in 2017. At that moment it has a REST API and detailed documentation already.
It was easy to write a client library which I still use in a task management application. Recently I tried to adapt an engine for a completely different task and need a client library for that. "I want to evolve!" I thought and decided to look around to see if there is OAS spec for Camunda engine.

I was surprised but there is [one](https://docs.camunda.org/manual/7.15/reference/rest/openapi/) exists. Specs are distributing in jar packages that could be found [there](https://app.camunda.com/nexus/service/rest/repository/browse/camunda-bpm/org/camunda/bpm/camunda-engine-rest-openapi/). First I fetched a stable distributive 7.15 but the spec was not full enough and I switched to the most recent one (7.16-alpha3). There are recommendations on client generations but I have enough experience and made the same thing in my own way.

```
$ cat Makefile
gen:
    sudo docker run --rm -v "${PWD}:/local" openapitools/openapi-generator-cli generate \
        -i /local/openapi.json \
        -g php \
        -o /local
   
$ make gen
```

Very easy :) But there are subtleties. Using it with PHP version less than 8.0 is hard as hell due to the large number of parameters in API calls. There could be 30-40+ easily. So the brand new PHP 8.0 [named arguments](https://wiki.php.net/rfc/named_params) feature made the client useful.

Example. Fetching process definitions:
```php
$defs = $this->api->ProcessDefinition;
$definitions = $defs->getProcessDefinitions(
    latest_version: true,
    startable_in_tasklist: true
);
```

And `getProcessDefinition` generated definition:
```php
public function getProcessDefinitions($process_definition_id = null,
    $process_definition_id_in = null,
    $name = null,
    $name_like = null,
    $deployment_id = null,
    $deployed_after = null,
    $deployed_at = null,
    $key = null,
    $keys_in = null,
    $key_like = null,
    $category = null,
    $category_like = null,
    $version = null,
    $latest_version = null,
    $resource_name = null,
    $resource_name_like = null,
    $startable_by = null,
    $active = null,
    $suspended = null,
    $incident_id = null,
    $incident_type = null,
    $incident_message = null,
    $incident_message_like = null,
    $tenant_id_in = null,
    $without_tenant_id = null,
    $include_process_definitions_without_tenant_id = null,
    $version_tag = null,
    $version_tag_like = null,
    $without_version_tag = null,
    $startable_in_tasklist = null,
    $not_startable_in_tasklist = null,
    $startable_permission_check = null,
    $sort_by = null,
    $sort_order = null,
    $first_result = null,
    $max_results = null
)
```

I really don't know how to use that mess without named parameters. Firstly I used 7.4 by habit and didn't assume to switch to 8.0 due to fear of problems with symfony 5.3 requirements. But the openapi client forced me to switch and that was seamless so my fears were unfounded.

Such tools as OpenAPI specs and client generation made programmers' jobs even more fun!
