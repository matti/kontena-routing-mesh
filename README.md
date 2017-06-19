# Kontena Routing Mesh

Inspired from https://devcenter.heroku.com/articles/http-routing

```
kontena/lb --> matti/kontena-routing-mesh --> service/web
```

## TODO

- `matti/kontena-routing-mesh` has `web` and `worker` services
- `kontena-routing-mesh/web` links to `kontena/lb` with `KONTENA_LB_VIRTUAL_HOSTS=service.example.com`
- a `service/web` links to `kontena/lb` with `KONTENA_LB_VIRTUAL_HOSTS=service-impl.example.com`
- Browser requests `service.example.com`
  - `kontena-routing-mesh/web` tries to proxy request to `service-impl.example.com`
    - if the response is a 503 from `kontena/lb` then
      - scale service to 1 and retry until timeout
    - else/eventually
      - return the response to the browser
      - store "a request has been made" for `kontena-routing-mesh/worker`:
- (loop) `kontena-routing-mesh/worker` scales `service/web` to 0 if no requests have been made to `service/web` within 30min
