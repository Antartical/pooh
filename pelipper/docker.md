## How can I use it in my docker environment?

Pelipper has the following dependencies:
- mailhog: SMTP server through the ones the email will be delivered


```yml
mailhog:
  image: mailhog/mailhog
  container_name: mailhog
  ports: 
    - 1025:1025
    - 8025:8025

pelipper:
  image: ghcr.io/antartical/pelipper
  container_name: pelipper
  ports:
    - "9000:9000"
  environment:
    - SMTP_HOST=mailhog
    - SMTP_PORT=1025
    - SMTP_USER=admin
    - SMTP_PASSWORD=admin
```
