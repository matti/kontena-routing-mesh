# Kontena Service Router

Inspired from https://devcenter.heroku.com/articles/http-routing, but per service.

```
kontena/lb --> matti/kontena-service-router --> service/web
```

## TODO

- `matti/kontena-service-router` has `web` and `worker` services
- `kontena-service-route`/web` links to `kontena/lb` with `KONTENA_LB_VIRTUAL_HOSTS=service.example.com`
- a `service/web` links to `kontena/lb` with `KONTENA_LB_VIRTUAL_HOSTS=service-impl.example.com`
- Browser requests `service.example.com`
  - `kontena-service-router`/web` tries to proxy request to `service-impl.example.com`
    - if the response is a 503 from `kontena/lb` then
      - scale service to 1 and retry until timeout
    - else/eventually
      - return the response to the browser
      - store "a request has been made" for `kontena-service-router/worker`:
- (loop) `kontena-service-router/worker` scales `service/web` to 0 if no requests have been made to `service/web` within 30min
