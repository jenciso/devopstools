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
