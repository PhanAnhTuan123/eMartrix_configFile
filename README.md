# eMartix Configuration Repository

This repository holds centralized configuration files for the eMartix microservices platform, served by Spring Cloud Config Server. It includes service-specific YAML files and a default `application.yml`.

## 📁 Repository Structure

```
eMartix_configFile/
├── .idea/                         # IDE project settings (optional)
├── application.yml               # Global default configuration
├── auth-service.yml              # Config for Auth Service
├── cart-service.yml              # Config for Cart Service
├── gateway-service.yml           # Config for API Gateway
├── product-service.yml           # Config for Product Service
├── service-registry.yml          # Config for Service Registry (Eureka)
└── README.md                     # This file
```

## 🔧 Usage with Spring Cloud Config Server

1. **Clone the repo** into your Config Server’s backend (e.g. a Git-backed config store).

2. **Configure Config Server**  
   In `config-server`’s `application.yml`:
   ```yaml
   spring:
     cloud:
       config:
         server:
           git:
             uri: https://github.com/PhanAnhTuan123/eMartrix_configFile.git
             default-label: main
   ```
3. **Service Bootstrap**  
   Each Spring Boot microservice must include:
   ```yaml
   spring:
     application:
       name: <service-name>      # e.g. auth-service
     cloud:
       config:
         uri: http://localhost:8888
         name: ${spring.application.name}
         profile: ${spring.profiles.active:default}
   ```
   On startup, the microservice will fetch `<name>-<profile>.yml` from Config Server. If `profile` is omitted or `default`, it falls back to `application.yml`.

## 🌐 Service-Specific Configuration

- **`application.yml`**:  
  Global defaults (ports, actuator settings, logging).

- **`auth-service.yml`**:  
  JWT secrets, OAuth2 endpoints, mail server settings.
  
- **`cart-service.yml`**:  
  Redis connection, cache TTL, feature toggles.
  
- **`gateway-service.yml`**:  
  Route definitions, CORS, rate limit and circuit breaker rules.
  
- **`product-service.yml`**:  
  Database (JPA) settings, search index configuration.
  
- **`service-registry.yml`**:  
  Eureka server settings (hostname, port, instance lease TTL).

## ⚙️ Profiles & Overrides

You can add profile-specific overrides by naming files as `<service>-<profile>.yml`, for example:

- `auth-service-dev.yml`
- `auth-service-prod.yml`

And activate via `--spring.profiles.active=prod` when running services.

## 🔄 Updating Configuration

1. Edit the appropriate YAML file in this repo.  
2. Commit & push your changes.  
3. Config Server automatically picks up changes (if enabled with webhook or `spring.cloud.config.server.git.refreshRate`).  
4. Clients will fetch new config on next refresh (via `/actuator/refresh` or native refresh mechanisms).

## 👥 Contributing

- Follow Git branching conventions:  
  - `main` for production-ready config.  
  - `feature/*` for work in progress.  
- Use clear commit messages: `feat(auth): rotate jwt secret`, `fix(cart): correct redis TTL`.

## 📄 License

This repository follows the same license as the eMartix platform. See the root LICENSE file for details.
