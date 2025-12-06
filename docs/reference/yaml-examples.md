---
title: YAML Examples
layout: default
parent: Reference
nav_order: 5
---

# YAML Examples

Common YAML patterns and configuration examples.

{: .fs-6 .fw-300 }

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## YAML Basics

### Syntax Rules

```yaml
# Comments start with hash
key: value

# Strings (quotes optional for simple strings)
name: John Doe
city: "New York"
message: 'Hello, World!'

# Numbers
age: 30
price: 19.99
scientific: 1.5e+10

# Booleans
is_active: true
is_deleted: false
enabled: yes
disabled: no

# Null values
empty: null
also_empty: ~
```

### Data Structures

#### Lists

```yaml
# List with dashes
fruits:
  - apple
  - banana
  - orange

# Inline list
colors: [red, green, blue]

# Nested lists
matrix:
  - [1, 2, 3]
  - [4, 5, 6]
  - [7, 8, 9]
```

#### Dictionaries (Maps)

```yaml
# Dictionary
person:
  name: John Doe
  age: 30
  city: New York

# Inline dictionary
user: {name: John, age: 30}

# Nested dictionaries
company:
  name: TechCorp
  address:
    street: 123 Main St
    city: Boston
    country: USA
```

---

## GitHub Actions

### Simple Workflow

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
```

### Matrix Strategy

```yaml
name: Multi-Platform Tests

on: [push]

jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
        node-version: [16, 18, 20]
        include:
          - os: ubuntu-latest
            node-version: 20
            experimental: true
        exclude:
          - os: windows-latest
            node-version: 16
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm ci
      - run: npm test
```

### Conditional Steps

```yaml
name: Deploy

on:
  push:
    branches: [main]
    tags:
      - 'v*'

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy to staging
        if: github.ref == 'refs/heads/main'
        run: ./deploy-staging.sh
      
      - name: Deploy to production
        if: startsWith(github.ref, 'refs/tags/v')
        run: ./deploy-production.sh
      
      - name: Run on schedule only
        if: github.event_name == 'schedule'
        run: ./cleanup.sh
```

---

## Docker Compose

### Full Stack Application

```yaml
version: '3.8'

services:
  # Frontend
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
      args:
        - NODE_ENV=production
    ports:
      - "3000:3000"
    environment:
      - API_URL=http://backend:5000
    depends_on:
      - backend
    networks:
      - frontend-network
    restart: unless-stopped

  # Backend API
  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgresql://user:${DB_PASSWORD}@db:5432/myapp
      - REDIS_URL=redis://cache:6379
      - JWT_SECRET=${JWT_SECRET}
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started
    volumes:
      - ./backend:/app
      - /app/node_modules
    networks:
      - frontend-network
      - backend-network
    restart: unless-stopped

  # Database
  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_DB=myapp
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - backend-network
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  # Cache
  cache:
    image: redis:7-alpine
    command: redis-server --appendonly yes
    volumes:
      - redis-data:/data
    networks:
      - backend-network
    restart: unless-stopped

  # Worker
  worker:
    build: ./backend
    command: python worker.py
    environment:
      - DATABASE_URL=postgresql://user:${DB_PASSWORD}@db:5432/myapp
      - REDIS_URL=redis://cache:6379
    depends_on:
      - db
      - cache
    networks:
      - backend-network
    restart: unless-stopped

networks:
  frontend-network:
    driver: bridge
  backend-network:
    driver: bridge

volumes:
  postgres-data:
  redis-data:
```

### Development Override

```yaml
# docker-compose.override.yml
version: '3.8'

services:
  frontend:
    build:
      target: development
    volumes:
      - ./frontend:/app
      - /app/node_modules
    command: npm run dev

  backend:
    volumes:
      - ./backend:/app
      - /app/node_modules
    command: npm run dev
    environment:
      - DEBUG=true

  db:
    ports:
      - "5432:5432"
```

---

## Kubernetes

### Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
        version: v1
    spec:
      containers:
      - name: web
        image: myapp:1.0
        ports:
        - containerPort: 8080
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url
        - name: CACHE_ENABLED
          value: "true"
        resources:
          requests:
            memory: "128Mi"
            cpu: "250m"
          limits:
            memory: "256Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
```

### Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
spec:
  selector:
    app: web
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
  type: LoadBalancer
```

### ConfigMap and Secret

```yaml
# ConfigMap
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  app.properties: |
    env=production
    debug=false
  database.host: "db.example.com"
  api.timeout: "30"

---
# Secret
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: dXNlcm5hbWU=  # base64 encoded
  password: cGFzc3dvcmQ=  # base64 encoded
stringData:
  url: "postgresql://user:pass@db:5432/myapp"
```

---

## Ansible

### Playbook

```yaml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  vars:
    http_port: 80
    max_clients: 200
  
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"
    
    - name: Install nginx
      package:
        name: nginx
        state: present
    
    - name: Copy nginx configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        owner: root
        group: root
        mode: '0644'
      notify: Restart nginx
    
    - name: Ensure nginx is running
      service:
        name: nginx
        state: started
        enabled: yes
    
    - name: Create web directory
      file:
        path: /var/www/html
        state: directory
        owner: www-data
        group: www-data
        mode: '0755'
  
  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
```

### Inventory

```yaml
# inventory.yml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
          ansible_host: 192.168.1.10
        web2.example.com:
          ansible_host: 192.168.1.11
      vars:
        ansible_user: deploy
        ansible_port: 22
    
    databases:
      hosts:
        db1.example.com:
          ansible_host: 192.168.1.20
          db_role: primary
        db2.example.com:
          ansible_host: 192.168.1.21
          db_role: replica
      vars:
        ansible_user: dbadmin
```

---

## CI/CD Examples

### GitLab CI

```yaml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_REF_SLUG

before_script:
  - echo "Starting job $CI_JOB_NAME"

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $DOCKER_IMAGE .
    - docker push $DOCKER_IMAGE
  only:
    - main
    - develop

test:unit:
  stage: test
  image: node:18
  script:
    - npm ci
    - npm run test:unit
  coverage: '/Coverage: \d+\.\d+%/'
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

test:integration:
  stage: test
  image: node:18
  services:
    - postgres:15
  variables:
    POSTGRES_DB: testdb
    POSTGRES_USER: user
    POSTGRES_PASSWORD: password
  script:
    - npm ci
    - npm run test:integration

deploy:staging:
  stage: deploy
  script:
    - ./deploy.sh staging
  environment:
    name: staging
    url: https://staging.example.com
  only:
    - develop

deploy:production:
  stage: deploy
  script:
    - ./deploy.sh production
  environment:
    name: production
    url: https://example.com
  only:
    - main
  when: manual
```

---

## Application Configuration

### Spring Boot (application.yml)

```yaml
spring:
  application:
    name: myapp
  
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: ${DB_USERNAME:user}
    password: ${DB_PASSWORD:password}
    driver-class-name: org.postgresql.Driver
    hikari:
      maximum-pool-size: 10
      minimum-idle: 5
      connection-timeout: 30000
  
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: false
    properties:
      hibernate:
        format_sql: true
        dialect: org.hibernate.dialect.PostgreSQLDialect
  
  cache:
    type: redis
    redis:
      time-to-live: 3600000
  
  redis:
    host: localhost
    port: 6379
    password: ${REDIS_PASSWORD:}
    timeout: 2000ms

server:
  port: 8080
  servlet:
    context-path: /api
  compression:
    enabled: true
    mime-types: text/html,text/xml,text/plain,application/json

logging:
  level:
    root: INFO
    com.example: DEBUG
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} - %msg%n"
  file:
    name: logs/application.log
    max-size: 10MB
    max-history: 30

management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics
  endpoint:
    health:
      show-details: when-authorized
```

---

## Advanced YAML Features

### Anchors and Aliases

```yaml
# Define anchor
default_settings: &defaults
  timeout: 30
  retries: 3
  log_level: info

# Use alias
development:
  <<: *defaults
  log_level: debug

production:
  <<: *defaults
  timeout: 60
```

### Multi-line Strings

```yaml
# Literal block (preserves newlines)
literal: |
  This text will preserve
  all newlines and
  spacing exactly as written.

# Folded block (joins lines)
folded: >
  This text will be
  folded into a single
  line with spaces.

# With strip/keep modifiers
strip_last: |-
  No newline at end
  
keep_last: |+
  Keeps newlines at end


# Inline
inline: "Single line text"
```

### Complex Keys

```yaml
# Complex key (map as key)
? {name: John, age: 30}
: employee_id: 12345

# List as key
? [red, green, blue]
: primary_colors
```

---

## Best Practices

### Do's

```yaml
# ✅ Use consistent indentation (2 spaces)
services:
  web:
    image: nginx

# ✅ Quote strings with special characters
message: "Hello: World"
path: "/usr/local/bin"

# ✅ Use meaningful names
database_connection_timeout: 30

# ✅ Group related configuration
database:
  host: localhost
  port: 5432
  credentials:
    username: user
    password: pass
```

### Don'ts

```yaml
# ❌ Don't use tabs for indentation
# ❌ Don't mix indentation styles

# ❌ Don't use ambiguous values without quotes
value: yes  # Could be boolean or string
value: "yes"  # Clearly a string

# ❌ Don't make deeply nested structures
# (more than 3-4 levels becomes hard to read)
```

---

## Validation

### Online Tools

- [YAML Lint](http://www.yamllint.com/)
- [YAML Validator](https://jsonformatter.org/yaml-validator)

### Command Line

```bash
# Using Python
python -c 'import yaml, sys; yaml.safe_load(sys.stdin)' < file.yaml

# Using Ruby
ruby -ryaml -e 'YAML.load_file("file.yaml")'

# Using yamllint
yamllint file.yaml
```

---

## Common Gotchas

### Boolean Values

```yaml
# These are all boolean true
yes: yes
on: on
true: true

# These are all boolean false
no: no
off: off
false: false

# To use as strings, quote them
country: "no"  # Norway
status: "off"  # Not boolean
```

### Numbers

```yaml
# These are numbers
version: 3.8
count: 42
octal: 0755
hex: 0xFF

# To use as strings, quote them
version: "3.8"
zip_code: "00501"
```

---

## Related Topics

- [Configuration Examples]({% link docs/reference/config-examples.md %})
- [Docker Quick Reference]({% link docs/reference/docker-reference.md %})
- [Git Commands]({% link docs/reference/git-commands.md %})
