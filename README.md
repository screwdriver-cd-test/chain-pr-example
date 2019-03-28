# Chain PR Example

To use chain PR feature in Screwdriver v4, set `true` in pipeline-level annotation `screwdriver.cd/chainPR`.  
Then, subsequent jobs in PR event will be chained according to the workflow in PR.

### Example

Firstly, set `true` in the `screwdriver.cd/chainPR` annotaion.  
This annotation must be configured in ***main branch***.

```
# screwdriver.yaml in main branch

shared:
    image: node:8

annotations:
  screwdriver.cd/chainPR: true

jobs:
    first-job:
        requires: [ ~pr, ~commit ]
        steps:
            - echo: echo "this is first job."
    second-job:
        requires: [ first-job ]
        steps:
            - echo: echo "this is second job."
```

Then, if you make a PR, subsequent jobs in PR branch will be chained according to workflow in PR evnet.  
In this example, `second-job` will executed after the `first-job`, and `third-job` will be executed after the `second-job`.
```
# screwdriver.yaml in PR branch

shared:
    image: node:8

jobs:
    first-job:
        requires: [ ~pr, ~commit ]
        steps:
            - echo: echo "this is first job."
    second-job:
        requires: [ first-job ]
        steps:
            - echo: echo "this is second job."
    third-job:
        requires: [ second-job ]
        steps:
            - echo: echo "this is third job."
```
