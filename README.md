# Dockerfile for Chrome

This Dockerfile builds an image for Google Chrome in headless mode. It is primarily intended for use in CI/CD environments which use Puppeteer.

An _docker-compose.yml_ file for a codebase which uses Puppeteer only needs to add this and expose port 9222 for the developer tools websocket:
```yml
services:
  ...

  chrome-headless:
    image: videoblocks/docker-chrome-headless
    ports:
      - 9222:9222
```

### Chrome in Non-Headless Mode

This repo also contains a [Dockerfile](ubuntu-chrome/Dockerfile) for launching an Ubuntu instance that runs Chrome in non-headless mode.
