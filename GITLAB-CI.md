## Pipelines

### Example Basic 

```yaml
build:
  script:
     - /bin/true
```

```yaml
build:
  script:
     - /bin/true
     - /bin/false 
```

### Example Node

```yaml
build:
  image: node:10
  script:
    - npm install
    - npm run grunt
```

### Example Pipeline

```yaml
stages:
  - build
  - test

build:
  stage: build
  script:
    - echo "this is building"
    - hostname
    - mkdir builds
    - touch builds/data.txt
    - echo "true" > builds/data.txt
  artifacts:
    paths:
      - builds/

test:
  stage: test
  script:
    - echo "this is testing"
    - hostname
    - test -f builds/data.txt
    - grep "true" builds/data.txt
```

with tags:

```yaml
stages:
  - build
  - test

build:
  stage: build
  tags:
    - my-runner
  script:
    - echo "this is building"
    - hostname
    - mkdir builds
    - touch builds/data.txt
    - echo "true" > builds/data.txt
  artifacts:
    paths:
      - builds/

test:
  stage: test
  tags:
    - my-runner
  script:
    - echo "this is testing"
    - hostname
    - test -f builds/data.txt
    - grep "true" builds/data.txt
```

### Example more stages

```yaml
stages:
  - test
  - build
  - deploy

test:
  - stage: test
  tags:
    - targettrust
  script:
    - echo $CI_COMMIT_REF_NAME
    - echo $CI_JOB_STAGE

build:
  stage: build
  script:
    - echo $CI_COMMIT_REF_NAME
    - echo $CI_BUILD_STAGE

deploy_staging:
  stage: deploy
  script:
    - echo $CI_COMMIT_REF_NAME
    - echo $STAGING
  environment:
    name: staging
    url: http://staging.api.enciso.website
  only:
    - master
```

### Example Pipeline build docker

```yaml
stages: 
  - build

build:
  stage: build
  image: docker:stable
  services:
    - docker:dind
  variables:
    DOCKER_TLS_CERTDIR: ""
  before_script:
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
    - docker info
    - export
  script:
    - docker build . -t $CI_REGISTRY_IMAGE
    - docker push $CI_REGISTRY_IMAGE
```

### Example Pipeline with tags

```yaml
stages: 
  - build

build-master-docker:
  stage: build
  image: docker:stable
  services:
    - docker:dind
  variables:
    DOCKER_TLS_CERTDIR: ""
  before_script:
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
    - docker info
  script:
    - docker build . -t $CI_REGISTRY_IMAGE
    - docker push $CI_REGISTRY_IMAGE
  only:
    - master
    
build-tag-docker: 
  stage: build
  image: docker:stable
  services:
    - docker:dind
  variables:
    DOCKER_TLS_CERTDIR: ""
  before_script:
    - docker login -u "$CI_REGISTRY_USER" -p "$CI_REGISTRY_PASSWORD" $CI_REGISTRY
    - docker info
    - export  
  script:
    - docker build . -t $CI_REGISTRY_IMAGE:$CI_COMMIT_TAG
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_TAG
  only:
    - tags
``` 
